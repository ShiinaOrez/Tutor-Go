# 不写代码的屁股：Go语言教程

[English Version](https://github.com/ShiinaOrez/Tutor-Go/README-EN.md)

## 目录

+ 0. [什么是Go语言](#什么是go语言)
+ 1. [Go语言的安装](#go语言的安装)
+ 2. [开始我们的第一个例子](#开始我们的第一个例子)
+ 3. [变量和数据类型](#变量和数据类型)
+ 4. [基本的程序结构](#基本的程序结构)

------

### 什么是Go语言

打开Go语言的[官网](https://golang.org/)我们可以看到这样一句话：

  Go是一种开源编程语言，可以轻松构建**简单**，**可靠**，**高效**的软件。

显然这句话中有三个关键字：简单，可靠，高效。

#### 1. 简单

Go语言的哲学，是简简单单的一句话：

  **Less is more.**

**Less is more.** 这句话来源于**Rob Pike**（Go语言的创造者之一）在2012年的6月份的演讲[Less is exponentially more.](https://commandcenter.blogspot.com/2012/06/less-is-exponentially-more.html)，这个演讲直译成中文的意思是：“少是指数级的多”，意译过来则是**大道至简**。

Go语言相信越是简单的东西（语义，规则等等...）越是比复杂的东西更加强大。（事实上，开发Go语言的契机，正是Google公司请了一些人，来给员工讲解C++ 0x相比较之前的一些新的特性。Rob和Ken还有Robert也在听讲的人群当中。它们认为C++想要让自身变得更好的途径不应该是增加更多的特性，而是应该减少不必要的东西。）

Go想要这样做：使用**足够简单的方式**来解决一切问题。并且它做到了。**Go语言的语法规范和C++相比简直不值一提。**

扩展阅读：
+ [大道至简](https://commandcenter.blogspot.com/2012/06/less-is-exponentially-more.html)
+ [Go语言：攀登的十年](https://commandcenter.blogspot.com/2017/09/go-ten-years-and-climbing.html)

#### 2. 可靠

在Go语言被设计的第一天，就有这样一个想法：**以C语言作为起点**。（虽然最后没这么做）

当程序员使用Go语言进行编程时，不需要像C语言的程序员那么提心吊胆去**用心**管理内存（这么说会显得使用其他编程语言的程序员不用心，但是事实上和C语言的管理内存相比，其他的工作真的只是小儿科）。Go语言会自动帮程序员管理内存，当然，程序员也还是要去在意类似于空指针，边界条件的判断等等（**不用心管理内存并不代表不用脑子！**）。

当然这个世界上的能够自动管理内存的编程语言不止一种，但是在当今的世界上对于**内存不安全**的语言是没有活路的。

扩展阅读：
+ [简单易懂的Go语言内存管理](https://yq.aliyun.com/articles/652551)

#### 3. 高效

也许Go语言是你人生中接触的第一门编程语言（我相信在不久的将来这个会变成现实的！）但是在现在（2019年）还是不太可能的，也就是说绝大多数人是因为Go语言的**特性**，或者说不得已的需求而接触的这门编程语言。

如果你是因为被Go语言的特性所吸引，那么你一定听说过一下几个关键词：**静态强类型**，**可编译**，**高并发**。

静态语言所带来的好处是在开发时的高效：编辑器可以有足够好的代码补全功能，很好的语法和类型检查，虽然和动态弱类型语言相比较，短期的开发时间较长，但是比动态语言更适合开发大型项目，且更加利于后期的维护，也就是说从长远的角度看是效率更好的。

以前的单CPU时代，人们尚且知道划分CPU时间片来进行并发（不是真正的并行计算，而是宏观上的并发），在计算能力更加发达的今天，多计算单元、多核心、多机器已经是司空见惯的模式了，在这个时候，Go语言继承自CSP、扎根于语言深部的高并发特性使之能够被称为是“二十一世纪编程语言”。而关于并发的部分会在后面介绍。

扩展阅读：
+ [静态语言和动态语言的区别 - 掘金](https://juejin.im/entry/5c7373426fb9a049bd42eff4)

-----

### Go语言的安装

不怎么用Windows，所以暂时只有Linux/Ubuntu 18.04的安装

#### Linux/Ubuntu 18.04

① 获取最新的golang安装源，并且更新到apt中
``add-apt-repository ppa:longsleep/golang-backports``

② apt更新
``apt-get update``

③ 安装golang
``sudo apt-get install golang-go``

④ 使用``version``查看版本
``go version``

#### 关于环境变量：

**GOROOT**: Go语言编译、工具、标准库等的安装路径。
**GOPATH**：Go语言的工作目录。

#### 良好的目录结构：

```bash
go/
├── bin
├── pkg
└── src
    ├── github.com
    │   ├── Muxi-X
    │   ├── ...
    │   └── ShiinaOrez
    │       ├── goProject1 <- 你的仓库
    │       └── ...
    ├── golang.org
    └── ...
```

------

### 开始我们的第一个例子

众所周知，第一个例子永远是Hello（World之类的），我们也是一样的。

**你的代码应该位于golang的工作目录下**

创建一个文件：``hello.go``或者叫什么都无所谓，只要是``.go``文件就可以。

文件的内容如下：

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, golang!")
}
```

保存，关闭，在文件目录下运行以下命令来**直接运行**：

``go run hello.go``

或者使用**编译成可执行文件**的形式来运行：

``go build -o hello && ./hello``

然后会获得以下输出：

```bash
Hello, golang!
```

#### 稍稍分析一下：

``package``关键字用来说明本文件是属于哪个包的，Go语言默认``main``包为运行或者编译的目标包。不然的话会引起以下错误：

```bash
go run: cannot run non-main package
```

``import``关键字用于导入我们运行时需要的包，在下文中使用了``fmt.Println``方法，所以需要导入``fmt``包。

然后我们使用``func``关键字声明了一个函数``main``，这是一个没有参数也没有返回值的函数，它将是程序的入口。

在``main``函数中我们调用了``fmt.Println()``方法，输入了一个字符串为参数。

扩展阅读：
+ [深入理解fmt包](https://studygolang.com/articles/17400)

------

### 变量和数据类型

一般来说，强类型语言中的变量是**盒子**，而弱类型语言中的变量则多数是**标签**。

因为强类型，所以一个变量能够分配的内存空间相对来说是便于计算的，因此语言内部会在内存中为你的变量开辟一块内存空间，而这个内存空间的地址会被映射为变量名，而内存空间内存放的数据是什么并不被关心，就像是一个盒子，里面放了不同的数据。

#### 变量的声明：

在Go语言中，声明一个新变量的方式有两种：``var``关键字和``:=``运算符。

**1. 使用var关键字**：
```go
var hello string // 声明一个变量hello，类型为string，默认值为string的零值，也就是空字符串。
var hello string = "Hello, golang!"
var hello, world string = "Hello", "World" // 可以同时声明多个变量并且初始化

var hello = "Hello, golang!" // 根据右侧值的类型来推断变量类型
```

**2. 使用:=运算符**
``:=``运算符用于**简化新变量声明**的步骤，不希望程序员每次用到新变量都去使用``var``来声明。因此``:=``运算符只能用于**新变量的声明**。
```go
hello := "Hello, golang!" // 等价为 var hello = "Hello, golang"，也会进行类型的推断

var hello string; hello := "Hello, golang!" // 会报错，即使声明的变量未曾使用过，也不是一个新变量，因此不能使用 := 运算符

hello, err := someFunc() // 便捷地接收函数返回的结果，前提也是一定是新的变量名
```

#### Go语言的数据类型：
Go语言的类型系统非常的**简单**且**多变**，不同的组合形式可以产生成百上千上万的类型，也就是说，**通过不同方式的组合，Go语言中可以有无数个类型**。

**1. 基本的数据类型**

| 类型名称 | 标识符 | 描述 |
| ------ | ----- | ---- |
| 布尔类型 | bool | 布尔值仅有``true``和``false``两种值 |
| 字符串类型 | string | 在Go语言中的字符串是不可变的常量 |
| 字节类型 | byte | 字节类型，实质上是uint8 |
| Unicode字符类型 | rune | 实质上是int32类型 |
| 有符号整数类型 | int, int8, int16, int32, int64 | ``int``是基于具体架构的类型，后面的数字代表在内存中所占的位数 |
| 无符号整数类型 | uint, uint8, uint16, uint32, uint64 | ``uint``是基于具体架构的类型，后面的数字代表在内存中所占的位数 |
| 浮点数类型 | float32, float64, complex64, complex128 | ``float32``和``float64``都是遵循[IEEE-754标准](https://zh.wikipedia.org/wiki/IEEE_754)，数字依然表示位数，而``complex64``和``complex128``分别表示32位和64位的实数和虚数 |

**2. 派生类型**

**2.1 指针类型（pointer）**

Go语言是内存安全的，但是开放了灵活的指针给程序员使用，又不涉及复杂的指针运算。

在Go语言中的类型都有对应的指针类型，以``*``为标识符，例如：

```go
var stringPointer *string
var intPointer *int
```

指针类型的变量存储着内存地址。可以使用``&``取地址符来获取一个变量的地址。

Go语言中的空指针为``nil``。

**2.2 数组类型（array）**

Go语言中的任意类型都可以有相应**确定长度**的数组类型：

```go
var fourStringArray [4]string // 一个长度为4的字符串数组
var int2dArray [4][5]int // 一个长度为4的 长度为5的int数组 的数组，也就是一个二维数组
```

值得注意的是：这里的**数组名不代表数组的开始地址**！，需要使用``&``来取得首地址，也不建议对它进行指针运算。

定长的原因很简单：可以很好的**确定**内存空间的大小。

**2.3 结构化类型（struct）**

结构体，没有人会陌生，如果这不是你接触的第一门语言的话。

结构化的自定义类型被广泛应用于组织和传递数据，以及Go语言的面向对象也是以结构体作为支撑的。
例子：

```go
var myName struct {
    FirstName  string // 字段名在前，类型名称在后
    LastName   string
} // 声明了一个结构体变量

// 定义一个新的结构体类型

type Name struct {
    FirstName  string
    LastName   string
}

// 如何初始化一个结构体变量
myName := Name {FirstName: "Shiina", LastName: "Mashiro"} // 最后不需要添加逗号

myName := Name {
    FirstName: "Shiina",
    LastName:  "Mashiro, // 需要添加逗号
}

// 使用某个字段
lastName := myName.LastName
```

也可以用组合的方式定义新的结构体：

```
type Info struct {
    Tel     string
    Address string
}

type Detail struct {
    Name
    Info
}
```

Go语言中没有类 (class)，但是会给相应的类型绑定方法，具体的形式会在后面面向对象的部分进行讲解。

**2.4 通道类型（channel）**

如果你学习过[CSP](https://zh.wikipedia.org/wiki/%E4%BA%A4%E8%AB%87%E5%BE%AA%E5%BA%8F%E7%A8%8B%E5%BC%8F)，或者你是仰慕Go语言的并发语义和并发模式而来，那么你一定知道什么是通道（channel）：**通道是一个可以带有一定缓冲的队列**，用于goroutine之间的通信。

通过一个例子来看一下如何使用（channel可是Go语言的特色之一啊，睁大眼睛）：

```go
func tryChannel() {
    ch := make(chan int, 10)       // 使用make函数构造一个缓冲容量为10的int通道
    for i:=0; i<10; i++ { ch<- i } // 循环的向通道中塞十个数据
    for value := range ch {        // 遍历通道（循环的取出数据）
        fmt.Println(value) 
        if value == 9 { break }    // 这里如果不结束循环的话，会引发DeakLock，具体的原因会在后面讲解，现在只是为了介绍类型
    }
    close(ch)                      // 手动关闭通道
}
```

如果在``调用make()``方法的时候，没有传入通道的缓冲容量，则默认为0，也就是说同时传入和传出，否则的话只会阻塞。如果程序中的所有goroutine都阻塞了，则会触发DeadLock，这也解释了例子中为什么要中断循环。

channel是具有队列的性质的，也就是[FIFO, First in First out](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics))，因此把channel当做队列数据结构来用的情况也是很多的（但是我更建议去手动实现和管理）。

**2.5 函数类型（function）**

在越来越多的支持[面向对象](https://zh.wikipedia.org/zh/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)编程语言中，函数被当做[一等公民](https://habens.github.io/blog/first-class-citizens/)看待。因此也就有专门的函数对象，变量，类型。

函数变量的类型由几个因素组成：**函数名**、**接收者**、**参数**、**返回值**

例子：

```go
type compareFunction func(int, int) int
var min compareFunction = func(x, y int) int {
    if x < y {
        return x
    }
    return y
}

fmt.Println(compareFunction(10, 20))
```

同时函数当然也可以作为参数传入函数。

```go
func less(x, y int, comp func(int, int) bool) {
    if comp(x, y) {
        return x
    }
    return y
}

func tryLess() {
    comp := func(x, y int) bool { return x < y }
    fmt.Println(less(10, 20, comp))
}
```

匿名函数的使用等等，会在后面进行介绍。

**2.6 切片类型（slice）**

人们总是抱怨数组的定长不够方便，而在[C++ STL](https://www.geeksforgeeks.org/the-c-standard-template-library-stl/)的[vector](https://www.geeksforgeeks.org/vector-in-cpp-stl/)出现后，大家就开始疯狂使用这个名为动态数组的东西，还可以自己开辟内存空间，这真是太好用了！

动态数组出现之后，人们又在其基础之上发明了切片（slice），不仅长度可变，而且有很多利用``[start:end:step]``进行的操作。最典型的便是[Python中的列表](https://www.liaoxuefeng.com/wiki/1016959663602400/1017269965565856)。

但是这是Go语言，所以我们来看看Go语言的切片：

```go
type intSlice []int  // 在类型定义上，和数组除了长度没有区别

func trySlice() {
    ints := intSlice{1, 2, 3, 4, 5}
    fmt.Println(len(ints), ints[1:3])// 使用len方法获取切片的长度，也可以使用cap方法获取切片的容量
    
    anotherInts := make(intSlice, 5) // 使用make方法创建一个长度为5的切片
    copy(anotherInts, ints)          // 使用copy方法复制内容到新切片
    fmt.Println(append(ints, 6))     // 使用append方法增加新元素到切片，但是不是原地操作
    fmt.Println(append(ints, anotherInts...))
}
```

**2.7 接口类型（interface）**

如果说能让程序员在这个强类型强到天上的语言中找到什么慰藉的，大概就是强大的**接口**了，至于接口是什么，可以看[这个](https://en.wikipedia.org/wiki/Interface_(computing))

定义接口的方式和定义结构体的方式相似：
```go
type HumanNature interface {
    Speak() string   // 接口匹配方法
    See(interface{}) // 空接口默认可以匹配所有的类型
}
```

**接口的知识太多了，放在以后赘述。**

**2.8 映射类型（map）**

映射，很方便，很实用。因为查询的效率，无序性等等。

map的派生类型是由**键类型**和**值类型**共同决定的。常见的构建方式是使用``make()``方法。

```go
func tryMap() {
    newMap := make(map[int]string)
    newMap[1] = "C"           // 插入键值对
    
    if value, have := newMap[2]; have { // 通常用这种方式检查是否有对应的键值对
        // ...some code here
    } else { fmt.Println("Key: 2 Not Found!") }
    newMap[2] = "C++"; newMap[3] = "Java"
    
    for key, value := range newMap { // 通过这种形式遍历map
        fmt.Println(key, "-", value)
    }
    delete(newMap, 1)                // 通过内置的delete方法来删除键值对
}
```

map是使用Go语言时非常常用的类型，不论是编写并发代码还是正常代码，都是**非常重要**的。

我的介绍不尽详细，我**个人的疏忽**可能会导致这个教程的不正确性和不稳定性，欢迎阅读者提出issue。
还有，我饿了，就先写到这里吧。 -- 2019-09-04

------

### 基本的程序结构

众所周知的基本程序结构有**顺序**，**分支**，**循环**。

顺序就不再赘述，接下来讲解基本常见的分支和循环的书写和常用模式。

#### 常见的分支程序结构和模式：

**1. if-else**

这是**所有**程序员都逃不开的结构，这实在是太重要了。``if-else``背后所潜藏的思想是非常深刻的。**除了True，就是False**。也就是**对于全集取子集和补集**，众所周知：世界是连续的，而计算机是离散的。而``if-else``背后的集合和真值的思想正是**离散**数学中的概念。

结论：``if-else``是**将连续的世界离散化的重要发明**。

**普通的**``if-else``长这个样子：

```go
if condition_1 { // condition_1的值一定是布尔类型才可以，Go语言的if语句条件非常苛刻，不接受任何布尔类型之外的值
    // ...some code here
} else if condition_2 {
    // ...some code here
} else {
    // ...some code here
}
```

由于Go语言``if``语句的严苛，我们不可以这么写：

```go
if 1 {} else {}        // 错
if "string" {} else {} // 错
if !nil {} else {}     // 错
```

**多个返回值的**（或者需要预处理语句的）``if-else``是这个样子的：（之前的类型map介绍中已经提及过了）

```go
if result, statu := function(); statu { // 在条件语句中的子语句是生效的，但是作用域和生命周期仅限于本语句的作用域
    // ...some code here, using result  // 使用分号``;``来分割判定的表达式和产生表达式值的语句
} else {
    // ...some code here
}
// can not use result
```

在Go语言的规范中，值是否存在，调用是否产生错误，**产生的值都是习惯性的放在返回结果的最后一个**，因此判断时的习惯也是一样的。

**2. switch-case**

为了避免众多的``if-else if-else``来进行大量的值的判断。人们发明了开关语句，也就是经典的``switch-case-default``语句。

大多数语言的``switch-case``语法都是相同的，但是在使用Go语言的``switch-case``语句时，需要小心，因为Go语言的``switch-case``语句是默认每个``case``后面**自带**``break``的，只是**隐藏**了而已。

```go
switch value {
case value_1:
    // ...some code here
    // 这里有一个隐藏的break，也就是说不会继续执行下一个case
case value_2:
    // ...some code here
    break // 但是这里就算加了break也不会有问题
case value_3:
    // ...some code here
    fallthrough // fallthrough语句可以突破默认的break，可以继续进入下个case
default:
    // ...some code here
}
```

在Go语言中，大家都乐于使用在类型上没那么严格的接口，但是回过头来又要使用对应的强类型，这就要用到**类型断言**，在别的语言中也很常见的一种语法。而在Go语言这样常见的环境下，更是有对应的``switch-type``模式：

```go
switch value := i.(type) {
case T:
    // here v has type T
case S:
    // here v has type S
default:
    // no match; here v has the same type as i
}
```

**3. select-case**

``select-case``是Go语言特有的语法，用于选择不同的通信操作。哪个通信操作最先不阻塞（收到结果），则最先进入哪个``case``，相比较``switch-case``而言，``select-case``的判断是无序的。语法如下：

```go
select {
case communicationClause1:
    // ...some code here
case communicationClause2:
    // ...some code here
default:
    // ...some code here
}
```

在真实的使用场景中，常常和``for``一起组合为``for-select``使用，这些将会在后面介绍。

#### 常见的循环程序结构和模式：

在Go语言中，表示循环的关键字就只有``for``。而``for``中可以有三种循环的模式：

**正常的for循环**：

正常的``for``循环遵循以下格式：

```go
for initial; condition; after { // 和基本所有语言中的模式都是一样的
    // ...some code here
}

for i:=0; i<10; i++ {}          // 比如这样的
```

**while形式的for**：

Go语言中的while循环也使用``for``关键字来实现：

```
for condition {
    // ...some code here
}
```

**死循环**：

永真循环，也就是死循环，你可以使用while循环形式来实现，也可以用普通的形式来实现。当然也可以用Go语言提供的方式来实现。

```go
for ;true; {} // 使用普通的for循环形式
for true {}   // 使用while循环的形式实现
for {}        // 使用Go语言提供的死循环的形式
```

------
