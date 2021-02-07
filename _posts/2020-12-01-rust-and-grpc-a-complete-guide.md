---
layout: post
title: "러스트 및 gRPC: 완벽한 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/rust-grpc-tonic.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/rust-grpc-tonic.png?fit=730%2C487&ssl=1)

gRPC는 구글이 개발한 오픈 소스 원격 프로시저 콜 시스템이다. gRPC는 데이터 센터 내외부의 통신을 가능하게 하여 모바일, IoT 장치에서 데이터를 효율적으로 전송하고, 백엔드를 하나로 변환한다.

gRPC는 로드 밸런싱, 인증, 추적 등을 위한 플러그형 지원과 함께 제공되며, HTTP/2를 통한 양방향 스트리밍을 지원하며, 10개 언어로 관용적인 구현을 제공한다.

또한 gRPC는 효율적인 클라이언트 라이브러리를 생성할 수 있으며 프로토콜 버퍼 형식을 사용하여 데이터를 와이어를 통해 전송할 수 있다. 프로토콜 버퍼는 데이터 전송을 위한 이진 형식이다. 이 버퍼는 바이너리이므로 프로토콜 버퍼를 빠르게 직렬화할 수 있습니다. 각 메시지의 구조를 미리 정의해야 합니다.

## gRPC 지원(Rust in Rust

러스트 커뮤니티는 많은 gRPC 구현체를 개발했는데, 특히 `토닉`과 `grpc` 크레이트 등이 그렇다. 둘 다 gRPC 프로토콜의 완전한 구현을 제공한다.

### '토닉'이란?

`토닉`은 빠른 속도로 생산 가능한 gRPC 라이브러리로, 즉시 사용 가능한 비동기/대기 기능을 지원합니다. 유연성과 신뢰성에 초점을 맞춥니다. tonic은 HTTP/2를 통해 gRPC 프로토콜을 완전히 구현하고 있으며 tonic은 Rustlang으로 프로토콜 버퍼를 컴파일할 수 있는 지원을 내장하고 있다. 또한 양방향 스트리밍을 지원합니다.

### grpc란?

grpc는 생산 준비가 돼 있지 않지만 눈여겨볼 만하다. 이 상자는 작동하는 gRPC 프로토콜 구현체를 가지고 있으며 TLS를 지원합니다.

톤과 grpc가 동작하는 모습을 보여주기 위해 데모 RPC 앱을 만드는 과정을 살펴보자.

## '토닉'을 이용한 gRPC 앱 구축

먼저 `카고 new grpc-demo-tonic`을 이용한 Rust 프로젝트를 만들겠습니다. gRPC 서버와 클라이언트의 코드를 저장할 src/server.rs과 src/client.rs을 각각 생성한다. 프로토콜 버퍼를 컴파일하기 위해 `build.rs`을 추가할 것이다.

프로토콜 버퍼를 Rust 코드로 컴파일하려면 build.rs과 같은 일부 기본 보일러 플레이트가 필요하다. Cargo.toml과 파일 구조 변경도 필요하다.

`카고.톰`:

```undefined
[package]
name = "grpc-demo-tonic"
version = "0.1.0"
authors = ["anshul <anshulgoel151999@gmail.com>"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]

# server binary
[[bin]]
    name = "server"
    path = "src/server.rs"

# client binary
[[bin]]
    name = "client"
    path = "src/client.rs"
```

파일 구조

```css
├── build.rs
├── Cargo.lock
├── Cargo.toml
├── src
    ├── client.rs
    └── server.rs
```

프로토콜 버퍼 파일을 만드는 것부터 시작합니다.

```cpp
    // version of protocol buffer used
    syntax = "proto3";

    // package name for the buffer will be used later
    package hello;

    // service which can be executed
    service Say {
    // function which can be called
      rpc Send (SayRequest) returns (SayResponse);
    }

    // argument
    message SayRequest {
    // data type and position of data
      string name = 1;
    }

    // return value
    message SayResponse {
    // data type and position of data
      string message = 1;
    }



We can include generated rust code in-app using `tonic`. Let’s create an `hello.rs` file to reuse in both server and client.

    // this would include code generated for package hello from .proto file
    tonic::include_proto!("hello");
```

