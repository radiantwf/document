# beego学习笔记

#### 安装
命令行运行：

```
go get -v -u github.com/astaxie/beego
go get -v -u github.com/beego/bee
```

#### 新建项目
命令行运行：

```
cd $GOPATH/src
bee new hello
cd hello
```

#### 调试运行
命令行运行：

```
bee run hello
```
一旦程序开始运行，您就可以在浏览器中打开 http://localhost:8080/ 进行访问。
 
#### 简单事例
```
package main

import (
    "github.com/astaxie/beego"
)

type MainController struct {
    beego.Controller
}

func (this *MainController) Get() {
    this.Ctx.WriteString("hello world")
}

func main() {
    beego.Router("/", &MainController{})
    beego.Run()
}
```

#### bee 工具命令详解

我们在命令行输入 `bee`，可以看到如下的信息：

```
Bee is a tool for managing beego framework.

Usage:

	bee command [arguments]

The commands are:

    new         create an application base on beego framework
    run         run the app which can hot compile
    pack        compress an beego project
    api         create an api application base on beego framework
    bale        packs non-Go files to Go source files
    version     show the bee & beego version
    generate    source code generator
    migrate     run database migrations
```
#### CPU多核使用设置
```
runtime.GOMAXPROCS(runtime.NumCPU())
```
#### 允许CORS跨域访问设置
```
beego.InsertFilter("*", beego.BeforeRouter, cors.Allow(&cors.Options{
    AllowMethods: []string{"GET", "POST", "PUT", "DELETE"},
    AllowOrigins: []string{"*"},
    AllowHeaders: []string{"Origin", "Accept", "Cache-Control", "Pragma", "Content-Type", "Authorization", "X-Auth-Token"},
}))
```
#### 上传文件
```
f, h, err := c.GetFile("file")
if tid != "" && err == nil {
    defer f.Close()
    filename := h.Filename
    ...
}
```
#### 下载文件
```
size := f.Size()
c.Ctx.ResponseWriter.Header().Set("Content-Disposition",
    fmt.Sprintf(`inline; filename="%s"`, escapeQuotes(f.Name())))
c.Ctx.ResponseWriter.Header().Set("Content-Length", strconv.FormatInt(size, 10))
_, err = io.Copy(c.Ctx.ResponseWriter, f)
f.Close()
```
