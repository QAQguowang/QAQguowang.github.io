---
title: Golang-语法-4
date: 2020-01-18 18:25:28
tags:
	- Golang
	- 语言学习
---

## 指针

- 区别与`C/C++`中的指针，Go语言中的指针不能进行偏移和运算，是安全指针。
- 与`C/C++`相同，`&`是取地址操作符，`*`是解引用操作符。

### new

- new是一个内置函数

  ```go
  func new(Type) *Type
  ```

  - Type表示类型，new函数只接受一个参数，这个参数是一个类型。
  - *Type表示类型指针，new函数返回一个指向该类型内存地址的指针。

- `var a *int`只是声明了一个指针型变量a但是没有初始化，指针作为引用类型需要初始化后才会拥有内存空间，才可以给他赋值。

  ```go
  func main() {
      var a *int
      a = new(int)
      *a = 10
      fmt.Println(*a)
  }
  ```

### make

- make也是用于内存分配的，区别于new，它只是用于slice、map以及chan的内存创建，而且它返回的类型就是这三个类型本身，而不是它们的指针类型，因为这三种类型就是引用类型，所以就没有必要返回它们的指针了。

  ```go
  func make(t Type,size ...IntegerType) Type
  ```

- make函数是无可替代的，我们在使用slice、map以及channel的时候，都需要使用make进行初始化，然后才可以对它们进行操作。

  ```go
  func main() {
      var b map[string]int
      b = make(map[string]int, 10)
      b["hello"] = 100
      fmt.Println(b)
  }
  ```

### new与make的区别

- 二者都使用来做内存分配的。
- make只用与slice、map以及channel的初始化，返回的还是这三个引用类型本身。
- new用于类型的内存分配，并且内存对应的值为类型零值，返回的是指向类型的指针。

## 结构体

- Go语言中没有“类”的概念，也不支持“类”的继承等面向对象的概念。Go语言中通过结构体的内嵌再配合接口比面向对象具有更高的扩展性和灵活性。

### 结构体的定义

- 使用`type`和`struct`关键字来定义结构体。

  ```go
  type 类型名 struct {
      字段名 字段类型
      字段名 字段类型
      字段名 字段类型
      .......
  }
  ```

- 与C/C++大同小异，只是格式上面发上了变化。

  ```go
  type person struct {
      name, city string
      age int8
  }
  ```

### 结构体实例化

- var 结构体实例 结构体类型
- 结构体中访问结构体字段通过 `. `。

### 匿名结构体

- 在定义一些临时数据结构等场景下还可以使用匿名结构体。

  ```go
  package main
       
  import (
      "fmt"
  )
       
  func main() {
      var user struct{Name string; Age int}
      user.Name = "小王子"
      user.Age = 18
      fmt.Printf("%#v\n", user)
  }
  ```

### 创建指针类型结构体

- new对结构体进行实例化，得到的是结构体的地址。
- `var p = new(person)`

### 取结构体的地址实例化

- 使用`&`对结构体进行取地址操作相当于对该结构体类型进行了一次`new`实例化操作。

### 使用键值对初始化

- 使用键值对对结构体初始化时，键对应结构体的字段，值对应该字段的初始值。

  ```go
  p := person{
      name : "wang",
      city : "beijing",
      age : 18,
  }
  //对结构体指针进行初始化
  p2 := &person{
      name : "li",
      city : "shanghai",
      age : 19,
  }
  ```

### 使用值的列表初始化

- 初始化结构体的时候可以简写，也就是初始化的时候不写键，直接写值。

  ```go
  p3 := &person{
      "zhao",
      "shenzhen",
      20,
  }
  ```

- 必须初始化结构体的所有字段。

- 初始值的填充顺序必须与字段在结构体中的声明顺序一致。

- 该方式不能和键值初始化方式混用。

### 结构体的内存布局

- 结构体占用一块连续的内存。

### 构造函数

