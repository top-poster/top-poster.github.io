---
layout: post
title: "Chrono-TZ를 이용한 Rust에서의 시간대 처리"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/timezone-handling-in-rust-with-chrono-tz.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/timezone-handling-in-rust-with-chrono-tz.png?fit=730%2C487&ssl=1)

최신 웹 애플리케이션(특히 전 세계 사용자에게 서비스를 제공하는 애플리케이션)에서 날짜와 시간을 처리할 때 시간대를 처리하는 것이 문제가 될 수 있습니다.

활용 사례에 따라서는 시간대별로 다소 까다로울 수 있다. IANA 표준 시간대 데이터베이스와 이 데이터베이스의 릴리스 기록을 간략히 살펴보면 다른 문제가 있습니다. 표준 시간대는 변경될 수 있습니다. 지난 5년 동안 북한, 러시아, 아이티, 칠레 등지에서 시간대 변화가 있었다. 이러한 변경사항 중 일부는 서머타임 추가 또는 제거를 포함하며, 다른 일부는 다른 요인에 기초한다.

프로그램에서 정확한 시간대를 사용해야 하는 경우 최신 상태로 유지하는 것이 중요합니다. 예를 들어, 각 표준 시간대의 특정 시점에 모든 사용자에게 이메일 또는 알림을 보내려고 한다고 가정해 보겠습니다. 국가에서 제공하는 알림 양과 데이터베이스 변경 내용을 전파하는 속도에 따라 앱에서 표준 시간대가 잘못되었을 수 있습니다.

이 튜토리얼에서는 Chrono 및 Chrono-TZ 라이브러리를 사용하여 Rust에서 날짜 및 시간대를 처리하는 방법을 보여 줍니다. 크로노(Chrono)는 Rust에서 날짜 및 시간을 처리하는 GO-to-Cartine이고 크로노-TZ는 시간대를 처리하는 확장판이다.

사용자가 시간대를 포함하는 RFC3339 형식(1996-12-19T16:39:57+02:00)으로 날짜를 추가할 수 있는 간단한 웹 서비스를 구축한다. 그런 다음 이러한 날짜는 메모리에 저장되고 사용자는 원하는 시간대로 가져와 변환할 수 있습니다.

이 앱을 빌드할 때 Rust에서 날짜를 구문 분석하고 포맷하는 방법과 표준 시간대를 구문 분석하고 변환하는 방법을 시연합니다.

## 세우다

계속하려면 최근 Rust 설치(1.39+)와 cURL과 같은 HTTP 요청을 보내는 도구가 필요합니다.

먼저 새 Rust 프로젝트를 만듭니다.

```bash
cargo new rust-timezone-example
cd rust-timezone-example
```

다음으로 Cargo.toml 파일을 편집하고 필요한 종속성을 추가합니다.

```undefined
[dependencies]
tokio = { version = "0.2", features = ["macros", "rt-threaded", "sync"] }
warp = "0.2"
chrono = { version = "0.4", features = ["serde"] }
chrono-tz = "0.5"
serde = {version = "1.0", features = ["derive"] }
serde_json = "1.0"
```

서비스는 아래 토키오를 사용하여 워프로 작성될 것이다. 수신 요청 페이로드를 역직렬화하는 데 `serde`를 사용합니다.

## 데이터 저장

메모리 내 데이터 저장 메커니즘을 날짜의 공유 벡터 형태로 사용할 것입니다.

```undefined
use chrono::prelude::*;
use tokio::sync::RwLock;
use std::sync::Arc;

type Dates = Arc<RwLock<Vec<DateTime<Utc>>>>;
```

날짜 유형은 날짜의 데이터 저장소를 나타냅니다. 앞에 다소 무서운 타입의 리스트가 있지만, 사실 전혀 화려하지 않습니다.

안쪽에서 바깥쪽으로 갑시다. UTC에 날짜를 저장하겠습니다. 이렇게 하면, 우리는 들어오는 날짜의 시간대를 추적할 필요가 없습니다. 우리는 단순히 그들 사이를 전환할 수 있습니다. 데이트타임(Date Time)과 utc(Utc)형은 크로노의 서곡에서 따온 것이다.

