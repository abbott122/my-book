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

**StorageClass** 是一个 Kubernetes API 对象，它代表了不同 **存储类型的抽象** 。它定义了存储的“等级”或“配置文件”，例如“快速 SSD”、“通用 HDD”或“备份存储”。

**StorageClass 的作用：**
* **动态供给**：当用户创建 PVC 并指定了 StorageClass 时，Kubernetes 的 **存储供给控制器** 会调用相应的存储插件，动态地创建 PV 并将其与 PVC 绑定。
* **简化管理**：管理员无需预先创建大量 PV，只需定义好 StorageClass，让用户按需创建 PVC，从而实现了存储的自动化管理。

在 Kubernetes 中，**StorageClass** 是一个非常重要的抽象层，它定义了不同类型的存储。通过 StorageClass，管理员可以提供不同质量、性能和特性的存储选项，而用户只需通过 **PersistentVolumeClaim (PVC)** 来请求所需的存储，无需关心底层实现。

NFS 和 Ceph 是两种常见的后端存储系统，它们与 Kubernetes 的集成方式各有特点。

#### NFS

**NFS** 是一种传统的网络文件系统，它通过网络共享一个目录，允许多个客户端读写。在 Kubernetes 中使用 NFS，通常需要一个\*\*外部存储供给器（External Provisioner）\*\*来动态管理存储。

##### 工作原理

1.  **管理员部署 NFS Provisioner**：这是一个 Kubernetes Pod，它监听 PVC 请求。
2.  **创建 StorageClass**：管理员定义一个 `StorageClass`，并将其 `provisioner` 设置为 NFS Provisioner。
3.  **用户创建 PVC**：用户创建一个 PVC，并指定刚才定义的 NFS StorageClass。
4.  **动态供给**：NFS Provisioner 接收到 PVC 请求后，会在 NFS 服务器上为该 PVC 创建一个独立的子目录，并为之生成一个对应的 **PersistentVolume (PV)**。
5.  **Pod 使用**：Pod 通过 PVC 挂载 PV，从而使用 NFS 提供的持久存储。

##### NFS StorageClass 示例

下面是一个典型的 NFS StorageClass YAML 文件：

```yaml
# nfs-storageclass.yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: nfs-storage
provisioner: k8s.io/nfs-subdir-external-provisioner # NFS Provisioner 的名称
parameters:
  archiveOnDelete: "false" # 定义删除 PVC 时是否保留数据
```

**特点：**

  * **简单易用**：对于已有的 NFS 基础设施，集成相对简单。
  * **支持 ReadWriteMany**：多个 Pod 可以同时读写同一个 PV，这对于某些需要共享文件系统的应用非常有用。
  * **高可用性**：取决于底层的 NFS 服务器。如果 NFS 服务器是单点的，它将是一个 SPOF (单点故障)。

-----

#### Ceph StorageClass

**Ceph** 是一个功能强大的开源、软件定义的分布式存储系统，它提供了块存储（RBD）、文件存储（CephFS）和对象存储（RADOSGW）。在 Kubernetes 中，通常使用 **Rook** 或 **Ceph-CSI 驱动**来管理 Ceph 存储。

#### 工作原理

1.  **部署 Ceph 集群**：首先，需要在集群内部或外部部署一个 Ceph 集群。使用 Rook 可以将 Ceph 作为一个 Pod 在 Kubernetes 中运行，极大地简化了部署和管理。
2.  **部署 Ceph-CSI 驱动**：这是 Kubernetes 的 CSI（Container Storage Interface）插件，负责与 Ceph 集群通信。
3.  **创建 StorageClass**：管理员定义一个或多个 StorageClass，并将其 `provisioner` 设置为 `ceph.csi.ceph.com`。
4.  **动态供给**：当用户创建 PVC 时，Ceph-CSI 驱动会动态地在 Ceph 集群中创建一个 RBD 卷或 CephFS 卷，并生成 PV。

##### Ceph StorageClass 示例 (使用 Rook)

