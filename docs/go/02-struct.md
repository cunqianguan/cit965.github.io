---
sidebar_label: '程序结构'
sidebar_position: 1
title: 程序结构
---

## 基础组成

Go 语言的基础组成有以下几个部分：

- 包声明
- 引入包
- 函数
- 变量
- 语句 & 表达式
- 注释

## Hello，world！
接下来让我们来一段简单的代码，该代码会在控制台输出"Hello World!":

```go
package main

import "fmt"

func main() {
    // 这是一段注释
   fmt.Println("Hello, World!")
}
```

1. 第一行代码 *package main* 定义了包名。你必须在源文件中非注释的第一行指明这个文件属于哪个包，如：package main。package main表示一个可独立执行的程序，每个 Go 应用程序都包含一个名为 main 的包。
2. 下一行 *import "fmt"* 告诉 Go 编译器这个程序需要使用 fmt 包（的函数，或其他元素），fmt 包实现了格式化 IO（输入/输出）的函数。
3. 下一行 *func main()* 是程序开始执行的函数。main 函数是每一个可执行程序所必须包含的，一般来说都是在启动后第一个执行的函数（如果有 init() 函数则会先执行该函数）。

## 执行 Go 程序

1. 安装go语言运行环境，你可以在[官网](https://go.dev/dl/)下载安装包

2. 将上面的代码保存为 main.go 的文件,并在你的控制台输入下面的命令 `$ go run main.go`, 看看会发生什么吧！ 如果你暂时不想在自己计算机安装go，你可以在 [云编辑器](https://go.dev/play/) 上在线运行！