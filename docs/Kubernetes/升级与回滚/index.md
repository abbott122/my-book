在 Kubernetes 中，**集群升级 (Cluster Upgrade)** 和 **回滚 (Rollback)** 是核心的运维操作，它们确保了集群能够安全地从一个版本迁移到另一个版本，并在出现问题时能够快速恢复。

-----

### Kubernetes 集群升级

集群升级是将 Kubernetes 版本从旧版本更新到新版本的过程，通常涉及以下几个关键组件的升级：

1.  **控制平面 (Control Plane)**：这是集群的大脑，包括 **API Server**、**Scheduler**、**Controller Manager** 和 **etcd**。这是升级中最关键、风险最高的部分。
2.  **数据平面 (Data Plane)**：这是集群的工作节点，主要包括 **kubelet** 和 **kube-proxy**。
3.  **插件 (Addons)**：集群中的一些附加组件，例如 DNS 服务（CoreDNS）和网络插件（CNI）。

#### 升级流程

标准的 Kubernetes 升级流程通常遵循以下步骤，以确保最小化停机时间：

1.  **升级主节点（Master Nodes）**：
      * 首先升级\*\*非领导者（non-leader）\*\*的主节点。这确保了如果升级失败，还有其他主节点可以继续处理请求。
      * 在升级前，会\*\*排空（drain）\*\*该主节点上的所有 Pod，以确保Pod不会被中断。
      * 升级核心组件（如 `kube-apiserver` 和 `kube-controller-manager`）。
      * 升级完成后，\*\*解禁（uncordon）\*\*主节点，允许 Pod 再次被调度到其上。
2.  **升级工作节点（Worker Nodes）**：
      * 一次升级一个或一组工作节点。
      * 首先\*\*排空（drain）\*\*该节点，将所有 Pod 迁移到其他健康节点。
      * 升级该节点上的 `kubelet` 和 `kube-proxy`。
      * 升级完成后，\*\*解禁（uncordon）\*\*该节点，使其重新参与 Pod 调度。

-----

### 集群回滚 (Cluster Rollback)

尽管升级过程经过精心设计，但新版本有时会引入意想不到的 bug 或与现有工作负载不兼容。在这种情况下，回滚至上一个稳定版本是至关重要的。

#### 回滚流程

回滚过程与升级过程相反，但通常更具风险，因为你可能需要在短时间内恢复多个组件。

1.  **回滚工作节点**：
      * 通常会选择性地回滚一组或一个工作节点。
      * \*\*排空（drain）\*\*节点。
      * 将 `kubelet` 和 `kube-proxy` 降级到旧版本。
      * \*\*解禁（uncordon）\*\*节点，使其重新接收流量。
2.  **回滚主节点**：
      * 这通常是最后一步，也是最危险的一步。
      * 回滚需要谨慎操作，确保 etcd 的数据兼容旧版本，否则可能导致集群状态损坏。

#### 注意事项

  * **数据兼容性**：etcd 数据库的版本必须与 Kubernetes API 服务器的版本兼容。从高版本降级到低版本可能会导致 etcd 的数据不兼容，这是回滚最大的挑战。
  * **提前备份**：在执行任何升级操作之前，务必备份 etcd 数据。这是唯一的“救命稻草”。
  * **工具支持**：大多数 Kubernetes 集群安装工具（如 `kubeadm` 或云服务商的控制台）都提供了升级和回滚的命令。例如，`kubeadm upgrade` 和 `kubeadm reset`。

-----

### 常用命令

在实践中，我们通常会使用 `kubeadm` 或云服务商的工具来管理集群升级。

  * **检查可用的升级版本**：

    ```bash
    kubeadm upgrade plan
    ```

  * **升级控制平面**：

    ```bash
    # 在主节点上执行
    sudo kubeadm upgrade apply v1.29.0
    # 然后升级 kubelet 和 kube-proxy
    sudo apt-get install -y kubelet kube-proxy
    sudo systemctl restart kubelet
    ```

  * **升级工作节点**：

    ```bash
    # 在工作节点上执行
    # 首先排空节点
    kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data
    # 升级 kubeadm
    sudo kubeadm upgrade node
    # 升级 kubelet 和 kube-proxy
    sudo apt-get install -y kubelet kube-proxy
    sudo systemctl restart kubelet
    # 解禁节点
    kubectl uncordon <node-name>
    ```

  * **回滚（使用 `kubeadm`）**：
    `kubeadm` 提供了 `reset` 命令来重置节点，这通常是回滚的第一步。但是，**降级操作通常需要更谨慎地手动执行**，因为其复杂性更高。

集群升级和回滚是高风险的操作，强烈建议在非生产环境（如开发或测试集群）中进行充分测试，并在执行前仔细阅读官方文档。