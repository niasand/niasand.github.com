---
title: 学习go之gRPC-gateway
date: 2018-09-09 22:27:17
tags: [grpc]
---

最近在学Go，对我愚笨的我来说，一个知识点，可能要看几百遍才能懂。比如之前看struct和interface，现在还不是特别熟练。然后项目上说要用protobuf，这个压缩效率比json高，对消息系统来说数据传输会更高效。

然后我就去网上找了个例子，写了一遍。这里就再写一遍，以来巩固一下自己所学的，而来给后来者可能有一些帮助。

首先的话要去装protobuf以及go的一些包，这里给出ubuntu的方法，mac的话大同小异，可以自行搜索解决。

```shell

apt-get update && \
apt-get -y install git unzip build-essential autoconf libtool
    
mkdir -p /tmp 
cd /tmp 
git clone https://github.com/google/protobuf 
cd protobuf 
./autogen.sh 
./configure 
make 
make check 
make install

go get github.com/golang/protobuf/proto   // golang的protobuf库文件
go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway  //生成gateway代码的库文件
go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-swagger //生成swagger.json的库文件
go get -u github.com/golang/protobuf/protoc-gen-go  //// 用于根据protobuf生成golang代码，语法 protoc --go_out=. *.proto

```


当你能够在命令行里面敲下protoc弹出它的usage的时候，就证明你安装成功了。

以下是我的文件目录结构，放在 $GOPATH/src目录下。

```
demo
├── client
│   └── main.go
├── demo_proto
│   ├── demo_proto.pb.go
│   ├── demo_proto.pb.gw.go
│   ├── demo_proto.proto
│   └── demo_proto.swagger.json
├── main.go
├── make.sh
└── server
    └── main.go
  
```

client目录：是grpc的客户端。

demo_proto目录：放.proto文件的，剩下的后缀文件都是自动生成出来的。

main.go：是项目主要运行程序，在这里启动restful服务，放在根目录下

make.sh：一些shell脚本

server目录：grpc的服务端。


我们先看demo_proto目录下的proto文件

```proto
syntax = "proto3";

import "google/api/annotations.proto";

package demo_proto;

//定一个一个Hello的服务
service Hello{
    rpc SendHelpInfoWithGetMethod(DemoProtoHelloRequest) returns (DemoProtoHelloResponse){
        option (google.api.http).get ="/get";
    }

    rpc SendInfoWithPostMethod (DemoProtoHelloRequest) returns (DemoProtoHelloResponse){
        option (google.api.http) = {
        post: "/post"
        body: "*"
        };
    }

}


message DemoProtoHelloRequest{
    string name = 1;
}

message DemoProtoHelloResponse {
    string message = 1;
}

```

接下来看client目录下的main.go 文件
line1: syntax = "proto3"; 指定protobuf的版本
line2: 导入一个包,主要是要用到这个包的数据结构，这里包是带回用来生成proto代码需要导入的。
line3: 声明一个包名，一般与文件目录名相同
line4: 定一个Hello的服务，定义好接口名字，方法名，参数以及返回体

根据上述写好的service们，我们就可以用用protoc去把proto文件编译成go代码，生成的代码中包含了客户端能够进行RPC调用的方法和服务端需要实现的接口。具体可见我的make.sh里面的脚本，生成的pb.go文件，给grpc server用的，pb.gw.go文件，给grpc-gateway用的，用于grpc和restful的相互转化，swagger就是给swagger用的，哈哈。

编译demo_proto文件称demo_proto.pb.go文件:

```shell

protoc --go_out=plugins=grpc:. demo_proto.proto


```protoc

make.sh脚本如下:

```shell
proto="demo_proto/demo_proto.proto"

protoc -I/usr/local/include -I. \
        -I$GOPATH/src \
        -I$GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis \
        --go_out=plugins=grpc:. ${proto}

protoc -I/usr/local/include -I. \
        -I$GOPATH/src \
        -I$GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis \
        --grpc-gateway_out=logtostderr=true:. ${proto}
        
protoc -I/usr/local/include -I. \
		-I${GOPATH}/src \
		-I${GOPATH}/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis \
		--swagger_out=logtostderr=true:. ${proto}
```


