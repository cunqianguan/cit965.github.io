---
sidebar_label: 13.string
sidebar_position: 13
title: 12.string
---

[//]: # (Welcome to tutorial no. 14 in [Golang tutorial series]&#40;https://golangbot.com/learn-golang-series/&#41;.)
欢迎收看【Golang教程系列】第14期教程(https://golangbot.com/learn-golang-series/)。

[//]: # (Strings deserve a special mention in Go as they are different in implementation when compared to other languages.)
字符串值得在Go中特别提及，因为与其他语言相比，它们在实现方面有所不同。

[//]: # (### What is a String?)

### 字符串是什么?

[//]: # (**A string is a [slice]&#40;https://golangbot.com/arrays-and-slices/&#41; of bytes in Go. Strings can be created by enclosing a set of characters inside double quotes `" "`.**)
字符串在Go语言中是一个字节数组的切片。可以通过在双引号（""）中的一组字符创建

[//]: # (Let's look at a simple example that creates a `string` and prints it.)
让我们看一个创建“字符串”并打印它的简单示例

```
package main

import (  
    "fmt"
)

func main() {  
    name := "Hello World"
    fmt.Println(name)
}
```

[//]: # ([Run in playground]&#40;https://play.golang.org/p/o9OVDgEMU0&#41;)

[//]: # (The above program will print `Hello World`.)
上面程序将会打印 Hello World

[//]: # (Strings in Go are [Unicode compliant]&#40;https://naveenr.net/unicode-character-set-and-utf-8-utf-16-utf-32-encoding/&#41; and)

[//]: # (are [UTF-8 Encoded]&#40;https://naveenr.net/unicode-character-set-and-utf-8-utf-16-utf-32-encoding/&#41;.)
Go中的字符串符合Unicode，并且是UTF-8编码的

[//]: # (### Accessing individual bytes of a string)

### 访问字符串中的单个字节

[//]: # (Since a string is a slice of bytes, it's possible to access each byte of a string.)
由于字符串是字节的切片，因此可以访问到字符串中的每个字节

```
package main

import (  
    "fmt"
)

func printBytes(s string) {  
    fmt.Printf("Bytes: ")
    for i := 0; i < len(s); i++ {
        fmt.Printf("%x ", s[i])
    }
}

func main() {  
    name := "Hello World"
    fmt.Printf("String: %s\n", name)
    printBytes(name)
}
```

[//]: # ([Run in playground]&#40;https://play.golang.org/p/B3KgBBQhiN9&#41;)

[//]: # (**%s is the format specifier to print a string.** In line no. 16, the input string is printed. In line no. 9 of the)

[//]: # (program above, **len&#40;s&#41; returns the number of bytes in the string** and we use)

[//]: # (a [for loop]&#40;https://golangbot.com/loops/&#41; to print those bytes in hexadecimal notation. **%x is the format specifier)

[//]: # (for hexadecimal.** The above program outputs)

**%s 是一个字符串的格式化标识符。**第16行代码，输入的字符串被打印出来。在上面代码的第9行，**len(s) 返回字符串中字节的数量**
我们使用一个for循环以十六进制打印这些字节。**%x 是十六进制的格式化标识符**。上面程序结果如下：

```
String: Hello World  
Bytes: 48 65 6c 6c 6f 20 57 6f 72 6c 64  
```

[//]: # (These are the [Unicode UT8-encoded]&#40;https://mothereff.in/utf-8#Hello%20World&#41; values of `Hello World`. A basic)

[//]: # (understanding of Unicode and UTF-8 is needed to understand strings better. I recommend)

[//]: # (reading [https://naveenr.net/unicode-character-set-and-utf-8-utf-16-utf-32-encoding/]&#40;https://naveenr.net/unicode-character-set-and-utf-8-utf-16-utf-32-encoding/&#41;)

[//]: # (to know more about Unicode and UTF-8.)

这些是[统一码UT8编码](https://mothereff.in/utf-8#Hello%20World)
“HelloWorld”的值。为了更好地理解字符串，需要对Unicode和UTF-8有一个基本的理解。
我推荐阅读读数[https://naveenr.net/unicode-character-set-and-utf-8-utf-16-utf-32-encoding/](https://naveenr.net/unicode-character-set-and-utf-8-utf-16-utf-32-encoding/)
了解有关Unicode和UTF-8的更多信息。

[//]: # (### Accessing individual characters of a string)

### 访问一个字符串中的单个字符

[//]: # (Let's modify the above program a little bit to print the characters of the string.)
让我们来修改上面程序的一些字节，以打印字符串中的字符

```
package main

import (  
    "fmt"
)

func printBytes(s string) {  
    fmt.Printf("Bytes: ")
    for i := 0; i < len(s); i++ {
        fmt.Printf("%x ", s[i])
    }
}

func printChars(s string) {  
    fmt.Printf("Characters: ")
    for i := 0; i < len(s); i++ {
        fmt.Printf("%c ", s[i])
    }
}

func main() {  
    name := "Hello World"
    fmt.Printf("String: %s\n", name)
    printChars(name)
    fmt.Printf("\n")
    printBytes(name)
}
```

[//]: # ([Run in playground]&#40;https://play.golang.org/p/ZkXmyVNsqv7&#41;)

[//]: # (In line no.17 of the program above, **%c format specifier is used to print the characters of the string** in)

[//]: # (the `printChars` method. The program prints)

在上面程序第17行，**在`printChars`方法中，%c 格式化标识符被用来打印字符串的字符。程序打印如下：

```
String: Hello World  
Characters: H e l l o   W o r l d  
Bytes: 48 65 6c 6c 6f 20 57 6f 72 6c 64  
```

[//]: # (Although the above program looks like a legitimate way to access the individual characters of a string, this has a)

[//]: # (serious bug. Let's find out what that bug is.)
尽管上面的程序看上去像是访问字符串单个字符的合法方式，但它有一个严重的错误。让我们找出那个bug是什么。

```
package main

import (  
    "fmt"
)

func printBytes(s string) {  
    fmt.Printf("Bytes: ")
    for i := 0; i < len(s); i++ {
        fmt.Printf("%x ", s[i])
    }
}

func printChars(s string) {  
    fmt.Printf("Characters: ")
    for i := 0; i < len(s); i++ {
        fmt.Printf("%c ", s[i])
    }
}

func main() {  
    name := "Hello World"
    fmt.Printf("String: %s\n", name)
    printChars(name)
    fmt.Printf("\n")
    printBytes(name)
    fmt.Printf("\n\n")
    name = "Señor"
    fmt.Printf("String: %s\n", name)
    printChars(name)
    fmt.Printf("\n")
    printBytes(name)
}
```

[//]: # ([Run in playground]&#40;https://play.golang.org/p/2hyVf8l9fiO&#41;)

[//]: # (The output of the above program is)
上述程序输出如下：

```
String: Hello World  
Characters: H e l l o   W o r l d  
Bytes: 48 65 6c 6c 6f 20 57 6f 72 6c 64 

String: Señor  
Characters: S e Ã ± o r  
Bytes: 53 65 c3 b1 6f 72  
```

[//]: # (In line no. 30 of the program above, we are trying to print the characters of **Señor** and it outputs **S e Ã ± o r**)

[//]: # (which is wrong. Why does this program break for `Señor` when it works perfectly fine for `Hello World`. The reason is)

[//]: # (that the Unicode code point of `ñ` is `U+00F1` and its [UTF-8 encoding]&#40;https://mothereff.in/utf-8#%C3%B1&#41; occupies 2)

[//]: # (bytes `c3` and `b1`. We are trying to print characters assuming that each code point will be one byte long which is)

[//]: # (wrong. **In UTF-8 encoding a code point can occupy more than 1 byte.** So how do we solve this? This is where **rune**)

[//]: # (saves us.)

在上述程序第30行，我们尝试打印 **Señor** 的字符，输出一个错误的 **S e Ã ± o r** 。为什么这个程序在“你好世界”中运行良好，却在“Señor”中中断。
原因是“ñ”的Unicode编码点是“U+00F1”及其[UTF-8编码](https://mothereff.in/utf-8#%C3%B1)占用2个字节“c3”和“b1”。我们试图打印字符，
假设每个代码点都有一个字节长，这是错误的**在UTF-8编码中，一个代码点可能占用超过1个字节。**
那么我们如何解决这个问题呢？这就是**rune**拯救我们的地方。

### Rune

[//]: # (A rune is a builtin [type]&#40;https://golangbot.com/types/&#41; in Go and it's the alias of int32. Rune represents a Unicode)

[//]: # (code point in Go. It doesn't matter how many bytes the code point occupies, it can be represented by a rune. Let's)

[//]: # (modify the above program to print characters using a rune.)

rune 是Go语言的内置[类型](https://golangbot.com/types/)
，它是int32的别名。在Go语言中Rune代表一个Unicode代码点。它不关心代码点占多少个字节，它可以用一个rune来表示。让我们修改上述程序用一个rune打印字符。

```
package main

import (  
    "fmt"
)

func printBytes(s string) {  
    fmt.Printf("Bytes: ")
    for i := 0; i < len(s); i++ {
        fmt.Printf("%x ", s[i])
    }
}

func printChars(s string) {  
    fmt.Printf("Characters: ")
    runes := []rune(s)
    for i := 0; i < len(runes); i++ {
        fmt.Printf("%c ", runes[i])
    }
}

func main() {  
    name := "Hello World"
    fmt.Printf("String: %s\n", name)
    printChars(name)
    fmt.Printf("\n")
    printBytes(name)
    fmt.Printf("\n\n")
    name = "Señor"
    fmt.Printf("String: %s\n", name)
    printChars(name)
    fmt.Printf("\n")
    printBytes(name)
}
```

[//]: # ([Run in playground]&#40;https://play.golang.org/p/n8rsfagm2SJ&#41;)

[//]: # (In line no. 16 of the program above, the string is converted to a [slice]&#40;https://golangbot.com/arrays-and-slices/&#41; of)

[//]: # (runes. We then loop over it and display the characters. This program prints,)

在上面程序的第16行，字符串被转换为一个runes的[切片](https://golangbot.com/arrays-and-slices/)。然后我们在上面循环并显示字符。这个程序打印如下：

```
String: Hello World  
Characters: H e l l o   W o r l d  
Bytes: 48 65 6c 6c 6f 20 57 6f 72 6c 64 

String: Señor  
Characters: S e ñ o r  
Bytes: 53 65 c3 b1 6f 72  
```

[//]: # (The above output is perfect. Just want we wanted 😀.)
上面输出是完美的。只是想要我们想要的 😀.

[//]: # (### Accessing individual runes using for range loop)

### 使用for range循环访问单个runes

[//]: # (The above program is a perfect way to iterate over the individual runes of a string. But Go offers us a much easier way)

[//]: # (to do this using the **for range** loop.)
上面的程序是一种完美的方法来迭代字符串的各个rune。但是Go语言给我们提供了一个更简单的方法，使用 **for range** 循环来实现这一点。

```
package main

import (  
    "fmt"
)

func charsAndBytePosition(s string) {  
    for index, rune := range s {
        fmt.Printf("%c starts at byte %d\n", rune, index)
    }
}

func main() {  
    name := "Señor"
    charsAndBytePosition(name)
}
```

[//]: # ([Run in playground]&#40;https://play.golang.org/p/0ldNBeffjYI&#41;)

[//]: # (In line no.8 of the program above, the string is iterated using `for range` loop. The loop returns the position of the)

[//]: # (byte where the rune starts along with the rune. This program outputs)
在上面程序的第8行中，使用 `for range` 循环来迭代字符串。循环返回rune与rune一起开始的字节。该程序输出

```
S starts at byte 0  
e starts at byte 1  
ñ starts at byte 2
o starts at byte 4  
r starts at byte 5  
```

[//]: # (From the above output, it's clear that `ñ` occupies 2 bytes since the next character `o` starts at byte 4 instead of)

[//]: # (byte 3 😀.)

从上面的输出中可以清楚地看出，“ñ”占据了2个字节，因为下一个字符“o”从字节4开始，而不是字节3😀.

[//]: # (### Creating a string from a slice of bytes)

### 从字节切片创建字符串

```
package main

import (  
    "fmt"
)

func main() {  
    byteSlice := []byte{0x43, 0x61, 0x66, 0xC3, 0xA9}
    str := string(byteSlice)
    fmt.Println(str)
}
```

[//]: # ([Run in playground]&#40;https://play.golang.org/p/Vr9pf8X8xO&#41;)

_byteSlice_ in line no. 8 of the program above contains the [UTF-8 Encoded](https://mothereff.in/utf-8#Caf%C3%A9) hex
bytes of the string `Café`. The program prints

```
Café  
```

What if we have the decimal equivalent of hex values. Will the above program work? Let's check it out.

```
package main

import (  
    "fmt"
)

func main() {  
    byteSlice := []byte{67, 97, 102, 195, 169}//decimal equivalent of {'\x43', '\x61', '\x66', '\xC3', '\xA9'}
    str := string(byteSlice)
    fmt.Println(str)
}
```

[Run in playground](https://play.golang.org/p/jgsRowW6XN)

Decimal values also work and the above program will also print `Café`.

### Creating a string from a slice of runes

```
package main

import (  
    "fmt"
)

func main() {  
    runeSlice := []rune{0x0053, 0x0065, 0x00f1, 0x006f, 0x0072}
    str := string(runeSlice)
    fmt.Println(str)
}
```

[Run in playground](https://play.golang.org/p/m8wTMOpYJP)

In the above program `runeSlice` contains the Unicode code points of the string `Señor` in hexadecimal. The program
outputs

```
Señor  
```

### String length

The `RuneCountInString(s string) (n int)` function of
the [utf8 package](https://golang.org/pkg/unicode/utf8/#RuneCountInString) can be used to find the length of the string.
This method takes a string as an argument and returns the number of runes in it.

As we discussed earlier, `len(s)` is used to find the number of bytes in the string and it doesn't return the string
length. As we already discussed, some Unicode characters have code points that occupy more than 1 byte. Using `len` to
find out the length of those strings will return the incorrect string length.

```
package main

import (  
    "fmt"
    "unicode/utf8"
)

func main() {  
    word1 := "Señor"
    fmt.Printf("String: %s\n", word1)
    fmt.Printf("Length: %d\n", utf8.RuneCountInString(word1))
    fmt.Printf("Number of bytes: %d\n", len(word1))

    fmt.Printf("\n")
    word2 := "Pets"
    fmt.Printf("String: %s\n", word2)
    fmt.Printf("Length: %d\n", utf8.RuneCountInString(word2))
    fmt.Printf("Number of bytes: %d\n", len(word2))
}
```

[Run in playground](https://play.golang.org/p/KBQg1qagnfC)

The output of the above program is

```
String: Señor  
Length: 5  
Number of bytes: 6

String: Pets  
Length: 4  
Number of bytes: 4  
```

The above output confirms that `len(s)` and `RuneCountInString(s)` return different values 😀.

### String comparison

The `==` operator is used to compare two strings for equality. If both the strings are equal, then the result is `true`
else it's `false`.

```
package main

import (  
    "fmt"
)

func compareStrings(str1 string, str2 string) {  
    if str1 == str2 {
        fmt.Printf("%s and %s are equal\n", str1, str2)
        return
    }
    fmt.Printf("%s and %s are not equal\n", str1, str2)
}

func main() {  
    string1 := "Go"
    string2 := "Go"
    compareStrings(string1, string2)

    string3 := "hello"
    string4 := "world"
    compareStrings(string3, string4)

}
```

[Run in playground](https://play.golang.org/p/JEAMexbvJ1s)

In the `compareStrings` function above, line no. 8 compares whether the two strings `str1` and `str2` are equal using
the `==` operator. If they are equal, it prints a corresponding message and
the [function](https://golangbot.com/functions/) returns.

The above program prints,

```
Go and Go are equal  
hello and world are not equal  
```

### String concatenation

There are multiple ways to perform string concatenation in Go. Let's look at a couple of them.

The most simple way to perform string concatenation is using the `+` operator.

```
package main

import (  
    "fmt"
)

func main() {  
    string1 := "Go"
    string2 := "is awesome"
    result := string1 + " " + string2
    fmt.Println(result)
}
```

[Run in playground](https://play.golang.org/p/RCL8SGkrBe9)

In the program above, in line no. 10, `string1` is concatenated to `string2` with a space in the middle. This program
prints,

```
Go is awesome  
```

The second way to concatenate strings is using the [Sprintf](https://golang.org/pkg/fmt/#Sprintf) function of the fmt
package.

The `Sprintf` function formats a string according to the input format specifier and returns the resulting string. Let's
rewrite the above program using `Sprintf` function.

```
package main

import (  
    "fmt"
)

func main() {  
    string1 := "Go"
    string2 := "is awesome"
    result := fmt.Sprintf("%s %s", string1, string2)
    fmt.Println(result)
}
```

[Run in playground](https://play.golang.org/p/AgqI29aQQDu)

In line no. 10 of the program above, `%s %s` is the format specifier input for `Sprintf`. This format specifier takes
two strings as input and has a space in between. This will concatenate the two strings with a space in the middle. The
resulting string is stored in `result`. This program also prints,

```
Go is awesome  
```

### Strings are immutable

Strings are immutable in Go. Once a string is created it's not possible to change it.

```
package main

import (  
    "fmt"
)

func mutate(s string)string {  
    s[0] = 'a'//any valid unicode character within single quote is a rune 
    return s
}
func main() {  
    h := "hello"
    fmt.Println(mutate(h))
}
```

[Run in playground](https://play.golang.org/p/bv4SlSd_hp)

In line no. 8 of the above program, we try to change the first character of the string to `'a'`. Any valid Unicode
character within a single quote is a rune. We try to assign the rune `a` to the zeroth position of the slice. This is
not allowed since the string is immutable and hence the program fails to compile with error **./prog.go:8:7: cannot
assign to s\[0\]**

To workaround this string immutability, strings are converted to a [slice](https://golangbot.com/arrays-and-slices/) of
runes. Then that slice is mutated with whatever changes are needed and converted back to a new string.

```
package main

import (  
    "fmt"
)

func mutate(s []rune) string {  
    s[0] = 'a' 
    return string(s)
}
func main() {  
    h := "hello"
    fmt.Println(mutate([]rune(h)))
}
```

[Run in playground](https://play.golang.org/p/GL1cm17IP1)

In line no.7 of the above program, the `mutate` function accepts a rune slice as an argument. It then changes the first
element of the slice to `'a'`, converts the rune back to string and returns it. This method is called from line no. 13
of the program. `h` is converted to a slice of runes and passed to `mutate` in line no. 13. This program outputs `aello`

I have created a single program in GitHub which includes everything we discussed. You can download
it [here](https://github.com/golangbot/stringsexplained).

That's it for strings. Have a great day.

Please share your valuable comments and feedback.

Like my tutorials? Please [support the content](https://golangbot.com/support-the-content/).

**Next tutorial - [Pointers](https://golangbot.com/pointers/)**