---
layout: post
title: "Rust 웹 서비스의 JSON 입력 유효성 검사에 대한 손쉬운 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/json-input-validation-in-rust-web-services.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/json-input-validation-in-rust-web-services.png?fit=730%2C487&ssl=1)

다소 복잡한 도메인 객체로 웹 서비스를 구축할 때는 입력 유효성 검사가 가장 중요하다. 사용자 입력의 검증은 보안뿐만 아니라 외부 입력도 신뢰하지 않으며 사용성 측면에서도 기본적으로 중요하다.

REST 끝점의 호출자가 오류가 발생하면 단순히 `400 불량 요청`을 표시하는 대신 무엇이 잘못되었는지 알려줄 수 있습니다. 최적의 입력 오류 처리는 잘못된 JSON이 발생할 경우 사용자에게 요청 페이로드에서 오류가 어디에 있는지 대략적으로 알려줄 수 있는 방식으로 수행됩니다.

요청 페이로드가 유효한 JSON인 경우 다음 단계는 해당 페이로드가 규격을 준수하는지 확인하는 것입니다. Rust는 환상적인 서드드 상자로, 잘못된 데이터 유형을 사용할 경우 구조물에 대한 역직렬화가 실패하기 때문에 여기에 도움이 됩니다. 다시 말하지만, 이러한 경우 사용자에게 이름이란 단순히 `JSON 구문 분석 오류`를 회신하는 대신 `개체`가 아니라 `문자열`이어야 한다고 말해야 합니다.

하지만 우리는 아직 완전히 끝나지 않았다. 수신 JSON이 제대로 검증되고 구조물에 구문 분석되면 비즈니스 논리에 따라 자체 검증이 시작됩니다. 예를 들어 `e-mail`이 실제로 규격에 따른 이메일인지, 또는 `username`에 금지된 문자가 포함되어 있지 않은지 등을 확인할 수 있습니다. 이 부분은 다른 부분보다 다루기 쉽지만, 종종 작고 오류가 발생하기 쉬운 유효성 검사 함수가 많이 발생하는데, 이 함수는 결합하고 증류하기 어려우며 명확하고 좋은 오류 메시지로 변환하기 어렵다.

이 튜토리얼에서는 Warp 웹 서비스 내에서 Rust에서 이러한 문제를 해결하는 방법에 대해 설명합니다. NAT 솔루션은 코드의 유지 보수성을 훼손하지 않고 직관적이고 사용하기 편리합니다.

시작해 봅시다!

## 세우다

따라서 최신 Rust 설치(1.39+)와 cURL과 같은 HTTP 요청을 만드는 도구만 있으면 됩니다.

먼저 새 Rust 프로젝트를 만듭니다.

```bash
cargo new rust-json-validation-example
cd rust-json-validation-example
```

그런 다음 `Cargo.toml` 파일을 편집하고 필요한 종속성을 추가합니다.

```undefined
tokio = { version = "0.2", features = ["macros", "sync", "rt-threaded"] }
warp = "0.2"
serde = {version = "1.0", features = ["derive"] }
serde_json = "1.0"
validator = "0.10"
validator_derive = "0.10"
serde_path_to_error = "0.1"
thiserror = "1.0.20"
bytes = "0.5.6"
```

들어오는 본문을 역직렬화하려면 웹 서버 및 서드에 Warp 및 Tokio가 필요합니다. `serde_path_to_error` 라이브러리는 유효성 검사를 개선하기 위한 첫 번째 목적지가 되며, `validator` 및 `validator_derive` 상자는 데이터 유효성 검사에 나중에 도움이 됩니다.

## JSON 차체 유효성 검사

잘못된 JSON 또는 성공적으로 역직렬화할 수 없는 항목을 보낼 때 뒤틀린 기본 동작을 시연하려면 단일 경로를 사용하여 작은 웹 서버를 생성해 보십시오.

