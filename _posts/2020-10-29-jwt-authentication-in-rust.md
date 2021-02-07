---
layout: post
title: "Rust의 JWT 인증"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/jwt-authentication-in-rust.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/jwt-authentication-in-rust.png?fit=730%2C487&ssl=1)

JWT(JSON Web Token)는 시스템 간의 속성 또는 클레임을 안전하게 표현하기 위한 표준입니다. 클라이언트-서버 방식으로 사용하여 상태 비저장 권한을 설정할 수 있는 반면, 쿠키는 본질적으로 상태 정보를 제공합니다.

하지만, 그것들은 그것보다 더 유연하며 또한 무수히 많은 다른 방법으로 사용될 수 있다. 대표적인 사용 사례는 마이크로 서비스 아키텍처의 안전한 사용자 상태 전파이다. 이러한 설정에서 JWT의 사용 사례는 전면을 향한 상태 저장 권한 메커니즘으로 백엔드 측으로 순수하게 제한될 수 있다. 로그인할 때 세션 토큰은 JWT에 매핑되며, 이 토큰은 마이크로서비스 클러스터 내에서 요청을 승인(액세스 제어)하는 데 사용되지만 사용자에 대한 상태(정보 배포)도 배포합니다.

이를 통해 다른 서비스나 클라이언트가 JWT에 저장된 정보를 다시 가져올 필요가 없다는 이점이 있습니다. 예를 들어 사용자 역할, 사용자 전자 메일 또는 정기적으로 액세스해야 하는 항목을 JWT 내부에 인코딩할 수 있습니다. 그리고 JWT는 암호로 서명되어 있기 때문에, JWT 안에 저장된 데이터는 안전하며 쉽게 조작할 수 없습니다.

이 튜토리얼에서는 Rust 웹 응용 프로그램에서 JWT를 사용하여 인증 및 인증을 구현하는 방법에 대해 설명합니다. JWT 자체에 대해서는 자세히 설명하지 않겠습니다. 이미 이 주제에 대한 훌륭한 자료가 있습니다.

빌드할 예제는 JWT의 액세스 제어 부분에 더 중점을 두므로 토큰 안에 사용자 ID와 사용자의 역할만 저장합니다. 사용자가 리소스에 액세스할 수 있도록 하는 데 필요한 모든 작업을 수행할 수 있습니다.

보안 관련 블로그 게시물에 대한 사용자 정의와 마찬가지로, 다음은 간단한 고지 사항입니다. 이 블로그 게시물에 표시된 코드는 프로덕션 상태가 아니므로 복사/붙여넣기해서는 안 됩니다. 이 예제의 유일한 목적은 인증/인증 시스템을 구축할 때 사용할 수 있는 몇 가지 개념, 매개 변수 및 라이브러리를 보여주는 것입니다.

그 얘긴 그만하고, 시작하자!

## 세우다

계속하려면 최근 Rust 설치(1.39+)와 cURL과 같은 HTTP 요청을 보내는 도구가 필요합니다.

먼저 새 Rust 프로젝트를 만듭니다.

```bash
cargo new rust-jwt-example
cd rust-jwt-example
```

그런 다음 `Cargo.toml` 파일을 편집하고 필요한 종속성을 추가합니다.

```undefined
[dependencies]
jsonwebtoken = "=7.2"
tokio = { version = "0.2", features = ["macros", "rt-threaded", "sync", "time"] }
warp = "0.2"
serde = {version = "1.0", features = ["derive"] }
serde_json = "1.0"
thiserror = "1.0"
chrono = "0.4"
```

저희는 토키오를 비동기 런타임으로 사용하는 경량 워프 라이브러리를 사용하여 웹 애플리케이션을 구축할 것입니다. JSON 처리에는 Serde를 사용하고 오류와 날짜를 처리하기 위해 이 오류와 Chrono를 각각 사용합니다.

