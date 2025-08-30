在 Kubernetes 中，**监控**和**日志**是运维工作不可或缺的两大支柱，它们帮助你了解集群和应用的健康状况、性能瓶颈以及潜在问题。

Kubernetes 本身并不提供完整的监控和日志解决方案，而是提供了开放的接口，让你可以集成各种云原生工具。最常见的解决方案是基于 **Prometheus** 和 **Grafana** 的监控栈，以及基于 **ELK** (Elasticsearch, Logstash, Kibana) 或 **Loki** 的日志栈。

---

### Kubernetes 监控

监控的核心目标是**收集、存储和可视化指标**。

#### 1. 核心组件

* **指标收集 (Metrics Collection)**:
    * **cAdvisor**: 这是内置在 kubelet 中的一个代理，它会自动收集节点上所有容器的资源使用情况，如 CPU、内存和文件系统使用量。
    * **Node Exporter**: 在每个节点上运行的 DaemonSet，用于收集节点的操作系统和硬件指标。
    * **kube-state-metrics**: 这是一个专门的组件，它将 Kubernetes API 对象的指标（例如 Deployment 的副本数、Pod 的状态）暴露为 Prometheus 可识别的格式。
    * **自定义指标**: 你的应用可以暴露自己的指标，如请求延迟、错误率等。

* **指标存储与查询 (Storage & Query)**:
    * **Prometheus**: 这是一个开源的监控系统，它通过 **Pull** 模式从上述 Exporter 中拉取指标数据，并以时间序列数据库的形式存储。它提供了一个强大的查询语言 **PromQL**，用于分析和聚合数据。

* **数据可视化与报警 (Visualization & Alerting)**:
    * **Grafana**: 一个流行的开源数据可视化工具。它可以连接到 Prometheus，通过强大的仪表盘功能将复杂的指标数据以图表形式展示出来，让你一眼就能了解集群和应用的健康状态。
    * **Prometheus Alertmanager**: 负责处理 Prometheus 发送的告警。它能进行告警去重、分组，并将告警发送到 Slack、邮件或 PagerDuty 等通知渠道。

---

### Kubernetes 日志

日志的核心目标是**收集、聚合和搜索应用及系统日志**。

#### 1. 核心组件

* **日志收集 (Log Collection)**:
    * **Log Agent**: 这是一个在每个节点上以 **DaemonSet** 形式运行的代理，负责收集该节点上所有容器的日志。常见的日志代理包括 **Fluentd**、**Filebeat** 或 **Loki 的 Promtail**。
    * **容器日志**: Kubernetes 会将容器的 `stdout` 和 `stderr` 流重定向到文件中，这些文件通常位于节点的 `/var/log/containers` 目录下。

* **日志存储与索引 (Storage & Index)**:
    * **Elasticsearch**: 一个强大的分布式搜索和分析引擎，非常适合存储和索引海量的日志数据，以便快速搜索和分析。
    * **Loki**: 一个开源的日志聚合系统，它的设计借鉴了 Prometheus，只对日志元数据进行索引。这使得它更轻量、成本更低。

* **日志可视化与分析 (Visualization & Analysis)**:
    * **Kibana**: 与 Elasticsearch 配套的可视化工具，用于探索和分析日志数据，创建仪表盘和报表。
    * **Grafana**: 不仅可以用于监控，还可以通过其 Loki 数据源来查询和可视化日志。

---

### 总结与实践

在 Kubernetes 中，监控和日志通常是分开管理的，但它们共同构成了可观测性 (Observability) 的核心。一个典型的实践方案是：

1.  **监控栈**:
    * 使用 **Prometheus** 搭配 **Node Exporter**、**kube-state-metrics** 来收集集群指标。
    * 使用 **Grafana** 构建仪表盘进行可视化，并配置 **Alertmanager** 进行告警。

2.  **日志栈**:
    * 使用 **Fluentd** 或 **Filebeat** DaemonSet 来收集节点上的所有日志。
    * 将日志发送到 **Elasticsearch**，并使用 **Kibana** 进行日志搜索和分析。

通过这种方式，你可以全面掌握 Kubernetes 集群的运行状态，从宏观的性能指标到微观的单条应用日志，实现对云原生应用的有效管理。