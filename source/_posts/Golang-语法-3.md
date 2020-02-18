---
title: Golang-语法-3
date: 2020-01-14 14:57:06
tags:
	- Golang
	- 语言学习
---

## map

- `map`是一种无序的基于`key-value`的数据结构，Go语言中的map是引用类型，必须初始化了才能使用。

- map的定义

  ```go
  map[keyType]valueType
  ```

  - keyType ：表示键的类型
  - valueType :表示值的类型

- map类型的默认初始值为nil，需要使用`make()`函数来分配内存。

  ```go
  make(map[keyType]valueType,[cap])
  ```

  其中cap表示map的容量。

- map的基本使用

  ```go
  func main() {
      scoreMap := make(map[string]int,8)
      scoreMap["张三"] = 90
      scoreMap["李四"] = 100
      fmt.Println(scoreMap)
  }
  ```

- map也支持在声明的时候填充元素

  ```go
  func main() {
      userInfo := map[string]string {
          "username" : "张三",
          "password" : "123456",
      }
      
      fmt.Println(userInfo)
  }
  ```

- 判断某个键是否存在

  - Go语言中判断map中的键是否存在的特殊写法

    ```go
    value, ok := map[key]
    ```

  - 例如

    ```go
    func main() {
    	scoreMap := make(map[string]int,8)
        scoreMap["张三"] = 90
        scoreMap["李四"] = 100
        
        v, ok := scoreMap["张三"]
        if ok {
            fmt.Println(v)
        } else {
            fmt.Println("查无此人")
        }
    }
    ```

    

- map的遍历

  - Go语言中使用`for range`进行map的遍历。

    ```go
    func main() {
        scoreMap := make(map[string]int,8)
        scoreMap["张三"] = 90
        scoreMap["李四"] = 100
    	scoreMap["王五"] = 60
        
        for k, v := range scoreMap {
            fmt.Println(k,v)
        }
        
        //如果只想遍历key
        /*
        for k := range scoreMap {
        	fmt.Println(k)
        }
        */
    }
    ```

  - 遍历map时的元素顺序与添加键值对的顺序无关。

- 使用delete函数删除键值对

  - 使用`delete()`内建函数从map中删除一组键值对

    ```go
    delete(map,key)
    ```

    - map表示要删除键值对的map
    - key表示要删除键值对的值

    ```go
    func main() {
        scoreMap := make(map[string]int,8)
        scoreMap["张三"] = 90
        scoreMap["李四"] = 100
    	scoreMap["王五"] = 60
        delete(scoreMap,"张三")
        
        for k, v := range scoreMap {
            fmt.Println(k,v)
        }
    }
    ```

    

- 按照指定顺序遍历map

  ```go
  func main() {
      rand.Seed(time.Now().UnixNano()) //初始化随机种子
      
      var scoreMap = make(map[string]int, 200)
      
      for i := 0; i < 100; i++ {
          key := fmt.Sprintf("stu%02d",i)
          value := rand.Intn(100)
          scoreMap[key] = value
      }
      
      //取出map中的所有ley存入切片
      keys := make([]string,0,200)
      for key := range scoreMap {
          keys = append(keys,key)
      }
      
      //对切片进行排序
      sort.Strings(keys)
      //按照排序后的key进行遍历
      for _, key := range keys {
          fmt.Println(key,scoreMap[key])
      }
  }
  ```

- 元素为map类型切片

  ```go
  func main() {
      //切片中的元素为map类型
      var mapSlice = make([]map[string]string,3)
      for index, value := range mapSlice {
          fmt.Printf("index:%d value:%v\n",index,value)
      }
      
      fmt.Println("after init")
      //对切片元素进行初始化
      //初始化了切片的第一个元素，第一个元素是map类型，并被初始化为10个[string]string 类型的键值对
      mapSlice[0] = make(map[string]string,10)
      mapSlice[0]["name"] = "张三"
      mapSlice[0]["password"] = "123456"
      mapSlice[0]["address"] = "沙河"
      
      for index, value := range mapSlice {
          fmt.Printf("index:%d value:%v\n",index,value)
      }
  }
  ```

  