下面是一个使用 Rook-Ceph 的 CephFS StorageClass 示例：

```yaml
# cephfs-storageclass.yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: cephfs-storage
provisioner: rook-ceph.cephfs.csi.ceph.com # CephFS CSI 驱动的名称
parameters:
  clusterID: rook-ceph
  fsName: myfs # 要使用的 CephFS 文件系统名称
```

**特点：**

  * **高度可扩展**：Ceph 可以从几个节点扩展到数千个节点，存储容量可以达到 PB 级甚至 EB 级。
  * **高可用性**：Ceph 通过数据复制和自动故障恢复来保证高可用性。
  * **支持多种存储类型**：可以同时提供块存储（用于数据库）、文件存储（用于共享文件）和对象存储。
  * **复杂性**：部署和管理 Ceph 比 NFS 复杂，但提供了更强大的功能和可靠性。

-----

### 总结：NFS vs. Ceph

| 特性 | **NFS StorageClass** | **Ceph StorageClass** |
| :--- | :--- | :--- |
| **底层技术** | 传统文件共享协议 | 软件定义的分布式存储 |
| **部署复杂度** | 相对简单 | 较高，但有 Rook 等工具简化 |
| **可扩展性** | 有限，依赖于单一 NFS 服务器 | 极高，可以水平扩展 |
| **高可用性** | 依赖于底层 NFS 服务器的冗余 | 内置高可用性，数据自动复制 |
| **访问模式** | 支持 ReadWriteMany | 支持 ReadWriteOnce, ReadOnlyMany, ReadWriteMany |
| **使用场景** | 简单的文件共享、小型集群 | 生产环境、大数据、数据库等需要高可用和高性能的场景 |

选择哪种 StorageClass 取决于你的需求。如果你只需要一个简单、可共享的文件系统，NFS 是一个不错的选择。如果你需要一个高可用、可扩展、功能丰富的存储解决方案来支持生产环境中的关键应用，那么 Ceph 是一个更强大的选择。




### 4. CSI (Container Storage Interface)

**CSI** 是一个行业标准，它定义了存储提供商如何将他们的存储系统暴露给 Kubernetes。CSI 使得第三方存储厂商能够独立开发插件，从而将存储系统集成到 Kubernetes 中。

**CSI 的重要性：**
* **解耦**：CSI 将存储逻辑从 Kubernetes 核心代码中分离，允许存储厂商独立于 Kubernetes 发布和更新其驱动。
* **灵活性**：它支持各种存储技术，包括块存储、文件存储和对象存储。

### 总结

Kubernetes 通过 **Volume**、**PersistentVolume/PersistentVolumeClaim** 和 **StorageClass** 这些抽象层，构建了一个强大而灵活的存储管理体系。这个体系将存储的底层实现与应用的需求解耦，并通过 **CSI** 提供了强大的可扩展性，让你的应用可以无缝地使用各种云原生存储解决方案。

在 Kubernetes 中，存储是一个至关重要的主题，它通过一系列抽象概念来管理容器化应用的持久化数据。本回答将通过一个案例，结合常用的命令，详细解释 Kubernetes 存储的工作原理。

### 核心概念回顾

在深入案例之前，先快速回顾几个核心概念：

  * **Volume**：Pod 的存储单元，与 Pod 的生命周期绑定。
  * **PersistentVolume (PV)**：集群中的一块**持久存储**，由管理员预先配置或由存储系统动态提供。它独立于 Pod 的生命周期，即使 Pod 被删除，数据也依然存在。
  * **PersistentVolumeClaim (PVC)**：用户对存储资源的**请求**。用户在其中声明需要的存储大小和访问模式。
  * **StorageClass**：定义了存储的“类型”或“配置文件”。它使得 Kubernetes 能够按需动态地创建 PV。

-----

## 存储案例：一个有状态应用的部署

假设我们有一个需要持久化数据的 WordPress 博客应用。我们需要确保即使 Pod 重新启动或被调度到不同的节点，用户上传的图片和文章数据也不会丢失。

