# inject学习笔记

#### 官方首页
[转到](https://github.com/facebookgo/inject)

#### 官方文档
[转到](https://godoc.org/github.com/facebookgo/inject)

#### 安装
命令行运行：

```
go get -v -u github.com/facebookgo/inject
```
#### 引入
```
import "github.com/facebookgo/inject"
```
#### 注入
The usage pattern for the library involves struct tags. It requires the tag format used by the various standard libraries, like json, xml etc. It involves tags in one of the three forms below:
```
`inject:""`
`inject:"private"`
`inject:"dev logger"`
```
The first no value syntax is for the common case of a singleton dependency of the associated type. The second triggers creation of a private instance for the associated type. Finally the last form is asking for a named dependency called "dev logger".
#### Example
```
package main

import (
    "fmt"
    "net/http"
    "os"

    "github.com/facebookgo/inject"
)

// Our Awesome Application renders a message using two APIs in our fake
// world.
type HomePlanetRenderApp struct {
    // The tags below indicate to the inject library that these fields are
    // eligible for injection. They do not specify any options, and will
    // result in a singleton instance created for each of the APIs.

    NameAPI   *NameAPI   `inject:""`
    PlanetAPI *PlanetAPI `inject:""`
}

func (a *HomePlanetRenderApp) Render(id uint64) string {
    return fmt.Sprintf(
        "%s is from the planet %s.",
        a.NameAPI.Name(id),
        a.PlanetAPI.Planet(id),
        )
}

// Our fake Name API.
type NameAPI struct {
    // Here and below in PlanetAPI we add the tag to an interface value.
    // This value cannot automatically be created (by definition) and
    // hence must be explicitly provided to the graph.

    HTTPTransport http.RoundTripper `inject:""`
}

func (n *NameAPI) Name(id uint64) string {
    // in the real world we would use f.HTTPTransport and fetch the name
    return "Spock"
}

// Our fake Planet API.
type PlanetAPI struct {
    HTTPTransport http.RoundTripper `inject:""`
}

func (p *PlanetAPI) Planet(id uint64) string {
    // in the real world we would use f.HTTPTransport and fetch the planet
    return "Vulcan"
}

func main() {
    // Typically an application will have exactly one object graph, and
    // you will create it and use it within a main function:
    var g inject.Graph

    // We provide our graph two "seed" objects, one our empty
    // HomePlanetRenderApp instance which we're hoping to get filled out,
    // and second our DefaultTransport to satisfy our HTTPTransport
    // dependency. We have to provide the DefaultTransport because the
    // dependency is defined in terms of the http.RoundTripper interface,
    // and since it is an interface the library cannot create an instance
    // for it. Instead it will use the given DefaultTransport to satisfy
    // the dependency since it implements the interface:
    var a HomePlanetRenderApp
    err := g.Provide(
        &inject.Object{Value: &a},
        &inject.Object{Value: http.DefaultTransport},
        )
    if err != nil {
        fmt.Fprintln(os.Stderr, err)
        os.Exit(1)
    }

    // Here the Populate call is creating instances of NameAPI &
    // PlanetAPI, and setting the HTTPTransport on both to the
    // http.DefaultTransport provided above:
    if err := g.Populate(); err != nil {
        fmt.Fprintln(os.Stderr, err)
        os.Exit(1)
    }

    // There is a shorthand API for the simple case which combines the
    // three calls above is available as inject.Populate:
    //
    //   inject.Populate(&a, http.DefaultTransport)
    //
    // The above API shows the underlying API which also allows the use of
    // named instances for more complex scenarios.

    fmt.Println(a.Render(42))
}
```
#### 注意事项
1、 被注入的对象需要为指针类型

2、 注意变量名首字母大小写