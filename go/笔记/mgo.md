# mgo学习笔记

#### 官方首页
[转到](http://labix.org/mgo)

#### 官方文档
[转到](https://godoc.org/gopkg.in/mgo.v2)

#### 安装
命令行运行：

    go get -v -u gopkg.in/mgo.v2
#### 引入
    import (
        "gopkg.in/mgo.v2"
        "gopkg.in/mgo.v2/bson"
    )
#### Example
    package main
 
    import (
        "fmt"
        "log"
        "gopkg.in/mgo.v2"
        "gopkg.in/mgo.v2/bson"
    )
 
    type Person struct {
        Name string
        Phone string
    }
 
    func main() {
        session, err := mgo.Dial("server1.example.com,server2.example.com")
        if err != nil {
                panic(err)
        }
        defer session.Close()
 
        // Optional. Switch the session to a monotonic behavior.
        session.SetMode(mgo.Monotonic, true)
 
        c := session.DB("test").C("people")
        err = c.Insert(&Person{"Ale", "+55 53 8116 9639"},
	               &Person{"Cla", "+55 53 8402 8510"})
        if err != nil {
                log.Fatal(err)
        }
 
        result := Person{}
        err = c.Find(bson.M{"name": "Ale"}).One(&result)
        if err != nil {
                log.Fatal(err)
        }
 
        fmt.Println("Phone:", result.Phone)
    }
#### 使用连接池
    var rootSession *mgo.Session
    
    // GetMongoSession 定义
    func GetMongoSession() (session *mgo.Session, err error) {
        if rootSession == nil {
            dbURL := beego.AppConfig.String("mongo")
            rootSession, err = mgo.Dial(dbURL)
            if err != nil {
                return
            }
        }
        session = rootSession.Copy()
        return
    }
#### GridFs
##### 保存文件
	file, err1 := session.UseDB("local").GridFS("fs").Create(filename)
	if err1 != nil {
		err = err1
		return
	}
	fileSize, err1 := io.Copy(file, f)
	if err1 != nil {
		err = err1
		return
	}
	err = file.Close()
##### 获取文件
	f, err1 := session.UseDB("local").GridFS("fs").OpenId(bson.ObjectIdHex(fileID))
##### 删除文件
	f, err1 := session.UseDB("local").GridFS("fs").RemoveId(bson.ObjectIdHex(fileID))
