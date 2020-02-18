---
title: Golang-语法-2
date: 2020-01-13 15:46:43
tags:	
	- Golang
	- 语言学习

---

## `Golang`语法2

### `Array`(数组)

- 数组的定义：` var 数组变量名 [元素数量] T`

  ```go
  var a [5] int
  var b [3] int 
  // a 和 b 是不同类型，因为它们的数组长度不同。
  ```

  - 数组的长度必须是常量，并且长度是数组类型的一部分，一旦定义，长度不能改变。

- 数组初始化

  - 方法一：初始化数组时可以用初始化列表来设置数组元素的值。

    ```go
    func ArrayDemo1()  {
    	var testArray [3] int //数组初始化为int类型的零值
    	var numArray = [3] int {1,2} //使用指定的初始值完成初始化
    	var cityArray = [3] string {"北京","上海","深圳"} //使用指定的初始值完成初始化
    	fmt.Println(testArray)
    	fmt.Println(numArray)
    	fmt.Println(cityArray)
    }
    ```

  - 方法二：让编译器根据数组初始值的个数自主推导数组的长度。

    ```go
    func ArrayDemo2()  {
    	var testArray [3] int
    	var numArray = [...] int {1,2,3}
    	var cityArray = [...] string {"西安","上海","杭州"}
    	fmt.Println(testArray)
    	fmt.Println(numArray)
    	fmt.Printf("numArray 类型: %T\n",numArray)
    	fmt.Println(cityArray)
    	fmt.Printf("cityArray 类型: %T\n",cityArray)
    }
    ```

  - 方法三：使用指定索引值的方式来初始化数组。

    ```go
    func ArrayDemo3()  {
    	a := [...] int {1 : 1,3 : 5}
    	fmt.Println(a)
    	fmt.Printf("a 类型: %T\n",a)
    }
    ```

- 数组的遍历

  - `for`循环遍历

    ```go
    func DisplayDemo1()  {
    	cityArray := [...] string {"北京","上海","杭州"}
    
    	for i := 0; i < len(cityArray); i++ {
    		fmt.Println(cityArray[i])
    	}
    }
    ```

    

  - `for range`遍历

    ```go
    func DisplayDemo2()  {
    	cityArray := [...] string {"北京","上海","杭州"}
    
    	for index, value := range cityArray {
    		fmt.Println(index,value)
    	}
    }
    ```

- 多维数组

  - 二维数组的定义

    ```go
    func ArrayDemo4() {
        cityArray := [3][2] string {
            {"陕西","西安"},
            {"广州","广东"},
            {"广西","桂林"},
        }
        
        fmt.Println(cityArray[0][1])
    }
    ```

    

  - 二维数组的遍历

    ```go
    func DisplayDemo3() {
         cityArray := [3][2] string {
            {"陕西","西安"},
            {"广州","广东"},
            {"广西","桂林"},
        }
        
        for _, v1 := range cityArray {
            for _, v2 := range v1 {
                fmt.Printf("%s\t",v2)
            }
            fmt.Println()
        }
    }
    ```

  - 注意：多维数组只有第一层可以使用 `...`来让编译器推导数组长度。

    ``` go
    //支持写法
    a := [...][2] string {
            {"陕西","西安"},
            {"广州","广东"},
            {"广西","桂林"},
    }
    
    //不支持写法
    a := [3][...] string {
            {"陕西","西安"},
            {"广州","广东"},
            {"广西","桂林"},
    }
    ```

- 数组是值类型

  - 数组是值类型，赋值和传参会复制整个数组。因此改变副本的值，不会改变本身的值。

    ```go
    func ModifyArray1(x [3]int){
    	x[0] = 100
    }
    
    func ModifyArray2(x [3][2]int)  {
    	x[2][0] = 100
    }
    func main()  {
    	a := [3]int {10,20,30}
    	arrayDemo.ModifyArray1(a)
        fmt.Println(a) //输出 [10,20,30]
    
    	b := [3][2]int {
    		{1,1},
    		{1,1},
    		{1,1},
    	}
    
    	arrayDemo.ModifyArray2(b)
    	fmt.Println(b) //输出[[1 1] [1 1] [1 1]]
    }
    ```

  - 数组支持 `“==”` 、`“!=”`操作符。

  - `[n]*T`表示指针数组 ，`*[n]T`表示数组指针

### `Slice`(切片)

- 切片的定义

  - 切片是一个拥有相同类型元素的可变长的序列。它是基于数组的一层封装。它非常灵活，支持自动扩容。

  - 切片是一个引用类型，它的内部包含地址、长度和容量。切片一般用于快速地操作一块数据集合。

  - 声明切片类型的基本语法：` var name []T` ，`name`表示变量名，`T`表示切片中的元素类型。

    ```go
    func SliceDemo1()  {
    
    	var a []string //声明一个字符串切片
    	var b = []int{}  //声明一个整形切片并初始化
    	var c = []bool{false,true} //声明一个布尔型切片并初始化
    	
    }
    ```

- 切片的长度和容量

  - 可以通过`len()`函数求切片的长度，通过`cap()`函数求切片的容量。

- 基于数组定义切片

  ```go
  func SliceDmeo2()  {
  
  	a := [5]int {1,2,3,4,5}
  	b := a[1:4]
  	fmt.Println(b)
  	fmt.Printf("type of b:%T\n",b)
  }
  ```