我们将使用 **StorageClass**、**PVC** 和 **Deployment** 来实现这个需求。

#### 步骤 1: 定义 StorageClass

首先，我们需要一个 StorageClass，它代表了我们想要的存储类型。例如，我们可以使用云提供商提供的 `gp2` 类型（AWS EBS）或 `standard` 类型（GCE Persistent Disk）。

`storageclass.yaml`

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-storage
provisioner: k8s.io/minikube-hostpath # 示例中，使用 minikube 的 hostPath
reclaimPolicy: Retain # 策略：删除 PVC 时保留 PV
```

  * `provisioner`：指定了提供存储的插件。
  * `reclaimPolicy`：定义了当 PVC 被删除时，PV 的处理方式。`Retain` 表示保留数据。

#### 步骤 2: 定义 PVC (持久卷声明)

接下来，我们定义一个 PVC 来请求 5Gi 的存储，并指定使用我们刚刚创建的 `fast-storage` 类型。

`pvc.yaml`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: wordpress-pvc
spec:
  accessModes:
    - ReadWriteOnce # 访问模式：只允许一个节点读写
  storageClassName: fast-storage
  resources:
    requests:
      storage: 5Gi
```

当这个 PVC 被创建时，Kubernetes 会自动调用 `fast-storage` 所定义的 `provisioner`，动态创建一个匹配的 PV。

#### 步骤 3: 在 Deployment 中使用 PVC

最后，我们创建一个 Deployment，并在 Pod 模板中引用我们定义的 PVC。

`wordpress-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
      - name: wordpress
        image: wordpress:latest
        ports:
        - containerPort: 80
        volumeMounts:
        - name: wordpress-volume
          mountPath: /var/www/html
  volumes:
  - name: wordpress-volume
    persistentVolumeClaim:
      claimName: wordpress-pvc # 引用 PVC 名称
```

  * `volumes`：定义了一个名为 `wordpress-volume` 的卷，并将其类型设置为 `persistentVolumeClaim`。
  * `persistentVolumeClaim`：指定了要引用的 PVC 名称，即 `wordpress-pvc`。
  * `volumeMounts`：将这个卷挂载到容器内的 `/var/www/html` 路径，这是 WordPress 存储数据的地方。

-----

### 常用命令

管理 Kubernetes 存储资源通常需要使用以下 `kubectl` 命令：

1.  **创建存储资源**：

    ```bash
    kubectl apply -f storageclass.yaml
    kubectl apply -f pvc.yaml
    kubectl apply -f wordpress-deployment.yaml
    ```

2.  **查看存储类**：

    ```bash
    kubectl get sc
    # 示例输出：
    # NAME           PROVISIONER                  RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
    # fast-storage   k8s.io/minikube-hostpath     Retain          Immediate           false                  10m
    ```

3.  **查看持久卷声明 (PVC)**：

    ```bash
    kubectl get pvc
    # 示例输出：
    # NAME              STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS    AGE
    # wordpress-pvc     Bound     pvc-436f52e5-f5f0-466f-8d76-50d4f1345594   5Gi        RWO            fast-storage    5m
    ```

    `STATUS` 为 `Bound` 表示 PVC 已经成功绑定到一个 PV。

4.  **查看持久卷 (PV)**：

    ```bash
    kubectl get pv
    # 示例输出：
    # NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                 STORAGECLASS     AGE
    # pvc-436f52e5-f5f0-466f-8d76-50d4f1345594   5Gi        RWO            Retain           Bound    default/wordpress-pvc   fast-storage     5m
    ```

    `STATUS` 为 `Bound` 表示 PV 已经成功与一个 PVC 绑定。

5.  **删除存储资源**：

    ```bash
    # 注意删除顺序：先删除 Deployment，再删除 PVC 和 PV
    kubectl delete deployment wordpress
    kubectl delete pvc wordpress-pvc
    # 手动删除 PV，因为 reclaimPolicy 是 Retain
    kubectl delete pv pvc-436f52e5-f5f0-466f-8d76-50d4f1345594
    ```