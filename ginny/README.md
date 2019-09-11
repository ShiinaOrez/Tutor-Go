# ginny - 基于Go的web后台开发实战
# Gin web development: Development Web Backend with Go.

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

## 第2章  以一个简单的实例开始

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
        	return
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