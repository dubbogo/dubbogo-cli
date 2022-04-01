# dubbogo-cli 工具

[toc]

## 1. 安装

dubbogo-cli 是 Apach/dubbo-go 生态的子项目，为开发者提供便利的应用模板创建、工具安装、接口调试等功能，以提高用户的研发效率。

执行以下指令安装dubbogo-cli 至 $GOPATH/bin

```
go install github.com/dubbogo/dubbogo-cli@latest
```

## 2. 功能概览

dubbogo-cli 支持以下能力

- 应用模板创建

  ```
  dubbogo-cli newApp .
  ```

  在当前目录下创建应用模板

- Demo 创建

  ```
  dubbogo-cli newDemo .
  ```

  在当前目录下创建 RPC 示例，包含一个客户端和一个服务端

- 开发相关工具安装

  ```
  dubbogo-cli install all
  ```

  一键安装以下等工具至 $GOPATH/bin

  - protoc

    用于 pb 接口编译

  - protoc-gen-go-triple

    用于 triple 协议接口编译

  - imports-formatter

    用于整理代码 import block

  - grpc_cli 用于调试 triple/grpc 接口

- 快速添加 hessian2 注册方法

- 查看 dubbo-go 应用注册信息

  - 查看 Zookeeper 上面的注册信息, 获取接口及方法列表

    ```bash
    $ dubbogo-cli show --r zookeeper --h 127.0.0.1:2181
    interface: com.dubbogo.pixiu.UserService
    methods: [CreateUser,GetUserByCode,GetUserByName,GetUserByNameAndAge,GetUserTimeout,UpdateUser,UpdateUserByName]
    ```

  - 查看 Nacos 上面的注册信息 【功能开发中】

  - 查看 Istio 的注册信息【功能开发中】

- 调试 Dubbo 协议接口

  - 

- 调试 Triple 协议接口

- 

  

  

## 3. 功能详解

### 3.1 Demo 应用介绍

```shell
.
├── api
│   ├── samples_api.pb.go
│   ├── samples_api.proto
│   └── samples_api_triple.pb.go
├── go-client
│   ├── cmd
│   │   └── client.go
│   └── conf
│       └── dubbogo.yaml
├── go-server
│   ├── cmd
│   │   └── server.go
│   └── conf
│       └── dubbogo.yaml
├── go.mod
└── go.sum
```



### 3.2 应用模板介绍

```
.
├── Makefile
├── api
│   ├── api.pb.go
│   ├── api.proto
│   └── api_triple.pb.go
├── build
│   └── Dockerfile
├── chart
│   ├── app
│   │   ├── Chart.yaml
│   │   ├── templates
│   │   │   ├── _helpers.tpl
│   │   │   ├── deployment.yaml
│   │   │   ├── service.yaml
│   │   │   └── serviceaccount.yaml
│   │   └── values.yaml
│   └── nacos_env
│       ├── Chart.yaml
│       ├── templates
│       │   ├── _helpers.tpl
│       │   ├── deployment.yaml
│       │   └── service.yaml
│       └── values.yaml
├── cmd
│   └── app.go
├── conf
│   └── dubbogo.yaml
├── go.mod
├── go.sum
└── pkg
    └── service
        └── service.go

```



### 3.3 快速添加 hessian2 注册方法 

#### main.go

```go
package main

//go:generate go run "github.com/dubbogo/dubbogo-cli" hessian --include pkg
func main() {

}
```

#### pkg/demo.go

```go
package pkg

type Demo0 struct {
	A string `json:"a"`
	B string `json:"b"`
}

func (*Demo0) JavaClassName() string {
	return "org.apache.dubbo.Demo0"
}

type Demo1 struct {
	C string `json:"c"`
}

func (*Demo1) JavaClassName() string {
	return "org.apache.dubbo.Demo1"
}

```

#### 执行 `go generate`

```shell
go generate
```

#### 日志

```shell
2022/01/28 11:58:11 === Generate start [pkg\demo.go] ===
2022/01/28 11:58:11 === Registry POJO [pkg\demo.go].Demo0 ===
2022/01/28 11:58:11 === Registry POJO [pkg\demo.go].Demo1 ===
2022/01/28 11:58:11 === Generate completed [pkg\demo.go] ===
```

#### 结果

pkg/demo.go

```go
package pkg

import (
	"github.com/apache/dubbo-go-hessian2"
)

type Demo0 struct {
	A string `json:"a"`
	B string `json:"b"`
}

func (*Demo0) JavaClassName() string {
	return "org.apache.dubbo.Demo0"
}

type Demo1 struct {
	C string `json:"c"`
}

func (*Demo1) JavaClassName() string {
	return "org.apache.dubbo.Demo1"
}

func init() {

	hessian.RegisterPOJO(&Demo0{})

	hessian.RegisterPOJO(&Demo1{})

}

```

### 3.4 以 gRPC 协议调试 dubbo-go 应用

### 3.5 以 Dubbo 协议调试 dubbo-go 应用



