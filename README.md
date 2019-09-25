# 不写代码的屁股：Go语言教程

[English Version](https://github.com/ShiinaOrez/Tutor-Go/README-EN.md)

## 目录

+ 0. [什么是Go语言](#什么是Go语言)
+ 1. [Go语言的安装](#Go语言的安装)
+ 2. [开始我们的第一个例子](#开始我们的第一个例子)
+ 3. [变量和数据类型](#变量和数据类型)
+ 4. [基本的程序结构](#基本的程序结构)
+ 5. [开始简单的并发](#开始简单的并发)
+ 6. [Go语言中的函数](#Go语言中的函数)
+ 7. [Go语言中的面向对象](#Go语言中的面向对象)

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

函数变量的类型由几个因素组成：**函数名**、**参数**、**返回值**

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

扩展阅读：

+ [Data types in Go](https://www.geeksforgeeks.org/data-types-in-go/)
+ [Understanding Data Types in Go](https://www.digitalocean.com/community/tutorials/understanding-data-types-in-go)
+ [Overview of Go Type System](https://go101.org/article/type-system-overview.html)

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
if 1 {
} else {
}     // 错

if "string" {
} else {
}     // 错

if !nil {
} else {
}     // 错
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

扩展阅读：
+ [select机制和常见的坑](https://wudaijun.com/2017/10/go-select/)

------

### 开始简单的并发

如果你是一个已经有了一定开发经验的程序员，那么你在接触Go语言之前会觉得编写并发代码是一件非常麻烦的事情：线程的管理，线程之间的通信等等，由于没有语言原语的支持，编写并发代码都是建立在大神编写的并发框架的代码之上的，正常情况下无法触碰到底层的具体逻辑和实现。

但是既然你现在开始接触神奇的Go语言了，你大可放下这方面的顾虑：因为Go语言提供了大量的[并发原语]()来支持从零开始的并发代码开发。接下来就使用最简单的例子开始一个并发代码的编写：

```go
package main

import "fmt"

func main() {
    go func() { // 使用go关键字来开启一个goroutine
        fmt.Println("Hello~Goroutine~")
    }()
}
```

上面这段代码已经是一个可以并发的代码了：如果你运行它，你会发现有时候并没有办法输出"Hello~Goroutine~"这段文字，这是因为**打印文字和主函数已经是并发运行**的了。

在Go语言中，包括主函数的程序运行在名为**goroutine**的**轻量级**进程上，而使用``go``关键字可以开启一个新的goroutine，从而达到并行的效果。因此在上面的一段代码中，主goroutine和匿名函数所在的goroutine是并行的，因此会存在**两种情况**：①主goroutine先结束 ②文字被先打印出来。

Goroutine是轻量级的，因此在Go语言的代码中你可以开启**成千上万**的goroutine，这在其他语言中是很难做到的。而进程之间的通信就交给Go语言的``channel``类型来完成（前面有介绍）。

```go
package main

import "fmt"

func main() {
    done := make(chan struct{}) // 定义一个空接口体类型的通道
    go func() {                 // 开启一个goroutine
        fmt.Println("Hello~Goroutine~")
        done<- struct{}{}       // 告诉done已经完成了工作
    }()                         // 匿名函数声明的同时进行调用
    <-done                      // 在通道内没有值之前，会堵塞在这里，因此程序不会在打印前结束
    close(done)                 // 记得关闭通道
}
```

现在使用了通道在goroutine之间进行通信，我们可以很好的来进行goroutine之间的同步了。

使用通信来进行goroutine之间的同步操作是基于CSP（通信顺序进程）的思想。但是很多人会更加习惯于去使用**锁**来进行同步，但是我认为锁是**面向资源**的，并不适合用于同步不同的线程或goroutine。

```go
// 使用锁的例子
package main

import (
    "fmt"
    "sync"
)

func main() {
    lock := new(sync.Mutex) // 声明一个锁
    lock.Lock()             // 锁上
    go func() {
        fmt.Println("Hello~Goroutine~")
        lock.Unlock()       // 开锁
    }()
    lock.Lock()             // 锁上，但是在锁是打开的之前也会阻塞
}
```

但是在同步goroutine的方面，还是**使用通道来进行通信**是比较推荐的方式。

扩展阅读：
+ [Go语言并发学习wiki](https://github.com/golang/go/wiki/LearnConcurrency)

------

### Go语言中的函数

#### 6.1 函数声明

函数的组成部分有**名字**, **形参列表**, **可选的返回列表**, **函数体**：

```go
func functionName(parameter-list) (result-list) {
    // body
}
```

我相信在前面写过的不少示例中，大家已经熟悉了这种声明方式。这是最普通，也是最基础的声明方法。但是Go语言中的函数还有和其他语言不同的地方。

当形参列表中的几个连续类型都相同时，可以选择只写一个类型标识符。

```go
func min(x, y int) int
func min(x int, y int) int // 二者是完全相同的
```

使用4种方式来声明具有**同样函数签名**的函数：

```go
func add(x int, y int) int { return x+y }      // 正常的声明
func sub(x, y int) (z int) { z = x-y; return } // 在返回值列表写好变量名，在返回时会自动返回该变量
func first(x int, _ int)   { return x }        // 下划线强调未使用
func zero(int, int)        { return 0 }        // 不使用参数的情况
```

Go语言中的函数声明是非常灵活的。

#### 6.2 递归

和所有的语言一样。Go语言也支持函数的**递归调用**，也就是自身调用自身。

```go
func dfs(node *TreeNode) { // 一个常见的对于二叉树的中序遍历的例子
    if node.Left != nil {
        dfs(node.Left) 
    } // 调用自身
    // use this node
    if node.Right != nil { 
        dfs(node.Right) 
    }
}
```

整个例子，自己实现一下是很有趣的：

```go
package main

import "fmt"

type TreeNode struct {
    Val    int
    Left   *TreeNode
    Right  *TreeNode
}

func dfs(node *TreeNode) {
    if node.Left != nil { 
        dfs(node.Left) 
    }
    fmt.Println(node.Val)
    if node.Right != nil { 
        dfs(node.Right) 
    }
}

func main() {
    // construct a tree here
    dfs(root)
}
```

#### 6.3 多个返回值

现在能够支持多个返回值的语言已经非常普遍了，如``Python``等等。Go语言也是支持多个返回值的，尤其是在错误处理方面，不像其他语言的``try-except``或者``try-catch``模式，而是直接在返回值中加入``err``的值。

下面是摘自[strconv包](https://golang.org/pkg/strconv/#pkg-index)的几个函数原型：

```go
func ParseBool(str string) (bool, error)
func ParseFloat(s string, bitSize int) (float64, error)
func ParseInt(s string, base int, bitSize int) (i int64, err error)
func ParseUint(s string, base int, bitSize int) (uint64, error)
```

可以很明显的看出来，由于从字符串解析不同的类型时，经常会出现无法解析，或者解析失败的情况，开发者们在内置函数的**最后一个**返回值放上了**错误信息**。而判定的方式也是非常简单的，因为``error``类型可以和``nil``进行比较：

```go
_, err := strconv.Atoi("&&")
if err != nil {
    // error handle
}
```

所以当我们编写自己的函数的时候也要注意返回错误：

```go
func yourHandler(str string) (int, error) {
    val, err := strconv.Atoi(str)
    if err != nil {
        return 0, err // 将错误返回上层
    }
    return val, nil   // 没有错误时返回nil
}
```

#### 6.4 函数变量

函数变量在前面的派生类型中已经介绍过了，这里只是简单写一下用法实例。

函数变量的**零值**是``nil``，因此函数变量可以和``nil``进行比较，但是函数变量之间本身是**不可以互相比较**的！

```go
var f func(int) int  // 变量f为nil
f(4)                 // 调用一个为零值的函数会出错宕机

if f != nil {
    f(4)             // 判断函数是否为空
}
```

现在在Go语言中，由于现在还不支持泛型程序设计，导致函数变量的地位还不是很高。但是在Go2出现泛型之后，函数变量的地位一定会飙升。

#### 6.5 匿名函数

在Go语言中，匿名函数是非常常见的，除了方便撰写小规模的函数之外，还因为Go语言中的``go``关键字后面**仅允许**接函数，这就导致了匿名函数的大量使用。

普通的使用匿名函数：

```go
x, y := 10, 20
fmt.Println(func(a, b int) int {
    if a > b {
        return a
    }
    return b
}(x, y))
```

结合闭包的特性使用匿名函数：

```go
func squares() func() int {
    var x int                 // 在返回的匿名函数的眼中，这是一个全局变量，这是由于闭包的性质决定的
    return func() int {
        x++
        return x*x
    }
}

func main() {
    f := squares()
    fmt.Println(f(), f(), f(), f()) // 1, 4, 9, 16
}
```

#### 6.6 变长函数

变长函数被调用的时候可以有可变的参数个数。最常见的就是``fmt.Println()``，那么如何声明一个变长函数呢？

在参数列表的，**最后的类型名称之前**使用省略号``...``，表示声明一个变长函数，调用这个函数的时候可以传递该类型任意数目的参数。

```go
func sum(vals ...int) int {
    tot := 0
    for _, val := range vals { // 在变长函数中，以slice的形式组织多个相同类型的参数
        tot += val
    }
    return tot
}
```

#### 6.7 延迟函数调用

所谓**延迟调用**，其实就是讲解一下``defer``关键字的作用。

有的时候，一些操作是需要成对进行的，比如**内存的声明和释放**（malloc、new和free），**文件的打开和关闭**（fopen和fclose）,**数据库的连接和释放**等等。

在编写代码的时候，忘记，或者配对的语句失效的情况是很常见的：比如程序直接panic了，那么接下来的语句也不会执行了。如果你学习过Python语言，那么你一定知道``with``语句和[上下文](https://www.jianshu.com/p/7bae11eaf84d)的概念（Python使用这种方式来确保能够让程序正常运行），而在Go语言中，使用了``defer``关键字来达到这种目的。

``defer``关键字的作用就是**延迟**目标函数的调用，在该语句的作用域退出前再进行执行。

比如：（对之前的示例进行改造）

```go
func main() {
    ch := make(chan struct{})
    defer close(ch)           // 相比之前的结构，我们现在可以使用更舒适的逻辑来编写程序
    
    go func() {
        fmt.Println("Hello, golang!")
        ch<- struct{}{}
    }
    <-ch
    // close(ch)实际会在这个位置执行
}
```

为了证明即使宕机了也可以执行``defer``，我们做个实验：

```go
func main() {
    defer fmt.Println("I'am defer.")
    
    f := func(int) int
    f(4)
}
```

多个``defer``语句执行的顺序是从后到前的，因为是[栈]()嘛。

#### 6.8 宕机和恢复

宕机和恢复，其实是讲``panic()``和``recover()``这两个Go语言的内置函数。

**1. panic**

当你的程序遇到不可操控的错误时，你的程序会**宕机**，也就是panic，当然我们可以自己去panic，这样往往比Go语言自动的panic更好也更可控，也更利于**trouble-shooting**，使用了panic函数之后你的程序会立刻退出。

```go
func main() {
    // ...do something
    if err != nil { // error can not handle
        panic(err)  //
    }
    // normal logic
}
```

**2. recover**

退出程序通常是正常处理宕机的方式，但是也有例外，在**一定情况**下是可以进行恢复的，比如在web开发时，一个具体的handler内部出现了足以让程序宕机的错误，但是这个时候应该做的是返回一个[500](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500)的状态，而不是一直阻塞在那里（程序都崩溃了怎么给response啊）。

如果内置的``recover``函数在延迟函数的内部调用，而且这个包含``defer``语句的函数发生了宕机，``recover``会终止当前的宕机状态并且返回宕机产生的值。函数**不会在宕机的位置继续运行**，而是正常返回。

例如：

```go
func main() {
    defer func() {
        if p := recover(); p != nil {
            err = fmt.Errorf("Internal Error: %v", p)
        }
    }()
    // ...some code here
    // 如果这里发生了宕机，会在defer语句中执行错误的输出
}
```

扩展阅读：

+ [Go语言闭包和匿名函数，一篇就够了](https://www.jianshu.com/p/faf7ef7fbcf8)
+ [Go语言panic和recover用法](https://www.jianshu.com/p/0cbc97bd33fb)
+ [Go语言defer特性详解](https://www.jianshu.com/p/57acdbc8b30a)

------

### Go语言中的面向对象

作为一门**现代**高级编程语言，Go语言当然支持**面向对象**的编程思想。但是和C++中**萌芽初现**的[面向对象](https://www.geeksforgeeks.org/object-oriented-programming-in-cpp/)以及Java信奉的[绝对的面向对象](https://www.geeksforgeeks.org/object-oriented-programming-oops-concept-in-java/)相比，Go语言的面向对象显然是太不合群了。

如果你使用过C++，那么你会知道面向对象的基础是对于**类**(``class``)的抽象，事实上Java也是这样。如果你对于面向对象是什么都不清楚的话，我觉得你需要看一下下面这篇文章：[维基百科-面向对象程序设计](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)

好了，现在开始我认为你了解了基本的面向对象程序设计的知识，那么你会不会也认为Go语言也会有一个名为``class``的关键字？毕竟C++，Java，Python都是这样的啊。

事实上，Go语言**确实没有**一个名为``class``的关键字用来声明一个类，甚至都没有类的概念。Go语言实现面向对象的基石是**结构体**。

接下来我们写一个最简单的面向对象的例子：

```go
package main

import "fmt"

type HumanName struct {
    FirstName   string
    LastName    string
}

type Human struct {
    Name   HumanName
    From   string
}

func (human *Human) Speak() { // 为类型Human声明方法Speak
    fmt.Println("I'm", human.Name.FirstName, human.Name.LastName+", and I came from", human.From)
}

func main() {
    tom := Human{
        Name: HumanName{
            FirstName: "Jupter",
            LastName:  "Tom",
        },
        From: "USA",
    }           // 创建一个Human类型的实例
    tom.Speak() // 调用实例的方法
}
```

现在你应该恍然大悟了，原来Go语言没有类，只有类型。只不过可以**给特定类型绑定上方法**。而原来类中的属性就对应复合类型中的字段。只是知道这点就可以让我们进行简单的面向对象编程了。但是这还远远不够，我们还有很多问题没有解释清楚。

#### 7.1  方法

比如绑定在**类型**上的方法和绑定在**类型指针**上的方法的区别？Go语言这样一个强类型语言，一定会在这上面有特别的区分吧！

```go
func (human *Human) Speak1() {} // 绑定在类型指针上，为指针方法
func (human Human)  Speak2() {} // 绑定在类型上，为值方法
// 非常奇妙的事情，明明是不同的方法接收者，但是却不能使用同样的方法名
```

在Go语言官方文档中有[关于方法的值接收者和指针接收者](https://golang.org/doc/effective_go.html#pointers_vs_values)的区别

对于没有涉及内存的操作（比如说打印一段话，使用值进行计算），值接收者和指针接收者没有什么区别，毕竟都可以使用``.``来取得值。

但是如果涉及到了值的改变，也就是对于内存的操作，事情就变得麻烦起来：

```go
package main

import "fmt"

type TestInt int

func (value TestInt) Increase1() {
    value += 1
}

func (pointer *TestInt) Increase2() {
    (*pointer) += 1
}

func main() {
    var number TestInt = 1
    numberPointer := &number
    
    fmt.Println(number) // 1
    number.Increase1()
    fmt.Println(number) // 1
    numberPointer.Increase2()
    fmt.Println(number) // 2
}
```

很明显，也是我们预料中的结果，因为一个是对于传入的临时值进行了改变，而没有涉及``main``函数中定义的``number``变量。而传入指针可以让我们很好的操作内存空间。

我们这个示例中，对于值接收者和指针接收者都有不同的方法。但是对于**值/指针**调用**值方法/指针方法**的情况，我们还需要进行探讨。

```go
// 我们只保留值方法
func (value TestInt) Increase() {
    value += 1
}

func main() {
    var number TestInt = 1
    numberPointer := &number

    fmt.Println(number) // 1
    number.Increase()
    fmt.Println(number) // 1
    numberPointer.Increase()
    fmt.Println(number) // 1
}
```

我们运行这个代码，居然没有任何的错误信息！它照常的运行了。只不过值方法仍然没能改变值的值。使用指针调用值方法的时候，只会传入指针对应的值，而这个转换过程是Go语言内部执行的。实际效果：

```go
(*numberPointer).Increase()
```

如果我们**只保留指针方法**呢？结果是显而易见的

```go
func (pointer *TestInt) Increase() {
    (*pointer) += 1
}

func main() {
    var number TestInt = 1
    numberPointer := &number
    
    fmt.Println(number) // 1
    number.Increase()
    fmt.Println(number) // 2
    numberPointer.Increase()
    fmt.Println(number) // 3
}
```

也就是说，当只有对应的指针方法存在时，会将值的指针传入来调用指针方法，也就是说实际是这样的：

```go
(&number).Increase()
```

也就是说，Go语言会自动寻找和匹配当前最好的方法匹配策略：

+ 如果同时兼有值方法和指针方法，会分别进行调用
+ 如果只有值方法，会隐式转换指针为值进行调用
+ 如果只有指针方法，会隐式转换值为指针进行调用

#### 7.2 embedding而非extends

说到面向对象的时候，大家就会想到几个词：**封装**，**继承**，**多态**

不太凑巧，你在Go语言中，可能除了封装都见不到...

传统意义上的继承是指一种``is a ``的关系，比如``pig is an animal``，猪是一个动物，那么猪**类**就应该继承自动物**类**，动物能做的猪也都能做，而猪又有自己特有的方法和属性。

但是很遗憾，Go语言中并没有继承的概念，有的只是**组合**。因为面向对象依托于结构体的实现，因此一个新的类型只能通过组合的方式来产生：

```go
type Eyes  string // 眼睛类型
type Mouth string // 嘴类型
type Nose  string // 鼻子类型
type Ears  string // 耳朵类型

func (ears *Ears) Hear(s string) { // 耳朵类型的指针听方法
    fmt.Println(s)
}

type Face struct { // 脸类型，由各个部分拼接而成
    Eyes
    Mouth
    Nose
    Ears
}

func main() {
    face := Face{}
    face.Hear("Golang is the best!") // 获得了耳朵的方法
}
```

就像上面的例子，``Face``获得了``Ears``的方法，和``is a``的关系相比，更像是一种寄生的关系。

并且，在Go语言中是**没有**[函数重载](https://www.runoob.com/cplusplus/cpp-overloading.html)的。

**嵌入和聚合**：

寄生的关系可以使用两种方式来实现：嵌入和聚合。

```go
type Head struct {
    Organ // 器官
    Face  face
    Hair  hair
}
```

Organ（器官）类型会有一些方法，比如说被福尔马林浸泡这种，那么我们调用头的浸泡福尔马林方法时，肯定不应该调用``head.organ.Formalin()``，而是应该调用``head.Formalin()``，这种将一个类型包含在另一个类型的内部的方式叫做**嵌入**。

相对应的，将一个类型声明为另一个类型的属性的方式叫做**聚合**，我们调用听的时候，肯定是想要去这样调用：``head.ear.Hear()``，而不是``head.Hear()``。

**在合适的嵌入的时候嵌入，在合适聚合的时候聚合。**

####  7.3  接口

当你看到这个教程的时候，我认为你是一个有一定面向对象编程经验的人，如果你对于接口这个概念不太理解，那么可以先去了解接口的定义和作用。

Go语言的接口类型是对于其他类型行为的抽象和概括，因为接口类型不会和特定的实现细节绑定在一起，通过这种抽象方式我们可以让对象更加灵活和更具有适应性。

如果你对于Python有足够的了解，那么你会知道面向对象思想中的**鸭子类型**和**白鹅类型**，所谓鸭子类型说的是：

如果一个Obejct走起路来像鸭子，叫起来像鸭子，那么它就可以是一个鸭子。或者只是在我们在意的方面和鸭子相似，我们便可以把它当做鸭子。

比如一个鸭子除羽毛器：它接收一个动物，然后除去它的羽毛。

当然，你可以放一只真的鸭子进去，然后你会得到一个没有羽毛的鸭子。但是你要是放一只山雀应该也没什么问题（大家要保护野生动物），因为除羽毛器不关心动物的品种，只关心有没有羽毛。

而白鹅类型则更像一个DNA检测器：它要求接受和猴子有相同基因片段的生物。那么你放进去一个猴子，它抽了一管血，并且检查出了相同的片段。你把自己放进去大概也不会有什么问题。但是如果你把一个猴子电动玩具放了进去：假设经过人工智能的训练，这个玩具几乎和真正的猴子的行为是一致的，这个DNA检测器可能会坏掉并且把你炸飞。

白鹅类型在乎的不是抽象的行为，而是**血统**，也就是面向对象中的继承关系。

Go语言实现的是鸭子类型。

接口的声明和定义我们在之前已经讲过了，这里不再赘述，只是分析Go语言中的``io.Reader``和``io.Writer``来丰富我们对于接口的认识：

**io.Reader** & **io.Writer**

首先看看具体的定义：

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}
```

啊，这两个接口是多么的简单。事实上这两个是最小粒度的接口，分别实现了读方法和写方法。

如果我们需要一个类型，既可以读，也可以写呢？我们是要声明一个具有读方法和写方法的接口吗？如果Go语言只支持继承的话，可能会演变出两种类型：

+ 1. 继承自Reader，额外实现了Write()方法的类型
+ 2. 继承自Writer，额外实现了Read()方法的类型

有点奇怪，定义一个新类型也有点奇怪。

所以我们使用Go语言中嵌入的方法来实现``io.ReadWriter``接口类型：

```go
type ReadWriter interface {
    Reader
    Writer
}
```

那么也就是说，我们可以通过嵌入的方式来扩展一个接口。从而可以构建不同粒度的接口。我们可以在我们自己的代码中扩展接口，从而利用许多既有的方法：比如可以给任意一个自定义类型定义好``Read()``方法，返回一个JSON字符串，从而完成自定义的JSON生成和解析。

**接口的显式转换和隐式转换**

在Go语言中对于接口类型的互相转换相对于其他类型而言较为宽松。

一般我们在使用除了接口以外的类型时，进行类型转换只能通过强制类型转换来实现，甚至连将一个``int``类型的值赋值给``int64``类型都坐不到。而接口和一般类型之间往往需要使用**类型断言**来实现。

但是接口之间的转换却是十分的方便（相对而言），这里借鉴柴先生书中的一个示例：

```go
var (
    // 隐式转换，*os.File满足io.ReadCloser接口
    a io.ReadCloser = (*os.File)(f)
    // 隐式转换
    b io.Reader     = a
    // 隐式转换
    c io.Closer     = a
    // 显式转化，io.Closer不满足io.Reader接口
    d io.Reader     = c.(io.Reader)
)
```

接口和接口之间灵活的转换，对于平常被强类型摁在地上摩擦的Go语言程序员而言简直就像是天使一般，但是在幸福的同时往往最可能犯错！接口之间的过于灵活让大家开始担忧无意之间类型适配的问题。

**于是大家给特定的接口特有的方法，这样便可以大概率的阻止无意义的适配。**

接口是Go语言面向对象编程时的利器，但是使用时也要特别注意！

------
