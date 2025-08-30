
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
### Go语言（Golang）中的变量与常量

在Go语言中，变量（Variables）和常量（Constants）是编程的基础概念。变量用于存储可变的数据，而常量用于存储不可变的值。Go语言强调简洁性和类型安全，变量和常量的声明方式简单明了。下面我将详细解释它们的语法、使用场景、区别，并提供示例代码。

#### 1. 变量（Variables）
变量是用于存储数据的内存位置，其值可以在程序运行过程中改变。Go语言是静态类型语言，变量必须有明确的类型（或通过类型推断确定）。

##### 声明方式
- **标准声明**：使用`var`关键字。
  ```go
  var variableName type = value  // 示例：var age int = 25
  ```
  - 如果不初始化，默认值为零值（例如，int为0，string为空字符串""）。
  - 可以省略类型，让编译器推断：`var age = 25`（推断为int）。

- **短变量声明**：在函数内部使用`:=`操作符，自动推断类型。这是Go中最常用的方式。
  ```go
  name := "Alice"  // 推断为string
  ```

- **多变量声明**：可以一次性声明多个变量。
  ```go
  var a, b, c int = 1, 2, 3
  // 或
  x, y := 10, "hello"
  ```

##### 特点
- **作用域**：变量的作用域取决于声明位置（全局、函数内、块级）。
- **可变性**：值可以修改，例如`age = 26`。
- **未使用变量**：Go编译器会报错，如果声明了变量但未使用（这有助于代码清洁）。
- **零值**：未初始化的变量自动赋零值（bool为false，numeric为0，string为""，指针为nil等）。

##### 示例代码
```go
package main

import "fmt"

func main() {
    var score int = 90  // 标准声明
    level := "A"        // 短声明

    fmt.Println("Score:", score)  // 输出: Score: 90
    score = 95                    // 修改变量
    fmt.Println("New Score:", score)  // 输出: New Score: 95
}
```

#### 2. 常量（Constants）
常量是程序运行过程中不可修改的值，通常用于定义固定数据，如数学常数或配置值。常量在编译时确定。

##### 声明方式
- 使用`const`关键字。
  ```go
  const constantName type = value  // 示例：const PI float64 = 3.14159
  ```
  - 可以省略类型，让编译器推断：`const PI = 3.14159`。
  - 常量必须在声明时初始化，不能后期赋值。

- **多常量声明**：可以使用括号分组。
  ```go
  const (
      Monday = 1
      Tuesday = 2
  )
  ```

- **iota枚举**：Go使用`iota`来实现枚举常量，从0开始递增。
  ```go
  const (
      Red = iota  // 0
      Green       // 1
      Blue        // 2
  )
  ```
  - iota在const块中自动递增，支持位移等操作。

##### 特点
- **不可变性**：一旦声明，不能修改（尝试修改会编译错误）。
- **作用域**：可以是全局或包级，常用于共享不变值。
- **类型**：常量可以是无类型（untyped），允许在不同上下文中使用（如int或float64）。
- **表达式**：常量值可以是编译时计算的表达式（如`const Area = 3 * 4`）。

##### 示例代码
```go
package main

import "fmt"

const PI = 3.14159  // 无类型常量

func main() {
    const radius = 5
    area := PI * float64(radius) * float64(radius)  // 使用常量计算
    fmt.Println("Area:", area)  // 输出: Area: 78.53975

    // PI = 3.14  // 错误：不能修改常量
}
```

#### 3. 变量与常量的比较
使用表格来清晰比较二者的区别：

| 方面       | 变量 (Variables)                  | 常量 (Constants)                  |
|------------|-----------------------------------|-----------------------------------|
| **关键字** | `var` 或 `:=`                    | `const`                           |
| **可变性** | 可修改值                          | 不可修改（编译错误）              |
| **初始化** | 可延迟初始化（默认零值）          | 必须在声明时初始化                |
| **类型**   | 必须有类型（或推断）              | 可无类型（untyped）               |
| **作用域** | 全局、函数、块级                  | 主要全局或包级                    |
| **使用场景**| 存储动态数据，如用户输入、计算结果| 存储固定值，如配置、数学常数      |
| **示例**   | `var x int = 10; x = 20`         | `const Y = 10; // Y不可变`        |

#### 注意事项
- **性能**：常量在编译时求值，效率更高；变量在运行时分配内存。
- **命名规范**：Go推荐驼峰式命名（CamelCase），常量常用全大写（如PI）。
- **错误处理**：尝试修改常量或未使用变量都会导致编译失败，这是Go的严格设计。
- 如果你是初学者，建议在实际代码中练习这些概念。Go的官方文档（golang.org）有更多细节。

