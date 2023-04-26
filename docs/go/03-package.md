---
sidebar_label: '包管理'
sidebar_position: 3
title: 包管理
---

### 什么是包，为什么要使用他们？
到目前为止，我们看到的 Go 程序只有一个文件，其中包含一个 main 函数和几个其他函数。在现实场景中，这种将所有源代码写入单个文件的方法不可扩展。重用和维护以这种方式编写的代码变得不可能。

包是位于同一目录中的 Go 源文件的集合, 包用于组织 Go 源代码，以提高可重用性和可读性。

例如，假设我们正在用 Go 编写一个财务应用程序，其中一些功能是单利计算、复利计算和贷款计算。组织此应用程序的一种简单方法是按功能。我们可以创建包 simpleinterest 、 compoundinterest 和 loan 。如果 loan 包需要计算单利，只需导入 simpleinterest 包即可。这样代码就可以重用了。

我们将通过创建一个简单的应用程序来学习包，以确定给定的本金、利率和持续时间（以年为单位）的单利。

### 程序入口

每个可执行的 Go 应用程序都必须包含 main 函数。该函数是程序执行的入口点。主要功能应该位于主包中。

package packagename 指定特定源文件属于包 packagename 。这应该是每个 go 源文件的第一行。

让我们开始为我们的应用程序创建主函数和主包。

运行以下命令在当前目录中创建一个名为 learnpackage 的目录,执行完后我们看到两个文件 `go.mod` 和 `main.go`
```shell
mkdir learnpackage && cd learnpackage && touch main.go  && go mod init learnpackage
```
我们看看 go.mod 中的内容如下：
```go
module learnpackage

go 1.20
```

module learnpackage 行指定模块的名称为 learnpackage 。正如我们之前提到的， learnpackage 将是导入在此模块中创建的任何包的基本路径。 go 1.20 行指定此模块中的文件使用 go version 1.20

我们把上一讲的 代码片段复制到 main.go 文件中：
```go
package main

import "fmt"

func main() {
    // 这是一段注释
   fmt.Println("Hello, World!")
}
```
然后执行 `go run main.go` 可以看到程序运行成功，如下图：

