---
layout: post
title: "하이퍼를 사용하여 Rust에 간단한 웹 서비스 쓰기"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/a-minimal-web-service-in-rust-using-hyper.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/a-minimal-web-service-in-rust-using-hyper.png?fit=730%2C487&ssl=1)

제 경험상 웹 서비스를 구축할 때, 간단한 것이 더 좋습니다. 완전한 기능을 갖춘 헤비급 웹 프레임워크를 가져와서 이를 중단한 후 "작업 완료"에 대한 기존 구성 방식을 사용하는 것이 좋습니다. 많은 개발자들이 이 접근 방식을 성공적으로 사용해 왔습니다. 저를 포함해서요.

하지만 잠시 후 숨겨진 복잡성이 몇 가지 단점이 있다는 것을 알게 되었습니다. 이러한 단점은 성능 저하, 막대한 양의 전이적 종속성 네트워크에서 발생하는 시간 손실 디버깅 문제, 그리고 단순히 어떤 일이 일어나고 있는지 모르는 문제 등입니다. 마찬가지로, 처음부터 모든 것을 쓰는 것은 최선이 아닙니다. 모든 단계에서 오류를 일으킬 수 있는 가능성이 있기 때문에 시간이 매우 많이 걸립니다.

널리 사용되는 오픈 소스 웹 프레임워크가 오류가 없다고 말하는 것은 아닙니다. 전혀 그렇지 않습니다. 하지만 적어도 그들에게는 더 많은 시선이 있고 더 많은 이해당사자들이 적극적으로 버그를 발견하고 고치고 있다.

내 생각에는 행복한 매체가 있다. 목표는 복잡성을 줄이면서 모든 것을 하나의 종속성으로 유지함으로써 얻을 수 있는 대부분의 편의성과 개발 속도를 유지하는 것입니다.

여러분의 경험에 따라, 여러분이 스스로 글을 쓰거나 마이크로 의존성을 사용하는 것이 다양하기 때문에, 이 달콤한 점은 다른 사람들에게 다를 수 있습니다. 그러나 여러 개의 작은 라이브러리를 가져와서 최소한의 시스템을 구축하려는 일반적인 접근 방식은 과거에 제게 잘 작동했습니다. 한 가지 장점은 의존성이 너무 작아서 실제로 제한된 시간 안에 그들의 저장소로 가서 읽고 이해하고, 필요한 경우 수정할 수 있다는 것입니다. 더욱이, 코드베이스에 있는 것(즉, 잘못될 수 있는 것)과 그렇지 않은 것에 대한 더 나은 개요를 얻을 수 있을 것입니다. 큰 프레임워크에서는 어렵습니다. 이를 통해 시스템을 현재 해결 중인 문제에 맞게 조정할 수 있습니다.

작업을 수행하기 위해 프레임워크와 싸울 필요 없이 시스템을 구축하는 데 필요한 기본 수준의 API를 정확하게 정의할 수 있습니다. 이를 위해서는 일정 수준의 경험이 필요합니다. 하지만 틀과 씨름하며 시간을 보낸다면 분명 노력할 가치가 있습니다.

이 튜토리얼에서는 웹 프레임워크를 사용하지 않고 Rust 웹 서비스를 구축하는 방법에 대해 설명합니다.

하지만 모든 것을 처음부터 다시 만들지는 않을 거예요. HTTP 서버의 경우 아래에 있는 tokio runtime을 사용하는 하이퍼를 사용합니다. 이러한 라이브러리 중 어느 것도 가장 가볍거나 최소화된 옵션은 아니지만, 두 라이브러리는 널리 사용되며 여기서 설명하는 개념은 사용된 라이브러리에 관계없이 적용된다.

우리는 그것을 과시하기 위해 다소 유연한 라우팅 API와 몇 가지 샘플 핸들러를 갖춘 기본 웹 서버를 구축할 것이다. 우리의 완제품이 현재 상태로 생산될 준비가 되어 있는 것은 아니지만, 튜토리얼이 끝날 때쯤에는 제품을 확장하여 사용할 수 있는 방법을 분명히 알고 계실 것입니다.

## 세우다

계속하려면 최근 Rust 설치(1.39+)와 cURL과 같은 HTTP 요청을 보내는 도구가 필요합니다.

먼저 새 Rust 프로젝트를 만듭니다.

