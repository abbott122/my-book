
## Go 简介与安装

### 📖 1.1 什么是 Go 语言？

**Go（或 Golang）** 是 Google 开发的一种开源编程语言，由 Robert Griesemer、Rob Pike 和 Ken Thompson 于 2007 年设计，2009 年正式发布。

#### 🧩 Go 的核心特性：

| 特性    | 说明                                |
| ----- | --------------------------------- |
| 简洁高效  | 语法简洁，易读易写                         |
| 静态类型  | 编译时检查类型，安全可靠                      |
| 原生并发  | 支持 goroutine 和 channel            |
| 快速编译  | 编译速度极快                            |
| 跨平台   | 一次编写，可编译运行于 Linux、macOS、Windows 等 |
| 标准库强大 | 自带强大、实用的标准库（HTTP、JSON、IO 等）       |

#### 🛠 Go 的典型应用场景：

* 后端 Web 服务（如：API 服务）
* 云原生项目（Kubernetes 就是用 Go 写的）
* 微服务架构
* DevOps 工具（如 Docker、Prometheus）
* 网络服务和 CLI 工具

---

### 💻 1.2 Go 开发环境安装
- 🧱 安装步骤：

#### ✅ 一、下载安装包

访问官网下载 Go 安装包：[https://go.dev/dl/](https://go.dev/dl/)

根据你的操作系统选择安装包：

* Windows：`.msi`
* macOS：`.pkg`
* Linux：`.tar.gz`

#### ✅ 二、安装并配置环境变量

-  📍 Windows / macOS（自动配置，无需手动设置）

安装后，打开终端（cmd / PowerShell / Terminal）执行以下命令检查：

```bash
go version
```

输出如：

```bash
go version go1.22.0 darwin/amd64
```

##### 📍 Linux 安装步骤示例：

```bash
# 1. 下载并解压
wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz

# 2. 配置环境变量
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc
source ~/.bashrc

# 3. 验证
go version
```

---

### 🗂 1.3 配置 Go 工作空间（GOPATH）

Go 的项目工作区建议如下结构（现代 Go 已不强制使用 `$GOPATH`）：

```bash
~/go/                  # Go 默认工作目录
├── bin/               # 可执行文件
├── pkg/               # 编译后的包
└── src/               # 源码项目
```

现代 Go 使用 `go mod` 进行依赖管理，不再依赖 GOPATH 也可以工作：

```bash
go mod init myproject
```

---

### 🧪 1.4 编写你的第一个 Go 程序

```go
// 文件名：hello.go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Golang!")
}
```

执行命令：

```bash
go run hello.go
```

输出：

```bash
Hello, Golang!
```

也可以编译为可执行文件：

```bash
go build hello.go
./hello
```






## 变量与常量（类型系统）
## 控制结构（if、for、switch）
## 数组、切片、Map（重点）
## 函数与返回值、多返回值
## 指针
## 包与模块（go mod）