JSON 웹 토큰을 처리하기 위해, 우리는 Rust 에코시스템 내에서 널리 사용되는 성숙하고 널리 사용되는 적절한 이름의 json 웹 토큰 상자를 사용할 것입니다.

## 웹서버

먼저 두 개의 끝점과 메모리 내 사용자 저장소가 있는 간단한 웹 서버를 만드는 것부터 시작하겠습니다. 실제 애플리케이션에서는 사용자 스토리지를 위한 데이터베이스가 있을 수 있습니다. 하지만 이 방법은 예에서 중요하지 않기 때문에, 간단히 메모리에 하드 코딩만 할 것입니다.

```undefined
type Result<T> = std::result::Result<T, error::Error>;
type WebResult<T> = std::result::Result<T, Rejection>;
type Users = Arc<RwLock<HashMap<String, User>>>;
```

여기서는 `결과`에 대한 두 가지 도우미 유형을 정의하여 응용프로그램 전체에 오류를 전파하기 위한 내부 결과 유형과 호출자에게 오류를 보내기 위한 외부 결과 유형을 지정한다.

또한 공유 해시맵인 사용자 유형도 정의합니다. 이 스토어는 메모리 내 사용자 저장소이며 다음과 같이 초기화할 수 있습니다.

```coffeescript
mod auth;
mod error;

#[derive(Clone)]
pub struct User {
    pub uid: String,
    pub email: String,
    pub pw: String,
    pub role: String,
}

#[tokio::main]
async fn main() {
    let users = Arc::new(RwLock::new(init_users()));
    ...
}

fn init_users() -> HashMap<String, User> {
    let mut map = HashMap::new();
    map.insert(
        String::from("1"),
        User {
            uid: String::from("1"),
            email: String::from("user@userland.com"),
            pw: String::from("1234"),
            role: String::from("User"),
        },
    );
    map.insert(
        String::from("2"),
        User {
            uid: String::from("2"),
            email: String::from("admin@adminaty.com"),
            pw: String::from("4321"),
            role: String::from("Admin"),
        },
    );
    map
}
```

우리는 사용자의 ID로 쉽게 검색할 수 있는 해시맵을 사용합니다. 여러 스레드가 동시에 사용자 맵에 액세스할 수 있기 때문에 맵은 `RwLock`으로 포장됩니다. 이것은 또한 그것이 마침내 스레드들 사이에서 지도를 공유할 수 있게 해주는 원자, 참조 계산 스마트 포인터인 `Arc`에 들어가는 이유이다.

비동기식 웹 서비스를 구축하고 있고 핸들러 미래에서 어떤 스레드를 실행할지 미리 알 수 없기 때문에, 우리는 스레드 주위에 전달되는 모든 것을 안전하게 만들어야 합니다.

사용자 맵을 사용자 역할 `사용자`와 역할 `관리`로 설정합니다. 나중에 `관리` 역할로만 액세스할 수 있는 엔드포인트를 만들 것입니다. 이렇게 하면 인증 논리가 의도한 대로 작동하는지 테스트할 수 있습니다.

Warp을 사용하기 때문에 사용자 맵을 엔드포인트로 전달하는 필터도 구축해야 합니다.

```xml
fn with_users(users: Users) -> impl Filter<Extract = (Users,), Error = Infallible> + Clone {
    warp::any().map(move || users.clone())
}
```

이 첫 번째 비트를 사용하면 몇 가지 기본 경로를 정의하고 웹 서버를 시작할 수 있습니다.

```php
#[tokio::main]
async fn main() {
    let users = Arc::new(RwLock::new(init_users()));

    let login_route = warp::path!("login")
        .and(warp::post())
        .and_then(login_handler);

    let user_route = warp::path!("user")
        .and_then(user_handler);
    let admin_route = warp::path!("admin")
        .and_then(admin_handler);

    let routes = login_route
        .or(user_route)
        .or(admin_route)
        .recover(error::handle_rejection);

    warp::serve(routes).run(([127, 0, 0, 1], 8000)).await;
}

pub async fn login_handler() -> WebResult<impl Reply> {
    Ok("Login")
}

pub async fn user_handler() -> WebResult<impl Reply> {
    Ok("User")
}

pub async fn admin_handler() -> WebResult<impl Reply> {
    Ok("Admin")
}
```