```bash
cargo new rust-minimal-web-example
cd rust-minimal-web-example
```

다음으로, `Cargo.toml` 파일을 편집하고 다음과 같은 종속성을 추가합니다.

```undefined
[dependencies]
futures = { version = "0.3.6", default-features = false, features = ["async-await"] }
hyper = "0.13"
tokio = { version = "0.2", features = ["macros", "rt-threaded"] }
serde = {version = "1.0", features = ["derive"] }
serde_json = "1.0"
route-recognizer = "0.2"
bytes = "0.5"
async-trait = "0.1"
```

이는 "최소한의" 웹 응용 프로그램에 대한 꽤 많은 의존성입니다. 그게 무슨 일이야?

전체 웹 프레임워크를 사용하는 것이 아니라 여러 개의 마이크로 라이브러리로 구성된 자체 시스템을 구축하려고 하기 때문에 직접적인 종속성의 수가 증가하더라도 전체적인 복잡성은 여전히 낮습니다.

우리가 반드시 걱정하는 직접 의존성의 수가 아니라, 전이적 의존성의 수와 이들을 하나로 묶는 코드의 양입니다.

하이퍼를 비동기 HTTP 서버로 사용하기 때문에 비동기 런타임도 필요합니다. 이 예에서는 tokio를 사용하지만 smol과 같은 경량 솔루션을 사용할 수도 있습니다.

들어오는 JSON을 처리하기 위해서는 serde와 serde_json의 종속성이 필요하다. 우리가 모든 것을 최소화하고 싶다면 나노소데를 사용하는 것 또한 피할 수 있을 것이다. 경로 인식기 상자는 /product/:product_id와 같은 매개 변수가 있는 경로를 처리할 수 있는 매우 작은 경량 라우터입니다.

나머지 라이브러리, 즉 `바이트`, `sync-trit`, `future`의 경우 라우터를 구축하기 위해 필요한 것이다. 그것들은 우리가 이미 사용하고 있는 것에 대한 전이적 의존성일 가능성이 매우 높기 때문에, 그것들은 무게를 더하지 않고 특별히 무겁지도 않습니다. 미래 같은 경우에도 선물용 상자를 사용할 수 있다.

## 핸들러 API

먼저 아래부터 구축한 후 핸들러에 필요한 API를 살펴보겠습니다. 그리고 나서, 우리는 라우터를 구현하고, 마지막에 모든 것을 조립할 것입니다.

이 예에서는 다음 세 가지 처리기를 만듭니다.

- 작동함을 표시하기 위해 문자열을 반환하는 기본 처리기 `GET/test`
- 잘못된 경우 오류를 반환하는 JSON 페이로드가 예상되는 처리기 `POST/send`
- 경로 매개 변수를 처리하는 방법을 보여주는 간단한 핸들러인 `GET/params/:some_param`

handler.rs에 구현하여 원하는 API를 확인해 보겠습니다.

```php
use crate::{Context, Response};
use hyper::StatusCode;
use serde::Deserialize;

pub async fn test_handler(ctx: Context) -> String {
    format!("test called, state_thing was: {}", ctx.state.state_thing)
}

#[derive(Deserialize)]
struct SendRequest {
    name: String,
    active: bool,
}

pub async fn send_handler(mut ctx: Context) -> Response {
    let body: SendRequest = match ctx.body_json().await {
        Ok(v) => v,
        Err(e) => {
            return hyper::Response::builder()
                .status(StatusCode::BAD_REQUEST)
                .body(format!("could not parse JSON: {}", e).into())
                .unwrap()
        }
    };

    Response::new(
        format!(
            "send called with name: {} and active: {}",
            body.name, body.active
        )
        .into(),
    )
}

pub async fn param_handler(ctx: Context) -> String {
    let param = match ctx.params.find("some_param") {
        Some(v) => v,
        None => "empty",
    };
    format!("param called, param was: {}", param)
}
```

test_handler는 매우 간단하지만, 우리는 이미 하나의 중요한 개념인 컨텍스트를 보았다. 핸들러 내에서 요청 상태(본문, 쿼리 매개 변수, 헤더 등)를 사용하려면 해당 상태를 가져올 수 있는 방법이 필요합니다.