- 切片再切片

  - 通过切片得到切片

    ```go
    func SliceDemo3()  {
    
    	a := [...]string {"北京","上海","广东","重庆"}
    	fmt.Printf("a:%v type:%T len:%d  cap%d\n",a,a,len(a),cap(a))
    	b := a[1:3]
    	fmt.Printf("b:%v type:%T len:%d  cap:%d\n",b,b,len(b),cap(b))
    	c := b[1:2]
    	fmt.Printf("c:%v type:%T len:%d  cap:%d\n",c,c,len(c),cap(c))
    }
    
    ```

  - 对切片在切片时，索引不能超过原有数组的长度，否则会发生越界。

- 使用`make()函数`构造切片

  - 如果需要动态创建一个切片，我们就需要使用`make()函数`： `make([ ]T, size, cap)`

    - T：切片的类型
    - size:切片中元素的数量
    - cap:切片容量

    ```go
    func SliceDemo4()  {
    	a := make([]int,2,10)
    	fmt.Println(a)
    	fmt.Println(len(a))
    	fmt.Println(cap(a))
    }
    ```

- 切片的本质

  - 切片的本质就是对底层数组的封装，它包含了三个信息：底层数组的指针、切片长度和切片容量。
  - 数组指针指向切片开始的位置，长度为切片开始到切片结束的位置，容量为切片开始到数组结束的位置。

- 切片不能直接比较

  - 切片不能直接比较，不能使用`==`来判断两个切片是否含有全部相等元素。切片唯一合法的比较操作是和`nli`比较。一个`nli`值的切片并没有底层数组，一个`nli`值的切片的长度和容量都是0。但是不能说一个长度和容量都是0的切片一定是`nli`。

    ```go
    var s1 []int	//len(s1)=0;cap(s1)=0;s1==nli
    s2 := []int{}	//len(s2)=0;cap(s2)=0;s2!=nli
    s3 := make([]int,0)	//len(s3)=0;cap(s3)=0;s3!=nli
    ```
  
- 切片的赋值拷贝
  
```go
  func SliceDemo5()  {
	s1 := make([]int,3)
  	s2 := s1
	s2[0] = 100
  	fmt.Println(s1)
  	fmt.Println(s2)
  }
```
​				拷贝前后，两个变量贡献底层数组，对一个切片修改内容，会影响另一个切片的内容。

- 切片遍历

  - 切片变量的方式和数组是一致的，支持索引遍历和`for range`遍历。

    ```go
    func main(){
        s := []int {1,3,4,5,6}
        
        for i := 0; i < len(s); i++ {
            fmt.Println(i,s[i])
        }
        
        for index,value := range s {
            fmt.Println(index,value)
        }
    }
    ```

- `append()`方法为切片添加元素

  - Go语言的内建函数`append()`可以为切片动态添加元素，每个切片会指向一个底层数组，每个数组的容量够用就会添加新增元素。当底层数组不能容纳新增元素时，切片就会自动按照一定策略进行扩容，此时该切片指向底层数组就会更换。扩容操作往往发生在`append()`函数调用时。

    ```go
    func SliceDemo6() {
    	var numSlice []int
    	for i := 0; i < 10; i++ {
    		numSlice = append(numSlice, i)
    		fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n",numSlice,len(numSlice),cap(numSlice),numSlice)
    	}
    }
    ```

    ```go
    [0]  len:1  cap:1  ptr:0xc0000200c0
    [0 1]  len:2  cap:2  ptr:0xc000020100
    [0 1 2]  len:3  cap:4  ptr:0xc000016440
    [0 1 2 3]  len:4  cap:4  ptr:0xc000016440
    [0 1 2 3 4]  len:5  cap:8  ptr:0xc00001c200
    [0 1 2 3 4 5]  len:6  cap:8  ptr:0xc00001c200
    [0 1 2 3 4 5 6]  len:7  cap:8  ptr:0xc00001c200
    [0 1 2 3 4 5 6 7]  len:8  cap:8  ptr:0xc00001c200
    [0 1 2 3 4 5 6 7 8]  len:9  cap:16  ptr:0xc00008c080
    [0 1 2 3 4 5 6 7 8 9]  len:10  cap:16  ptr:0xc00008c080
    
    ```

    从上面结果可以看出：

    - `append ()`函数将元素追加到切片的最后并返回该切片。
    - 切片numSlice的容量按照1,2,4,8这样的规则进行扩容，每次扩容后都是扩容前的两倍。

  - `append()`函数还支持一次追加多个元素。

    ```go
    var citySlice []string
    citySlice = citySlice.append(citySlice,"北京","上海")
    a := []string {"成都","重庆"}
    //追加切片
    citySlice = citySlice.append(citySlice,a...)
    ```

- 切片的扩容策略

  - 通过`$GOROOT/src/runtime/slice.go`源码查看。
  - 切片扩容会根据切片中元素的类型不同而做处理。

- 使用`copy()`函数复制切片

  - `copy()`函数可以迅速将一个切片的数据复制到另一个切片空间。

    `copy(destSlice,srcSlice []T)`

    ​	1.`destSlice`: 目标切片

    ​	2.`srcSlice`：数据来源切片  

    ```go
    func main() {
        a := []int {1,2,3,4,5}
        c := make([]int,5,5)
        copy(c,a)
    }
    ```

- 从切片中删除元素

  - `Go`没有删除切片元素的方法，可以使用切片本身的特性来删除元素。

    ```go
    func main() {
        a := []int {1,2,3,4,5}
        
        //删除索引为2的元素
        a = append(a[:2],a[3:]...)
        
    }
    ```

  - 要从切片中删除索引为`index`的元素，操作方法是` a = append(a[:index],a[index+1]...)`

  

  

  

  

  