这羊就在demo_proto目录下生成了demo_proto.pb.go文件

然后咱们在看server下的main.go代码

```go
package main

import (
	"log"
	"net"

	pb "demo/demo_proto"

	"golang.org/x/net/context"
	"google.golang.org/grpc"
	"google.golang.org/grpc/reflection"
)

const (
	port = ":10000"
)

type server struct{}

 
//当接收到请求的时候回调用该方法  参数由grpc自己根据请求进行构造 

func (s *server) SendHelpInfoWithGetMethod(ctx context.Context, in *pb.DemoProtoHelloRequest) (*pb.DemoProtoHelloResponse, error) {
	return &pb.DemoProtoHelloResponse{Message: "接收GET方法：" + in.Name}, nil
}

func (s *server) SendInfoWithPostMethod(ctx context.Context, in *pb.DemoProtoHelloRequest) (*pb.DemoProtoHelloResponse, error) {
	return &pb.DemoProtoHelloResponse{Message: "接收POST方法" + in.Name}, nil
}

func main() {
	lis, err := net.Listen("tcp", port)
	if err != nil {
		log.Fatal("监听端口失败: %v", err)
	}

	s := grpc.NewServer()
	pb.RegisterHelloServer(s, &server{})
	reflection.Register(s)
	if err := s.Serve(lis); err != nil {
		log.Fatal("启动服务失败:%v", err)
	}
}

```

可以看到server端主要就是实现了刚刚我们定义好的Hello服务。一个是SendHelpInfoWithGetMethod，另一个是SendInfoWithPostMethod。

咱们再来看看client中的main.go代码

```go
package main

import (
	pb "demo/demo_proto"
	"log"
	"os"

	"golang.org/x/net/context"
	"google.golang.org/grpc"
)

const (
	address     = "localhost:10000"
	defaultName = "Zhiwei Yang"
)

func main() {
	// 建立一个grpc连接
	conn, err := grpc.Dial(address, grpc.WithInsecure())

	if err != nil {
		log.Fatal("did not connect:%v", err)
	}

	defer conn.Close()
	// 新建一个客户端，方法为：NewXXXClinent(conn),XXX为你在proto定义的服务的名字
	c := pb.NewHelloClient(conn)

	name := defaultName

	if len(os.Args) > 1 {
		name = os.Args[1]
	}

	// 调用远程，并得到返回
	r, err := c.SendHelpInfoWithGetMethod(context.Background(), &pb.DemoProtoHelloRequest{Name: name})
	if err != nil {
		log.Fatal("打不了招呼，错误:%v", err)
	}
	log.Printf("客户端 get方法:%s", r.Message)

	r, err = c.SendInfoWithPostMethod(context.Background(), &pb.DemoProtoHelloRequest{Name: name})

	if err != nil {
		log.Fatal("不能打招呼啦，;%v", err)
	}

	log.Printf("客户端发请求:%s", r.Message)
}

```

client端干的事情是，首先新建了一个grpc连接，然后新建了一个对应服务的客户端，建好这个客户端后，通过这个客户端去调用远程server端已经实现好的服务，如c.SendHelpInfoWithGetMethod和c.SendInfoWithPostMethod 这两个服务。

到这里，我们的grpc的客户端和服务端都写完了。但是我现在特别想通过curl这样的工具来调用服务端的两个服务，咋搞呢？

于是我们就又在根目录下写了一个main.go服务。