- 值为切片类型的map

  ```go
  func main() {
      //map中值为切片类型
      var sliceMap = make(map[string][]string,3)
      fmt.Println(sliceMap)
      fmt.Println("after init")
      key := "中国"
      value, ok := sliceMap[key]
      if !ok {
          value = make([]string,0,2)
      }
      
      value = append(value,"北京","上海")
      sliceMap[key] = value
      fmt.Println(sliceMap)
  }
  ```

## 函数

- 函数定义

- 参数

  - 类型简写

  - 可变参数

    - 可变参数是指函数的参数数量不固定。Go语言中的可变参数通过在参数名后加`...`来标识。

    - 可变参数通常要做函数的最后一个参数。

      ```go
      func intSum(x ...int) int {
          fmt.Println(x)
          sum := 0
          for _, v := range x {
              sum += v
          }
          
          return sum
      }
      
      //调用
      ret1 := intSum()
      ret2 := intSum(10)
      ret3 := intSum(10,20)
      ```

    - 固定参数搭配可变参数使用

- 返回值

  - 多返回值

    - Go语言中函数支持多返回值，函数如果有多个返回值时在函数定义时必须使用`()`将所有的返回值包裹起来。

      ```go
      func calc(x,y int) (int,int) {
          sum := x + y
          sub := x - y
          
          return sum, sub
      }
      ```

  - 返回值命名

    - 函数定义时可以给返回值命名，并在函数体中直接使用这些变量，最后通过`return`关键字返回。

      ```go
      func calc(x,y int) (sum, sub int) {
          sum := x + y
          sub := x - y
          
          return 
      }
      ```

- 变量作用域

  - 全局变量
  - 局部变量

- 函数类型与变量

  - 定义函数类型

    - 可以使用`type`类型来定义一个函数类型：`type calculation func(int,int)int`，定义了一个`calculation`类型，它是一种函数类型，这种函数接收两个`int`类型的参数并且返回一个`int`类型的返回值。

      ```go
      func add(x, y int) int {
          return x + y
      }
      
      var c calculation
      c = add 
      ```

      

  - 函数类型变量

- 函数作为参数

  - 函数可以作为参数

    ```go
    func add(x, y int) int {
        return x + y
    }
    
    func calc(x, y int, op func(int, int) int)
    {
        return op(x,y)
    }
    
    func main() {
        ret2 := calc(10,20,add)
        fmt.Println(ret2)
    }
    ```

    

- 函数作为返回值

  - 函数也可以作为返回值

    ```go
    func do(s string) (func(int,int) int, error) {
        switch s {
            case "+":
            	return add,nil
            case "-":
            	return sub,nil
            default:
            	err := erros.New("无法识别的操作符")
            	return nil,err
        }
    }
    ```

    

