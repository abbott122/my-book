在 Kubernetes 中，**调度 (Scheduling)** 和 **抢占 (Preemption)** 是两个紧密相关的概念，它们共同确保了集群资源的有效利用，并保障关键应用的可用性。

---

### 1. Kubernetes 调度 (Scheduling)

调度是指将新创建的、未分配到节点的 Pod **放置**到合适的节点上运行的过程。这个任务由 **kube-scheduler** 组件来完成。调度器会持续监控 Kubernetes API 服务器，寻找那些 `nodeName` 字段为空的 Pod。

调度过程通常分为两个阶段：

#### 过滤 (Filtering)

过滤阶段会从集群中的所有节点中，选出那些**符合** Pod 要求的节点。如果一个 Pod 需要 2GB 内存，那么所有可用内存不足 2GB 的节点都会被过滤掉。常见的过滤规则包括：

* **资源检查**：节点的 CPU、内存、存储资源是否足够。
* **端口冲突**：Pod 声明的端口是否已经被节点上的其他服务占用。
* **节点选择器**：Pod 是否指定了 `nodeSelector` 或 `nodeAffinity`，要求只能在特定标签的节点上运行。
* **污点与容忍**：节点上的污点（Taints）和 Pod 的容忍（Tolerations）是否匹配。

过滤之后，会得到一个“可调度节点”列表。如果这个列表为空，Pod 将进入**待定 (Pending)** 状态，无法被调度。

#### 打分 (Scoring)

如果过滤阶段找到了一个或多个可调度节点，打分阶段就会为每个节点计算一个分数，以选出**最适合** Pod 运行的节点。打分规则通常基于以下因素：

* **资源利用率**：倾向于将 Pod 调度到资源利用率较低的节点上，以实现更均衡的负载。
* **Pod 亲和性与反亲和性**：根据 Pod 的亲和性规则，将相关联的 Pod 调度到同一节点上（亲和），或尽量分散开（反亲和）。
* **镜像本地性**：如果节点上已经存在 Pod 所需的容器镜像，那么这个节点的得分会更高，从而减少拉取镜像的时间。

最终，调度器会将 Pod 调度到得分最高的节点上。

---

### 2. Kubernetes 抢占 (Preemption)

抢占是调度机制的补充。当一个**高优先级**的 Pod 因为集群资源不足而无法被调度时，抢占机制就会介入。它允许调度器从一个节点上**驱逐 (Evict)** 优先级较低的 Pod，为高优先级 Pod 腾出资源。

抢占机制依赖于 **Pod 优先级 (Pod Priority)**，你需要通过 **PriorityClass** API 对象来定义不同级别的优先级。

#### 抢占的工作流程

1.  **定义优先级**：首先，创建一个或多个 `PriorityClass` 对象，每个对象都有一个唯一的名称和数值（值越高，优先级越高）。
2.  **Pod 声明**：在 Pod 的配置文件中，通过 `priorityClassName` 字段指定其优先级。
3.  **调度失败**：当一个高优先级的 Pod 处于 `Pending` 状态，并且过滤阶段没有找到任何可用的节点时，抢占逻辑被触发。
4.  **寻找受害者**：调度器会检查哪些节点上的低优先级 Pod 在被驱逐后，可以为高优先级 Pod 提供足够的资源。
5.  **驱逐**：调度器选择一个或多个优先级最低的 Pod 进行驱逐。这些 Pod 会收到一个体面的终止信号（`SIGTERM`），以便它们可以进行清理。
6.  **重新调度**：一旦低优先级 Pod 被驱逐，高优先级 Pod 就可以被调度到该节点上。被驱逐的低优先级 Pod 会被重新放回到调度队列中，等待有新的资源可用时再进行调度。

#### 抢占的注意事项

* **不是即时生效**：被驱逐的 Pod 有一个优雅终止时间，因此抢占不是瞬间完成的。
* **只发生在资源不足时**：抢占只会在集群没有足够资源来调度高优先级 Pod 的情况下发生。
* **可配置**：你可以为 `PriorityClass` 设置 `preemptionPolicy: Never`，来阻止该优先级 Pod 抢占其他 Pod。

### 总结

* **调度**是 Kubernetes 的常规行为，用于将 Pod **放置**到合适的节点。
* **抢占**是一种**紧急机制**，用于在资源紧张时，通过**驱逐**低优先级 Pod 来保障高优先级 Pod 的运行。

这两个机制的结合，使得 Kubernetes 能够灵活地管理集群资源，确保你的关键应用在任何情况下都能得到优先保障。