위의 조각에서 우리는 세 가지 핸들러를 정의한다.

- `POST/로그인` — 전자 메일 및 암호로 로그인합니다.
- `GET/user` — 모든 사용자의 끝점
- `GET/admin` — 관리자 전용 끝점

아직 .recovery(오류::handle_reject)에 대해 걱정하지 마십시오. 오류 처리 작업은 나중에 처리하도록 하겠습니다.

## 인증

사용자와 관리자가 인증할 수 있도록 로그인 기능을 구축해 보겠습니다.

첫 번째 단계는 login_handler 내부의 자격 증명을 얻는 것이다.

```undefined
#[derive(Deserialize)]
pub struct LoginRequest {
    pub email: String,
    pub pw: String,
}

#[derive(Serialize)]
pub struct LoginResponse {
    pub token: String,
}
```

이것은 로그인 메커니즘에 대해 정의하는 API입니다. 클라이언트는 전자 메일과 암호를 보내고 JSON 웹 토큰을 응답으로 수신합니다. 그러면 클라이언트가 이 토큰을 `인증: 베어러 $token` 헤더 필드입니다.

우리는 이것을 다음과 같이 `login_handler`의 본문으로 정의한다.

```php
async fn main() {
    ...
    let login_route = warp::path!("login")
        .and(warp::post())
        .and(with_users(users.clone()))
        .and(warp::body::json())
        .and_then(login_handler);
    ...
}
```

`login_handler`에서 서명과 구현은 다음과 같이 변경됩니다.

```php
pub async fn login_handler(users: Users, body: LoginRequest) -> WebResult<impl Reply> {
    match users.read() {
        Ok(read_handle) => {
            match read_handle
                .iter()
                .find(|(_uid, user)| user.email == body.email && user.pw == body.pw)
            {
                Some((uid, user)) => {
                    let token = auth::create_jwt(&uid, &Role::from_str(&user.role))
                        .map_err(|e| reject::custom(e))?;
                    Ok(reply::json(&LoginResponse { token }))
                }
                None => Err(reject::custom(WrongCredentialsError)),
            }
        }
        Err(_) => Err(reject()),
    }
}
```

무슨 일이야? 먼저, 우리는 공유된 `Users` 맵에 액세스하여 맵에 대한 읽기 잠금을 제공하는 `.read()itam`을 호출한다. 지금은 이것만 있으면 됩니다.

그런 다음 이 읽기 전용 버전의 사용자 맵을 반복하여 수신 본문에 제공된 e-메일 및 pw를 가진 사용자를 찾습니다.

사용자를 찾지 못하면 `잘못된 자격 증명 오류`를 반환하여 사용자에게 유효한 자격 증명을 사용하지 않았다고 알립니다. 그렇지 않으면 기존 사용자의 사용자 ID와 역할로 `auth::create_jwt`를 호출하여 토큰을 반환합니다. 이것은 우리가 발신자에게 다시 보내는 것입니다.

다음은 auth 모듈을 살펴보겠습니다.

auth.rs에서 먼저 유용한 데이터 유형과 상수를 정의한다.

```php
const BEARER: &str = "Bearer ";
const JWT_SECRET: &[u8] = b"secret";

#[derive(Clone, PartialEq)]
pub enum Role {
    User,
    Admin,
}

impl Role {
    pub fn from_str(role: &str) -> Role {
        match role {
            "Admin" => Role::Admin,
            _ => Role::User,
        }
    }
}

impl fmt::Display for Role {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            Role::User => write!(f, "User"),
            Role::Admin => write!(f, "Admin"),
        }
    }
}

#[derive(Debug, Deserialize, Serialize)]
struct Claims {
    sub: String,
    role: String,
    exp: usize,
}
```

