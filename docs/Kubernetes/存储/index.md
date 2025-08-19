# Kubernets 存储

Kubernetes 存储是一个复杂但至关重要的主题，它通过一系列抽象和控制器来管理容器化应用的持久化数据。Kubernetes 的核心思想是将存储从计算节点中解耦，从而让 Pod 无论在哪个节点上运行，都能访问到相同的数据。

这个存储模型主要由以下几个核心概念和控制器构成：

### 1. 存储卷 (Volume)

**存储卷**是 Kubernetes 中最基础的存储单元，它提供了 Pod 内部与外部存储的连接。Pod 中的容器可以使用一个或多个卷来共享数据或持久化数据。

与 Docker 中的卷不同，Kubernetes 的卷具有以下特点：

* **与 Pod 绑定**：卷的生命周期与 Pod 相同。当 Pod 被删除时，卷也会被清理，但其数据是否保留取决于卷的类型。
* **非永久性**：大多数类型的卷（例如 `emptyDir`）在 Pod 终止后会丢失数据。
* **多种类型**：Kubernetes 支持多种内置的卷类型，例如：
    * **`emptyDir`**：Pod 启动时创建的空目录，数据在 Pod 生命周期内可用，但 Pod 删除后即消失。通常用于临时存储或 Pod 内部不同容器间的数据共享。
    * **`hostPath`**：将节点上的文件或目录挂载到 Pod 中。这会使 Pod 依赖于特定节点，不推荐在生产环境中使用。

---

### 2. 持久化存储 (Persistent Volumes)

为了解决存储的持久性问题，Kubernetes 引入了 **持久化存储（Persistent Volumes, PV）** 和 **持久卷声明（Persistent Volume Claims, PVC）** 两个核心 API 对象。这两个对象共同构成了 Kubernetes 的存储抽象层。

#### 持久卷 (Persistent Volume, PV)

**PV** 是集群中由管理员预先配置或由存储系统动态提供的**持久存储**。它代表了存储资源的具体实现，比如一个 NFS 共享、一个云提供商的磁盘（AWS EBS、GCE PD）或一个 iSCSI 卷。

**PV 的特点：**
* **集群资源**：PV 独立于 Pod 的生命周期，是集群中的一种资源。
* **卷类型**：它定义了存储的具体类型、大小和访问模式（例如 ReadWriteOnce、ReadOnlyMany 等）。

#### 持久卷声明 (Persistent Volume Claim, PVC)

**PVC** 是用户对存储资源的**请求**。它是一个抽象对象，用户在其中声明他们需要的存储类型、大小和访问模式。用户不需要关心底层存储的具体实现。

**PVC 的作用：**
* **存储抽象**：PVC 将用户与底层存储实现解耦。用户只需声明需求，Kubernetes 就会自动寻找合适的 PV 来满足请求。
* **动态供给**：如果集群中没有与 PVC 匹配的 PV，Kubernetes 可以通过**存储类（StorageClass）** 动态地创建一个 PV 来满足请求。

#### PV 和 PVC 的生命周期

PV 和 PVC 的交互类似于生产者和消费者模型：

1.  **供给（Provisioning）**：管理员手动创建 PV，或通过**StorageClass**实现动态供给。
2.  **绑定（Binding）**：用户创建 PVC，Kubernetes 的**PVC 控制器**会寻找与 PVC 匹配的 PV，并将它们绑定。
3.  **使用（Using）**：Pod 通过挂载 PVC 来使用持久存储。
4.  **释放（Reclaiming）**：当 PVC 被删除时，PV 可以被保留、删除或回收，这取决于其**回收策略（`reclaimPolicy`）**。

---

### 3. 存储类 (StorageClass)

**StorageClass** 是一个 Kubernetes API 对象，它代表了不同**存储类型的抽象**。它定义了存储的“等级”或“配置文件”，例如“快速 SSD”、“通用 HDD”或“备份存储”。

**StorageClass 的作用：**
* **动态供给**：当用户创建 PVC 并指定了 StorageClass 时，Kubernetes 的**存储供给控制器**会调用相应的存储插件，动态地创建 PV 并将其与 PVC 绑定。
* **简化管理**：管理员无需预先创建大量 PV，只需定义好 StorageClass，让用户按需创建 PVC，从而实现了存储的自动化管理。

### 4. CSI (Container Storage Interface)

**CSI** 是一个行业标准，它定义了存储提供商如何将他们的存储系统暴露给 Kubernetes。CSI 使得第三方存储厂商能够独立开发插件，从而将存储系统集成到 Kubernetes 中。

**CSI 的重要性：**
* **解耦**：CSI 将存储逻辑从 Kubernetes 核心代码中分离，允许存储厂商独立于 Kubernetes 发布和更新其驱动。
* **灵活性**：它支持各种存储技术，包括块存储、文件存储和对象存储。

### 总结

Kubernetes 通过 **Volume**、**PersistentVolume/PersistentVolumeClaim** 和 **StorageClass** 这些抽象层，构建了一个强大而灵活的存储管理体系。这个体系将存储的底层实现与应用的需求解耦，并通过 **CSI** 提供了强大的可扩展性，让你的应用可以无缝地使用各种云原生存储解决方案。