如果需要更具体的示例、代码运行结果或其他Go主题（如指针、结构体），请提供更多细节！


## 控制结构（if、for、switch）

### Go语言（Golang）中的控制结构

Go语言的控制结构用于控制程序的执行流程，包括条件判断、循环和分支跳转。Go语言的控制结构设计简洁，语法直观，遵循“少即是多”的哲学，剔除了传统语言中一些冗余的语法（如括号或某些循环形式）。以下是Go语言中主要的控制结构：`if`、`switch`、`for`和`select`，以及相关的跳转语句（如`break`、`continue`、`goto`）。

---

#### 1. 条件语句：`if`
`if`语句用于根据条件执行代码块。Go的`if`语句不需要括号，但大括号是必须的。

##### 语法
```go
if condition {
    // 代码块
} else if condition {
    // 代码块
} else {
    // 代码块
}
```

##### 特点
- **初始化语句**：`if`支持在条件前执行初始化语句，用分号分隔。
  ```go
  if x := 10; x > 5 {
      fmt.Println("x is greater than 5")
  }
  ```
  - 初始化语句的作用域仅限于`if`块。
- **无括号**：条件表达式不需要括号，如`if x > 5`而非`if (x > 5)`。
- **短路求值**：逻辑表达式（如`&&`、`||`）会短路求值。

##### 示例
```go
package main

import "fmt"

func main() {
    score := 85
    if score >= 90 {
        fmt.Println("Grade: A")
    } else if score >= 80 {
        fmt.Println("Grade: B")
    } else {
        fmt.Println("Grade: C")
    }
}
```

---

#### 2. 分支语句：`switch`
`switch`用于多条件分支，Go的`switch`比其他语言（如C）更灵活，且默认不会“贯穿”（fallthrough）。

##### 语法
```go
switch expression {
case value1:
    // 代码块
case value2, value3:
    // 代码块
default:
    // 默认代码块
}
```

##### 特点
- **初始化语句**：类似`if`，支持初始化语句。
  ```go
  switch x := compute(); x {
  case 1:
      fmt.Println("One")
  default:
      fmt.Println("Other")
  }
  ```
- **表达式可选**：可以省略`switch`后的表达式，作为`if-else`的替代。
  ```go
  switch {
  case x > 5:
      fmt.Println("x > 5")
  case x < 0:
      fmt.Println("x < 0")
  }
  ```
- **无自动fallthrough**：每个`case`默认会跳出`switch`，需要显式使用`fallthrough`关键字。
- **类型开关**：`switch`可用于接口类型的类型断言。
  ```go
  var i interface{} = 42
  switch v := i.(type) {
  case int:
      fmt.Println("Integer:", v)
  case string:
      fmt.Println("String:", v)
  }
  ```

##### 示例
```go
package main

import "fmt"

func main() {
    day := "Monday"
    switch day {
    case "Monday", "Tuesday":
        fmt.Println("Workday")
    case "Saturday", "Sunday":
        fmt.Println("Weekend")
    default:
        fmt.Println("Other day")
    }
}
```

---

#### 3. 循环语句：`for`
Go语言只有一种循环结构：`for`循环。它非常灵活，涵盖了其他语言中`while`和`for`的功能。

##### 语法
- **完整形式**（类似C的for循环）：
  ```go
  for init; condition; post {
      // 代码块
  }
  ```
- **条件循环**（类似while）：
  ```go
  for condition {
      // 代码块
  }
  ```
- **无限循环**：
  ```go
  for {
      // 代码块
  }
  ```
- **范围循环**（用于迭代数组、切片、字符串、映射或通道）：
  ```go
  for index, value := range collection {
      // 代码块
  }
  ```

##### 特点
- **单一循环结构**：Go没有`while`或`do-while`，全用`for`实现。
- **range**：用于遍历集合，`index`和`value`可选择性忽略（如用`_`占位）。
- **作用域**：初始化语句的变量仅在循环体内有效。
- **跳出循环**：使用`break`终止循环，`continue`跳到下一次迭代。

##### 示例
```go
package main

import "fmt"

func main() {
    // 标准for循环
    for i := 0; i < 3; i++ {
        fmt.Println("i =", i)
    }

    // 类似while的循环
    x := 5
    for x > 0 {
        fmt.Println("x =", x)
        x--
    }

    // range遍历切片
    fruits := []string{"apple", "banana", "orange"}
    for i, fruit := range fruits {
        fmt.Printf("Index: %d, Fruit: %s\n", i, fruit)
    }
}
```

---

#### 4. 并发控制：`select`
`select`是Go语言中用于处理并发通道（channel）的控制结构，类似于`switch`，但专用于通道操作。

