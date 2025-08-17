<div style="background-color: #f0f8ff; border-left: 5px solid #1e90ff; padding: 10px; border-radius: 5px; margin: 10px 0;">
  Kubernetes 中的 Pod 是最小的可部署计算单元，它代表着一个或一组紧密关联的容器，这些容器共享存储和网络资源。Pod 是 Kubernetes 抽象出来的对象，而不是直接部署容器。一个 Pod 里的容器总是位于同一节点上，并且一同调度、管理和部署
</div>

## Pod 的核心概念

## 为什么需要 Pod？

## 如何定义一个 Pod？

## Pod 的状态

## Pod 的高级特性

### Probe（探针）
### Pod 生命周期管理

#### Pod  启动/运行阶段（过程）

> Pod 的 status 字段是一个 PodStatus 对象，该对象有一个 phase 字段。

- Pending
>Pod 已被 Kubernetes 集群接受，但一个或多个容器尚未设置并准备好运行。这包括 Pod 等待被调度的时间以及通过网络下载容器镜像的时间。
- Running
>Pod 已绑定到节点，并且所有容器都已创建。至少有一个容器仍在运行，或正在启动或重启过程中。

- Succeeded
>Pod 中的所有容器都已成功终止，将不会被重启。

- Failed
>Pod 中所有容器已终止，并且至少有一个容器以失败状态终止。也就是说，容器要么以非零状态退出，要么被系统终止，并且未设置为自动重启。

- Unknown
> 由于某种原因，无法获取 Pod 的状态。此阶段通常是由于与 Pod 应该运行的节点通信错误而发生的。


### Pod 调度策略
## Pod 的作用与限制