```undefined
type Result<T> = std::result::Result<T, Rejection>;

#[derive(Deserialize, Debug)]
struct CreateRequest {
    pub email: String,
    pub address: Address,
    pub pets: Vec<Pet>,
}

#[derive(Deserialize, Debug)]
struct Address {
    pub street: String,
    pub street_no: usize,
}

#[derive(Deserialize, Serialize, Debug)]
struct Pet {
    pub name: String,
}

#[tokio::main]
async fn main() {
    let basic = warp::path!("create-basic")
        .and(warp::post())
        .and(warp::body::json())
        .and_then(create_handler);

    let routes = basic
        .recover(handle_rejection);

    println!("Server started at localhost:8080!");
    warp::serve(routes).run(([127, 0, 0, 1], 8080)).await;
}

async fn create_handler(body: CreateRequest) -> Result<impl Reply> {
    Ok(format!("called with: {:?}", body))
}

#[derive(Serialize)]
struct ErrorResponse {
    message: String,
    errors: Option<Vec<FieldError>>,
}

#[derive(Serialize)]
struct FieldError {
    field: String,
    field_errors: Vec<String>,
}

pub async fn handle_rejection(err: Rejection) -> std::result::Result<impl Reply, Infallible> {
    let (code, message, errors) = if err.is_not_found() {
        (StatusCode::NOT_FOUND, "Not Found".to_string(), None)
    } else if let Some(e) = err.find::<warp::filters::body::BodyDeserializeError>() {
        (
            StatusCode::BAD_REQUEST,
            e.source()
                .map(|cause| cause.to_string())
                .unwrap_or_else(|| "BAD_REQUEST".to_string()),
            None,
        )
    } else {
        eprintln!("unhandled error: {:?}", err);
        (
            StatusCode::INTERNAL_SERVER_ERROR,
            "Internal Server Error".to_string(),
            None,
        )
    };

    let json = warp::reply::json(&ErrorResponse {
        message: message.into(),
        errors,
    });
    Ok(warp::reply::with_status(json, code))
}
```

이를 통해 요청 객체를 만들었습니다. 이 경우 e-메일, 주소, 애완동물 목록과 같은 일부 필드만 검증할 수 있습니다.

그리고 나서, 우리는 최소의 핸들러를 만들었습니다. 이것은 JSON의 몸체로 받아들여지고, 호출되면, 인쇄합니다.

그 아래에서, 우리는 `handle_reject` 도우미 내에서 몇 가지 기본적인 오류 처리를 정의했다. 이것은 오류에 대처하는 워프의 방식이며, 워프의 용어로 `거부`라고 불린다. 지금은 `필드 오류`를 무시할 수 있습니다. 나중에 데이터 유효성 검사를 수행할 때 다시 방문하겠습니다.

또한 본문 역직렬화 중에 발생하는 오류에 대한 사전 정의된 사례가 있습니다: `warp::filters::body::본문 역직렬화 오류`입니다. 이와 같은 오류가 발생하면 오류의 원인이 문자열로 변환된 400개의 오류를 반환합니다.

발신자 쪽에서 본 결과는 이렇습니다. cURL을 사용하여 잘못된 페이로드를 전송합시다.

```undefined
curl -X POST http://localhost:8080/create-basic -H "Content-Type: application/json" -d '{ "email": 1, "address": { "street": "warpstreet", "street_no": 1 }, "pets": [{ "name": "nacho" }] }'
```

다음과 같은 오류가 발생합니다.

```undefined
{"message":"invalid type: integer `1`, expected a string at line 1 column 13","errors":null}
```

최소한 오류가 발생한 행과 열을 알려주지만(이메일은 잘못된 페이로드의 숫자임) `이메일`이 문제였다는 것을 실제로 알려주는 것이 훨씬 좋을 것이다.

어떤 경우에도 `BodyDeserialize Error` 처리기가 없는 것보다는 훨씬 낫다.i이 경우 응답은 다음과 같다.

```undefined
{"message":"Internal Server Error","errors":null}
```

## JSON 검증 개선

오류를 개선하기 위해 이름에서 알 수 있듯이 serde_path_to_error` 상자를 사용하여 serde 오류를 발생한 JSON의 경로로 변환합니다.

다른 처리기를 만들어 어떤 변경 사항이 있는지 살펴보겠습니다.

```undefined
#[tokio::main]
async fn main() {
...
    let basic_path = warp::path!("create-path")
        .and(warp::post())
        .and(warp::body::aggregate())
        .and_then(create_handler_path);

    let routes = basic
        .or(basic_path)
        .recover(handle_rejection);
...
}