또한 시스템을 설계하는 방법에 따라 특정 공유 시스템(HTTP 클라이언트 또는 데이터베이스 저장소)을 처리기에서 사용할 수 있도록 할 수도 있습니다. 이 예에서는 컨텍스트 개체를 사용하여 이러한 항목을 캡슐화합니다. 이 경우 일부 실제 애플리케이션 상태의 더미 버전인 `ctx.state.state_thing` 변수의 콘텐츠를 반환한다. 나중에 구현 방법을 살펴보겠지만 처리기가 작업을 수행하는 데 필요한 모든 정보를 담고 있습니다.

`send_handler`의 경우, 우리는 이미 `Context`가 동작하고 있는 것을 볼 수 있다. 우리는 이 핸들러에 `SendRequest` JSON 페이로드가 예상되므로 `ctx.body_json() 방법을 사용하여 요청 본문을 `Send Request`로 구문 분석합니다. 이 문제가 발생하면 400개의 오류를 반환할 뿐입니다.

test_handler와 send_handler의 흥미로운 차이는 반환 유형이다. 다양한 유형의 값을 반환할 수 있기를 원합니다. 가장 기본적인 경우 문자열 또는

세 번째 핸들러인 `param_handler`는 경로에서 정의된 경로 매개 변수로 가기 위해 컨텍스트를 사용하는 방법을 보여준다. 모든 핸들러는 `sync` 기능이지만 `impl future`를 수작업으로 사용해 핸들러를 만들 수도 있다.

main.rs에서는 컨텍스트 및 일부 도우미 유형에 대한 정의를 추가할 수 있습니다.

```xml
use route_recognizer::Params;

type Response = hyper::Response<hyper::Body>;
type Error = Box<dyn std::error::Error + Send + Sync + 'static>;

#[derive(Clone, Debug)]
pub struct AppState {
    pub state_thing: String,
}

#[derive(Debug)]
pub struct Context {
    pub state: AppState,
    pub req: Request<Body>,
    pub params: Params,
    body_bytes: Option<Bytes>,
}

impl Context {
    pub fn new(state: AppState, req: Request<Body>, params: Params) -> Context {
        Context {
            state,
            req,
            params,
            body_bytes: None,
        }
    }

    pub async fn body_json<T: serde::de::DeserializeOwned>(&mut self) -> Result<T, Error> {
        let body_bytes = match self.body_bytes {
            Some(ref v) => v,
            _ => {
                let body = to_bytes(self.req.body_mut()).await?;
                self.body_bytes = Some(body);
                self.body_bytes.as_ref().expect("body_bytes was set above")
            }
        };
        Ok(serde_json::from_slice(&body_bytes)?)
    }
}
```

여기서는 일부 입력을 피하기 위한 `응답`과 일반 오류 유형인 `오류`를 정의한다. 실제로 사용자 정의 오류 유형을 사용하여 애플리케이션 전체에 오류를 전파하고자 할 수 있습니다.

다음으로, 우리는 앱스테이트 구조를 정의한다. 이 예에서, 이것은 단순히 앞에서 언급한 더미 적용 상태를 유지한다. 예를 들어 이 `AppState`는 캐시, 데이터베이스 저장소 또는 처리기가 액세스할 다른 애플리케이션 전체에 대한 공유 참조를 포함할 수도 있습니다.

컨텍스트 자체는 앱스테이트(AppState), 수신 요청(Request), 경로 파라미터(있는 경우), 본문_바이트(body_bytes)를 포함하는 구조일 뿐이다. body_json이라는 하나의 함수를 표시하는데, 이 함수는 body_bytes를 설정하고 이를 지정된 형식으로 구문 분석하려고 한다.

요청 수명 주기 동안 본문에 여러 번 액세스할 수 있기 때문에 여기서 `body_bytes`를 메모합니다. 이렇게 하면 한 번만 읽고 저장하면 됩니다(예: 미들웨어).

## 라우팅

이제 우리는 우리의 핸들러 API가 어떻게 보여야 하는지를 알았기 때문에, 우리는 그것을 수용할 수 있는 라우팅 메커니즘을 구축해야 한다.

여기에 나타난 접근 방식은 조수 프레임워크가 라우팅에 사용하는 것을 단순화(그리고 더 악화될 가능성이 있는) 버전이다. 경로를 정의할 수 있는 간단한 API가 있으면 좋습니다.

```php
let mut router: Router = Router::new();
router.get("/test", Box::new(handler::test_handler));
router.post("/send", Box::new(handler::send_handler));
router.get("/params/:some_param", Box::new(handler::param_handler));
```

우리는 `라우터`를 만들고 다른 HTTP 방법에 대해 도우미 방법을 사용하여 처리기를 추가한다. 이 예에서는 처리기 함수를 힙 할당의 포인터 유형인 `상자`로 묶는다.

우리는 또 다른 특성을 창조함으로써 이것을 피할 수 있었습니다. 그것은 전화를 건 사람에게서 이 복잡한 부분을 숨기는 것입니다. 하지만 이것은 전체 예를 더 복잡하게 만들었을 것입니다. 그래서 저는 그것을 빠뜨렸습니다.

다음으로 router.rs의 라우터 구현에 대해 알아보자. 먼저 라우터의 정의와 몇 가지 종속성을 살펴보겠습니다.

```php
use crate::{Context, Response};
use async_trait::async_trait;
use futures::future::Future;
use hyper::{Method, StatusCode};
use route_recognizer::{Match, Params, Router as InternalRouter};
use std::collections::HashMap;

