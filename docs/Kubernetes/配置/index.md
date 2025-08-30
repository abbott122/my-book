# Kuberntes 配置

在 Kubernetes 中，**配置**是管理应用设置、凭证和其他可配置数据的核心部分。它将应用代码与配置数据解耦，使得你可以轻松地在不同环境（如开发、测试、生产）中部署相同的应用，而无需修改容器镜像。

Kubernetes 提供了两种主要的对象来处理配置：**ConfigMap** 和 **Secret**。

---

### ConfigMap 控制器

**ConfigMap** 是一种用于存储非敏感配置数据的 API 对象，例如：

* 环境变量
* 命令行参数
* 配置文件（如 `application.properties`）

**ConfigMap 控制器** 负责管理 ConfigMap 对象的生命周期，并确保其数据可以被 Pod 访问。

#### ConfigMap 的工作原理

ConfigMap 控制器通过以下方式工作：

1.  **存储数据**：你可以将配置数据以键值对的形式存储在 ConfigMap 中，例如 `database.url=my-db`。
2.  **创建对象**：`kubectl` 会将数据封装成一个 ConfigMap API 对象发送给 Kubernetes。
3.  **Pod 引用**：你可以在 Pod 的配置中引用 ConfigMap。
4.  **数据注入**：ConfigMap 控制器与 **kubelet**（在每个节点上运行的代理）协同工作，将 ConfigMap 中的数据**注入**到 Pod 中。

#### 如何将 ConfigMap 数据注入 Pod？

主要有三种方式：

1.  **作为环境变量**：将 ConfigMap 中的键值对作为 Pod 的环境变量。
2.  **作为命令行参数**：将 ConfigMap 的数据传递给容器的启动命令。
3.  **作为数据卷**：将 ConfigMap 挂载为 Pod 中的一个只读数据卷。每个键值对都将变成数据卷中的一个文件。

---

### Secret 控制器

**Secret** 是另一种用于存储敏感配置数据的 API 对象，例如：

* 数据库密码
* API 令牌
* SSH 密钥

**Secret 控制器** 负责管理 Secret 对象，并以更安全的方式处理它们。

#### Secret 的工作原理

Secret 的工作方式与 ConfigMap 类似，但有以下关键区别：

1.  **数据编码**：Secret 中的数据在存储到 `etcd` 之前会经过 **Base64 编码**。这并非加密，而是为了确保数据不会以明文形式存储或传输。为了增强安全性，通常会配合 **etcd 加密**。
2.  **权限控制**：Secret 可以通过 **RBAC（基于角色的访问控制）** 进行更细粒度的权限控制，只允许特定的用户或服务账号访问。
3.  **Pod 引用**：与 ConfigMap 类似，Secret 可以作为环境变量或数据卷注入到 Pod 中。当作为数据卷挂载时，它也会生成一个只读文件，但文件权限通常更严格。

#### Secret 的最佳实践

* **不要在 `Git` 中存储明文 Secret**：尽管 Secret 会被编码，但 Base64 很容易被解码。应使用工具（如 **`kubeseal`**、**`sops`**）对 Secret 进行加密，并在 Git 中存储加密后的 Secret。
* **使用 Secret 而非 ConfigMap 存储敏感数据**：这是为了利用 Secret 提供的更强的权限控制和加密选项。

---

### 总结：ConfigMap vs. Secret

| 特性 | **ConfigMap** | **Secret** |
| :--- | :--- | :--- |
| **用途** | 存储非敏感配置数据 | 存储敏感数据（密码、密钥） |
| **安全性** | 明文存储 | Base64 编码（非加密） |
| **访问控制** | 默认可被所有有权限的用户访问 | 更严格的 RBAC 权限控制 |
| **场景** | 数据库 URL、日志级别、应用配置 | 数据库密码、API Token、TLS 证书 |

ConfigMap 和 Secret 的主要价值在于将应用的**可执行部分（容器镜像）**与**可配置部分（配置数据）**解耦。这使得应用的部署和管理更加灵活和安全，是实现云原生应用部署的关键。