async fn create_handler_path(buf: impl Buf) -> Result<impl Reply> {
    let des = &mut serde_json::Deserializer::from_reader(buf.reader());
    let body: CreateRequest = serde_path_to_error::deserialize(des)
        .map_err(|e| reject::custom(Error::JSONPathError(e.to_string())))?;
    Ok(format!("called with: {:?}", body))
}

#[derive(Error, Debug)]
enum Error {
    #[error("JSON path error: {0}")]
    JSONPathError(String),
}

impl warp::reject::Reject for Error {}

pub async fn handle_rejection(err: Rejection) -> std::result::Result<impl Reply, Infallible> {
...
    } else if let Some(e) = err.find::<Error>() {
        match e {
            Error::JSONPathError(_) => (StatusCode::BAD_REQUEST, e.to_string(), None),
        }
...
}
```

보시는 것처럼 `create-path` 경로에 다른 처리기를 추가했습니다. 여기서 가장 큰 차이점은 "warp::body:::json()" 대신 "warp::body::aggregate()"를 사용했다는 것인데, 이것은 우리가 개입할 기회를 갖기 전에 자동으로 페이로드와 오류를 역직렬화했을 것이다.

이 경우 페이로드의 집계된 바이트 버퍼가 작동하기를 원했다. 실제로 이 패턴을 많이 재사용한다면 사용자 정의 `json()` 필터를 직접 작성할 수 있습니다.

처리기에서 우리는 deserializer를 만들고 `serde_path_to_error::deserialize` 함수를 사용하여 수신 JSON 버퍼를 `CreateRequest` 구조로 역직렬화했다. 이 작업이 실패하면 아래에 정의된 Warp의 Reject 특성을 구현하는 사용자 정의 오류 유형인 JSONPathError를 트리거합니다. 이것은 사용자 정의 오류 처리를 수행하는 간단한 방법입니다.

지금 어떤 반응이 나오는지 한번 볼까요? 우리는 이전과 같은 화물을 보낼 것입니다.

```undefined
{"message":"JSON path error: email: invalid type: integer `1`, expected a string at line 1 column 13","errors":null}
```

만약 애완동물의 이름과 같은 구조 아래쪽에 오류가 있다면요?

```undefined
curl -X POST http://localhost:8080/create-path -H "Content-Type: application/json" -d '{ "email": "mario@zupzup.org", "address": { "street": "warpstreet", "street_no": 1 }, "pets": [{ "name": 1 }] }'
```

이 경우 다음과 같은 오류가 발생합니다.

```undefined
{"message":"JSON path error: pets[0].name: invalid type: integer `1`, expected a string at line 1 column 107","errors":null}
```

훨씬 낫네요. 이제 이 오류는 호출자에게 어떤 필드가 문제인지 알려주며, 문제를 해결하는 방법에 대한 힌트도 제공합니다. 역직렬화 오류 처리 측면에서 볼 때, 우리가 더 잘 할 수 있을 것 같지는 않다.

한 가지 다른 접근 방식은 예를 들어 GUI가 응답을 통해 깨끗한 오류 메시지를 만들 수 있도록 파싱하기 쉬운 데이터 구조에 응답을 넣는 것입니다. 하지만 우리는 잘못된 요청을 바로잡는 것에 대해 이야기 하고 있기 때문에, 이 경우 이것은 그만큼 중요하지 않습니다. 데이터 검증 단계에 이르면 관련성이 커집니다.

## 데이터 유효성 검사

이 경우 데이터 유효성 검사는 제공된 데이터가 e-메일 또는 url이 유효하거나 https:// URL만 허용하려는 값 등 어떤 값의 사양을 준수하는지 확인하는 것을 의미합니다.

이를 위해 각 필드와 들어오는 각 구조에 대한 검증 함수를 작성하기만 하면 됩니다. 하지만 이는 지루해집니다. 또한 더 큰 앱에서 다양한 복잡한 도메인 개체와 결합하기 어려우며, 코드와 특히 오류가 발생하기 쉬운 논리 복제로 이어집니다.

Rust 생태계에 있는 다른 라이브러리들 중에서, 검증자 상자는 여기에서 큰 도움이 된다. 이 매크로 기반 라이브러리를 사용하면 구조체 및 필드에 대한 검증 규칙을 선언적으로 정의할 수 있습니다. 이러한 접근 방식은 마시멜로, 자바 스프링 등과 같은 다른 언어로 된 많은 라이브러리들이 있기 때문에 친숙하게 들릴 수 있다.

실제로 어떻게 작동하는지 살펴보겠습니다.

```undefined
use validator::{Validate, ValidationErrors, ValidationErrorsKind};

