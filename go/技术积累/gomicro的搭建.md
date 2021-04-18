### go　micro的搭建

micro框架的使用
hello world
#### 1.定义服务接口契约
protocol buffer官网
```
syntax = "proto3";
package proto;

service Greeter {
    rpc Hello(HelloRequest) returns (HelloResponse) {}
}

message HelloRequest {
    string name = 1;
}

message HelloResponse {
    string nickname = 1;
}
```
#### 2.生成go代码
* 安装代码生成工具
go get github.com/micro/protobuf/{proto,protoc-gen-go}
* 生成代码
protoc --go_out=plugins=micro:. greeter.proto
#### 3.实现接口契约
```
type Greeter struct{}

func (g *Greeter) Hello(ctx context.Context, req *proto.HelloRequest, rsp *proto.HelloResponse) error {
    rsp.Nickname = "Hello " + req.Name
    return nil
}
```
#### 4.启动服务
```
func main() {
    service := micro.NewService(
        // 设置服务名称
        micro.Name("go.micro.srv.greeter"),
        // 设置服务版本
        micro.Version("v1"),
        // consul验证服务是否健康的轮询时间
        micro.RegisterTTL(time.Second*30),
        // 服务多久更新一次consul中的状态
        micro.RegisterInterval(time.Second*10),
    )
    // 主要是获取启动参数
    service.Init()
    // 注册处理程序
    proto.RegisterGreeterHandler(service.Server(), new(Greeter))
    // 启动后，会启一个协程每隔一段时间更新一次consul中的状态
    if err := service.Run(); err != nil {
        log.Println(err)
    }
}
```
#### 5.客户端调用
```
func main() {
    c := proto.NewGreeterClient("go.micro.srv.greeter", nil)
    resp, err := c.Hello(context.TODO(), &proto.HelloRequest{Name: "JREAM"})
    if err != nil {
        log.Println(err)
        return
    }
    log.Println("result: ", resp.Nickname)
}
```
#### 请求数据包
![示例图](https://github.com/zhangchao1/learnNotes/blob/master/assets/go/grpc_request.png)
#### 一次请求过程
![示例图](https://github.com/zhangchao1/learnNotes/blob/master/assets/go/request_process.png)
#### 主要模块
![示例图](https://github.com/zhangchao1/learnNotes/blob/master/assets/go/main_module.png)
#### service
* registry：服务注册和发现
* transport：服务之间同步通信
* selector：负载均衡
* codec：编解码器
* metadata：类似http中header数据，每个请求中默认会带有context-type、timeout等信息
* broker：服务之间异步通信，现在主要使用场景是调用日志服务（trace）。
![示例图](https://github.com/zhangchao1/learnNotes/blob/master/assets/go/grpc_broker.png)
#### 一些常见使用
* 根据版本号选择服务节点
```
// server.go
type Greeter struct{}

func (g *Greeter) Hello(ctx context.Context, req *proto.HelloRequest, rsp *proto.HelloResponse) error {
    rsp.Nickname = "Hello " + req.Name
    return nil
}

func main() {
    service := micro.NewService(
        micro.Name("go.micro.srv.greeter"),
        micro.Version("v1"),
    )

    service.Init()

    proto.RegisterGreeterHandler(service.Server(), new(Greeter))

    if err := service.Run(); err != nil {
        log.Println(err)
    }
}

// client.go
func main(){
    c := proto.NewGreeterClient("go.micro.srv.greeter", nil)
    resp, err := c.Hello(context.TODO(), &proto.HelloRequest{Name: "JREAM"}, func(opts *client.CallOptions) {
        opts.SelectOptions = append(opts.SelectOptions, selector.WithFilter(selector.FilterVersion("v1")))
    })
    if err != nil {
        log.Println(err)
        return
    }
    log.Println("result: ", resp.Nickname)
}
```
* 根据标签选择服务节点
```
// server.go
type Greeter struct{}

func (g *Greeter) Hello(ctx context.Context, req *proto.HelloRequest, rsp *proto.HelloResponse) error {
    rsp.Nickname = "Hello " + req.Name
    return nil
}

func main() {
    service := micro.NewService(
        micro.Name("go.micro.srv.greeter"),
        micro.Version("v1"),
        // 类似标签，在client端就可以通过指定label来指定请求节点
        micro.Metadata(map[string]string{"test": "test"}),
    )

    service.Init()

    proto.RegisterGreeterHandler(service.Server(), new(Greeter))

    if err := service.Run(); err != nil {
        log.Println(err)
    }
}

// client.go
func main() {
    c := proto.NewGreeterClient("go.micro.srv.greeter", nil)
    resp, err := c.Hello(context.TODO(), &proto.HelloRequest{Name: "JREAM"}, func(opts *client.CallOptions) {
        opts.SelectOptions = append(opts.SelectOptions, selector.WithFilter(selector.FilterLabel("test", "test")))
    })
    if err != nil {
        log.Println(err)
        return
    }
    log.Println("result: ", resp.Nickname)
}
```
* 替换默认broker
```
import(
    // 引入kafka插件
    _ "github.com/micro/go-plugins/broker/kafka"
    "github.com/micro/go-micro"
)

func main(){
    s:=micro.NewService()

    service.Init()

    if err := service.Run(); err != nil {
        log.Println(err)
    }
}
// 启动命令
// go run main.go --broker=kafka --broker_address=127.0.0.1:9200
// MICRO_BROKER=kafka MICRO_BROKER_ADDRESS=127.0.0.1 go run main.go

扩展，例如记录服务之间调用日志(trace)

type Greeter struct{}

func (g *Greeter) Hello(ctx context.Context, req *proto.HelloRequest, rsp *proto.HelloResponse) error {
    rsp.Nickname = "Hello " + req.Name
    return nil
}

func main() {
    service := micro.NewService(
        micro.Name("go.micro.srv.greeter"),
        micro.Version("v1"),
    )

    service.Init(
        // 设置Trace
        // 每次调用其他服务或者接收到请求时都会记录下来
        trace.SetTrace(serviceName, serviceVersion),
    )

    proto.RegisterGreeterHandler(service.Server(), new(Greeter))

    if err := service.Run(); err != nil {
        log.Println(err)
    }
}
```
#### 新增启动参数
```
type Greeter struct{}

func (g *Greeter) Hello(ctx context.Context, req *proto.HelloRequest, rsp *proto.HelloResponse) error {
    rsp.Nickname = "Hello " + req.Name
    return nil
}

func main() {
    service := micro.NewService(
        micro.Name("go.micro.srv.greeter"),
        micro.Version("v1"),
        // 新增启动参数
        micro.Flags(
            cli.StringFlag{
                Name:  "test",
                Usage: "This environment",
            },
        ),
    )

    service.Init(
        // 获取启动参数
        micro.Action(func(c *cli.Context) {
            env := c.String("test")
            if len(env) > 0 {
                log.Println("Environment set to", env)
            }
        }),
    )

    proto.RegisterGreeterHandler(service.Server(), new(Greeter))

    if err := service.Run(); err != nil {
        log.Println(err)
    }
}
```
#### 一些坑
RegisterTTL必须大于RegisterInterval,否则会出现none valiable。如果都不设置，就表示不启用心跳（健康监测）。

#### protoc的安装
https://github.com/google/protobuf/releases下载对应系统的protoc的版本
ubuntu系统：
下载完成以后
解压以后，将bin下的protoc的文件拷贝到/usr/bin/protoc
然后安装protoc-gen-go
使用
go get -u -v github.com/micro/protobuf/{proto,protoc-gen-go}
#### go的微服务的参考文档
go-micro：https://github.com/micro/go-micro
consul:https://www.consul.io/
go consul的库地址：
https://github.com/hashicorp/consul