---
title: Golang-语法-1
date: 2020-01-12 17:58:22
tags:	#标签
	- Golang
	- 语言学习
categories: 	#分类
	- Golang

---

## `Golang` 语法1

### 数据类型

### 变量声明

- 变量声明一般使用 `var`关键字： `var indetifier1,indetifier2 type` ，可以一次声明多个变量。

- 指定变量类型，如果没有初始化，则默认变量为零值

  ```go
  var v_name v_type //没有进行初始化，所以此时v_name的值为0
  v_name = value
  
  ```

  - 数值类型为0。

  - 布尔类型为`false`

  - 字符串为 “ ” （空字符串）

  - 一下几种类型为`nil`

    ```go
    var a *int
    var a []int
    var a map[string] int
    var a func(string) int
    var a error //error 是接口
    ```

- 省略`var` ，如果 `:=`左侧如果没有出现新的变量，就会产生编译错误。

  <u>`v_name := value`</u>

- 一个特殊的变量名`_` ,任何赋给他的值都被丢弃。

- 因使分解关键字写法，一般用于声明全局变量。

  ```go
  var (
      a int
      b bool
  )
  
  //g,h := 123, "hello" 不带声明的格式只能在函数体中出现
  ```





### 常量

- 常量是一个简单的标识符，在程序运行时，不会被修改的量。

- 常量中的数据类型只可能是布尔型、数字型和字符串型。

- 定义格式 `const identifier [type] = value` 

  - 可以省略类型说明符。

    显式类型转换 ：`const b string = "abc"` 

    隐式类型转换：`const b = "abc"` 

- `iota`

### 控制结构

#### `if-else`

-  `Go`中`if-else`条件判断的格式如下：

  ```go
  
  func Ifdemo1()  {
  	score := 65
  	if score >= 90 {
  		fmt.Println("A")
  	}else if score >75{
  		fmt.Println("B")
  	}else {
  		fmt.Println("C")
  	}
  }
  ```

- `if`的特殊写法：

  ```go
  func Ifdemo2()  {
  
  	if score := 80;score >= 90 {
  		fmt.Println("A")
  	}else if score >75{
  		fmt.Println("B")
  	}else {
  		fmt.Println("C")
  	}
  }
  ```

  可以在`if`表达式之前添加一个执行语句，再根据变量值进行判断。

#### `goto` 

- `goto`语句通过标签进行代码间的无条件跳转。`goto`语句可以在快速跳出循环、避免重复退出上有一定帮助。`Go`语言中使用`goto`语句能简化一些代码的实现过程。

- 双层嵌套循环退出：

  ```go
  func Gotodemo1()  {
  	var breakFlag bool
  	for i := 0; i < 10; i++ {
  		for j := 0; j < 10; j++ {
  			if j == 2 {
  				//内层终止条件
  				breakFlag = true
  				break
  			}
  		}
  		//外层for循环
  		if breakFlag {
  			break
  		}
  	}
  }
  ```

- 使用`goto`替换：

  ```go
  func Gotodemo2()  {
  	for i := 0; i < 10; i++{
  			for j := 0; j < 10; j++{
  				if j == 2 {
  					goto breakTag
  				}
  				fmt.Printf("%v-%v\n",i,j);
  			}
  	}
  	return
  	//goto 标签
  	breakTag:
  		fmt.Println("循环结束！")
  }
  
  ```

  

#### `for` 

- `for`循环的初始语句可以被忽略，但是初始语句后的分号必须要写。

  ```go
  func ForDemo1() {
      i := 0
      for ; i < 10; i++ {
          fmt.Println(i)
      }
  }
  ```

- `for`循环的初始语句和结束语句都可以省略。

  ```go
  func ForDemo2() {
      i := 0
      for i < 10 {
          fmt.Println(i)
      }
  }
  ```

  这种写法类似`while`，在`Golang`中没有`while`。

#### `range` 