![](https://raw.githubusercontent.com/mouuii/picture/master/%E6%88%AA%E5%B1%8F2023-04-26%20%E4%B8%8A%E5%8D%8810.01.11.png)

### 创建一个自定义的包

属于一个包的源文件应该放在它们自己的单独文件夹中。 Go 中的约定是将此文件夹命名为与包同名。

让我们在 learnpackage 文件夹中创建一个名为 simpleinterest 的文件夹。 mkdir simpleinterest 将为我们创建这个文件夹.

simpleinterest 文件夹中的所有文件都应以 package simpleinterest 行开头，因为它们都属于 simpleinterest 包

以下将是我们应用程序的目录结构
```yaml
├── learnpackage
│   ├── go.mod
│   ├── main.go
│   └── simpleinterest
│       └── simpleinterest.go
```

将以下代码添加到 simpleinterest.go 文件。

```go
package simpleinterest

//Calculate calculates and returns the simple interest for a principal p, rate of interest r for time duration t years
func Calculate(p float64, r float64, t float64) float64 {  
    interest := p * (r / 100) * t
    return interest
}
```

在上面的代码中，我们创建了一个函数 Calculate ，它计算并返回单利。这个函数以大写C开头，这样我们的主包就可以引用这个函数。
:::tip

在 Go 语言中，一个包下的某个函数名如果以大写字母开头，那么它是一个公共函数，可以被其他包导入并使用；如果以小写字母开头，那么它是一个私有函数，只能在当前包内部使用。这是 Go 语言中的一种封装机制，有助于实现模块化和代码重用。
:::

### 导入我们自定义的包

要使用自定义包，我们必须先导入它。导入路径是模块的名称加上包的子目录和包名。在我们的例子中，模块名称是 learnpackage ，包 simpleinterest 在 learnpackage 下的 simpleinterest 文件夹中

所以 import "learnpackage/simpleinterest" 行将导入 simpleinterest 包。

如果我们有这样的目录结构

```yaml
learnpackage  
│   └── finance
│       └── simpleinterest
```

那么导入语句将是 import "learnpackage/finance/simpleinterest"

将以下代码添加到 main.go
```go
package main

import (  
    "fmt"
    "learnpackage/simpleinterest"
)

func main() {  
    fmt.Println("Simple interest calculation")
    p := 5000.0
    r := 10.0
    t := 1.0
    si := simpleinterest.Calculate(p, r, t)
    fmt.Println("Simple interest is", si)
}
```

上面的代码导入 simpleinterest 包，使用 Calculate 函数求单利。标准库中的包不需要模块名称前缀，因此“fmt”可以在没有模块前缀的情况下工作。当应用程序运行时，输出将是
```shell
Simple interest calculation  
Simple interest is 500 
```

### 编译成可执行程序

要将 Go 语言程序编译成可执行文件，请遵循以下步骤：
1. 首先，确保已经正确安装了 Go 语言编译器。可以在官方网站下载并安装适合您操作系统的 Go 语言编译器。安装完成后，通过在命令行或终端中输入 `go version` 命令来检查是否安装成功。

2. 编写 Go 程序。使用文本编辑器或集成开发环境（IDE）编写一个简单的 Go 语言程序。例如，创建一个名为 main.go 的文件，内容如下：
3. 打开命令行或终端，导航到包含 main.go 文件的目录。
4. 输入以下命令以编译程序 `go build`
5. 这将在当前目录下生成一个可执行文件。文件名将根据您的操作系统和程序的包名而有所不同。例如，在 Windows 上，您可能会看到名为 main.exe 的文件；在 macOS 或 Linux 上，可能会看到名为 main 的文件。

运行可执行文件。在命令行或终端中，根据您的操作系统输入相应的命令：
- 对于 Windows：
```shell
./main.exe
```
- 对于 macOS 或 Linux：
```shell
./main
```
这将运行编译好的程序，并输出 "Hello, World!"。

请注意，如果您希望在编译时为可执行文件指定不同的名称，可以使用 -o 选项，例如：
```shell
go build -o myprogram
```
这将生成一个名为 myprogram（或 myprogram.exe，取决于操作系统）的可执行文件。

### init 函数

在 Go 语言中，init 函数是一个特殊的函数，用于在程序启动时执行包级别的初始化操作。每个包可以包含一个或多个 init 函数。它们在程序执行之前自动执行，无需显式调用。以下是关于 init 函数的一些重要信息：

- init 函数在包导入时自动执行。当您导入一个包时，该包中的所有 init 函数都会在程序启动之前自动执行，这样可以完成一些包级别的初始化工作。

- 不需要显式调用 init 函数。Go 语言会自动执行每个包中的所有 init 函数。

- 可以在每个文件中定义多个 init 函数。它们将按照在文件中的声明顺序执行。

- 多个包的 init 函数执行顺序是根据包的依赖关系确定的。在导入的包之间存在依赖关系时，会首先执行被依赖包的 init 函数，然后再执行依赖它的包的 init 函数。

- init 函数不能有参数和返回值。

下面是一个简单的 init 函数示例：
```go
package main

import "fmt"

var globalVar int

func init() {
    fmt.Println("Initializing globalVar")
    globalVar = 10
}

func main() {
    fmt.Println("Value of globalVar:", globalVar)
}
```
在这个例子中，init 函数在 main 函数之前执行，用于初始化全局变量 globalVar 的值。当运行此程序时，将看到以下输出：
```sh
Initializing globalVar
Value of globalVar: 10
```
:::tip

注意 init 函数在 main 函数之前执行，用于完成全局变量的初始化。

:::

下面回到我们的 learnpackage 代码，让我们将 init 函数添加到 simpleinterest.go 文件。
```go
package simpleinterest

import "fmt"

/*
 * init function added
 */
func init() {  
    fmt.Println("Simple interest package initialized")
}
//Calculate calculates and returns the simple interest for principal p, rate of interest r for time duration t years
func Calculate(p float64, r float64, t float64) float64 {  
    interest := p * (r / 100) * t
    return interest
}
```
我们添加了一个简单的初始化函数，它只打印 Simple interest package initialised

现在让我们修改主包。我们知道，在计算单利时，本金、利率和期限都应该大于零。我们将使用 main.go 文件中的 init 函数和包级变量来定义此检查。

将 main.go 修改为以下内容：
```go
package main 

import (  
    "fmt"
    "learnpackage/simpleinterest" //importing custom package
    "log"
)
var p, r, t = 5000.0, 10.0, 1.0

/*
* init function to check if p, r and t are greater than zero
 */
func init() {  
    println("Main package initialized")
    if p < 0 {
        log.Fatal("Principal is less than zero")
    }
    if r < 0 {
        log.Fatal("Rate of interest is less than zero")
    }
    if t < 0 {
        log.Fatal("Duration is less than zero")
    }
}

func main() {  
    fmt.Println("Simple interest calculation")
    si := simpleinterest.Calculate(p, r, t)
    fmt.Println("Simple interest is", si)
}
```

以下是对 main.go 所做的修改


- p、r 和 t 变量从 main 函数级别移动到包级别。

- 添加了一个初始化函数。如果使用 log.Fatal 函数，本金、利率或持续时间小于零，则 init 函数会打印日志并终止程序执行。

初始化顺序如下:
1. 导入的包首先被初始化。因此，首先初始化 simpleinterest 包，然后调用它的 init 方法。
2. 接下来初始化包级变量 p、r 和 t。
3. init函数在main中被调用。
4. 最后调用main函数。

如果运行该程序，您将获得以下输出。
```sh
Simple interest package initialized  
Main package initialized  
Simple interest calculation  
Simple interest is 500  
```

正如预期的那样，首先调用 simpleinterest 包的 init 函数，然后是包级变量 p 、 r 和 t 的初始化。接下来调用主包的init函数。它检查 p 、 r 和 t 是否小于零，如果条件为真则终止。我们将在单独的教程中详细了解 if 语句。现在您可以假设 if p < 0 将检查 p 是否小于 0，如果是，程序将终止。我们为 r 和 t 编写了类似的条件。在这种情况下，所有这些条件都不成立，程序继续执行。最后调用main函数。