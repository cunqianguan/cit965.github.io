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

1. `package main` 

     每个 go 文件都必须以 package name 语句开头。包用于提供代码划分和可重用性。这里使用包名 main 。 main 函数应该始终驻留在 main 包中。

2. `import "fmt"` 

    import 语句用于导入其他包。在我们的例子中， fmt 包被导入，它将在 main 函数中使用，将文本打印到标准输出。

3. `func main(){}` 

    func 关键字标记函数的开始。 main 是一个特殊的函数。程序执行从 main 函数开始。 { 和 } 大括号表示主函数的开始和结束。

4. `fmt.Println("Hello, World!")` 

    fmt 包的 Println 函数用于将文本写入标准输出。 package.Function() 是调用包中函数的语法。
## 执行 Go 程序

1. 在本地电脑安装go语言运行环境，你可以在[官网](https://go.dev/dl/)下载安装包安装。

2. 将上面的代码保存为 main.go 的文件,并在你的控制台输入下面的命令 `$ go run main.go`, 看看会发生什么吧！ 如果你暂时不想在自己计算机安装go，你可以在 [云编辑器](https://go.dev/play/) 上在线运行！虽然这有限制，但是当我们想要运行简单的程序时，这种方法就派上用场了，因为它使用浏览器并且不需要在本地安装 Go :)

## 开启你的编程之路

编程就是如此简单，你已经运行了一个程序！让我们选择golang这门语言作为我们入门编程的第一门语言吧,它比 python、c++、java 简单太多了！