##### 语法
```go
select {
case <-ch1:
    // 接收ch1的数据
case ch2 <- value:
    // 向ch2发送数据
default:
    // 无通道可用时执行
}
```

##### 特点
- **随机选择**：当多个通道同时就绪，`select`随机选择一个执行。
- **非阻塞**：`default`分支使`select`非阻塞。
- **典型场景**：处理多个goroutine的通信。

##### 示例
```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go func() {
        time.Sleep(1 * time.Second)
        ch1 <- "from ch1"
    }()
    go func() {
        time.Sleep(2 * time.Second)
        ch2 <- "from ch2"
    }()

    select {
    case msg1 := <-ch1:
        fmt.Println(msg1)
    case msg2 := <-ch2:
        fmt.Println(msg2)
    case <-time.After(3 * time.Second):
        fmt.Println("Timeout")
    }
}
```

---

#### 5. 跳转语句
Go支持以下跳转语句，用于控制程序流：

- **`break`**：跳出当前循环或`switch`/`select`。
  ```go
  for i := 0; i < 10; i++ {
      if i == 5 {
          break  // 退出循环
      }
      fmt.Println(i)
  }
  ```

- **`continue`**：跳到下一次循环。
  ```go
  for i := 0; i < 5; i++ {
      if i%2 == 0 {
          continue  // 跳过偶数
      }
      fmt.Println(i)  // 输出1, 3
  }
  ```

- **`goto`**：跳转到指定标签（不推荐频繁使用，易导致代码混乱）。
  ```go
  if x := 10; x > 5 {
      goto Label
  }
  fmt.Println("This won't print")
Label:
  fmt.Println("Jumped to Label")
  ```

- **`return`**：从函数返回，终止函数执行。

---

#### 注意事项
- **简洁性**：Go的控制结构避免了复杂语法（如C的`do-while`或多余括号），代码更易读。
- **严格性**：未使用的变量或标签会导致编译错误，强制开发者编写清晰代码。
- **并发支持**：`select`是Go并发模型的核心，与goroutine和channel紧密相关。
- **性能**：`switch`和`for`经过优化，适合高性能场景；`goto`虽可用，但应谨慎使用。

---

#### 示例综合代码
以下是一个综合示例，展示多种控制结构的结合：

```go
package main

import "fmt"

func main() {
    numbers := []int{1, 2, 3, 4, 5}

    // for 和 if
    for _, num := range numbers {
        if num%2 == 0 {
            fmt.Println(num, "is even")
        } else {
            fmt.Println(num, "is odd")
        }
    }

    // switch
    grade := 85
    switch {
    case grade >= 90:
        fmt.Println("A")
    case grade >= 80:
        fmt.Println("B")
    default:
        fmt.Println("C or below")
    }

    // 无限循环与break
    count := 0
    for {
        if count >= 3 {
            break
        }
        fmt.Println("Count:", count)
        count++
    }
}
```

---

#### 总结
Go的控制结构包括：
- **条件**：`if`（带可选初始化）和`switch`（支持表达式、类型开关）。
- **循环**：`for`（涵盖标准、while、range形式）。
- **并发**：`select`（处理通道通信）。
- **跳转**：`break`、`continue`、`goto`、`return`。

Go的控制结构设计简洁、高效，适合并发和高性能场景。如果你需要更深入的讲解（例如并发场景中的`select`优化）或特定代码示例，请告诉我！
## 数组、切片、Map（重点）
在 Go 语言中，数组、切片和 Map 是三种非常重要的数据结构，它们各有特点和用途。掌握它们是编写高效 Go 程序的关键。下面我将详细为你介绍这三者。

### 数组 (Array)

数组是一种**值类型**，它的大小在声明时就已固定，且不能改变。数组中的所有元素都具有相同的类型，并以连续的内存块形式存储。

**声明与初始化：**

```go
// 声明一个长度为 3 的 int 数组，元素默认初始化为 0
var a [3]int

// 声明并初始化
b := [3]int{1, 2, 3}

// 长度自动推断
c := [...]int{4, 5, 6, 7}
```

**特点：**

  * **固定长度：** 声明后长度无法改变。
  * **值类型：** 赋值或作为函数参数传递时，会复制整个数组。这可能导致性能开销，尤其当数组较大时。
  * **连续内存：** 元素在内存中是连续存放的。

由于其固定长度的限制，数组在实际开发中用得相对较少，更常用的是切片。

-----

### 切片 (Slice)

切片是 Go 语言中最常用的动态数组，它是一个**引用类型**。切片是对底层数组的一个“视图”或“引用”，它本身不存储任何数据，而是指向一个底层数组。

**切片的结构：**

切片由三个部分组成：