날짜 목록을 저장하여 이 날짜를 `Vec`로 지정합니다. 비동기식 웹 서비스의 데이터 스토리지로 사용하기 때문에 이 데이터 스토리지를 서로 다른 스레드에서 동시에 읽고 쓸 수 있습니다. 이러한 이유로, 우리는 그 주변에 있는 토키오 런타임에서 비동기식 `RwLock`을 넣을 것이며, 그래서 그것에 접근하고자 하는 사람은 누구나 잠금을 획득해야 한다. RwLock은 여러 판독기에 잠금을 허용하지만 한 번에 하나의 쓰기 잠금만 활성화할 수 있습니다. 이를 통해 데이터 레이스가 발생하지 않습니다.

마지막으로, 우리는 메모리 안전한 방법으로 스레드에서 공유할 수 있도록 전체를 `아크` 안에 넣는다.

## 웹서버

다음 단계는 기본 웹 서비스를 설정하고 임시 데이터 저장소로 이를 연결하는 것입니다. 먼저 main의 warp 서버부터 시작하겠습니다.

```php
use warp::{http::StatusCode, reply, Filter, Rejection, Reply};
use std::convert::Infallible;

#[tokio::main]
async fn main() {
    let dates: Dates = Arc::new(RwLock::new(Vec::new()));

    let create_route = warp::path("create")
        .and(warp::post())
        .and(with_dates(dates.clone()))
        .and(warp::body::json())
        .and_then(create_handler);
    let fetch_route = warp::path("fetch")
        .and(warp::get())
        .and(warp::path::param())
        .and(with_dates(dates.clone()))
        .and_then(fetch_handler);

    println!("Server started at localhost:8080");
    warp::serve(create_route.or(fetch_route))
        .run(([0, 0, 0, 0], 8080))
        .await;
}

fn with_dates(dates: Dates) -> impl Filter<Extract = (Dates,), Error = Infallible> + Clone {
    warp::any().map(move || dates.clone())
}
```

POST/create와 GET/fetch/$time zone의 두 개의 경로를 생성한다. 두 경우 모두 "main"의 시작 부분에서 초기화하는 "date" 데이터 스토리지를 처리기에 원자 참조를 복사하는 "with_dates"라는 워프 필터를 통해 처리기에 전달한다.

생성 끝점은 또한 "DateTimeRequest" 유형의 페이로드를 사용합니다.

```css
use serde::Deserialize;

#[derive(Deserialize)]
struct DateTimeRequest {
    date_time: String,
}
```

마지막으로, 우리는 이러한 경로에 대해 두 가지 자리 표시자 핸들러 함수를 정의해야 한다.

```js
type Result<T> = std::result::Result<T, Rejection>;

async fn create_handler(dates: Dates, body: DateTimeRequest) -> Result<impl Reply> {
    Ok("")
}

async fn fetch_handler(time_zone: String, dates: Dates) -> Result<impl Reply> {
    Ok("")
}
```

이것들은 아직 재미있는 일을 하지는 않지만, 우리가 다음에 다룰 것은 그것입니다.

## 날짜 만들기

날짜를 만드는 데 사용되는 엔드포인트의 첫 번째 단계는 들어오는 `body.date_time` 문자열을 유효한 날짜로 구문 분석하는 것입니다. 그런 다음, 그렇게 되면 날짜를 UTC로 변환하여 데이터 저장소로 푸시하여 성공 메시지를 반환합니다.

Rust 코드로 번역된 것을 보자.

```php
async fn create_handler(dates: Dates, body: DateTimeRequest) -> Result<impl Reply> {
    let dt: DateTime<FixedOffset> = match DateTime::parse_from_rfc3339(&body.date_time) {
        Ok(v) => v,
        Err(e) => {
            return Ok(reply::with_status(
                format!("could not parse date: {}", e),
                StatusCode::BAD_REQUEST,
            ))
        }
    };

    dates.write().await.push(dt.with_timezone(&Utc));

    Ok(reply::with_status(
        format!("Added date with timezone: {} as UTC", dt.timezone()),
        StatusCode::OK,
    ))
}
```

우리는 크로노의 `DateTime::parse_from_rfc3339` 함수를 사용하여 날짜를 구문 분석합니다. 여기에 나오는 것이 `날짜 시간`이다. 오프셋은 시간대를 설명합니다. 크로노에서 임의의 사용자 정의 형식으로 날짜를 구문 분석하는 방법은 더 많습니다.

이 작업이 실패하면 오류를 반환합니다. 그렇지 않으면 `date`를 사용하여 데이터 저장소에 대한 쓰기 잠금을 얻게 됩니다.UTC로 변환된 날짜를 쓰기(.ait)하고 날짜 벡터로 밀어넣습니다.