pub struct Router {
    method_map: HashMap<Method, InternalRouter<Box<dyn Handler>>>,
}
```

라우터는 내부에 method_map을 포함하는 구조체일 뿐이다. 이는 등록된 경로를 HTTP 방식으로 구분하는 해시맵에 불과하다. 실제로는 fnv와 같이 보다 효율적인 `해시맵` 구현을 사용할 수 있지만, 이는 우리의 예에 적합하다.

맵의 항목은 `route_인식자`를 사용하여 만든 라우터인 `InternalRouter` 유형이며, 각 라우터는 다음에 살펴볼 `BoxboxdynHandler` 유형의 값을 보유한다.

```php
#[async_trait]
pub trait Handler: Send + Sync + 'static {
    async fn invoke(&self, context: Context) -> Response;
}

#[async_trait]
impl<F: Send + Sync + 'static, Fut> Handler for F
where
    F: Fn(Context) -> Fut,
    Fut: Future + Send + 'static,
    Fut::Output: IntoResponse,
{
    async fn invoke(&self, context: Context) -> Response {
        (self)(context).await.into_response()
    }
}
```

이것은 좀 더 복잡하다. 라우터 내부의 처리기 기능을 참조하고 이에 대한 요청 컨텍스트를 전달하기 위해 처리기 특성을 정의합니다.

그런 다음 F는 컨텍스트를 취하고 미래를 반환하는 함수인 F에 대해 핸들러 특성을 구현합니다.

또한 이러한 선물에는 `Into Response`를 실행하는 유형이 있다고 정의한다.

Into Response 특성은 다음과 같습니다.

```php
pub trait IntoResponse: Send + Sized {
    fn into_response(self) -> Response;
}

impl IntoResponse for Response {
    fn into_response(self) -> Response {
        self
    }
}

impl IntoResponse for &'static str {
    fn into_response(self) -> Response {
        Response::new(self.into())
    }
}

impl IntoResponse for String {
    fn into_response(self) -> Response {
        Response::new(self.into())
    }
}
```

이러한 단순한 특성 때문에 취급자로부터 문자열, 정적 str, response를 반환할 수 있다. 여기에 오류를 자동으로 처리하고 반환하는 `Result<T,E`와 같은 임의의 다른 유형을 추가할 수 있다.

그러나 핸들러 특성으로 돌아간다. 이 특성에는 `context`를 처리기에 전달하기 위한 방향성인 `invoke`라는 한 가지 기능만 포함되어 있습니다. 호출 함수는 주어진 컨텍스트를 가진 자기(취급자 함수)를 호출하고 미래를 기다리며 결과를 응답으로 바꾼다.

이를 통해 컨텍스트를 사용하고 응답 대상(IntoResponse)을 라우터에 반환하는 비동기 기능을 추가할 수 있습니다. 나이스

다음으로 라우터 구현에 대해 알아보자.

```php
pub struct RouterMatch<'a> {
    pub handler: &'a dyn Handler,
    pub params: Params,
}


impl Router {
    pub fn new() -> Router {
        Router {
            method_map: HashMap::default(),
        }
    }

