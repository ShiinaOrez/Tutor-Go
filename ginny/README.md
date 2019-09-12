# ginny - 基于Go的web后台开发实战

Gin web development: Development Web Backend with Go.

# 第一部分  Gin简介

## 第1章  安装

### 1.1 Go语言开发环境

欢迎加入Go语言的大家庭！从现在开始你要开始跟随我一起学习使用Go语言的[gin](https://github.com/gin-gonic/gin)框架来进行开发的知识，再次表示欢迎。

关于Go语言的安装和基本的工作环境配置，可以参考[这里](https://github.com/ShiinaOrez/Tutor-Go#go%E8%AF%AD%E8%A8%80%E7%9A%84%E5%AE%89%E8%A3%85)，接下来我将认为你已经了解了Go语言开发的基本环境知识。

那么，我们开始吧！

### 1.2 安装gin

安装gin的方式一般有两种：

1. 使用``go get``进行安装：

```bash
$ go get -u github.com/gin-gonic/gin
```

2. 使用``git clone``来下载包：

```bash
$ cd ~/go/src/github.com/gin-gonic && git clone https://github.com/gin-gonic/gin
```

使用``go get``进行安装是官方的推荐方法，但是由于[GFW](https://zh.wikipedia.org/wiki/%E9%98%B2%E7%81%AB%E9%95%BF%E5%9F%8E)的存在，导致国内下载很慢，可以去使用代理进行下载。

使用``git clone``可能会有依赖安装不全的情况，可以一个个的手动下载。。。

接下的章节我们将认为你的依赖和环境都是安装好的。

## 第2章  以一个简单的注册接口开始

### 2.1  从Hello, gin!开始

学习任何东西的第一个例子都会是Hello, world式的例子，我们也不例外。

将一下代码放在你的工程目录下，然后保存：

```go
package main

import "github.com/gin-gonic/gin"

func main() {
    router := gin.Default()
    router.GET("/hello", func(c *gin.Context) {
        c.JSON(200, gin.H{
            "message": "Hello, gin!",
        })
    })

    router.Run()
}
```

然后使用以下命令运行它：（不出意外我们默认运行目录下的``main.go``文件）

	提示：可以在本教程的附属仓库 https://github.com/ShiinaOrez/ginny.git 中通过标签``v0.1.0``来查看这个示例。

```bash
$ go build -o main && ./main
```

不出意外的话（如果你没有抄错代码），你会成功运行这段代码，并且获得这样一段输出：

```bash
[GIN-debug] [WARNING] Now Gin requires Go 1.10 or later and Go 1.11 will be required soon.

[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:	export GIN_MODE=release
 - using code:	gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /hello                    --> main.main.func1 (3 handlers)
[GIN-debug] Environment variable PORT is undefined. Using port :8080 by default
[GIN-debug] Listening and serving HTTP on :8080

```

倒数第三行告诉我们，我们声明了一个路由``/hello``的处理函数为``main.main.func1``，之所名字这么古怪是因为我们使用了匿名函数。

现在打开浏览器，访问以下地址：``localhost:8080/hello``，你会获得一个[JSON](https://www.json.org/)字符串的输出（可以使用Google Chrome浏览器的[JSON Handle插件](https://chrome.google.com/webstore/detail/json-handle/iahnhfdhidomcpggpaimmmahffihkfnj)，可以更好的可视化JSON的结构）。

现在让我们回头看一下代码：

```go
package main
```

说明这段代码属于包：main

```go
import "github.com/gin-gonic/gin"
```

引入``gin``包，引入这个包是使用这个框架的前提。

```go
    router := gin.Default()
```

使用``Default()``方法来获取一个基本的路由变量。

```go
    router.GET("/hello", func(c *gin.Context) {
        c.JSON(200, gin.H{
            "message": "Hello, gin!",
        })
    })
```

使用一个匿名函数作为路由``/hello``的处理函数，注意处理函数必须是一个```func (*gin.Context)```类型的函数，这个函数使用传入的上下文变量（context就是上下文的意思，在这里是一个网络请求的上下文，一些请求和响应的信息都是从这里进行传递的）的``JSON()``方法返回了一个JSON格式的字符串。而``gin.H()``方法其实就是简化JSON的生成，本质是一个``map[string]interface{}``。

所以最后我们使用``Run()``方法来启动我们的路由：

```go
    router.Run()
```

如果对于这方面的阅读有困难的话，可以去看看我写的[Go语言教程](https://github.com/ShiinaOrez/Tutor-Go)。

接下来我们将以一个真实的博客后台项目来进行``gin``框架的学习。

### 2.2  根据参数返回不同的字符串

```go
    router.GET("/hello", func(c *gin.Context) {
        name, ok := c.GetQuery("name")
        if !ok {
        	c.JSON(400, gin.H{
                "message": "Bad Request!",
        	})
        	return // 这里若是不返回，还会继续执行下面的逻辑
        }
        c.JSON(200, gin.H{
                "message": "Hello, " + name + "!",
        })
    })
```

我们使用``gin.Context``的``GetQuery()``方法来获取请求中**查询字符串**（[Query String](https://www.techopedia.com/definition/1228/query-string)）中包含的参数，同时返回的两个值，第一个返回值代表值，第二个返回值代表是否包含这个参数。

我们现在再访问``localhost:8080/hello?name=jojo``，你会得到一个这样的JSON字符串输出：

```json
"{message:"Hello, jojo!"}"
```

将参数改为``gin``之后，你会发现输出变成了这样：

```json
"{message:"Hello, gin!"}"
```

如果不包含这个参数的话，比如说访问这个地址：``localhost:8080/hello``

那么你会收获一个[Bad Request](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/400)的错误。

	提示：可以在本教程的附属仓库 https://github.com/ShiinaOrez/ginny.git 中通过标签``v0.1.1``来查看这个示例。


### 2.3  提交JSON数据 -- POST请求

客户端给服务端（也就是后台）发送请求时，数据一般包含在以下几个方面：

+ 路径中（path）
	+ 路径参数（path parameters），比如用户id
	+ 查询字符串（query string），比如用户名或文章名
+ 头部中（header）
	+ cookies
	+ 自定义头部，比如令牌（token）
	+ 一些固有规则的字段（比如content-type和user-agent）
+ 请求的载荷（payload）

一般来说，GET请求是不会主动发送大量的数据的，一般数据以路径和头部的形式进行携带，因为GET的本意就是**获取资源**而非发送大量的数据。而POST请求一般会携带较多的数据，以请求的载荷（payload）进行发送，而发送时会有各种组织数据的形式，JSON就是其中一种。

我们之前已经使用JSON返回过数据了，相信读者也一定了解JSON了，那么我们这次就来试着处理一下JSON发送过来的数据。

首先我们要学会如何在Go语言中使用结构体表示JSON数据：

```go
type JSONTest struct {
    Username    string    `json:"username"`
    Password    string    `json:"password"`
}
```

众所周知，JSON组织数据的形式其实就是一个字典（dict），而Go语言访问结构体中字段是以**属性**而并非**键值对**的方式来访问的，因此需要在结构体中对字段打上"备注"，来让解析JSON字符串的时候能够把相对应的值以字典的形式进行存储。

有了这个结构体之后我们再来看看怎么获得请求发送给我们的数据呢？

使用``gin.Context``的``BindJSON()``方法来获取JSON数据。比如以下实例：

```go
func handle(c *gin.Context) {
    var data JSONTest
    err := c.BindJSON(&data)  // 该方法会返回一个error类型的变量，如果不为nil则为绑定失败
    // use data.Username or data.Password
}
```

所以我们现在可以撰写这样一个路由的处理函数：接受传送的用户名和密码，然后返回二者的拼接字符串。

```go
// ./main.go
package main

import "github.com/gin-gonic/gin"

type RegisterPayload struct {
	Username  string  `json:"username"`
	Password  string  `json:"password"`
}

func main() {
	router := gin.Default()
	router.POST("/register", func(c *gin.Context) {
        var data RegisterPayload
        if err := c.BindJSON(&data); err != nil {
        	c.JSON(400, gin.H{
                "message": "Bad Request", // 如果绑定失败，那么我们认定它为一个坏请求，按照规范，状态码应该为400
        	})
        	return
        }
        c.JSON(200, gin.H{
            "result": data.Username + data.Password,
        })
        return
	})

	router.Run()
}
```

	提示：可以在本教程的附属仓库 https://github.com/ShiinaOrez/ginny.git 中通过标签``v0.1.2``来查看这个示例。

现在有个不是问题的问题浮现出来：我们该如何创建一个POST请求？

这里推荐使用工具[Postman](https://www.getpostman.com/)，可以创建一个这样的请求：

+ 方法为POST
+ 路径为localhost:8080/register
+ Body选择raw->JSON

这样填写：

```postman
{
	"username": "I'm ",
	"password": "jojo!"
}
```

然后点击**Send**，就可以了，不出意外的话你会收到一个"I'm jojo!"的响应。

### 2.4  连接数据库

现在我们遇到了问题，我们的数据只能够在函数中进行操作，而没有办法持久化的来进行数据的存储。

显然我们可以使用一个map变量来充当数据库的作用：

```go
    database := make(map[string]string) // 以Username为key，Password为value
```

然后我们就可以实现这样的一个注册（register）接口：如果没注册过，返回用户名和密码的拼接字符串，并且添加到数据库中，否则返回401错误

```go
    if _, has := database[data.Username]; has {
    	c.JSON(401, gin.H{
            "message": "User already existed.",
    	})
    }
```

	提示：可以在本教程的附属仓库 https://github.com/ShiinaOrez/ginny.git 中通过标签``v0.1.3``来查看这个示例。


**是时候连接一个数据库了!**很明显我们使用变量的方法无法进行数据的持久化，因为我们不能保证程序一直可以运行。

我们这里选择使用[mysql](https://www.mysql.com/cn/)数据库，mysql是一种[关系型数据库](https://zh.wikipedia.org/zh-hans/%E5%85%B3%E7%B3%BB%E6%95%B0%E6%8D%AE%E5%BA%93)。可以存储各个表之间的关系，我们将主要使用它来进行我们博客系统的开发。

#### 2.4.1  安装mysql

我们讨论在Linux/Ubuntu 18.04LTS上安装的情况（欢迎贡献其他环境的方式！）：

```bash
$ sudo apt update                 // 更新软件源
$ sudo apt install mysql-server   // 安装mysql-server
$ sudo mysql_secure_installation  // 运行安全脚本，进行配置
$ systemctl status mysql.service  // 检查运行情况
```

	一般来说mysql会运行在机器的 3306 端口。

在本教程中，数据库名称为``go_blog``，密码为``ginny``，但是需要特别提醒大家：**这类敏感信息应该使用环境变量来书写！**

由于本教程不着重于数据库的使用，因此大家可以参考以下资料：

+ [安装mysql](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04)
+ [mysql教程](http://www.runoob.com/mysql/mysql-tutorial.html)
+ [关系型数据库和非关系型数据库总结](https://juejin.im/post/5d01ed78e51d45775d516f6f)

#### 2.4.2  使用Go语言连接mysql-server

假设我们使用以下脚本创建了表：

```sql
CREATE TABLE `users` (
    `id`       int unsigned   NOT NULL AUTO_INCREMENT,
    `username` varchar(64)    NOT NULL,
    `password` varchar(64)    NOT NULL,
  
    PRIMARY KEY (`id`)
)
```

我们使用user表来存储用户的账号信息：用户名和未加密的明文密码（开发时不要这样做！）

我们导入Go语言的内置包：``database/sql``，并且创建一个数据库对象：

```go
package main

import (
	"database/sql"
    _ "github.com/go-sql-driver/mysql"  // 导入驱动
	
	"fmt"
)

func GetDatabase(username, password, host, port, dbname string) (*sql.DB, error) {
    address := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s", username, password, host, port, dbname)
    db, err := sql.Open("mysql", address)
    if err != nil { // 打开失败
    	return nil, err
    }
    err = db.Ping()
    if err != nil { // 连接失败
    	return nil, err
    }
    return db, nil
}
```

然后我们使用这个函数来获取数据库变量：

```go
    db, err := GetDatabase("root", "ginny", "127.0.0.1", "3306", "go_blog")
    if err != nil {
    	panic("Link to database failed! Reason: " + err.Error())  // 结束程序并且打印错误原因
    }
```

然后使用sql语句来对数据库进行查询：

```go
    userID := 0
    if err = db.QueryRow("SELECT id FROM users WHERE username = ?", data.Username).Scan(&userID); err != nil {
    	db.Query("INSERT INTO users (username, password) VALUES (?, ?)", data.Username, data.Password)
    	c.JSON(200, gin.H{
            "message": data.Username + data.Password,
    	})
        return
    } else {
        c.JSON(401, gin.H{
            "message": "User already existed.",
        })
    }
```

然后我们就实现了一个和之前表面上差不多的注册接口，不过我们的数据已经完成了本地的持久化存储！

	提示：可以在本教程的附属仓库 https://github.com/ShiinaOrez/ginny.git 中通过标签``v0.1.4``来查看这个示例。

**扩展阅读**：
+ [什么是SQL注入，如何防止？](https://www.calhoun.io/what-is-sql-injection-and-how-do-i-avoid-it-in-go/)


## 第3章  整理你的代码 -- 实现一个简单的用户系统

代码写到现在，你会发现我们已经拥有了一个简短（只有60行！），但是却能实现一个基本的后台接口，且能和数据库有交互的程序！这实在是很让人激动！那么接下来保持这股热情，让我们来实现一个基本的用户系统吧！

在此之前，我们要整理一下我们的代码（尽管它只有60行）：

### 3.1  开始整理我们的代码结构

我们的``main.go``里现在包括了几个部分：声明路由，建立数据库连接，还有一个处理函数，但是这些东西都放在一起就会觉得有点乱。所以我们来建立一个良好的目录吧！

重建后的目录结构如下：

```
.
├── handler
│   ├── handler.go
│   ├── login
│   └── register
│       └── register.go
├── model
│   ├── init.go
│   └── user.go
├── router
│   └── router.go
├── main.go

```

然后我们来解释一下各个部分的作用：

+ handler
	+ handler其实就是之前在main.go中的对应路由的处理函数，虽然我们当时使用了匿名函数的方式，但是真正的业务逻辑代码其实就是这个处理函数，因此要抽离出来，这样便于维护和trouble shooting
+ model
	+ model是模型，主要对应数据库的获取，连接，各个表结构的映射和增删查改的方法。
+ router
	+ router是路由表，包括以后一些中间件存放的目录
+ main.go
	+ 应用程序的入口，包含最基本的逻辑

**那么我们开始着手重构吧!**

首先，我们要把数据库的连接和模型都放入model：

```go
// ./model/init.go

package model

import (
	"fmt"
	"log"
	"database/sql"
	
	_ "github.com/go-sql-driver/mysql"
)

type Database struct {  // 对数据库进行封装
	Self *sql.DB
}

var DB *Database

func getDatabase(username, password, host, port, dbname string) (*sql.DB, error) {
    address := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s", username, password, host, port, dbname)
    fmt.Println("Start to connect:", address)
    db, err := sql.Open("mysql", address)
    if err != nil {
    	return nil, err
    }
    err = db.Ping()
    if err != nil {
    	return nil, err
    }
    return db, nil
}

func (db *Database) Init() {
	newdb, err := getDatabase("root", "ginny", "127.0.0.1", "3306", "go_blog")
	if err != nil {
		log.Fatal(err)  // 如果有错误，那么打印，而不是直接退出
	}
	DB = &Database{
		Self: newdb,
	}
}

func (db *Database) Close() {
	DB.Self.Close()
}
```

这样我们就可以通过调用``model.DB.Self``来使用``*sql.DB``，所以接下来我们要手动封装一些对于数据库增删查改的方法。

```go
// ./model/user.go
package model

// 检查数据库中是否有相同用户名的用户
func CheckUserByUsername(username string) bool {
	userID := 0
    err := DB.Self.QueryRow("SELECT  FROM users WHERE username = ?", username).Scan(&userID)
    return err == nil
}

// 检查用户名密码是否匹配
func CheckPasswordValidate(username, password string) bool {
    var record string
	err := DB.Self.QueryRow("SELECT password FROM users WHERE username = ?", username).Scan(&record)
	return password == record && err == nil
}

func CreateUser(username, password string) {
    DB.Self.Query("INSERT INTO users (username, password) VALUES (?, ?)", username, password)
}
```

在将数据库操作抽离出来之后，我们可以抽离路由中的处理函数。为什么要先抽离数据库操作呢？因为处理函数的内部实现是依赖数据库操作的。

```go
// ./handler/register/register.go

package register

import (
	"github.com/ShiinaOrez/ginny/model"
	"github.com/gin-gonic/gin"
)

type RegisterPayload struct {
	Username  string  `json:"username"`
	Password  string  `json:"password"`
}

func Register(c *gin.Context) {
    var data RegisterPayload
    if err := c.BindJSON(&data); err != nil {
    	c.JSON(400, gin.H{
            "message": "Bad Request!",
    	})
    	return
    }
    if model.CheckUserByUsername(data.Username) {
        c.JSON(401, gin.H{
            "message": "User Already Existed!",
        })
        return
    }
    model.CreateUser(data.Username, data.Password)
    c.JSON(200, gin.H{
        "message": "Register Successful!",
    })
    return
}
```

到这里我们已经把独立的业务逻辑抽离出来了，可以明显的觉得这样去撰写接口的逻辑是更加简洁和便于维护的。

接下来是router。

```
// ./router/router.go

package router

import (
    "github.com/gin-gonic/gin"
    "github.com/ShiinaOrez/ginny/handler/register"
)

var Router *gin.Engine

func InitRouter() {
    Router = gin.Default()
    Router.POST("/register", register.Register)
    return
}
```

我们定义了``Router``这个全局变量，并且定义了初始化函数。到现在我们已经把model, handler, router都抽离出来了，那么我们的``main.go``应该如何撰写呢？

```go
// ./main.go
package main

import (
	"fmt"

    "github.com/ShiinaOrez/ginny/router"
    "github.com/ShiinaOrez/ginny/model"
)

func main() {
	model.DB.Init()        // 初始化数据库
	defer model.DB.Close() // 记得关闭数据库

	router.InitRouter()    // 初始化路由
    router.Router.Run()    // 运行
    fmt.Println("Running... Successful!")
}
```

然后使用以下命令来运行你的程序：

```bash
go build -o main && ./main
```

	提示：可以在本教程的附属仓库 https://github.com/ShiinaOrez/ginny.git 中通过标签``v0.2.0``来查看这个示例。