1.  **指向底层数组的指针 (pointer)**
2.  **长度 (length)：** 切片当前包含的元素数量
3.  **容量 (capacity)：** 从切片起点到其底层数组末尾的元素数量

**声明与初始化：**

```go
// 从数组创建切片
arr := [5]int{1, 2, 3, 4, 5}
s := arr[1:4] // s 是 arr 的一个切片，包含 arr[1]、arr[2]、arr[3]

// 直接声明切片
s2 := []int{1, 2, 3}

// 使用 make 函数创建
// make([]Type, length, capacity)
s3 := make([]int, 5)        // 长度为 5，容量为 5
s4 := make([]int, 0, 10)    // 长度为 0，容量为 10
```

**操作：**

  * **`len()` 和 `cap()`：** 分别用于获取切片的长度和容量。
  * **`append()`：** 向切片追加元素。当容量不足时，`append` 会自动创建一个更大的底层数组，并将原有数据复制过去，这会改变切片的底层数组指针。
  * **`copy()`：** 复制切片。`copy(dst, src)` 会将 `src` 切片中的元素复制到 `dst` 切片中，复制的元素数量取决于两者中较小的那个长度。

**切片的扩容机制：**

当使用 `append` 添加元素，而当前容量不足以容纳新元素时，Go 会进行扩容。扩容的规则是：

  * 如果原容量小于 1024，新容量会翻倍。
  * 如果原容量大于或等于 1024，新容量将增加 25% (1.25 倍)。

**切片是引用类型：**

当一个切片作为参数传递给函数时，实际上是传递了切片的指针、长度和容量，而不是复制整个底层数组。这使得切片在函数间传递非常高效。

```go
func modifySlice(s []int) {
    s[0] = 99
}

mySlice := []int{1, 2, 3}
modifySlice(mySlice)
fmt.Println(mySlice) // 输出: [99 2 3]
```

-----

### Map（哈希表）

Map 是一种**键值对**的集合，它提供了快速的查找、插入和删除操作。Map 是一个**引用类型**，它无序存储键值对。

**声明与初始化：**

  * **`make()` 函数：**
    `make(map[KeyType]ValueType, initialCapacity)`

<!-- end list -->

```go
// 声明一个空的 map，键为 string，值为 int
var myMap map[string]int

// 必须使用 make 初始化才能使用
myMap = make(map[string]int)

// 声明并初始化，长度为 5
myMap2 := make(map[string]int, 5)

// 字面量初始化
ages := map[string]int{
    "Alice": 30,
    "Bob":   40,
}
```

**操作：**

  * **赋值：** `myMap["key"] = value`
  * **取值：** `value, ok := myMap["key"]`
      * 这个语法非常重要，它允许你检查键是否存在。如果键不存在，`value` 将是值类型的零值，`ok` 为 `false`。
  * **删除：** `delete(myMap, "key")`
  * **遍历：** 使用 `for range` 循环。

<!-- end list -->

```go
// 遍历 map
for key, value := range ages {
    fmt.Printf("Name: %s, Age: %d\n", key, value)
}
```

**特点：**

  * **无序：** 遍历 Map 的顺序是**不固定**的，每次遍历的结果都可能不同。
  * **键的唯一性：** Map 中的键是唯一的。如果你用相同的键赋新值，会覆盖旧值。
  * **引用类型：** Map 作为函数参数传递时，传递的是引用，修改会影响到原始 Map。

### 总结与对比

| 特性     | 数组 (Array)             | 切片 (Slice)                 | Map (哈希表)                  |
|----------|--------------------------|------------------------------|-------------------------------|
| **类型** | 值类型                   | 引用类型                     | 引用类型                      |
| **长度** | 固定，不可变             | 动态，可变                   | 动态，可变                    |
| **存储** | 连续内存                 | 引用底层数组，非连续         | 散列存储，无序                 |
| **用途** | 极少使用，适用于固定大小 | 最常用，动态集合             | 键值对存储，快速查找           |
| **适用场景** | 性能敏感且大小已知      | 大多数列表、集合场景         | 需要快速通过键查找值的场景     |
| **操作** | 索引访问 `a[i]`          | 索引访问 `s[i]`、`append`、`copy` | 键访问 `m[k]`、`delete`     |

总的来说，在 Go 语言中：

  * **数组**主要用于**底层**或**性能敏感**的固定大小数据。
  * **切片**是日常开发中处理**集合数据**的**首选**。
  * **Map** 则是处理**键值对**数据的**不二选择**。

理解并正确使用这三种数据结构，是写出地道、高效 Go 代码的关键。


## 函数与返回值、多返回值
## 指针
## 包与模块（go mod）