    pub fn get(&mut self, path: &str, handler: Box<dyn Handler>) {
        self.method_map
            .entry(Method::GET)
            .or_insert_with(InternalRouter::new)
            .add(path, handler)
    }

    pub fn post(&mut self, path: &str, handler: Box<dyn Handler>) {
        self.method_map
            .entry(Method::POST)
            .or_insert_with(InternalRouter::new)
            .add(path, handler)
    }

    pub fn route(&self, path: &str, method: &Method) -> RouterMatch<'_> {
        if let Some(Match { handler, params }) = self
            .method_map
            .get(method)
            .and_then(|r| r.recognize(path).ok())
        {
            RouterMatch {
                handler: &**handler,
                params,
            }
        } else {
            RouterMatch {
                handler: &not_found_handler,
                params: Params::new(),
            }
        }
    }
}

async fn not_found_handler(_cx: Context) -> Response {
    hyper::Response::builder()
        .status(StatusCode::NOT_FOUND)
        .body("NOT FOUND".into())
        .unwrap()
}
```

new 함수는 method_map이 비어 있는 라우터를 생성한다. 그런 다음 get과 post의 도우미를 추가한다. 간단히 말하면 퍼트, 삭제 등의 도우미는 생략되지만, 그 이상일 뿐이다.

이러한 도우미에서 우리는 `method_map`에 이미 주어진 `Method`에 대한 항목이 있는지 확인하고, 그렇지 않다면 그 안에 새 라우터를 생성한다. 어떤 경우든 경로와 핸들러 기능이 있는 항목은 내부의 라우터에 추가된다.

이렇게 경로를 추가하는데 실제 라우팅은 어떻게 하나요? 여기서 경로 함수가 나온다. 수신 경로 및 요청 `방법`과 함께 호출되고 `라우터 매치`를 반환합니다. `RouterMatch` 구조는 반환된 핸들러 함수와 `route_인식자`가 계산한 경로 매개 변수(있는 경우)에 대한 참조를 보관합니다.

기본적으로 `method_map`은 주어진 HTTP 메소드를 가진 항목을 요청하고 기본 내부 라우터는 주어진 경로를 요청한다. 내부 라우터는 `일치`를 반환합니다. `Match`에는 반환된 후 `RouterMatch`로 포장되는 처리기 및 경로 매개 변수가 포함됩니다. 더

## 모든 것을 종합하다.

라우터가 있고, 컨텍스트가 있으며, 처리기를 사용할 준비가 되었습니다. 이제 모든 것을 함께 연결해서 잘 되길 바라는 일만 남았다.

첫째, main.rs의 주요 기능:

```php
use bytes::Bytes;
use hyper::{
    body::to_bytes,
    service::{make_service_fn, service_fn},
    Body, Request, Server,
};
use route_recognizer::Params;
use router::Router;
use std::sync::Arc;

mod handler;
mod router;