#[derive(Deserialize, Debug, Validate)]
struct CreateRequest {
    #[validate(email)]
    pub email: String,
    #[validate]
    pub address: Address,
    #[validate]
    pub pets: Vec<Pet>,
}

#[derive(Deserialize, Debug, Validate)]
struct Address {
    #[validate(length(min = 2, max = 10))]
    pub street: String,
    #[validate(range(min = 1))]
    pub street_no: usize,
}

#[derive(Deserialize, Serialize, Debug, Validate)]
struct Pet {
    #[validate(length(min = 3, max = 20))]
    pub name: String,
}

#[tokio::main]
async fn main() {
...
    let basic_path_validator = warp::path!("create-validator")
        .and(warp::post())
        .and(warp::body::aggregate())
        .and_then(create_handler_validator);

    let routes = basic
        .or(basic_path)
        .or(basic_path_validator)
        .recover(handle_rejection);
...
}

async fn create_handler_validator(buf: impl Buf) -> Result<impl Reply> {
    let des = &mut serde_json::Deserializer::from_reader(buf.reader());
    let body: CreateRequest = serde_path_to_error::deserialize(des)
        .map_err(|e| reject::custom(Error::JSONPathError(e.to_string())))?;

    body.validate()
        .map_err(|e| reject::custom(Error::ValidationError(e)))?;
    Ok(format!("called with: {:?}", body))
}

#[derive(Error, Debug)]
enum Error {
    #[error("JSON path error: {0}")]
    JSONPathError(String),
    #[error("validation error: {0}")]
    ValidationError(ValidationErrors),
}

...
```

우리는 또 다른 핸들러인 `create-validator`를 추가했는데, 이것은 우리가 `create-path`에 사용했던 것과 동일한 접근 방식을 사용한다. 그래서 우리는 멋진 JSON 입력 처리를 확립한 다음 들어오는 데이터의 유효성을 확인하기 위해 `body.validate()`라고 부르며, 오류가 있는 새로운 사용자 정의 오류를 전파했다.

그러나 실제 소스는 데이터 구조의 정의에 있습니다. 거기서 우리는 구조물에 대한 검증 특성을 도출하고 각 분야별로 검증 매크로를 사용하여 이 필드의 검증 여부 및 방법에 대한 규칙을 정의했다. 전자 메일 검증, 문자열 길이와 같은 기본 사항 또는 사용자 지정 유효성 검사 기능과 같은 고급 사항일 수 있습니다. 더 많은 예제를 보려면 설명서를 참조하십시오. 이 문서는 매우 강력하고 사용자 정의가 가능합니다.

위의 예에서는 `Validation Error`에 대한 오류 처리가 조금 복잡하기 때문에 생략되었습니다. 유효성 검사기 상자에서 얻은 `Validation Errors` 개체는 우리가 원하는 모든 정보를 포함하고 있으며 문자열로도 다소 유용한 오류를 제공할 수 있습니다. 그러나 우리의 경우 오류를 발생된 필드에 매핑할 수 있는 사용자 정의 필드 오류 구조로 분석하고자 합니다.

이것을 성취하기 위한 기본적인 방법을 살펴봅시다.

```undefined
pub async fn handle_rejection(err: Rejection) -> std::result::Result<impl Reply, Infallible> {
...
    } else if let Some(e) = err.find::<Error>() {
        match e {
            Error::JSONPathError(_) => (StatusCode::BAD_REQUEST, e.to_string(), None),
            Error::ValidationError(val_errs) => {
                let errors: Vec<FieldError> = val_errs
                    .errors()
                    .iter()
                    .map(|error_kind| FieldError {
                        field: error_kind.0.to_string(),
                        field_errors: match error_kind.1 {
                            ValidationErrorsKind::Struct(struct_err) => {
                                validation_errs_to_str_vec(struct_err)
                            }
                            ValidationErrorsKind::Field(field_errs) => field_errs
                                .iter()
                                .map(|fe| format!("{}: {:?}", fe.code, fe.params))
                                .collect(),
                            ValidationErrorsKind::List(vec_errs) => vec_errs
                                .iter()
                                .map(|ve| {
                                    format!(
                                        "{}: {:?}",
                                        ve.0,
                                        validation_errs_to_str_vec(ve.1).join(" | "),
                                    )
                                })
                                .collect(),
                        },
                    })
                    .collect();

                (
                    StatusCode::BAD_REQUEST,
                    "field errors".to_string(),
                    Some(errors),
                )
            }
        }
...
}

