# 网络控制
Kubernetes 的网络模型是一个非常重要的概念，它通过一系列控制器和组件来确保集群中的所有 Pod 能够高效、安全地通信。这个模型的核心目标是**“扁平化网络”**：让每个 Pod 都有一个独立的 IP 地址，并且所有 Pod 都可以无需 NAT（网络地址转换）直接相互通信。

实现这一目标主要依赖以下几个关键的“控制器”或组件：

## 1. CNI (Container Network Interface)

**CNI** 严格来说不是 Kubernetes 内置的控制器，而是一个**标准**。它定义了一个接口，让各种网络插件（CNI 插件）能够与 Kubernetes 集成，负责配置 Pod 的网络。

**核心职责：**

* **IP 地址分配**：当一个 Pod 被创建时，CNI 插件会为其分配一个唯一的 IP 地址。

* **网络连接**：CNI 插件会配置网络，确保 Pod 能够与其他 Pod 通信。不同的 CNI 插件实现方式不同，例如使用**Overlay**（叠加网络）或**Underlay**（底层网络）。

* **插件多样性**：CNI 的存在让你可以根据需求选择不同的网络解决方案，例如：

    * **Calico**：以其强大的网络策略和性能著称。

    * **Flannel**：简单易用，适合初学者。

    * **Cilium**：提供基于 eBPF 的高性能网络和安全功能。


## 2. Service 控制器与 kube-proxy

**Service 控制器**和 **kube-proxy** 共同实现了 Kubernetes 的服务发现和负载均衡。

* **Service 控制器**：这是一个核心控制器，负责**监控** Service 和 Endpoints 对象。当 Service 对象被创建、更新或删除时，Service 控制器会确保相应的 Endpoints 对象被更新，该对象包含了 Service 后端 Pod 的 IP 地址列表。
* **kube-proxy**：这是一个在**每个节点**上运行的网络代理。它**不直接管理** Service，而是**监听** Service 和 Endpoints 的变化，并根据这些信息在节点上配置网络规则。

**它们如何协同工作：**

1. **Service 控制器**：你创建一个 Service 对象，并用**标签选择器**指定要代理的 Pod。Service 控制器会持续跟踪这些 Pod 的 IP 地址。

2. **kube-proxy**：它观察到新的 Service 和 Endpoints，然后利用 Linux 内核的网络功能（如 **iptables** 或 **IPVS**）在节点上设置转发规则。

3. **流量转发**：当有流量发送到 Service 的 IP 地址时，kube-proxy 配置的规则会拦截这些流量，并将其负载均衡地转发到 Service 后端的一个健康 Pod 上。

这个机制实现了 Pod 的**“可替换性”**：Pod 可以随时被创建或销毁，但 Service 的 IP 地址保持不变，从而为上游应用提供了稳定的入口。


## 3. Ingress 控制器

**Ingress 控制器**是负责管理从集群**外部**到集群**内部**服务的 HTTP 和 HTTPS 流量的控制器。

* **Ingress**：它是一个 Kubernetes API 对象，用于定义一组路由规则，例如，将 `myapp.example.com` 的流量路由到 `my-app-service`。
* **Ingress 控制器**：这是一个**外部**组件，通常是一个负载均衡器（如 Nginx、Traefik、HAProxy），它**监听** Ingress 对象并根据其中的规则动态配置自身的路由。

**Ingress 的优势：**
* **集中式路由**：你可以通过一个 Ingress 对象管理多个服务的外部访问规则，而不是为每个服务创建一个 NodePort 或 LoadBalancer。
* **高级功能**：Ingress 控制器通常支持 SSL/TLS 终止、基于域名的虚拟主机、路径路由和请求重写等高级功能。


## 4. Network Policy 控制器

**Network Policy 控制器**是一个用于实现**Pod 间防火墙规则**的控制器。

* **Network Policy**：这是一个 Kubernetes API 对象，允许你定义一组入站（Ingress）和出站（Egress）的网络规则，以控制 Pod 之间的通信。
* **Network Policy 控制器**：这个控制器通常由 CNI 插件实现（例如 Calico 或 Cilium），它**监听** Network Policy 对象，并将其规则转化为实际的网络防火墙规则。

**主要作用：**
* **安全隔离**：默认情况下，所有 Pod 都可以相互通信。通过 Network Policy，你可以创建“微隔离”策略，例如，只允许前端服务访问后端数据库，从而增强应用的安全性。

这些控制器共同构建了 Kubernetes 强大而灵活的网络体系。它们将复杂的网络配置抽象化，让开发者和运维人员可以专注于应用本身，而无需关心底层的网络细节。