역할 열거는 단순히 관리자 역할과 사용자 역할의 매핑이기 때문에 문자열에 구애받지 않아도 되기 때문에 보안상 중요한 작업에 너무 오류가 발생하기 쉽다.

또한 이 역할이 JWT 내에 저장되므로 `역할` 열거에서 문자열로 변환하는 도우미 방법도 정의한다.

또 다른 중요한 유형은 클레임이다. 이 데이터는 JWT가 내부에 저장하여 예상할 수 있는 데이터입니다. 이 경우 서브는 이른바 `누구`라는 소재를 그린다. exp는 토큰의 만료 날짜입니다. 사용자 `역할`도 맞춤형 데이터 포인트로 넣었습니다.

두 상수는 예상되는 `Authorization` 헤더의 접두사와 매우 중요한 `J`입니다.WT_SECRET`. 이것은 JSON 웹 토큰에 서명하는 열쇠입니다. 실제 시스템에서는 정기적으로 변경되는 길고 안전하게 저장된 문자열입니다. 이 비밀이 새면 누구나 이 비밀과 함께 만들어진 모든 JWT를 해독할 수 있다. 또한 각 사용자마다 다른 암호를 사용할 수 있습니다. 예를 들어, 이 암호를 변경하기만 하면 데이터 침해 시 모든 사용자의 토큰을 쉽게 무효화할 수 있습니다.

다음은 `create_jwt` 함수입니다.

```php
use jsonwebtoken::{decode, encode, Algorithm, DecodingKey, EncodingKey, Header, Validation};

pub fn create_jwt(uid: &str, role: &Role) -> Result<String> {
    let expiration = Utc::now()
        .checked_add_signed(chrono::Duration::seconds(60))
        .expect("valid timestamp")
        .timestamp();

    let claims = Claims {
        sub: uid.to_owned(),
        role: role.to_string(),
        exp: expiration as usize,
    };
    let header = Header::new(Algorithm::HS512);
    encode(&header, &claims, &EncodingKey::from_secret(JWT_SECRET))
        .map_err(|_| Error::JWTTokenCreationError)
}
```

먼저 이 토큰의 만료 날짜를 계산합니다. 이 경우에는 앞으로 60초 정도로만 설정했습니다. 토큰이 만료될 때까지 오래 기다릴 필요가 없기 때문에 테스트하기에 좋습니다.

만료 세트는 다른 전략을 사용하여 정의할 수 있지만, 이러한 토큰은 보안에 중요하며 합리적인 정보를 보유하기 때문에 어느 시점에 만료될 것이 분명합니다. 일부 시스템은 새로 고침 토큰 메커니즘에 의존하여 짧은(분/시간) 만료 시간을 설정하고 호출자에게 새로 고침 토큰을 제공하므로 이전 토큰이 만료된 경우 새 토큰을 가져오는 데 사용할 수 있습니다.

다음으로, 우리는 사용자의 ID, 사용자의 역할 및 만료 날짜를 사용하여 `청구` 구조를 만듭니다. 그 후 우리는 `json webtoken` 상자와 첫 번째 상호작용을 하게 된다.

이전에 JWT를 다룬 적이 있는 경우, JWT가 세 부분으로 구성되어 있다는 것을 알 수 있습니다.

- 헤더
- 페이로드
- 서명

우리가 새로운 헤더를 만들고 이 헤더를 인코딩하고 위에 언급한 비밀과 함께 페이로드(청구)를 인코딩하기 때문에 이것은 여기에 반영된다. 이 작업이 실패하면 오류를 반환합니다. 그렇지 않으면 결과 JWT를 반환합니다.

이제 사용자가 서비스에 로그인할 수 있지만 아직 인증을 처리할 수 있는 메커니즘이 없습니다. 다음부터 살펴보도록 하죠.

## 허가

우리는 auth.rs 모듈 안에 있다. Warp를 사용하기 때문에 미들웨어와 같은 추가 기능을 취급자에게 추가하는 가장 좋은 방법은 필터를 사용하는 것입니다.

그래서 우리는 `with_auth` 필터를 정의한다.

```undefined
use warp::{
    filters::header::headers_cloned,
    http::header::{HeaderMap, HeaderValue, AUTHORIZATION},
    reject, Filter, Rejection,
};