fn validation_errs_to_str_vec(ve: &ValidationErrors) -> Vec<String> {
    ve.field_errors()
        .iter()
        .map(|fe| {
            format!(
                "{}: errors: {}",
                fe.0,
                fe.1.iter()
                    .map(|ve| format!("{}: {:?}", ve.code, ve.params))
                    .collect::<Vec<String>>()
                    .join(", ")
            )
        })
        .collect()
}
```

보시다시피, 이것은 여러분이 기대했던 것보다 조금 더 복잡합니다. 이론적으로, 임의적으로 깊이 내포된 구조의 경우, 결과적인 필드 오류 구조를 임의로 깊게 내포할 수도 있습니다. 결단을 내려 컷오프가 1등급이라고 규정할 필요가 있다. 깊이 `1`을 넘어 기본 필드 오류의 전체 문자열을 맨 위 필드 오류에 추가하기만 하면 된다.

실제로 재귀로 이 문제를 해결할 수 있지만, 여기서는 단순하게 처리하겠습니다.

여기 무슨 일이 일어나고 있는지 코드를 살펴봅시다. 위에서 언급한 바와 같이 라이브러리에서 ValidationErrors 개체를 가져오는데, 이를 Vec<FieldError>로 구문 분석하고자 한다. 지금까지, 너무 좋아.

오류() 반복기를 사용하여 `Validation Errors`를 반복하면 `Validation ErrorKinds` 컬렉션이 나온다. 다음과 같은 세 가지 오류 유형이 있습니다.

- 필드란 `e-mail`과 같은 필드에서 발생한 오류를 의미합니다. 쉬움
- 구조체란 내포 구조체에서 발생한 오류를 의미합니다.
- 리스트는 중첩된 리스트에서 오류가 발생했음을 의미합니다.

각 오류 종류에 대해 필드 이름과 실제 오류가 두 개씩 표시됩니다. 필드 이름은 필드 에러에서 필드 속성에 간단히 입력할 수 있습니다. `이왕이면 `이래.

필드 오류가 좀 더 심합니다. 우리는 위에서 언급한 세 가지 사례와 일치시킬 필요가 있다. `Validation ErrorsKind::필드(Field)는 다소 단순하며 오류를 문자열로 포맷할 뿐입니다.

`Validation ErrorsKind::Struct, 우리는 이 구조 내의 모든 오류를 반복하여 문자열로 포맷하고 구조 내에 이러한 관련 오류 목록을 반환하는 `validation_errs_to_str_vec` 도우미를 부른다.

마지막 경우 `ValidationErrorsKind::List, 리스트의 모든 항목도 다시 구조가 될 수 있기 때문에 우리는 이 둘을 조합하여 각 필드에 대해 `validation_errs_to_str_vec`라고 부르며, 오류가 발생한 리스트 인덱스가 있는 문자열에 태그를 지정합니다.

