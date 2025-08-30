# DaemonSet 控制器
在 Kubernetes 中，**DaemonSet 控制器** 是一种特殊的控制器，它确保**集群中的每个（或部分）节点上都运行一个 Pod 副本**。

与 Deployment 和 StatefulSet 不同，DaemonSet 关注的不是 Pod 的副本数量，而是 Pod 在**每个节点**上的存在。它的主要用途是运行那些需要在每个节点上执行的系统级后台任务。

---

### DaemonSet 的工作原理

DaemonSet 控制器会持续监控集群中的节点。当一个新节点加入集群时，它会自动在上面调度一个 DaemonSet Pod。如果一个节点被移除，该节点上的 Pod 也会被垃圾回收。

这就像一个“守护神”，它会确保你的指定任务在集群的每个角落都始终保持运行。

### 常见的 DaemonSet 用例

DaemonSet 非常适合以下场景：

* **日志收集器**：例如，在每个节点上运行一个 **Fluentd** 或 **Logstash** 代理，负责收集该节点上的所有日志，并将其发送到中央日志系统。
* **监控代理**：在每个节点上运行一个 **Prometheus Node Exporter** 或其他监控代理，用于收集节点的性能指标（CPU、内存、磁盘等）。
* **网络插件**：许多网络插件（如 **Calico** 或 **Flannel**）都以 DaemonSet 的形式部署，以确保每个节点都有一个网络代理来管理其网络。
* **集群存储守护进程**：例如，在每个节点上运行一个存储守护进程，以使节点能够访问特定的存储卷。

---

### DaemonSet 的关键特性

* **一对一节点-Pod 关系**：一个节点上通常只有一个 DaemonSet Pod。除非你明确地配置了选择器，否则 DaemonSet 会部署到所有满足条件的节点上。
* **自动部署**：当新节点加入集群时，DaemonSet 会自动在该节点上创建 Pod，无需手动干预。
* **自动清理**：当节点从集群中移除时，DaemonSet 会自动删除该节点上的 Pod，以释放资源。
* **节点选择器**：你可以使用 **`nodeSelector`** 或 **`affinity`** 来限制 DaemonSet 只在符合特定标签的节点上运行。这对于混合类型的集群非常有用，例如，你可能只想在有 GPU 的节点上运行特定的 DaemonSet。

---

### DaemonSet 与 Deployment 的区别

理解 DaemonSet 的关键是将其与 Deployment 进行对比：

| 特性 | **DaemonSet** | **Deployment** |
| :--- | :--- | :--- |
| **部署策略** | 每个（或部分）节点一个 Pod | 指定数量的 Pod 副本 |
| **主要目标** | 确保每个节点都有后台任务 | 确保应用副本数，提供高可用性 |
| **Pod 数量** | 由集群中符合条件的节点数决定 | 由用户在 `replicas` 中指定 |
| **典型用例** | 日志收集、监控、网络插件 | Web 服务、API 网关、微服务 |

**总结：** 如果你的目标是让一个守护进程在所有节点上运行，那么 **DaemonSet** 是最合适的选择。它通过自动化的方式简化了这类任务的部署和管理，是维护 Kubernetes 集群健康和可观测性的重要工具。

在 Kubernetes 中，**DaemonSet 控制器** 确保在集群中的**每个（或部分）符合条件的节点上**都运行一个 Pod 副本。它通常用于运行那些需要在每个节点上执行的系统级后台任务。

-----

## DaemonSet 的核心使用案例

DaemonSet 的设计使其成为部署以下类型应用的理想选择：

1.  **日志收集器**：在每个节点上运行一个日志代理，如 **Fluentd**、**Filebeat** 或 **Loki**。这些代理负责收集节点及其上所有 Pod 的日志，并将其发送到中央日志系统（如 Elasticsearch、Splunk 或 Grafana Loki）。这确保了所有节点的日志都被完整地收集。

2.  **监控代理**：在每个节点上运行一个监控代理，如 **Prometheus Node Exporter** 或 **Datadog Agent**。这些代理负责收集节点的资源使用指标（CPU、内存、磁盘、网络）和系统信息，并将其发送到中央监控系统。

3.  **集群存储守护进程**：在每个节点上运行一个分布式存储守护进程，如 **Ceph** 或 **GlusterFS**。这使得每个节点都能够直接参与到集群存储系统中，提供本地存储能力。

4.  **网络插件**：大多数网络解决方案（CNI）都是以 DaemonSet 的形式部署的，例如 **Calico**、**Flannel** 或 **Cilium**。这些 Pod 负责配置每个节点的网络接口，确保 Pod 之间的通信。

-----

### DaemonSet 示例：部署 Fluentd 日志代理

下面是一个典型的 DaemonSet YAML 文件，用于在每个节点上部署 Fluentd 日志代理。该代理会挂载节点的 `/var/log` 目录，从而收集所有日志。

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-agent
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-agent
  template:
    metadata:
      labels:
        name: fluentd-agent
    spec:
      tolerations:
      # 允许在 master 节点上调度 Pod
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule
      containers:
      - name: fluentd
        image: fluent/fluentd-kubernetes-daemonset:v1.16.0-debian-fluentd-1.16
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
```

这个配置文件确保了：

  * **在每个节点上运行**：DaemonSet 控制器会在每个满足条件的节点上创建一个 Pod。
  * **访问节点日志**：通过 `volumes` 和 `volumeMounts` 将节点的 `/var/log` 目录挂载到 Pod 中，使其能够读取日志。
  * **容忍主节点**：`tolerations` 字段确保了即使主节点带有 `NoSchedule` 污点，DaemonSet 也能在该节点上运行，这对于集群监控和日志收集至关重要。

-----

### DaemonSet 常用管理命令

管理 DaemonSet 的命令与管理其他 Kubernetes 对象类似：

1.  **创建 DaemonSet**：

    ```bash
    kubectl apply -f fluentd-daemonset.yaml
    ```

    这条命令会根据你的 YAML 文件创建 DaemonSet，然后 Kubernetes 会自动在每个节点上部署一个 Pod。

2.  **查看 DaemonSet 列表**：

    ```bash
    kubectl get daemonsets -n kube-system
    ```

    此命令会列出 `kube-system` 命名空间下的所有 DaemonSet，并显示它们的状态，包括期望的 Pod 数量、当前正在运行的 Pod 数量以及就绪的 Pod 数量。

3.  **查看 DaemonSet 详细信息**：

    ```bash
    kubectl describe daemonset fluentd-agent -n kube-system
    ```

    这条命令提供了关于 DaemonSet 的所有详细信息，包括其 Pod 模板、事件日志和当前状态。这是排查问题时最有用的命令。

4.  **更新 DaemonSet**：
    只需修改 YAML 文件并再次运行 `kubectl apply`。DaemonSet 支持**滚动更新**，它会以可控的方式逐步替换所有节点上的旧 Pod。

5.  **删除 DaemonSet**：

    ```bash
    kubectl delete daemonset fluentd-agent -n kube-system
    ```

    删除 DaemonSet 会自动删除所有由其管理的 Pod，从而从集群中的每个节点上移除该应用。