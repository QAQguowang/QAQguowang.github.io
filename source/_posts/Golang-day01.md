---
title: Golang-day01
date: 2020-01-12 17:29:49
tags:
	- Golang
	- 语言学习
categories:
	- Golang
---

# Golang第一个程序

## `hello world`

```go
package main
import "fmt"

func main(){
    fmt.Println("hello world")
}
```

- 包声明

  `package main`

- 引入包

  `import "fmt"`

- 函数

  `func main()`

- 变量

- 语句 & 表达式

- 注释

  - `//` 单行注释
  - `/* ......*/` 多行注释

## 编译

使用`go build` 

1. 在项目目录下执行 `go build`
2. 在其他路径下执行`go build` ，需要在后面加上项目路径(从`GOPATH`路径写)。
3. `go build -o hello.exe`

使用 `go run`，以执行脚本文件的方式运行。

使用 `go install`。

## 注意

- `{` 不能单独放在一行，否则代码运行时会产生错误。

## 注释

##  `Golang` 的新类型

- 复数

- 错误

- `byte`和`rune`类型

  - `Go`语言的字符有以下两种：

    - 1.`uint8`类型，或者叫做`byte`型，代表了`ASCII`码的一个字符。
    - 2.`rune`类型，代表一个`UTF-8`字符。

    当需要处理中文、日文或者其他复合字符时，则需要用到`rune`类型。`rune`实际上是一个`int32`。

### `Golang`字符串

- 字符串常用操作

|                  方法                   | 介绍           |
| :-------------------------------------: | -------------- |
|               `len(str)`                | 求长度         |
|           `+`或`fmt.Sprintf`            | 拼接字符串     |
|             `strings.Split`             | 分割           |
|           `strings.contains`            | 判断是否包含   |
| `strings.HasPrefix`,`strings.HasSuffix` | 前缀/后缀判断  |
| `strings.Index()`,`strings.LastIndex()` | 子串出现的位置 |
|  `strings.Join(a[]strings,sep string)`  | join操作       |

- 修改字符串

  要修改字符串，需要现将其转换成`[]rune`或`[]byte`，完成后再转换为`string`，无论那种转换，都会重新分配内存，并复制字节数组。

  ```go
  func ChangeString()  {
  	s1 := "big"
  
  	//强制类型转换
  	bytesS1 := []byte(s1)
  	bytesS1[0] = 'p'
  	fmt.Println(string(bytesS1))
  
  	s2 := "猪头娃"
  
  	//强制类型转换
  	runeS2 := []rune(s2)
  	runeS2[1] = '猪'
  	fmt.Println(string(runeS2))
  }
  ```

### `Golang`类型转换

- `Go`语言中只有强制类型转换，没有隐式类型转换。该语法只能在两个类型之间支持相互转换时使用。

- 强制类型转换的语法如下：

  ​	`T(表达式)`

  - 其中 `T`表示要转换的类型。表达式包括变量、复杂算子和函数返回值等。

### `Golang`保留字

```go
/*Golang保留字*/
break default func interface select case defer go map struct chan 
else goto package switch const fallthrough if range type continue
for import return var
```



#### `Golang`语法有一些严谨，稍微有些不适应。