이 마지막 부분을 따라가는데 어려움이 있었다면, 너무 걱정하지 마세요. 실제로 반환된 유효성 검사 오류는 상황에 따라 다른 방식으로 구문 분석되고 다시 전달됩니다. 이것은 단지 그것을 하는 하나의 단순하고, 인정하건대, 불완전한 방법이다.

즉, 데이터 유효성 검사 규칙을 위반할 경우 어떤 오류가 발생하는지 살펴보겠습니다.

먼저 잘못된 전자 메일에 대해 알아보겠습니다.

```undefined
curl -X POST http://localhost:8080/create-validator -H "Content-Type: application/json" -d '{ "email": "chipexample.com", "address": { "street": "warpstreet", "street_no": 1 }, "pets": [{ "name": "nacho" }] }'
```

=>

```undefined
{"message":"field errors","errors":[{"field":"email","field_errors":["email: {\"value\": String(\"chipexample.com\")}"]}]}
```

오류가 발생하고 필드가 올바르게 설정되며 필드 오류에는 잘못된 값을 표시하는 잘못된 전자 메일 오류도 포함됩니다.

이제 여기에 또 다른 오류를 추가하고 규칙에 맞지 않는 너무 긴 거리 이름을 사용해 봅시다.

```undefined
curl -X POST http://localhost:8080/create-validator -H "Content-Type: application/json" -d '{ "email": "chipexample.com", "address": { "street": "warpstreetistoolonghere", "street_no": 1 }, "pets": [{ "name": "nacho" }] }'
```

=>

```bash
{"message":"field errors","errors":[{"field":"address","field_errors":["street: errors: length: {\"max\": Number(10), \"min\": Number(2), \"value\": String(\"warpstreetistoolonghere\")}"]},{"field":"email","field_errors":["email: {\"value\": String(\"chipexample.com\")}"]}]}%
```

두 번째 오류가 올바르게 추가되었고 여기에 예상되는 길이에 대한 정보를 얻을 수 있습니다. 포맷된 내부 오류 문자열에서 볼 수 있듯이, 우리는 여기서 훨씬 더 멀리 가서 사용자 필드에 `max`와 `min`을 제공하는 멋지고 구조화된 오류를 만들 수 있다.

마지막으로, 중첩된 케이스를 사용해보고 너무 짧은 애칭으로 보내자.

```undefined
curl -X POST http://localhost:8080/create-validator -H "Content-Type: application/json" -d '{ "email": "chip@example.com", "address": { "street": "warpstreet", "street_no": 1 }, "pets": [{ "name": "" }, {"name": "a"}] }'
```

=>

```undefined
{"message":"field errors","errors":[{"field":"pets","field_errors":["0: \"name: errors: length: {\\\"value\\\": String(\\\"\\\"), \\\"min\\\": Number(3), \\\"max\\\": Number(20)}\"","1: \"name: errors: length: {\\\"value\\\": String(\\\"a\\\"), \\\"max\\\": Number(20), \\\"min\\\": Number(3)}\""]}]}
```

이 오류들은 목록 내의 오류들이고, 우리가 이것을 아주 간단한 방법으로 구현했기 때문에, 우리는 단지 필드 에러 배열 내부와 필드 에러 배열에 에러 문자열이 연결되어 있기 때문에, 우리는 단지 인덱스 0과 1에 대한 에러 문자열을 얻는데, 이것은 이름이 너무 짧았음을 보여준다.

이 튜토리얼의 전체 코드는 GitHub에서 찾을 수 있습니다.

## 결론

JSON 입력 검증은 모든 최신 웹 애플리케이션의 핵심 관심사이며, Rust 에코시스템은 이미 이를 처리할 수 있는 몇 가지 훌륭한 도구를 보유하고 있습니다.

이 예에서는 JSON 역직렬화 오류를 처리하고 다음 단계에서 데이터를 검증하는 방법을 살펴보았다. 여러 가지 방법이 있습니다. 생태계에서 두 개의 특정 라이브러리를 보여주기 위한 무수한 접근법 중 하나를 검토했습니다. 하지만 저는 이것이 여러분이 Rust의 유사한 문제를 어떻게 해결할 수 있는지 느끼게 해주기를 바랍니다.