在 Kubernetes 中，配置管理是应用部署的关键环节。它通过将配置数据与应用镜像分离，使得应用更具可移植性和可扩展性。本回答将通过两个核心的配置对象：**ConfigMap** 和 **Secret**，结合具体的案例和常用命令，来详细解释配置管理。

-----

## 核心概念回顾

  * **ConfigMap**：用于存储**非敏感**的配置数据，如环境变量、命令行参数或配置文件。它以键值对的形式存储，可以被 Pod 轻松访问。
  * **Secret**：用于存储**敏感**数据，如密码、API 密钥或 SSH 密钥。它通过 Base64 编码，并以加密的方式存储在 Etcd 中，以确保数据的安全。

-----

### 配置案例：一个 Web 应用的部署

假设我们有一个 Web 应用，它需要一些配置：

  * 数据库地址：`database-url`（非敏感）
  * 网站标题：`site-title`（非敏感）
  * 数据库密码：`database-password`（敏感）

我们将使用 **ConfigMap** 和 **Secret** 来管理这些配置，并在 Deployment 中引用它们。

#### 步骤 1: 创建 ConfigMap 和 Secret

首先，我们通过 `kubectl` 命令来创建 ConfigMap 和 Secret。

**创建 ConfigMap**：

```bash
kubectl create configmap web-config --from-literal=database-url=mysql.database.svc.cluster.local --from-literal=site-title="My Awesome Blog"
```

**创建 Secret**：

```bash
kubectl create secret generic web-secret --from-literal=database-password="supersecurepassword123"
```

这个命令会将密码进行 Base64 编码并存储为 Secret。

#### 步骤 2: 在 Deployment 中引用配置

接下来，我们创建一个 Deployment，并在 Pod 模板中引用我们刚刚创建的 `ConfigMap` 和 `Secret`。

`web-app-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: web-app
        image: my-web-app:1.0
        ports:
        - containerPort: 80
        env:
          # 从 ConfigMap 引用非敏感配置
          - name: DB_URL
            valueFrom:
              configMapKeyRef:
                name: web-config
                key: database-url
          - name: SITE_TITLE
            valueFrom:
              configMapKeyRef:
                name: web-config
                key: site-title
          # 从 Secret 引用敏感配置
          - name: DB_PASSWORD
            valueFrom:
              secretKeyRef:
                name: web-secret
                key: database-password
```

在这个配置中：

  * `DB_URL` 和 `SITE_TITLE` 这两个环境变量的值来自 `web-config` ConfigMap 中的对应键。
  * `DB_PASSWORD` 环境变量的值来自 `web-secret` Secret 中的 `database-password` 键。

-----

### 常用命令

管理 ConfigMap 和 Secret 的命令与管理其他 Kubernetes 对象类似：

1.  **创建配置**：

      * 通过文件：`kubectl create configmap <name> --from-file=<file>`
      * 通过字面值：`kubectl create configmap <name> --from-literal=<key>=<value>`
      * 创建 Secret：`kubectl create secret generic <name> --from-literal=<key>=<value>`

2.  **查看 ConfigMap/Secret 列表**：

    ```bash
    kubectl get configmaps
    kubectl get secrets
    ```

3.  **查看 ConfigMap/Secret 详细内容**：

      * 查看 ConfigMap 详细信息：
        ```bash
        kubectl describe configmap web-config
        ```
      * 查看 ConfigMap 内容：
        ```bash
        kubectl get configmap web-config -o yaml
        ```
      * 查看 Secret 内容（注意：内容是 Base64 编码的）：
        ```bash
        kubectl get secret web-secret -o yaml
        ```
      * **解码 Secret 内容**：
        ```bash
        kubectl get secret web-secret -o jsonpath='{.data.database-password}' | base64 --decode
        ```

4.  **更新配置**：

      * 修改 ConfigMap/Secret YAML 文件：`kubectl apply -f configmap.yaml` 或 `kubectl apply -f secret.yaml`
      * 更新后，需要重启或重新部署 Pod 才能使配置生效，因为 Pod 默认只在启动时加载配置。

5.  **删除配置**：

    ```bash
    kubectl delete configmap web-config
    kubectl delete secret web-secret
    ```