- `Go`语言中可以使用`for range`遍历数组、切片、字符串、`map`及通道。通过`for range`遍历的返回值有以下规律：
  - 数组、切片、字符串返回索引和值。
  - `map`返回键和值。
  - 通道只返回通道内的值。

#### `break`

- 在`Go`中break的用法更加灵活，break语句可以在语句后面添加标签，表示退出某个标签对应的代码块，标签要求必须定义在对应的`for`、`switch`和`select`的代码块上。

  ```go
  //break新用法
  func Breakdemo()  {
  	BREAKDEMO:
  		for i := 0; i < 10; i++ {
  			for j:= 0; j < 10; j++ {
  				if j == 2 {
  					//帮助break跳出多层循环
  					break BREAKDEMO
  				}
  				fmt.Printf("%v-%v\n",i,j)
  			}
  		}
  		fmt.Println("...")
  }
  ```

#### `continue`

- `continue`语句可以结束当前循环，开始下一次的循环迭代过程，仅限在`for`循环内使用。

- 在`continue`语句后面添加标签时，表示开始标签对应的循环。

  ```go
  //continue
  func Continuedemo()  {
  	forloop1:
  		for i := 0; i < 10; i++ {
  			for j := 0; j < 10; j++ {
  				if j == 2 && i == 2 {
  					continue forloop1
  				}
  				//打印不会出现 2-2
  				fmt.Printf("%v-%v\n",i,j)
  			}
  		}
  }
  ```

  

#### `switch`

- 多个`case`值时，中间用`,`隔开。

  ```go
  func TestSwicth(){
      switch n := 7; n {
          case 1,3,5,7,9 :
          	fmt.Println("奇数")
      	case 2,4,6,8,10 :
          	fmt.Println("偶数")
      	default:
          	fmt.Println(n)
      }
  }
  ```

  

- `fallthrough` 

  `fallthrough` 语法可以执行满足条件的`case`的下一个`case`，是为了兼容`C`语言中的`case`涉及的。

  ```go
  func SwitchDemo() {
      s := "a"
      switch {
      	case s == "a" :
          	fmt.Println("a")
          	fallthrough
          case s == "b":
          	fmt.Println("b")
          case s == "c":
          	fmt.Println("c")
          	default:
          fmt.Println("...")
      }
  }
  ```

  输出： ` a b`

### 内建函数

- `close` 用于 `channel` 通讯，使用它来关闭`channel` 。
- `delete` 用于在`map` 中删除实例。
- `len`和`cap`可用于不同的类型，`len`用于返回字符串、`slice`和数组的长度。
- `new`用于各种类型的内存分配。
- `make`用于内建类型的内存分配。
- `copy`用于复制`slice`。
- `append`用于追加`slice`。
- `panic`和`recover`用于异常处理机制。
- `print`和`println`是底层打印函数，可以在不引入`fmt`包的情况下使用。
- `complex`、`real`和`imag`全部用于处理复数。





### `select`语句

- `select` 是 `Go`中的一个控制结构，类似于通信的`Switch`语句。每一个`case`必须是一个通信操作，要么是发送，要么是接收。
- `select`随机执行一个可运行的`case`。如果没有`case`可运行，他将阻塞，直到有`case`可运行。一个默认的子句应该总是可运行的。

```go
select {
    case communication clause :
    	statement(s);
    case communication clause :
    	statement(s);
    ....
    default:
    	statement(s);
}
```

- ​	`select`语法：

  - 每个`case`都必须是一个通信。

  - 所有的`channel`表达式都会被求值。

  - 所有被发送的表达式都会被求值。

  - 如果任意某个通信可以进行，他就进行，其他被忽略。

  - 如果有多个`case`都可以运行，`select`会随机公平的选出一个执行。其他不会执行。

    否则：

    ​	1.如果有 `default`子句，则执行该子句。

    ​    2.如果没有`default`子句，`select`将阻塞，指导某个通信可以运行;

    ​	`go`不会重新对`channel`或值进行求值。