- 匿名函数和闭包

  - 匿名函数

    - 函数还可以作为返回值，但是在Go语言中函数内部不能再像之前那样定义函数，只能定义匿名函数。匿名函数就是没有函数名的函数，匿名函数的定义如下：

      ```go
      func(参数)(返回值) {
          函数体
      }
      ```

    - 匿名函数因为没有函数名，所以没办法像普通函数那样调用，所以匿名函数需要保存到某个变量或者作为立即将执行函数：

      ```go
      func main() {
          //将匿名函数保存到变量
          add := func(x, y int) {
              fmt.Println(x + y)
          }
          
          add(10, 20) //通过变量调用匿名函数
          //自执行函数:匿名函数定义完加()直接执行
          func(x, y int) {
              fmt.Println(x + y)
          }(10,20)
      }
      ```

    - 匿名函数多用于实现回调函数和闭包。

  - 闭包

    - 闭包指的是一个函数和与其相关的引用环境组合而成的实体。简单来说，`闭包 = 函数 + 引用环境`。
  
    ```go
      package main
    
      import "fmt"
    
      //闭包
      func adder() func(int) int  {
      	var x int
      	return func(y int) int {
      		x += y
      		return x
      	}
      }
      func main()  {
      	//将adder()函数的返回值赋给 f
      	var f = adder()
      	fmt.Println(f(10)) //相当于调用了adder()函数内的匿名函数 10
      	fmt.Println(f(20)) // 30
      	fmt.Println(f(30)) // 60
      
      	f1 := adder()
      	fmt.Println(f1(40)) // 40
      	fmt.Println(f1(50))	// 90
    
      }
      ```
  
      变量`f`是一个函数并且引用了其外部作用域中的`x`变量，此时`f`就是一个闭包。
  
      ```go
      func makeSuffixFunc(suffix string) func(string) string  {
      	return func(name string) string {
      		if !strings.HasSuffix(name,suffix) {
      			return name + suffix
      		}
      		return name
      	}
      	
      }
      func main()  {
      	jpgFunc := makeSuffixFunc(".jpg")
      	txtFunc := makeSuffixFunc(".txt")
      	fmt.Println(jpgFunc("test"))
      	fmt.Println(txtFunc("test"))
      
      }
      
      ```
  
      
  
  - defer语句
  
    - Go语言中的`defer`语句会将其后面跟随的语句进行延迟处理。在`defer`归属的函数即将返回时，将延迟处理的语句按`defer`定义的逆序进行执行，也就是说，先被`defer`的语句最后被执行，最后被`defer`的语句先执行。
  
    - 首先得理解`return`的实现机制。在Go语言中，return分为给返回值赋值和RET指令两步。而`defer`语句执行的时机就在返回值赋值操作后。
    
      ```go
      func main() {
          fmt.Println("start")
          defer fmt.Println(1)
          defer fmt.Println(2)
          defer fmt.Println(3)
          fmt.Println("end")
      }
      
      //输出结果为 
      /*
      start
      3
      2
      1
      end
    */
      ```
    
      - 由于`defer`语句延迟调用的特性，所以`defer`语句能 非常方便的处理资源释放问题。
      
    - 经典案例
    
      ```go
      /*
      f1()函数使用了匿名返回值，匿名返回值时，return语句有一个赋值过程
      将x赋值给返回值，例如  retValue = x
      然后检查是否有defer，若有则执行
      返回刚才创建的返回值
      */
      //defer操作是对x执行的，而不是retValue，所以f1()返回值是 5
      func f1() int {
      	x := 5
      	defer func() {
      		x++
      	}()
      	return x
      }
      /*
      使用了命名返回值，返回值在方法定义时已经被定义，所以没有创建retValue的过程，
      x就是retValue,则defer是对x执行的。
      */
      //返回值是6
      func f2() (x int) {
      	defer func() {
      		x++
      	}()
      	return 5
      }
      //返回值是 5
      func f3() (y int) {
      	x := 5
      	defer func() {
      		x++
      	}()
      	return x
      }
      //返回值是 5
      func f4() (x int) {
      	defer func(x int) {
      		x++
      	}(x)
      	return 5
      }
      func main() {
      	fmt.Println(f1())
      	fmt.Println(f2())
      	fmt.Println(f3())
      	fmt.Println(f4())
      }
      ```
    
  - painc/recover
  
    - Go语言中目前（Go1.12）是没有异常机制，但是使用`panic/recover`模式来处理错误。 `panic`可以在任何地方引发，但`recover`只有在`defer`调用的函数中有效。 首先来看一个例子：
  
      ```go
      func funcA() {
      	fmt.Println("func A")
      }
      
      func funcB() {
      	panic("panic in B")
      }
      
      func funcC() {
      	fmt.Println("func C")
      }
      func main() {
      	funcA()
      	funcB()
      	funcC()
      }
      ```
  
      输出:
  
      ```go
      func A
      panic: panic in B
      
      goroutine 1 [running]:
      main.funcB(...)
              .../code/func/main.go:12
      main.main()
              .../code/func/main.go:20 +0x98
      ```
  
      程序运行期间`funcB`中引发了`panic`导致程序崩溃，异常退出了。这个时候我们就可以通过`recover`将程序恢复回来，继续往后执行。
  
      ```go
      func funcA() {
      	fmt.Println("func A")
      }
      
      func funcB() {
      	defer func() {
      		err := recover()
      		//如果程序出出现了panic错误,可以通过recover恢复过来
      		if err != nil {
      			fmt.Println("recover in B")
      		}
      	}()
      	panic("panic in B")
      }
      
      func funcC() {
      	fmt.Println("func C")
      }
      func main() {
      	funcA()
      	funcB()
      	funcC()
      }
      ```
  
    - 注意：
  
      - `recover()`必须搭配`defer`使用。
      - `defer`一定要在可能引发`panic`的语句之前定义。