### '토닉'을 사용하여 gRPC 서버 생성

구조물에 대해 `Say` 특성을 구현하여 서비스를 만듭니다. 서비스에 여러 개의 RPC가 포함될 수 있습니다. 러스트는 비동기식을 지원하지 않기 때문에 우리는 이러한 한계를 극복하기 위해 `asyc_tratit` 매크로를 사용해야 한다.

다음 코드를 `server.rs`에 추가합니다.

```php
    use tonic::{transport::Server, Request, Response, Status};
    use hello::say_server::{Say, SayServer};
    use hello::{SayResponse, SayRequest};
    mod hello; 

    // defining a struct for our service
    #[derive(Default)]
    pub struct MySay {}

    // implementing rpc for service defined in .proto
    #[tonic::async_trait]
    impl Say for MySay {
    // our rpc impelemented as function
        async fn send(&self,request:Request<SayRequest>)->Result<Response<SayResponse>,Status>{
    // returning a response as SayResponse message as defined in .proto
            Ok(Response::new(SayResponse{
    // reading data from request which is awrapper around our SayRequest message defined in .proto
                 message:format!("hello {}",request.get_ref().name),
            }))
        }
    }

    #[tokio::main]
    async fn main() -> Result<(), Box<dyn std::error::Error>> {
    // defining address for our service
        let addr = "[::1]:50051".parse().unwrap();
    // creating a service
        let say = MySay::default();
        println!("Server listening on {}", addr);
    // adding our service to our server.
        Server::builder()
            .add_service(SayServer::new(say))
            .serve(addr)
            .await?;
        Ok(())
    }
```

위의 예에서는 비동기 런타임 및 실행자에 `tokio`를 사용합니다. 마이세이 구조는 세이 서비스를 구현한다. tonic이 제공하는 서버 타입은 서비스를 이용하며 주어진 주소에 gRPC 프로토콜을 지원하는 HTTP 서버를 생성한다.

### '토닉'을 사용하여 gRPC 클라이언트 생성

gRPC는 요청과 응답을 기계적으로 읽을 수 있는 형식으로 정의하기 때문에 클라이언트측 코드를 구현할 필요가 없다. 프로토콜 버퍼 컴파일러에서 생성된 코드는 직접 가져와 사용할 수 있는 클라이언트 코드를 이미 포함하고 있습니다.

```undefined
use hello::say_client::SayClient;
use hello::SayRequest;
mod hello;
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
// creating a channel ie connection to server
    let channel = tonic::transport::Channel::from_static("http://[::1]:50051")
    .connect()
    .await?;
// creating gRPC client from channel
    let mut client = SayClient::new(channel);
// creating a new Request
    let request = tonic::Request::new(
        SayRequest {
           name:String::from("anshul")
        },
    );
// sending request and waiting for response
    let response = client.send(request).await?.into_inner();
    println!("RESPONSE={:?}", response);
    Ok(())
}
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/output-cargo-run-bin-server.png?resize=523%2C139&ssl=1)

소형 애플리케이션을 테스트하려면 `cargo run --bin server`와 `cargo run --bin client`를 차례로 실행하십시오.

## grpc를 이용한 gRPC 앱 구축

먼저 cargo new grpc-demo-grpc로 rust 프로젝트를 만든다. 우리는 `토닉` 데모처럼 서버와 클라이언트라는 두 개의 바이너리를 추가해야 한다. 우리는 또한 `https/hello.dips` 파일과 `build.rs`을 추가해야 한다.

`카고.톰`:

```undefined
[package]
name = "grpc-demo-grpc"
version = "0.1.0"
authors = ["anshul <anshulgoel151999@gmail.com>"]
edition = "2018"

[[bin]]
name="server"
path="./src/server.rs"

[[bin]]
name="client"
path="./src/client.rs"


[dependencies]
protobuf        = "2"
httpbis         = { git = "https://github.com/stepancheg/rust-http2" }
grpc ="*"
grpc-protobuf="*"