- Go语言的结构体没有构造函数，我们可以自己实现。

  ```go
  func newPerson(name, city string, age int8) *person{
      return &person{
          name : name,
          city : city,
          age : age,
      }
  }
  
  func main() {
      //调用构造函数
      p4 := newPerson("zhang","beijing",20)
      fmt.Printf("%#v\n",p4)
  }
  ```

### 方法和接收者

- Go语言中的`方法（Method）`是一种作用于特定类型变量的函数。这种特定类型变量叫做`接收者（Receiver）`。接收者的概念就类似于其他语言中的`this`或者 `self`。

  ```go
  func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
      函数体
  }
  ```

  - 接收者变量：接收者中的参数变量名在命名时，官方建议使用接收者类型名的第一个小写字母，而不是`self`、`this`之类的命名。例如，`Person`类型的接收者变量应该命名为 `p`，`Connector`类型的接收者变量应该命名为`c`等。
  - 接收者类型：接收者类型和参数类似，可以是指针类型和非指针类型。
  - 方法名、参数列表、返回参数：具体格式与函数定义相同。

  ```go
  type Person struct {
      name string
      age int8
  }
  
  func newPerson(name, city string, age int8) *Person{
      return &Person{
          name : name,
          city : city,
          age : age,
      }
  }
  
  //方法
  func (p Person) Dream() {
      fmt.Printf("%s\n",p.name)
  }
  func main() {
      p := newPerson("zhang","beijing",20)
      p.Dream()
  }
  ```

- 方法与函数的区别是，函数不属于任何类型，方法属于特定类型。

### 值类型的接收者

- 当方法作用于值类型接收者时，Go语言会在代码运行时将接收者的值复制一份。在值类型接收者的方法中可以获取接收者的成员值，但修改操作只是针对副本，无法修改接收者变量本身。

### 什么时候应该使用指针类型接收者

- 需要修改接收者中的值。
- 接收者是拷贝代价比较大的大对象。
- 保证一致性，如果有某个方法使用了指针接收者，那么其他方法也应该是指针接收者。

### 任意类型添加方法

- 在Go语言中，接收者的类型可以是任何类型，不仅仅是结构体，任何类型都可以拥有方法。

  ```go
  //将int类型定义为MyInt类型
  type MyInt int 
  
  //为MyInt添加一个SayHello的方法
  func (m MyInt) SayHello() {
      fmt.Println("Hello\n")
  }
  
  func main() {
      var m1 MyInt
      m1.SayHello()
      m1 = 100
      fmt.Printf("%#v  %T\n",m1,m1)
  }
  ```

- 非本地类型不能定义方法，也就是说我们不能给别的包的类型定义方法。

### 结构体的匿名字段

- 结构体允许其成员字段在声明时没有字段名而只有类型，这种没有名字的字段就称为匿名字段。

  ```go
  type Person struct {
      string
      int
  }
  
  func main() {
      p := Person{
          "张三",
          18,
      }
      
      fmt.Println(p.string, p.int)
  }
  ```

- 匿名字段默认采用类型名作为字段名，结构体要求字段名称必须唯一，因此一个结构体中同种类型的匿名字段只能有一个。

### 嵌套结构体

### 结构体的继承

- Go语言中使用结构体也可以实现其他编程语言中面向对象的继承。

  ```go
  type Animal struct {
      name string
  }
  
  func (a *Animal) move() {
      fmt.Printf("%s move!\n",Animal.name)
  }
  
  type Dog struct {
      Feet int8
      *Animal //通过嵌套匿名结构体实现继承
  }
  
  func (d *Dog) wang() {
      fmt.Printf("%s wang\n",d.name)
  }
  
  func main() {
      d := &Dog{
          Feet : 4,
          Animal : &Animal{
              name : "xiaoli",
          }
      }
      
      d.wang()
      d.move()
  }
  ```

### 结构体字段可见性

- 结构体中字段大写开头表示可公开访问，小写表示私有。

