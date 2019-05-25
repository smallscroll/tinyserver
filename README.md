# tinyserver
GO并发服务器框架

Demo：

- demoServer.go

- config
  - tinyserver.json



*demoServe.go*

```go
/*
	tinyserver demo
*/

package main

import (
	"fmt"
	"tinyserver/tsinterface"
	"tinyserver/tsnet"
)

//客户端消息ID为1时对应的路由:

//自定义一个路由继承自BaseRouter
type hiRouter struct {
	tsnet.BaseRouter
}

//通过自定义路由进行重写以实现业务方法
func (rp *hiRouter) Handler(request tsinterface.IRequest) {
	err := request.GetConnection().Send(100, []byte("重写后的Handler... Hi~"))
	if err != nil {
		fmt.Println(err)
	}
}

//客户端消息ID为2时对应的路由:

//自定义一个路由继承自BaseRouter
type helloRouter struct {
	tsnet.BaseRouter
}

//通过自定义路由进行重写以实现业务方法
func (rp *helloRouter) Handler(request tsinterface.IRequest) {
	err := request.GetConnection().Send(200, []byte("重写后的Handler... Hello~"))
	if err != nil {
		fmt.Println(err)
	}
}

func main() {
	//创建一个server
	s := tsnet.NewServer("demo")

	//添加自定义路由：客户端发送不同的消息，服务器根据消息处理不同的业务
	s.AddRouter(1, &hiRouter{})    //发送消息ID为1时对应的路由
	s.AddRouter(2, &helloRouter{}) //发送消息ID为2时对应的路由

	//让server对象启动服务
	s.Serve()
}

```