```go
package main

import (
	"flag"
	"net/http"
	"path"
	"strings"

	"github.com/golang/glog"

	"google.golang.org/grpc"
	
	gw "demo/demo_proto"
	
	"github.com/grpc-ecosystem/grpc-gateway/runtime"
	"golang.org/x/net/context"
)

const (
	grpcPort = "10000"
)

var (
	getEndPoint = flag.String("get", "localhost:"+grpcPort, "endPoint of your service")
	postPoint   = flag.String("post", "localhost:"+grpcPort, "endpoint of you service")

	swaggerDir = flag.String("swagger_dir", "demo_proto", "含有swagger json文件的路径")
)

func newGateway(ctx context.Context, opts ...runtime.ServeMuxOption) (http.Handler, error) {
	mux := runtime.NewServeMux(opts...)
	dialOpts := []grpc.DialOption{grpc.WithInsecure()}
	err := gw.RegisterHelloHandlerFromEndpoint(ctx, mux, *getEndPoint, dialOpts)

	if err != nil {
		return nil, err
	}

	err = gw.RegisterHelloHandlerFromEndpoint(ctx, mux, *postPoint, dialOpts)
	if err != nil {
		return nil, err
	}
	return mux, err
}

func Run(address string, opts ...runtime.ServeMuxOption) error {
	ctx := context.Background()
	ctx, cancel := context.WithCancel(ctx)

	defer cancel()

	mux := http.NewServeMux()

	mux.HandleFunc("/swagger/", serveSwagger)
	// serverSwaggerUI(mux)
	gw, err := newGateway(ctx, opts...)
	if err != nil {
		return err
	}

	mux.Handle("/", gw)
	return http.ListenAndServe(address, mux)
}

func serveSwagger(w http.ResponseWriter, r *http.Request) {
	if !strings.HasSuffix(r.URL.Path, ".swagger.json") {
		glog.Errorf("Swagger JSON not found: %s", r.URL.Path)
		http.NotFound(w, r)
		return
	}

	glog.Infof("Serving %s", r.URL.Path)
	p := strings.TrimPrefix(r.URL.Path, "/swagger/")
	p = path.Join(*swaggerDir, p)
	http.ServeFile(w, r, p)
}

func serverSwaggerUI(mux *http.ServeMux) {
	fileServer := http.FileServer(&assetfs.AssetFS{
		Asset: swagger.Asset,
		AssetDir: swagger.AssetDir,
		Prefix: "third_party/swagger-ui",
	})

	prefix :="/swagger-ui/"
	mux.Handle(prefix,http.StripPrefix(prefix,fileServer))
}

func main() {
	flag.Parse()
	defer glog.Flush()

	if err := Run(":8080"); err != nil {
		glog.Fatal(err)
	}
}

```
可以看到上面的我们定义好的get和post的endpoint，后面我们会根据这个来调用。

newGateway这个方法就是把我们grpc里面的两个服务注册一下，然后返回一个http的handler。然后我们在run里面用newGateway这个函数，就完成了restful和grpc的相互转化。

serveSwagger这个函数就是用来把swagger的json文件暴露给用户。其实我想在本地实现显示swagger-ui的，不知道为什么，一跑起main.go函数，内存就猛烈增长到10个G，电脑一下就卡死了。先放弃。如果有需要展示，可以把json文件复制到这个网站（`editor.swagger.io`）上去，就可以可视化API了。

main函数里面就是把http server启起来了，端口是8080。

最后，我们只要将server/main.go和demo/main.go 这两个同时启起来，就可以调用了。


```shell

curl -X POST http://127.0.0.1:8080/post -d '{"name":"今天是个好天气，我是jerry"}'


```


![curl -X POST http://127.0.0.1:8080/post -d '{"name":"今天是个好天气，我是jerry"}'](http://jerry-yang.qiniudn.com/2018-09-10FvrOQwPELHN-1Ha9gbqQrsf-pZuc)


```shell

curl -X GET 'http://127.0.0.1:8080/get?name=jerry'


```

![curl -X GET 'http://127.0.0.1:8080/get?name=jerry'](http://jerry-yang.qiniudn.com/2018-09-10FiDkNp7W2_CN4pubPOoKurCaIe9k)


流程如下：curl用post向gateway发送请求，gateway作为proxy将请求转化一下通过grpc转发给Hello的server端，Hello的server端通过grpc返回结果，gateway收到结果后，转化成json返回给前端。