#[tokio::main]
async fn main() {
    let some_state = "state".to_string();

    let mut router: Router = Router::new();
    router.get("/test", Box::new(handler::test_handler));
    router.post("/send", Box::new(handler::send_handler));
    router.get("/params/:some_param", Box::new(handler::param_handler));

    let shared_router = Arc::new(router);
    let new_service = make_service_fn(move |_| {
        let app_state = AppState {
            state_thing: some_state.clone(),
        };

        let router_capture = shared_router.clone();
        async {
            Ok::<_, Error>(service_fn(move |req| {
                route(router_capture.clone(), req, app_state.clone())
            }))
        }
    });

    let addr = "0.0.0.0:8080".parse().expect("address creation works");
    let server = Server::bind(&addr).serve(new_service);
    println!("Listening on http://{}", addr);
    let _ = server.await;
}
```

우리는 `라우터`를 만들고 몇 가지 경로를 추가한다. 그런 다음, 우리는 스레드 간에 공유할 수 있는 스마트 포인터인 `Arc`의 내부에 `라우터`를 넣고 `하이퍼`가 제공하는 `make_service_fn` 함수를 사용하여 수신 요청에 대해 어떤 일이 일어나야 하는지를 정의한다.

서비스 폐쇄 내부에서 미리 정의된 더미 문자열을 복제하는 `AppState`를 생성한다. 우리는 또한 공유 라우터를 `sync` 블록으로 전달하기 전에 이 폐쇄 안에서 복제해야 한다. 이는 `sync` 블록의 내부가 나중에 여러 가지 다른 스레드에서 실행될 수 있고 실행될 것이기 때문에 우리가 주는 모든 것이 충분히 오래 지속될 수 있도록 해야 하기 때문이다.

sync 블록 내에서 service_fn을 사용하여 들어오는 모든 요청에 대해 어떤 작업이 수행되어야 하는지를 정의합니다. 우리는 폐쇄를 제공하고, 라우터 및 상태를 그 안에서 이동하며, 더 아래로 내려가서 라우터, 수신 요청 및 애플리케이션 상태를 살펴볼 `route()` 함수를 호출한다.

그 후에는 서버에게 시작할 포트를 알려주고 실제로 시작할 수 있는 하이퍼(hyper) 보일러 플레이트만 남게 된다.

마지막으로 살펴볼 부분은 경로() 함수이다.

```js
async fn route(
    router: Arc<Router>,
    req: Request<hyper::Body>,
    app_state: AppState,
) -> Result<Response, Error> {
    let found_handler = router.route(req.uri().path(), req.method());
    let resp = found_handler
        .handler
        .invoke(Context::new(app_state, req, found_handler.params))
        .await;
    Ok(resp)
}
```

여기서 우리는 수신 요청에서 핸들러 함수로 이동합니다. 우리는 라우터의 `route` 기능을 요청 경로 및 방법과 함께 사용하여 `라우터 매치`로 이동합니다. 그런 다음 반환된 처리기에서 .invoke()를 호출하여 응용 프로그램 상태, 요청, 라우터 매치에서 params를 포함하는 새로운 컨텍스트 객체를 부여한다. 우리는 처리자의 장래를 기다리고 반응을 돌려준다.

간단한 경로 기능이지만 여기에 CORS 미들웨어나 어떤 종류의 미들웨어를 추가하는 것은 상상할 수 있다.
— 예를 들어 로깅과 같은 기능을 사용할 수 있습니다. 여기서 권한 부여 처리를 수행하여 수신되는 `권한 부여` 헤더를 확인하고 호출자가 요청된 리소스에 액세스할 수 있는지 확인할 수도 있습니다.

다 됐다! 더 이상 어쩔 수 없다! cargo run을 실행하고 curl을 사용하여 요청을 보내는 방식으로 작동되는지 살펴보겠습니다.

```undefined
curl http://localhost:8080/test
test called, state_thing was: state

curl http://localhost:8080/params/1234
param called, param was: 1234

curl -X POST http://localhost:8080/send -d '{"name": "chip", "active": true }'
send called with name: chip and active: true

curl -X POST http://localhost:8080/send -d '{"name": fsdfds, "active": true }'
HTTP/1.1 400 Bad Request
could not parse JSON: expected ident at line 1 column 11
```

아주 멋져요! 전체 예제 코드는 GitHub에서 찾을 수 있습니다.

## 결론

제 생각에는 단순성은 웹 서비스와 소프트웨어를 일반적으로 구축하는 데 있어 핵심적인 가치라고 생각합니다. 코드베이스가 클수록, 더 많은 소프트웨어 패키지에 의존할수록, 그 시스템의 본질적이고 부수적인 복잡성을 다루기가 어렵다. 따라서 성능이 저하되고 전혀 보지 못한 미묘한 버그가 발생할 수 있습니다. 대부분의 경우 일반 용도로 최적화된 도구로 특정 작업을 시도하게 될 수 있습니다.

좋은 곳을 찾는 데는 실험과 용기가 필요할 것이다. 이 모든 것은 여러분 자신(또는 여러분의 팀)이 수용하고자 하는 복잡성 수준과 구현하려는 프로젝트에 달려 있습니다.

당신이 직접 모든 것을 만들 필요는 없어요. 실제로, 일부 부분, 특히 암호화 라이브러리와 같은 보안상 중요한 부분에서는 항상 배틀 테스트 솔루션을 사용해야 합니다.

작은 경량 라이브러리와 약간의 자체 쓰기 코드를 사용하여 최소(또는 그 근처에)로 간주될 수 있는 시스템을 구성하면 성능, 유지보수성 및 코드 품질을 개선하는 데 도움이 될 수 있다. 저는 이 방법을 적극 추천하며 더 많은 개발자들이 이 방법을 채용하기를 원합니다.