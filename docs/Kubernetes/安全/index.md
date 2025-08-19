# Kubernetes 安全
Kubernetes 安全是一个涉及多个层面、需要综合考虑的复杂主题。它不仅仅是保护集群本身，更包括保护集群上运行的应用和数据。

Kubernetes 的安全模型通常可以分为四个主要层面：**集群安全、Pod 安全、网络安全和数据安全**。

---

### 1. 集群安全 (Cluster Security)

这是最基础也是最重要的一环，旨在保护 Kubernetes 核心组件和 API 服务器。

* **API 服务器访问控制**：API 服务器是 Kubernetes 的大脑，所有操作都通过它进行。它的安全至关重要。
    * **认证 (Authentication)**：确保只有被信任的实体（用户、服务账户、其他组件）能够与 API 服务器通信。
    * **授权 (Authorization)**：使用 **RBAC（基于角色的访问控制）** 限制用户和应用能做什么。例如，只允许开发团队访问他们自己命名空间中的 Pod，禁止访问其他命名空间或核心集群组件。
* **etcd 安全**：`etcd` 是 Kubernetes 的分布式数据库，存储了集群的所有状态，包括 Secret。
    * **加密**：必须对 `etcd` 数据库进行加密，防止数据泄露。
    * **网络隔离**：`etcd` 应该只允许 API 服务器访问，并且通信必须通过 TLS 加密。
* **Kubelet 安全**：`kubelet` 是在每个节点上运行的代理，负责管理 Pod。
    * **API 认证**：`kubelet` 必须使用证书进行认证，确保其向 API 服务器发送的请求是可信的。

---

### 2. Pod 安全 (Pod Security)

这个层面旨在保护 Pod 和容器本身，防止恶意或配置不当的 Pod 影响整个集群。

* **容器镜像安全**：
    * **扫描**：使用漏洞扫描工具（如 Trivy, Anchore）在镜像构建阶段或推送到仓库时扫描已知漏洞。
    * **签名与验证**：确保只运行来自受信任来源的镜像。
* **Pod 安全准入 (Pod Security Admission, PSA)**：这是一个内置的 Kubernetes 控制器，用于对创建 Pod 的请求进行强制性安全检查。
    * **`privileged` 特权**：限制 Pod 使用 `privileged` 模式，因为这会赋予容器对主机的完全控制权。
    * **`root` 用户**：限制容器以 `root` 用户身份运行，因为这可能导致权限升级。
    * **文件系统**：限制容器挂载敏感主机路径或使用可读写的根文件系统。
* **Secret 管理**：
    * **`etcd` 加密**：虽然 Secret 默认是 Base64 编码，但这并非加密。必须配置 `etcd` 加密，确保 Secret 在数据库中以加密形式存储。
    * **外部 Secret 存储**：使用外部 Secret 管理系统（如 HashiCorp Vault、AWS Secrets Manager）来管理敏感数据，Pod 启动时再通过 sidecar 或 CSI 驱动注入 Secret。

---

### 3. 网络安全 (Network Security)

这个层面旨在控制 Pod 之间的通信，实现“最小权限”网络访问原则。

* **网络策略 (Network Policies)**：默认情况下，所有 Pod 都可以相互通信。网络策略是 Pod 间的防火墙规则，可以限制特定 Pod 之间的流量。
    * **微隔离**：例如，只允许前端 Pod 访问后端 Pod，禁止直接访问数据库 Pod。
* **Ingress 和 Egress 流量控制**：
    * **Ingress 流量**：使用 Ingress 控制器和 Web 应用防火墙（WAF）来保护进入集群的流量。
    * **Egress 流量**：使用网络策略或服务网格（如 Istio）来控制 Pod 可以访问的外部服务。

---

### 4. 数据安全 (Data Security)

这个层面关注持久化数据的安全，即 Pod 使用的存储卷。

* **卷加密**：确保在云提供商或存储系统中，持久卷（PV）是加密的。
* **访问控制**：使用 RBAC 限制哪些用户或服务账户可以创建或访问 PVC。
* **CSI 驱动**：使用支持加密和访问控制的 CSI（Container Storage Interface）驱动。

---

### 总结

Kubernetes 安全不是单一的工具或配置，而是一套需要从**平台、Pod、网络和数据**四个层面协同工作的综合策略。从一开始就将安全融入到你的 CI/CD 流程和集群设计中，而不是事后才考虑，是确保 Kubernetes 环境安全的关键。