在 Kubernetes 中，**调度 (Scheduling)** 和 **抢占 (Preemption)** 是两个核心机制，它们共同决定了 Pod 如何被放置在集群中，并确保了关键应用的可用性。

-----

### 调度 (Scheduling)

**调度**是指将新创建的、未分配到节点的 Pod 放置到最合适的节点上运行的过程。这个任务由 **kube-scheduler** 组件来完成。调度器会持续监控 Kubernetes API 服务器，寻找那些 `nodeName` 字段为空的 Pod，并为它们选择一个最佳的运行节点。

这个过程通常分为两个阶段：

1.  **过滤 (Filtering)**：调度器会从所有节点中，选出那些满足 Pod 资源请求、节点选择器、污点/容忍等条件的节点。如果 Pod 需要 2GB 内存，所有可用内存不足的节点都会被过滤掉。
2.  **打分 (Scoring)**：对于过滤后剩下的“可调度”节点，调度器会根据一系列规则为它们打分，选出得分最高的节点。打分规则通常包括：
      * **资源均衡**：倾向于将 Pod 调度到资源利用率较低的节点，以避免资源集中。
      * **亲和性/反亲和性**：尽量将相关 Pod 放在一起或分开。
      * **镜像本地性**：倾向于将 Pod 调度到已经有其所需镜像的节点上，以加快启动速度。

### 案例：使用节点选择器进行调度

假设你有一个需要在 SSD 存储节点上运行的数据库 Pod。你可以通过在 Pod 的 YAML 文件中添加 `nodeSelector` 标签来控制调度器：

```yaml
# db-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-db-pod
spec:
  nodeSelector:
    disktype: ssd # 调度器只会将 Pod 放置到带有 "disktype: ssd" 标签的节点上
  containers:
  - name: db-container
    image: mysql:8.0
```

-----

### 抢占 (Preemption)

**抢占**是调度机制的补充。当一个**高优先级**的 Pod 因为集群资源不足而无法被调度时，抢占机制会介入，允许调度器从一个节点上**驱逐**（Evict）优先级较低的 Pod，为高优先级 Pod 腾出资源。

抢占机制依赖于 **Pod 优先级 (Pod Priority)**，你需要通过 **PriorityClass** API 对象来定义不同级别的优先级。

### 案例：使用优先级与抢占

假设你的集群资源紧张，但你的 `db-pod` 是核心服务，需要优先运行。你可以通过以下步骤实现抢占：

1.  **创建 PriorityClass**：定义一个名为 `high-priority` 的高优先级类别。
    ```yaml
    # high-priority-class.yaml
    apiVersion: scheduling.k8s.io/v1
    kind: PriorityClass
    metadata:
      name: high-priority
    value: 1000000
    globalDefault: false
    description: "This priority class should be used for critical services."
    ```
2.  **为 Pod 指定优先级**：在 `db-pod` 的 YAML 文件中，添加 `priorityClassName`。
    ```yaml
    # db-pod-with-priority.yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: my-db-pod
    spec:
      priorityClassName: high-priority # 为 Pod 指定高优先级
      containers:
      - name: db-container
        image: mysql:8.0
        resources:
          requests:
            cpu: "1"
            memory: "2Gi"
    ```

当这个 `db-pod` 无法被调度时，如果其他节点上有正在运行的低优先级 Pod 占用了它所需的资源，调度器就会选择性地驱逐那些 Pod，直到 `db-pod` 能够被调度。

-----

### 常用命令

管理调度和抢占相关的资源，你可以使用以下 `kubectl` 命令：

1.  **查看节点标签**：
    ```bash
    kubectl get nodes --show-labels
    ```
2.  **为节点添加或移除污点**：
    ```bash
    # 添加污点
    kubectl taint nodes node1 key1=value1:NoSchedule
    # 移除污点
    kubectl taint nodes node1 key1=value1:NoSchedule-
    ```
3.  **查看优先级类别**：
    ```bash
    kubectl get priorityclass
    ```
4.  **查看 Pod 状态和调度事件**：
    当一个 Pod 因为调度问题处于 `Pending` 状态时，`kubectl describe` 是最有用的命令。
    ```bash
    kubectl describe pod <pod-name>
    ```
    在输出的 **Events** 部分，你会看到调度器为什么未能调度该 Pod 的详细原因，例如“0/3 nodes are available: 3 nodes didn't have enough memory”。
5.  **查看调度日志**：
    如果需要更深入的排查，可以查看 `kube-scheduler` Pod 的日志：
    ```bash
    kubectl logs -n kube-system -f <kube-scheduler-pod-name>
    ```