pub fn with_auth(role: Role) -> impl Filter<Extract = (String,), Error = Rejection> + Clone {
    headers_cloned()
        .map(move |headers: HeaderMap<HeaderValue>| (role.clone(), headers))
        .and_then(authorize)
}
```

이 필터는 `. 및 (with_auth(역할::Admin) 예를 들어 이 처리기는 `Admin` 역할을 가진 사용자만 액세스할 수 있습니다.

실제 시스템에서는 데이터베이스, 캐시 또는 다른 외부 시스템에 연결할 가능성이 매우 높기 때문에 저는 sync 필터를 만들기로 결정했습니다. 이 경우 반드시 필요한 것은 아니지만, 사용자 저장소가 고정 메모리 내 맵이 아닌 경우라면 어떤 경우에도 유용합니다.

사용자를 인증하려면 몇 가지 단계를 수행해야 합니다.

- `Authorization` 헤더 가져오기, 없으면 실패
- 헤더의 형식이 올바른지 확인(`Bearer $JWT`), 그렇지 않은 경우 실패함
- 헤더에서 JWT 문자열을 추출합니다. 그렇지 않으면 실패합니다.
- JWT를 디코딩합니다. JWT가 잘못되었거나 만료된 경우 실패
- JWT에 저장된 역할을 확인하고 지정된 `역할`과 비교합니다. 예를 들어 JWT 역할이 `사용자`이지만 끝점에 `관리자`가 필요한 경우 실패합니다.
- JWT에서 `uid`를 추출하여 장식된 핸들러에 전달합니다.

꽤 많은 걸음을 내딛는군요! 여기 있는 모든 버그는 심각한 구멍을 만들기 때문에, 우리는 조심스럽게 오류 처리에 접근할 필요가 있다.

위의 `with_auth` 함수에서는 `headers_cloned() warp filter를 사용하여 지도 내에 저장된 요청 헤더의 복사본을 얻습니다. 그런 다음 이를 역할(role)과 묶어서 권한 부여 기능의 핵심인 권한 부여 함수로 넘긴다.

```coffeescript
async fn authorize((role, headers): (Role, HeaderMap<HeaderValue>)) -> WebResult<String> {
    match jwt_from_header(&headers) {
        Ok(jwt) => {
            ...
        }
        Err(e) => return Err(reject::custom(e)),
    }
}
```

이것은 `async` 함수이기 때문에 필터에 `그리고_then`을 사용해야 합니다. 위에서 언급했듯이, 이것은 이 예에서는 필요하지 않지만, 실제 예에서는 승인에 필요한 외부 시스템에 핸들을 넘길 수도 있습니다. 예를 들어 세션 토큰을 내부 토큰에 매핑하거나 필요한 메타데이터를 가져오는 캐시 또는 데이터베이스가 있습니다.

이 예에서는 처음에 헤더 맵을 사용하여 `jwt_from_header` 함수를 호출하여 `Authorization` 헤더에서 JWT를 얻는다.

```php
fn jwt_from_header(headers: &HeaderMap<HeaderValue>) -> Result<String> {
    let header = match headers.get(AUTHORIZATION) {
        Some(v) => v,
        None => return Err(Error::NoAuthHeaderError),
    };
    let auth_header = match std::str::from_utf8(header.as_bytes()) {
        Ok(v) => v,
        Err(_) => return Err(Error::NoAuthHeaderError),
    };
    if !auth_header.starts_with(BEARER) {
        return Err(Error::InvalidAuthHeaderError);
    }
    Ok(auth_header.trim_start_matches(BEARER).to_owned())
}
```

이 함수는 처음 두어 단계를 수행하여 `Authorization` 헤더가 있는지, 유효한지, `Bearer` 접두사가 포함되어 있는지, JWT를 추출한다. 모든 것이 잘 되면 이 문자열을 발신자에게 반환합니다.

authorizate 기능으로 돌아가면 다음 단계는 JWT를 디코딩하여 유효한 클레임 구조를 얻는 것이다.

```php
async fn authorize((role, headers): (Role, HeaderMap<HeaderValue>)) -> WebResult<String> {
    match jwt_from_header(&headers) {
        Ok(jwt) => {
            let decoded = decode::<Claims>(
                &jwt,
                &DecodingKey::from_secret(JWT_SECRET),
                &Validation::new(Algorithm::HS512),
            )
            .map_err(|_| reject::custom(Error::JWTTokenError))?;

            if role == Role::Admin && Role::from_str(&decoded.claims.role) != Role::Admin {
                return Err(reject::custom(Error::NoPermissionError));
            }

            Ok(decoded.claims.sub)
        }
        Err(e) => return Err(reject::custom(e)),
    }
}
```

JWT가 만료되었거나 형식이 잘못되었거나 잘못된 경우 이 디코딩 단계가 실패하고 여기서 중지됩니다. json 웹토큰 라이브러리는 공식 설명서에 잘 설명되어 있는 검증 단계에 대한 사용자 정의 옵션도 제공한다.

유효성 검사가 제대로 수행되면 사용자 역할을 확인할 수 있습니다. 관리 끝점에 있는 경우 JWT 역할도 관리 역할이어야 합니다. 그렇지 않으면 권한 없음 오류를 발생시킵니다.

우리는 이 두 역할만 가지고 있기 때문에, 이 검사는 꽤 쉽지만, 몇 개의 역할만 있으면, 꽤 복잡해질 수 있습니다. 이러한 접근 제어를 안전하고 유지관리 가능한 방법으로 처리하는 데 유용한 라이브러리는 또한 잘 유지관리된 러스트 상자를 가지고 있는 캐스빈입니다.

일단 사용자가 역할 검사를 통과하면, 우리는 장식된 핸들러에 사용자의 ID를 전달합니다. 이 기능은 사용자 ID가 사용자 프로필 또는 개인 데이터 가져오기와 같은 많은 개인화된 엔드포인트와 관련이 있기 때문에 유용합니다.

이렇게 하면 with_auth 필터가 완료되고 main에서 처리기에만 사용하면 됩니다.

```js
async fn main() {
    ...
    let user_route = warp::path!("user")
        .and(with_auth(Role::User))
        .and_then(user_handler);
    let admin_route = warp::path!("admin")
        .and(with_auth(Role::Admin))
        .and_then(admin_handler);
    ...
}

pub async fn user_handler(uid: String) -> WebResult<impl Reply> {
    Ok(format!("Hello User {}", uid))
}

pub async fn admin_handler(uid: String) -> WebResult<impl Reply> {
    Ok(format!("Hello Admin {}", uid))
}
```

쉬웠어요! 기존 처리기를 필터로 장식하고 수신 사용자 ID를 처리기 서명에 넣기만 하면 됩니다. 이 사용자 ID도 인쇄하여 나중에 테스트할 수 있습니다.

## 오류 처리

보안을 위해서는 올바른 오류 처리가 중요합니다. 당신은 너무 많은 정보를 외부에 유출하는 만능 취급자를 원하지 않는다. 오류는 시스템 내부 작동에 대해 아무것도 밝히지 않고 발신자에게 도움이 되어야 한다.

error.rs 모듈에서는 먼저 사용자 정의 오류 유형인 오류 응답 유형을 정의하고 이러한 오류를 처리기에서 반환하는 데 사용할 수 있도록 warp의 거부 특성을 구현한다.

```undefined
#[derive(Error, Debug)]
pub enum Error {
    #[error("wrong credentials")]
    WrongCredentialsError,
    #[error("jwt token not valid")]
    JWTTokenError,
    #[error("jwt token creation error")]
    JWTTokenCreationError,
    #[error("no auth header")]
    NoAuthHeaderError,
    #[error("invalid auth header")]
    InvalidAuthHeaderError,
    #[error("no permission")]
    NoPermissionError,
}

#[derive(Serialize, Debug)]
struct ErrorResponse {
    message: String,
    status: String,
}

impl warp::reject::Reject for Error {}
```

마지막으로, 처음에 main에서 사용되었던 handle_reject 기능을 추가한다.

```php
pub async fn handle_rejection(err: Rejection) -> std::result::Result<impl Reply, Infallible> {
    let (code, message) = if err.is_not_found() {
        (StatusCode::NOT_FOUND, "Not Found".to_string())
    } else if let Some(e) = err.find::<Error>() {
        match e {
            Error::WrongCredentialsError => (StatusCode::FORBIDDEN, e.to_string()),
            Error::NoPermissionError => (StatusCode::UNAUTHORIZED, e.to_string()),
            Error::JWTTokenError => (StatusCode::UNAUTHORIZED, e.to_string()),
            Error::JWTTokenCreationError => (
                StatusCode::INTERNAL_SERVER_ERROR,
                "Internal Server Error".to_string(),
            ),
            _ => (StatusCode::BAD_REQUEST, e.to_string()),
        }
    } else if err.find::<warp::reject::MethodNotAllowed>().is_some() {
        (
            StatusCode::METHOD_NOT_ALLOWED,
            "Method Not Allowed".to_string(),
        )
    } else {
        eprintln!("unhandled error: {:?}", err);
        (
            StatusCode::INTERNAL_SERVER_ERROR,
            "Internal Server Error".to_string(),
        )
    };

    let json = warp::reply::json(&ErrorResponse {
        status: code.to_string(),
        message,
    });

    Ok(warp::reply::with_status(json, code))
}
```

이 중 대부분은 Warp에서 Reject를 처리하고 마지막에 JSON 응답으로 변환하기 위한 보일러 플레이트입니다.

흥미로운 부분은 우리가 우리의 관습적인 `오류` 타입을 다룰 때 입니다. 이 경우 상태 코드에서 발생할 수 있는 오류를 매핑합니다. 오류의 표시 구현을 유용한 오류 메시지만 포함하도록 정의했기 때문에 오류를 간단히 문자열화할 수 있습니다.

오류에 내부 컨텍스트를 추가하는 경우 여기에서 매우 주의해야 하며 보안 관련 오류를 외부에 노출하기 위해 항상 새로운 오류, 가벼운 오류 및 제한된 오류를 정의해야 합니다. 스택 추적과 같은 내부 작업에 대한 정보를 누설해서는 안 됩니다.

또한 실제 시스템에서는 민감한 정보를 포함하지 않도록 세심하게 조작된 `Security Error` 유형을 정의하고 가능한 모든 인증 관련 사례에 완벽하게 매핑하는 것이 타당할 수 있다.

## 테스트

이제 인증 메커니즘과 권한 부여 메커니즘이 모두 구현되었으므로 마지막 단계는 이것이 작동하는지 확인하는 것입니다.

포트 8000에서 로컬로 웹 서버를 시작하는 `카고 런`을 사용하여 서버를 시작할 수 있습니다.

그런 다음 `사용자`로 로그인하여 다음 두 개의 엔드포인트에 액세스할 수 있습니다.

```undefined
curl http://localhost:8000/login -d '{"email": "user@userland.com", "pw": "1234"}' -H 'Content-Type: application/json'

{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwicm9sZSI6IlVzZXIiLCJleHAiOjE2MDMxMzQwODl9.dWnt5vfcGdwypEQUr3bLMrZYfdyxj3v6-io6VREWHXebMUCKBddf9xGcz4vHrCXruzx42zrS3Kygiqw3xV8W-A"}

curl http://localhost:8000/user -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwicm9sZSI6IlVzZXIiLCJleHAiOjE2MDMxMzQwODl9.dWnt5vfcGdwypEQUr3bLMrZYfdyxj3v6-io6VREWHXebMUCKBddf9xGcz4vHrCXruzx42zrS3Kygiqw3xV8W-A' -H 'Content-Type: application/json'

Hello User 1

curl http://localhost:8000/admin -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxIiwicm9sZSI6IlVzZXIiLCJleHAiOjE2MDMxMzQwODl9.dWnt5vfcGdwypEQUr3bLMrZYfdyxj3v6-io6VREWHXebMUCKBddf9xGcz4vHrCXruzx42zrS3Kygiqw3xV8W-A' -H 'Content-Type: application/json'

{"message":"no permission","status":"401 Unauthorized"}
```

지금까지, 너무 좋아. 로그인에 성공하여 유효한 JWT를 반환했습니다. 이 JWT를 사용하여 `/user` 및 `/admin`에 대한 인증 요청을 했습니다. 예상대로 첫 번째는 효과가 있었고 두 번째는 오류를 반환했다.

다음 번에 관리자를 사용해 보겠습니다.

```undefined
curl http://localhost:8000/login -d '{"email": "admin@adminaty.com", "pw": "4321"}' -H 'Content-Type: application/json'

{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIyIiwicm9sZSI6IkFkbWluIiwiZXhwIjoxNjAzMTM0MjA1fQ.uYglVKRvb3h0bDC0Uz8FwGTu4v__Rl3toVI9fMI4_IT8keKde_SZRFQ4ii_PKzI4wjmDsZlnpULe6Tg0vWfEnw"}

curl http://localhost:8000/admin -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIyIiwicm9sZSI6IkFkbWluIiwiZXhwIjoxNjAzMTM0MjA1fQ.uYglVKRvb3h0bDC0Uz8FwGTu4v__Rl3toVI9fMI4_IT8keKde_SZRFQ4ii_PKzI4wjmDsZlnpULe6Tg0vWfEnw' -H 'Content-Type: application/json'

Hello Admin 2

curl http://localhost:8000/user -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIyIiwicm9sZSI6IkFkbWluIiwiZXhwIjoxNjAzMTM0MjA1fQ.uYglVKRvb3h0bDC0Uz8FwGTu4v__Rl3toVI9fMI4_IT8keKde_SZRFQ4ii_PKzI4wjmDsZlnpULe6Tg0vWfEnw' -H 'Content-Type: application/json'

Hello User 2
```

좋습니다! 관리자가 양쪽 끝점에 모두 액세스할 수 있으며 올바른 사용자 ID를 기록했습니다. 이것이 실제 시스템이라면 검증, 성공 및 오류 사례에 대한 전체 테스트 세트를 작성할 것입니다.

인증 관련 엔드포인트를 퍼지하는 것도 구현의 견고성을 높이는 좋은 방법이다. 수십억 개의 임의의 값을 무언가에 보내는 것만큼 이상한 에지 사례가 남지 않도록 보장하는 것은 없습니다!

전체 예제 코드는 GitHub에서 찾을 수 있습니다.

## 결론

본 튜토리얼에서는 JSON 웹 토큰을 사용하여 기본 인증 및 권한 부여 모델을 구현했습니다.

json webtoken 상자는 Rust 생태계에서 성숙하고 널리 사용되는 옵션이다. 이 예제를 위해 warp를 사용했지만, 여기서 사용된 아이디어와 기법은 다른 Rust 웹 프레임워크로 매우 잘 번역될 것이다.

JWT는 인증을 처리하고 정보를 안전하게 배포할 수 있는 강력한 도구이며, Rust 커뮤니티는 웹 서비스 분야의 성숙도를 높이기 위한 좋은 신호로 다시 한 번 작업을 수행했습니다.