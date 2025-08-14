## zabbix 企业分布式监控系统


## **第一部分：Zabbix 基础入门**

### **第1章：监控系统概述与Zabbix简介**
* 什么是监控系统？为什么需要监控？
* 监控系统的核心指标：可用性、性能、告警
* Zabbix 介绍：发展历史、核心组件（Server, Agent, Proxy, Frontend）
* Zabbix 的优势与适用场景

### **第2章：Zabbix 环境搭建**
* Linux 系统基础准备（CentOS/Ubuntu）
* 数据库安装与配置（MySQL/MariaDB）
* Zabbix Server、Zabbix Agent 的安装与配置
* Zabbix Web 界面（Frontend）的安装与配置
* 初次登录与界面概览

### **第3章：Zabbix 核心概念**
* **主机（Host）** 与 **主机群组（Host Group）**
* **监控项（Item）**：数据采集的基本单位
* **触发器（Trigger）**：定义问题的逻辑表达式
* **事件（Event）** 与 **告警（Alert）**
* **动作（Action）**：告警通知与远程命令执行
* **模板（Template）**：批量配置的利器

---

## **第二部分：Zabbix 监控实战**

### **第4章：基本监控配置**
* 使用 Zabbix Agent 监控 Linux 主机（CPU、内存、磁盘、网络）
* 使用 SNMP 监控网络设备（交换机、路由器）
* 使用 IPMI 监控服务器硬件状态
* 监控 Windows 主机

### **第5章：高级监控技术**
* **自定义监控项（UserParameter）**：满足特殊监控需求
* **键值对监控（Key-Value Monitoring）**
* **Zabbix Agent 主动模式与被动模式** 的区别与应用
* 使用 Zabbix LLD **（Low-Level Discovery）** 自动发现监控项
    * 自动发现文件系统、网卡、端口等

### **第6章：数据库与Web服务监控**
* MySQL/MariaDB 数据库监控：性能指标、连接数、慢查询等
* PostgreSQL/MongoDB 监控（根据需要选择）
* Nginx/Apache Web 服务器监控：访问量、状态码、响应时间等
* Tomcat/Java 应用监控

---

## **第三部分：Zabbix 告警与优化**

### **第7章：告警通知与动作**
* 配置 **媒介类型（Media Type）**：邮件、短信、微信、钉钉
* 创建 **动作（Action）**：定义告警发送规则
* 配置告警升级与恢复通知
* 通过脚本实现自定义告警通知

### **第8章：数据可视化与报表**
* **图表（Graph）** 的创建与定制
* **屏幕（Screen）** 和 **幻灯片（Slide Show）**：组合展示监控数据
* 创建 **仪表盘（Dashboard）**
* 生成 Zabbix 报表

### **第9章：Zabbix 性能优化与故障排查**
* Zabbix Server 性能瓶颈分析
* 数据库性能优化建议
* Zabbix Proxy 的部署与应用场景
* Zabbix Cache、History Cache、Trend Cache 的优化
* Zabbix 常见故障排查：Agent 不可用、数据采集延迟、告警不发送等

---

## **第四部分：Zabbix 高级进阶**

### **第10章：Zabbix API 应用**
* Zabbix API 介绍与认证
* 通过 API 实现主机批量增删改查
* 通过 API 实现模板导入导出
* 使用 Python 脚本调用 Zabbix API

### **第11章：Zabbix 分布式监控**
* Zabbix Proxy 的原理与部署
* 通过 Proxy 解决跨网络、跨地域监控问题
* Proxy 与 Server 的通信模式

### **第12章：高可用与大规模部署**
* Zabbix Server 高可用方案（keepalived）
* 数据库高可用方案（MHA/Galera）
* 大规模 Zabbix 部署架构设计
* Zabbix 维护与备份