마지막으로, 수신 날짜로부터 구문 분석된 시간대를 보여주는 성공 메시지를 사용자에게 반환합니다.

## 가져오기 날짜

저장된 모든 날짜를 가져오기 위해 사용자는 `GET/fetch/$timezone` 끝점을 사용할 수 있습니다. 여기서 `$timezone`은 `UTC`, `GMT`, `아프리카/알제리` 또는 여러 시간대 Chrono-TZ 및 IANA 지원 중 하나일 수 있습니다.

`유럽/비엔나`와 같은 경우 호출자는 `/`를 `%2`로 URL 인코딩해야 합니다.엔드포인트에 전화할 때 F를 사용하면 URL 디코딩을 다시 해야 합니다. 이 기본 예에서는 이 사례를 처리하기 위해 간단한 교체를 실시하겠습니다.

다음 단계는 수신 시간대를 구문 분석하고 유효한 시간대인 경우 이 표준시로 변환된 모든 저장된 날짜를 사용자에게 반환하는 것입니다.

한 가지 가능한 솔루션을 살펴보겠습니다.

```php
async fn fetch_handler(time_zone: String, dates: Dates) -> Result<impl Reply> {
    let parsed_time_zone = time_zone.replace("%2F", "/");
    let tz: Tz = match parsed_time_zone.parse() {
        Ok(v) => v,
        Err(e) => {
            return Ok(reply::with_status(
                format!("could not parse timezone: {}", e),
                StatusCode::BAD_REQUEST,
            ))
        }
    };

    Ok(
        match serde_json::to_string(
            &dates
                .read()
                .await
                .iter()
                .map(|t: &DateTime<Utc>| t.with_timezone(&tz).to_rfc3339())
                .collect::<Vec<_>>(),
        ) {
            Ok(v) => reply::with_status(v, StatusCode::OK),
            Err(e) => {
                return Ok(reply::with_status(
                    format!("could not serialize json: {}", e),
                    StatusCode::INTERNAL_SERVER_ERROR,
                ))
            }
        },
    )
}
```

교체 작업 후, 우리는 들어오는 문자열을 "chrono_tz:: ".parse() 함수를 사용하여 구문 분석한다.Tz. 이 작업이 실패하면 오류를 반환합니다.

그렇지 않으면, 다음 단계는 `dates.read().wait`를 사용하여 데이터 저장소의 읽기 잠금을 획득한 후 벡터를 반복하여 `.with_timezone` 도우미를 사용하여 지정된 시간대로 변환된 날짜 내의 UTC 날짜를 매핑하는 것이다.

마지막으로, 우리는 입력과 일치하도록 날짜를 RFC3339 날짜 문자열로 변환한다. 그런 다음 결과 벡터가 JSON으로 직렬화되고 사용자에게 반환됩니다.

다 됐다! 더 이상 어쩔 수 없다! 예상대로 되는지 봅시다.

먼저 `카고 런`을 사용하여 서버를 실행한 후 cURL을 사용하여 날짜를 작성합니다.

```undefined
curl -X POST http://localhost:8080/create -d '{"date_time": "1996-12-19T16:39:57+02:00"}' -H "content-type: application/json"
Added date with timezone: +02:00 as UTC
```

지금까지, 너무 좋아. 우선 UTC로 가져오자.

```undefined
curl http://localhost:8080/fetch/UTC
["1996-12-19T14:39:57+01:00"]
```

그리고 또 다른 시간대인 아프리카/알제리에서는 UTC+01:00입니다.

```undefined
curl http://localhost:8080/fetch/Africa%2FAlgiers
["1996-12-19T15:39:57+00:00"]
```

아주 멋져요! UTC+02:00에 날짜를 저장한 후 UTC+01:00 (1시간 단축)인 UTC와 함께 가져왔습니다.

다른 시간대 조합으로 재미있게 해 볼 수 있습니다. GitHub의 전체 예제 코드를 확인하십시오.

## 결론

특히 시간대를 사용하는 경우 시간과 날짜 처리가 까다롭습니다. 다행히도, Rust의 생태계는 우리에게 필요한 모든 도구를 제공합니다.

크로노 라이브러리는 견고하고 완전하며 널리 사용되며 훌륭한 API를 가지고 있다. 이와 같은 상자는 표준 시간대를 처리하는 기술적 측면을 문제 삼지 않습니다. 날짜 및 시간에 따른 실제 비즈니스 문제를 처리하는 것은 이상한 API, 불일치 및 버그를 통해 문제를 해결할 필요 없이 충분히 어렵습니다.