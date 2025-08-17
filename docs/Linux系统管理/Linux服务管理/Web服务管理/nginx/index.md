
# Nginx

## **一、Nginx 基础概述**

### 1. Nginx 简介

   * Nginx 的历史与发展
<div style="border:1px solid #0099cc; background-color:#e6f7ff; padding:10px; border-radius:5px;">
  <strong>诞生背景：</strong> </p>
  <ul>
  <li>Nginx 由俄罗斯工程师 Igor Sysoev 于 2004 年首次发布。</li>
  <li>设计初衷是为了解决 高并发连接 和 高性能 Web 服务的需求，特别是应对 C10K（同时处理一万个连接）问题。</li>
  </ul>
  <strong>发展历程：</strong> </p>
  <ul>
    <li>2004 年：Nginx 开源发布。</li>
    <li>2011 年：成立商业公司 Nginx, Inc.，提供企业支持与增强版。</li>
    <li>2019 年：被 F5 Networks 收购，继续保持开源社区版。</li>
  </ul>
  <strong>现状：</strong>
  <ul> 
    <li>已成为全球主流的 Web 服务器和反向代理软件。</li>
    <li>在 高并发、负载均衡、反向代理 场景中应用广泛。</li>
    <li>全球网站占有率（如 Netcraft 统计）：Nginx 与 Apache 长期处于市场前两位。</li>

</div>
   * Nginx 与 Apache、Lighttpd 对比
  
| 特性          | **Nginx**              | **Apache**      | **Lighttpd**  |
| ----------- | ---------------------- | --------------- | ------------- |
| **并发处理模型**  | 事件驱动，异步非阻塞             | 基于进程/线程，多进程或多线程 | 事件驱动，轻量级      |
| **性能（高并发）** | 高并发场景表现优异              | 高并发时性能下降明显      | 高并发支持较好，但功能有限 |
| **资源消耗**    | 内存和 CPU 占用低            | 内存占用较高，线程/进程开销大 | 占用低，但扩展性不佳    |
| **功能丰富度**   | 功能丰富，支持反向代理、负载均衡、缓存    | 模块众多，功能最全面      | 功能较少，适合小型场景   |
| **适用场景**    | 静态资源服务、反向代理、负载均衡、微服务网关 | 动态 Web 服务、传统网站  | 小型站点、嵌入式环境    |


   * Nginx 的应用场景（Web 服务、反向代理、负载均衡、缓存）
<div style="border:1px solid #0099cc; background-color:#e6f7ff; padding:10px; border-radius:5px;">
  <strong>Web：</strong> </p>
  <ul>
  <li>作为 Web 服务器，支持静态页面（HTML、CSS、JS、图片、视频等）高效处理。</li>
  <li>与 PHP-FPM、Tomcat、Python Web 框架（Flask、Django）配合，构建动态网站。</li>
  </ul>
  <strong>反向代理：</strong> </p>
  <ul>
    <li>客户端访问请求先到 Nginx，由 Nginx 转发到后端应用服务器。</li>
    <li>提供负载分担、安全保护、隐藏真实后端 IP。</li>
  </ul>
   <strong>负载均衡：</strong> </p>
  <ul>
    <li>支持多种调度算法（轮询、权重、IP 哈希、最少连接等）。</li>
    <li>常用于分布式架构，将流量分发到多个后端服务器，提高可靠性和性能。</li>
  </ul>
  <strong>缓存服务：</strong> </p>
  <ul>
    <li>支持 静态缓存 和 动态缓存（Proxy Cache、FastCGI Cache）。</li>
    <li>降低后端服务器压力，提高响应速度。</li>
  </ul>
  <strong>其他应用：</strong> </p>
  <ul>
    <li>作为 API 网关，管理微服务架构的请求。</li>
    <li>配合 HTTPS / TLS 提供安全通信。</li>
    <li>配合 监控工具（Prometheus、Grafana） 用于可观测性。</li>
  </ul>
</div>


### 2. Nginx 架构与特点

   * 事件驱动模型
   * 模块化设计
   * 高并发与高性能的实现

---

## **二、Nginx 安装与配置**

### 1. 安装方式

   * 源码编译安装
   * 包管理器安装（YUM、APT）
   * Docker 部署 Nginx
### 2. 基础配置文件解析

   * `nginx.conf` 主配置结构
   * 全局块 / events 块 / http 块 / server 块 / location 块
   * 常用指令：worker\_processes、worker\_connections、error\_log、pid

---

## **三、Web 服务配置**

1. 静态资源服务

   * 配置 `root` 与 `index`
   * MIME 类型配置
   * 访问日志和错误日志
2. 虚拟主机配置

   * 基于域名的虚拟主机
   * 基于端口的虚拟主机
   * 基于 IP 的虚拟主机

---

## **四、反向代理与负载均衡**

1. 反向代理

   * proxy\_pass 配置
   * 设置请求头（X-Forwarded-For、X-Real-IP）
   * 反向代理与正向代理对比
2. 负载均衡

   * 轮询（Round Robin）
   * 权重（Weight）
   * IP 哈希（IP Hash）
   * 最少连接（Least Connections）
   * upstream 配置

---

## **五、Nginx 与安全**

1. HTTPS 配置

   * SSL/TLS 基础
   * 自签名证书配置
   * 配置 Let’s Encrypt 免费证书
2. 访问控制

   * 基于 IP 的访问限制
   * 基于密码的访问控制（Basic Auth）
3. 防盗链、防刷请求

---

## **六、Nginx 高级功能**

1. 缓存配置

   * Proxy Cache 缓存
   * FastCGI Cache 缓存
   * 缓存清理策略
2. 动静分离

   * 静态文件由 Nginx 处理
   * 动态请求转发到后端应用（PHP-FPM、Tomcat、Flask、Django 等）
3. Rewrite 与重定向

   * rewrite 规则
   * permanent / redirect / return

---

## **七、Nginx 性能优化**

1. Worker 进程与连接数调优
2. Gzip 压缩配置
3. Keepalive 长连接
4. 日志切割与优化

---

## **八、Nginx 实战案例**

1. LNMP 架构（Linux + Nginx + MySQL + PHP）
2. Nginx + Tomcat/Java Web 应用
3. Nginx + Flask/Django
4. Nginx + Docker + 微服务网关

---

## **九、Nginx 运维与监控**

1. 常见错误与排查方法

   * 502 Bad Gateway
   * 504 Gateway Timeout
   * 413 Request Entity Too Large
2. Nginx 日志分析
3. 状态监控模块（stub\_status）
4. 配合 Prometheus + Grafana 监控

---

## **十、Nginx 模块扩展**

1. 官方模块与第三方模块
2. 动态模块编译
3. 常见第三方模块案例：

   * Lua 模块（OpenResty）
   * GeoIP 模块
   * Headers More 模块

---
