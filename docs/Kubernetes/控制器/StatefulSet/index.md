# StatefulSet 控制器
在 Kubernetes 中，**StatefulSet 控制器**是用于管理**有状态应用**的核心控制器。与 Deployment 控制器不同，StatefulSet 确保每个 Pod 都具有稳定的、独一无二的身份，即使它们被重新调度或重启。

简单来说，StatefulSet 为每个 Pod 提供了一个持久的“身份卡”，这张卡包括：

1.  **稳定的网络标识**：Pod 的主机名和网络标识在重启后保持不变。
2.  **稳定的持久存储**：每个 Pod 都有一个与其生命周期绑定的、独有的持久卷。



## StatefulSet 的核心特性

StatefulSet 的设计初衷是为了解决有状态应用的特定需求，比如数据库、消息队列或分布式文件系统。其核心特性包括：

### 1. 稳定的网络标识（Stable Network ID）

每个 StatefulSet 的 Pod 都会被赋予一个独特的、可预测的主机名和 DNS 标识。例如，一个名为 `web` 的 StatefulSet，其 Pod 将被命名为 `web-0`, `web-1`, `web-2`，并且它们的主机名也与此一致。这使得集群中的其他 Pod 可以通过这些稳定的主机名来发现并连接到特定的 Pod。

### 2. 有序的部署和伸缩

StatefulSet 严格按照顺序启动和停止 Pod。

* **部署**：Pod 按照从 `0` 到 `N-1` 的索引顺序创建。只有当 `web-0` 成功运行后，`web-1` 才会开始创建，依此类推。
* **缩容**：Pod 按照从 `N-1` 到 `0` 的索引顺序终止。例如，如果从 3 个副本缩减到 2 个，`web-2` 会首先被终止。

这种有序性对于有状态应用（如主从数据库）至关重要，因为它确保了服务的正确启动和关闭顺序。

### 3. 稳定的持久存储（Stable Persistent Storage）

StatefulSet 会为每个 Pod 创建一个独立的持久卷（PersistentVolumeClaim）。这些卷与 Pod 的身份（例如 `web-0`）绑定，即使 Pod 被删除并重新创建，它也会重新挂载到相同的持久卷上。这保证了 Pod 数据的持久性，使得应用状态得以保存。


## StatefulSet 与 Deployment 的对比

| 特性 | **Deployment** (无状态) | **StatefulSet** (有状态) |
| :--- | :--- | :--- |
| **应用类型** | 无状态应用（Web 服务器、微服务） | 有状态应用（数据库、消息队列） |
| **Pod 身份** | 无独特身份，Pod 可相互替换 | 独一无二、可预测的身份（例如 `web-0`） |
| **Pod 命名** | 随机哈希后缀（`nginx-6799fc88d8-fghij`） | 有序索引后缀（`nginx-0`, `nginx-1`） |
| **启动/终止顺序** | 无序，并行启动和终止 | 严格按照索引顺序启动和终止 |
| **持久存储** | 通常无独立存储，或共享存储 | 每个 Pod 都有独立的、稳定的持久卷 |
| **使用场景** | 无需保存会话的应用、可扩展的 API | 数据库、缓存系统、分布式文件系统 |



## 总结

**StatefulSet 控制器**是 Kubernetes 中专门为有状态应用设计的工具。它通过提供稳定的网络标识、有序的部署和独有的持久存储，确保了应用的持久性、可预测性和可靠性。如果你需要部署一个数据库集群或任何需要保持数据和身份的应用，**StatefulSet** 是你的首选。


在 Kubernetes 中，**StatefulSet 控制器** 是用于管理**有状态应用**的核心控制器。它通过为 Pod 提供稳定的、独一无二的身份来确保数据持久性和应用的有序性。

-----

## StatefulSet 案例

### StatefulSet 的核心使用案例

与 Deployment 用于无状态应用不同，StatefulSet 专为那些需要维护状态的应用而设计，例如：

1.  **数据库**：无论是单机数据库（如 MySQL、PostgreSQL）还是分布式数据库集群（如 Cassandra、MongoDB），都需要 Pod 拥有稳定的网络标识和持久存储。StatefulSet 可以确保每个数据库 Pod 都有唯一的名称（例如 `db-0`、`db-1`），并始终连接到相同的持久卷。

2.  **消息队列**：像 Kafka、RabbitMQ 这样的消息队列需要 Pod 以特定的顺序启动和停止。StatefulSet 的有序部署和缩容机制完美匹配这一需求，确保了服务的可靠性。

3.  **分布式文件系统**：对于像 Ceph、GlusterFS 这样的分布式存储，每个节点都需要一个稳定的网络身份来参与集群。StatefulSet 确保每个 Pod 的身份都是可预测的，便于集群内部的成员发现。

-----

### StatefulSet 示例：部署 MySQL 主从复制

下面是一个使用 StatefulSet 部署一个简单的 MySQL 主从复制集群的简化案例。这个例子展示了 StatefulSet 如何提供稳定的网络身份和持久存储。

首先，你需要定义一个 **Headless Service** 来为 Pod 提供稳定的网络标识：

```yaml
# mysql-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql-headless
  labels:
    app: mysql
spec:
  ports:
  - port: 3306
    name: mysql
  clusterIP: None  # 关键：创建 Headless Service
  selector:
    app: mysql
```

`clusterIP: None` 是关键，它告诉 Kubernetes 不分配一个负载均衡 IP，而是通过 DNS 为每个 Pod 创建独立的记录（例如 `mysql-0.mysql-headless`）。

接下来，定义 **StatefulSet** 本身：

```yaml
# mysql-statefulset.yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql-headless  # 绑定到 Headless Service
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: mypassword
        ports:
        - containerPort: 3306
  volumeClaimTemplates:
  - metadata:
      name: mysql-data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 10Gi
```

在这个配置中：

  * `serviceName: mysql-headless`：将 StatefulSet 与 Headless Service 关联。
  * `replicas: 3`：定义了 3 个 Pod 副本，它们将按顺序命名为 `mysql-0`、`mysql-1`、`mysql-2`。
  * `volumeClaimTemplates`：这是 StatefulSet 的核心特性。它会为每个 Pod 自动创建一个**独立的持久卷声明（PVC）**。例如，`mysql-0` 将获得一个名为 `mysql-data-mysql-0` 的 PVC，并与之永久绑定。

-----

### StatefulSet 常用管理命令

管理 StatefulSet 的命令与管理其他 Kubernetes 对象类似，但有一些特定于其有序性的行为：

1.  **创建 StatefulSet**：

    ```bash
    kubectl apply -f mysql-service.yaml
    kubectl apply -f mysql-statefulset.yaml
    ```

    Kubernetes 会首先创建 `mysql-0`，当它就绪后，再创建 `mysql-1`，以此类推。

2.  **查看 StatefulSet 列表**：

    ```bash
    kubectl get statefulset
    kubectl get pods -l app=mysql
    ```

3.  **缩容 StatefulSet**：

    ```bash
    kubectl scale statefulset mysql --replicas=1
    ```

    当从 3 个副本缩容到 1 个时，Kubernetes 会首先终止 `mysql-2`，然后是 `mysql-1`。这种倒序终止对于数据库集群来说至关重要，可以确保从节点在主节点之前被移除。

4.  **删除 StatefulSet**：

    ```bash
    kubectl delete statefulset mysql
    ```

    默认情况下，删除 StatefulSet **不会**删除其关联的 Pod 和持久卷。这是为了防止数据意外丢失。你需要手动删除 Pod 和 PVC：

    ```bash
    kubectl delete pods -l app=mysql
    kubectl delete pvc -l app=mysql
    ```