[build-dependencies]
protoc-rust-grpc = "0.8.2"
```

build.rs:

`protoc_rust_grpc`는 PATH 변수에 `protoc` 컴파일러가 필요하다. 공식 웹사이트에서 다운로드할 수 있습니다.

```cpp
fn main() {
    // compile protocol buffer using protoc
    protoc_rust_grpc::Codegen::new()
    .out_dir("src")
    .input("./proto/hello.proto")
    .rust_protobuf(true)
    .run()
    .expect("error compiling protocol buffer");
}
```

### grpc를 사용하여 서버 생성

grpc 상자의 API는 tonic 상자의 API와 매우 유사합니다. tonic처럼 grpc는 gRPC 통신을 위한 코드를 생성한다.

서비스 창출을 위해 세이(Say) 특성은 구조 위에서 구현된다. 구현된 struct는 grpc 상자가 제공하는 ServerBuilder의 struct에서 add_server 방식으로 전달됩니다.

```php
use grpc::{ServerHandlerContext,ServerRequestSingle,ServerResponseUnarySink};
// importing generated gRPC code
use hello_grpc::*;
// importing types for messages
use hello::*;
mod hello;
mod hello_grpc;
struct MySay;
impl Say for MySay {
    // rpc for service
    fn send(
        &self,
        _: ServerHandlerContext,
        req: ServerRequestSingle<SayRequest>,
        resp: ServerResponseUnarySink<SayResponse>,
    ) -> grpc::Result<()> {
        // create Response
        let mut r = SayResponse::new();
        let name = if req.message.get_name().is_empty() {
            "world"
        } else {
            req.message.get_name()
        };
        // sent the response
        println!("greeting request from {}", name);
        r.set_message(format!("Hello {}", name));
        resp.finish(r)
    }
}


fn main() {

    let port =50051;
    // creating server
    let mut server = grpc::ServerBuilder::new_plain();
    // adding port to server for http
    server.http.set_port(port);
    // adding say service to server
    server.add_service(SayServer::new_service_def(MySay));
    // running the server
    let _server = server.build().expect("server");
    println!(
        "greeter server started on port {}",
        port,
    );
    // stopping the program from finishing
    loop {
        std::thread::park();
    }
}
```

### grpc를 사용하여 클라이언트 생성

RPC 호출은 클라이언트를 생성하고 데이터를 전송하는 것만큼 간단합니다. 요청을 만들어 클라이언트를 통해 보낸 후 응답을 기다리기만 하면 됩니다.

```php
use std::env;
use std::sync::Arc;

// importing generated gRPC code
use hello_grpc::*;
// importing types for messages
use hello::*;
mod hello;
mod hello_grpc;

use grpc::ClientStub;
use grpc::ClientStubExt;
use futures::executor;
fn main() {

    let name = "anshul";
    let port =50051;
    let client_conf = Default::default();
// create a client
    let client=SayClient::new_plain("::1", port, client_conf).unwrap();
// create request
    let mut req = SayRequest::new();
    req.set_name(name.to_string());
// send the request
    let resp = client
        .send(grpc::RequestOptions::new(), req)
        .join_metadata_result();
// wait for response
    println!("{:?}", executor::block_on(resp));
}
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/rpc-call-response.png?resize=720%2C61&ssl=1)

## 결론

녹은 gRPC에 대한 지원이 뛰어나다. 특히 `토닉`은 빠르고 생산 준비가 가능한 gRPC 구현이다.

이 튜토리얼에서는 `토닉`과 `grpc` 상표를 모두 사용하여 gRPC 앱을 만드는 방법에 대해 배웠다. 우리는 프로토콜 버퍼를 탐색하여 Rust 코드로 컴파일하는 방법을 살펴봤다.

토닉과 grpc 모두 네이티브 tls 상자를 이용한 TLS 기반 인증을 지원한다. 이는 빙산의 일각에 불과하며 `토닉`과 `grpc` 모두 오류 처리, 로드 밸런싱, 인증 등의 거의 모든 기능을 갖추고 있다.

다음은 간단한 시각적 비교입니다.