---
layout: post
title: "Rust의 비동기 I/O 및 비동기/대기 패키지"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/asynchronous-i-o-and-async-await-packages-in-rust.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/asynchronous-i-o-and-async-await-packages-in-rust.png?fit=730%2C487&ssl=1)

러스트의 비동기화 이야기는 한동안 진행되어 왔다. 그러나 작년 말 `아닌카/대기` 구문이 안정화되면서 일이 정말 잘 풀리기 시작했다.

비동기식은 JavaScript와 같은 다른 언어와는 다르게 Rust에서 작동합니다. 한 가지 주요한 차이점은 Rust의 비동기화에는 실행자가 작업해야 한다는 것이다. 그것은 표준 도서관에서는 이용할 수조차 없다.

이 튜토리얼에서는 세 가지 Rust 비동기 패키지를 살펴보고, 운영 준비 상태를 평가하고, 빠르고 안정적이며 동시성이 높은 애플리케이션을 구축하는 방법을 시연합니다.

- 토키오
- 비동기 표준
- 선물

## Rust에서 비동기식은 어떻게 작동합니까?

그 핵심에는 Rust의 비동기/대기 기능이 `Futures` 위에 구축되어 있다. 만약 여러분이 자바스크립트에서 온다면, 미래에 대한 약속 같은 것을 생각할 수 있습니다. 그것은 아직 계산을 마치지 않은 가치들입니다. 그러나 약속과 달리, 미래는 그들이 명시적으로 여론조사를 받기 전까지는 아무런 진전을 이루지 못할 것이다. 여기가 실행자들이 들어오는 곳이다. 실행자는 진행 준비가 되었을 때 폴링하여 사용자의 미래를 관리하는 런타임입니다. 앞서 말씀드린 것처럼 표준 라이브러리에는 표준 라이브러리가 포함되어 있지 않습니다. 비동기 Rust를 시작하려면 외부 실행자 상자를 사용해야 합니다.

이렇게 하면 불필요한 복잡성이 가중된다고 생각할 수 있지만, 대부분의 다른 언어처럼 단일 솔루션을 사용할 필요 없이 애플리케이션 요구에 맞게 조정된 실행자를 더 효과적으로 제어할 수 있게 됩니다.

비동기식은 주로 IO에 바인딩된 애플리케이션의 OS 스레드에 대한 대안으로 사용됩니다. OS 스레드와는 달리 미래를 개척하는 데 비용이 많이 들고 수백만 개의 스레드도 동시에 실행할 수 있습니다.

Rust에 대한 상위 비동기식을 살펴보겠습니다.

## 1. 토키오

Tokio는 비동기 Rust를 처리하는 가장 인기 있는 상자입니다. Tokio는 실행자 외에도 여러 표준 라이브러리 유형의 비동기 버전을 제공합니다.

이 상자의 대부분의 기능은 활성화해야 하는 선택적 기능 뒤에 있습니다. 이렇게 하면 기능이 필요하지 않을 때 컴파일 시간과 이진 크기를 줄일 수 있습니다. 우리는 시범을 위해 "전체" 기능을 사용하여 모든 기능을 사용할 수 있도록 하겠습니다.

```undefined
[dependencies]
tokio = { version = "0.3", features = ["full"] }
```

이제 Tokio 런타임이 생성되어 나중에 실행할 수 있습니다.

```css
fn main() {
    tokio::runtime::Runtime::new().unwrap().block_on(async {
        println!("Hello world");
    })
}
```

여기서 우리의 응용 프로그램 코드는 `block_on` 방식으로 비동기 블록 안으로 들어갈 수 있다. 이 보일러 플레이트는 대신 비동기 주 기능을 사용할 수 있는 매크로로 교체할 수 있습니다.

일반적으로 Tokio 런타임에 부트스트랩하는 방법은 다음과 같습니다.

```undefined
#[tokio::main]
async fn main() {
    println!("Hello world");
}
```

이제 비동기식인 만큼 기다림도 써봐야 한다. 단순하게 하기 위해서, `잠`을 사용해서 기다릴 미래를 돌려보자.

```undefined
use std::time::Duration;
use tokio::time::sleep;

#[tokio::main]
async fn main() {
    let (v1, v2, v3) = tokio::join!(
        async {
            sleep(Duration::from_millis(1500)).await;
            println!("Value 1 ready");
            "Value 1"
        },
        async {
            sleep(Duration::from_millis(2800)).await;
            println!("Value 2 ready");
            "Value 2"
        },
        async {
            sleep(Duration::from_millis(600)).await;
            println!("Value 3 ready");
            "Value 3"
        },
    );

    assert_eq!(v1, "Value 1");
    assert_eq!(v2, "Value 2");
    assert_eq!(v3, "Value 3");
}
```

여기서, 우리는 `tokio::join!`을 사용하여 동시에 여러 개의 미래를 실행했다. 그것은 모든 선물들이 완성될 때까지 기다렸다가 각각의 결과를 한 번에 돌려줄 것이다. 이 경우, 세 번째 미래가 먼저 완료되고, 그 다음으로는 첫 번째, 마지막으로 두 번째가 완료됩니다.

### 차단코드

또한 `수면`을 사용하면 대기 시간을 사용하지 않고 비동기 상황에서 차단했을 때 어떤 일이 일어나는지 살펴볼 수 있는 좋은 기회입니다.

두 번째 미래의 절전 모드를 다음과 같이 변경합니다.

```cpp
std::thread::sleep(Duration::from_millis(2800));
```

여기서는 미래를 반환하지 않고 전체 스레드를 차단하는 표준 라이브러리 sleep을 사용했습니다. 지금 이것을 실행하면 두 번째 미래가 차단된 한 모든 미래가 진행되지 않음을 알 수 있습니다. 우리가 기다림을 사용하지 않았기 때문에, 두 번째 미래는 `도키오` 런타임에 대한 통제권을 다시 줄 수 있을지 알 수 없다. 런타임이 차단되면 모든 미래도 차단됩니다.

다행히 토키오가 우리 뒤를 봐주고 있어. tokio::task 모듈에는 바둑의 고루틴과 유사한 녹색 스레드가 구현되어 있습니다. spawn_blocking을 사용하면 Tokio 런타임이 전용 스레드 풀 내에서 차단 코드를 실행하여 다른 미래가 계속 진행되도록 할 수 있습니다.

이것을 `수면` 차단에 사용하면 두 번째 미래는 다음과 같습니다.

```css
async {
    tokio::task::spawn_blocking(|| {
        std::thread::sleep(Duration::from_millis(2800));
    })
    .await
    .unwrap();
    println!("Value 2 ready");
    "Value 2"
},
```

이제 코드가 다시 한 번 예상대로 실행됩니다. 물론, 이것은 조작된 예이지만, 차단 슬립은 어떤 CPU가 많이 사용되는 차단 코드로 대체될 수 있고 나머지는 토키오가 처리할 것이다.

### 토키오 태스크

Tokio의 스레드 풀에서 차단 코드를 산란하면 잘 할 수 있지만, 미래와 비동기/대기 기능을 최대한 활용하기 위해 비동기 코드를 위에서 아래로 사용하자. 우리는 `std::thread::spawn`의 비동기 버전인 `tokio::task::spawn`을 사용하여 자신의 백그라운드 작업으로 미래를 생성한다. 백그라운드에서 실행할 작업을 받을 수 있는 비동기 작업자 풀을 만들어 이를 테스트합니다.

열거형을 사용하여 작업 목록을 정의하는 것부터 시작합니다.

```undefined
#[derive(Debug)]
enum Message {
    SendWelcomeEmail { to: String },
    DownloadVideo { id: usize },
    GenerateReport,
    Terminate,
}
```

여기서 주목해야 할 가장 중요한 것은 근로자들이 더 이상 필요 없을 때 업무 처리를 중단하라는 메시지를 담은 `종료` 변형이다. 우리는 또한 "디버그"를 파생하여 메시지를 나중에 출력할 수 있습니다.

다음으로, 우리는 토키오가 제공하는 채널 중 하나를 사용하여 근로자들과 소통해야 합니다.

```cpp
use std::sync::Arc;
use tokio::sync::mpsc::unbounded_channel;
use tokio::sync::Mutex;

#[tokio::main]
async fn main() {
    let (sender, receiver) = unbounded_channel();
    let receiver = Arc::new(Mutex::new(receiver));
}
```

여기서는 표준 라이브러리의 MPSC 채널에 대한 비동기식 대안인 무제한 채널을 사용합니다. 생산 시에는 응용 프로그램이 과부하 상태일 때 역압력을 제공하는 제한된 크기의 채널인 `tokio::sync::mpsc::channel`을 사용하는 것이 좋습니다. 복수 작업자 간에 공유되기 때문에 수신기도 아크(Arc)와 무텍스(Mutex)로 포장돼 있다. 채널을 통해 보낼 값의 유형을 유추할 수 없기 때문에 아직 컴파일되지 않습니다.

이제 일꾼 몇 명을 낳자.

```js
let size = 5;
let mut workers = Vec::with_capacity(size);

for id in 0..size {
    let receiver = Arc::clone(&receiver);
    let worker = tokio::spawn(async move { /* ... */ });
    workers.push(worker);
}
```

이로 인해 일부 근로자가 생겨나면서 조인핸들을 벡으로 밀어 넣는다. 이제 메시지 처리기를 채우는 일만 남았습니다.

```php
use std::time::Duration;
use tokio::time::sleep;

// ...

let worker = tokio::spawn(async move {
    loop {
        let message = receiver
            .lock()
            .await
            .recv()
            .await
            .unwrap_or_else(|| Message::Terminate);
        println!("Worker {}: {:?}", id, message);
        match message {
            Message::Terminate => break,
            _ => sleep(Duration::from_secs(1 + id as u64)).await,
        }
    }
});
```

이것은 각 메시지를 인쇄하면서 영원히 반복됩니다. 종료 메시지가 나타나면 루프가 끊어집니다. 다른 어떤 메시지에도, 그것은 잠을 잔다. 자신의 응용 프로그램에서는 처리하려는 각 메시지와 비교하여 실제 논리를 설명합니다.

뮤텍스 가드의 수명에 주의하십시오. let Some(메시지) = receiver.locktime...`을 사용하지 않는 이유는 루프의 내용이 실행될 때까지 뮤텍스 가드를 삭제하지 않기 때문입니다. 즉, 메시지를 처리하는 동안 뮤텍스가 잠겨 한 번에 하나의 작업자만 작업할 수 있습니다.

근로자들이 깨끗하게 해지를 할 수 있도록 하려면 주 기능이 끝나기 전에 모두 종료 메시지를 보내는 것이 중요하다. 이렇게 하지 않으면 작업이 중단되고 불량 상태가 될 수 있습니다.

```bash
for _ in &workers {
    let _ = sender.send(Message::Terminate);
}
for worker in workers {
    let _ = worker.await;
}
```

첫 번째 루프는 작업자가 있는 횟수만큼 메시지를 보냅니다. 그런 다음 두 번째는 각 작업자가 완료될 때까지 기다립니다. let_ =을(를) 사용하고 있습니다.`여기서 결과를 버린다. 이러한 코드는 더 이상 작업자가 필요하지 않은 지점 바로 앞에 배치되어야 합니다. 우리의 경우, 그것은 "메인"의 맨 끝에 있다.

여기서 "보내는" 방법 뒤에 "대기"를 사용하지 않았다는 점에 유의하십시오. 그것은 우리가 송신자를 절대 차단하지 않는 무제한 채널을 사용했기 때문입니다. 경계 채널은 송신자를 차단할 수 있으므로 송신 시 `대기`를 사용해야 합니다. try_send를 사용할 수도 있습니다.

이 정리 코드 위에는 작업자에게 작업을 보내도록 하겠습니다.

```css
sender.send(Message::DownloadVideo { id: 10 }).unwrap();
sender.send(Message::GenerateReport).unwrap();
sender.send(Message::SendWelcomeEmail { to: "hi@example.com".into() }).unwrap();
sender.send(Message::DownloadVideo { id: 25 }).unwrap();
```

이 예에서는 채널, 뮤텍스를 사용하고 비동기 작업을 생성했습니다. 표준 라이브러리 등가물을 사용한 경우 각 API에는 인식 가능한 API가 있습니다.

토키오는 이미 생산 준비가 완료되었다는 것이 저의 강한 의견입니다. 그렇긴 하지만, 현재 버전은 곧 출시될 버전 1.0의 베타 릴리스로 처리되고 있다. 이것이 공개되면, 토키오 팀은 최소 5년 동안 그것을 유지할 것을 약속할 것이다. 저는 전체 토키오 스택이 비슷한 고품질로 되어 있으며, 정말 기쁘게 사용할 수 있다는 것을 알았다.

## 2. 비동기 표준

이름에서 알 수 있듯이, 비동기-std는 Rust 표준 라이브러리의 비동기 버전이 되려고 시도한다. 주요 경쟁사인 토키오와 비슷한 목표를 가지고 있지만, 인기가 적고, 따라서 야생에서 전투 테스트를 덜 받는다.

Tokio와 비동기 std의 API는 동일하지 않지만, 그것들은 상당히 유사하며 대부분의 개념들이 둘 사이에 전달된다. 다시 말하지만, Rust 표준 라이브러리에 대한 경험이 있는 경우 이러한 두 라이브러리는 이미 친숙하게 느껴질 것입니다. 여전히 궁금하신 분들은 GitHub에 비동기 std 포트와 함께 마지막으로 한 예에 대한 코드를 공개했습니다.

가장 중요한 것은 Tokio와 비동기-std가 100% 호환되지 않는다는 것이다. 다른 라이브러리가 런타임에 작동하지 않을 수 있습니다.

현재 핵심 언어와 표준 라이브러리는 비동기/대기 지원에 대한 최소값만 제공하며 나머지는 커뮤니티에서 작성한 상자에 의해 작성됩니다. 시간이 지남에 따라, 올바른 추상화가 발견됨에 따라, 기초적인 조각들 중 일부는 표준 라이브러리에 병합될 수 있고 더 많은 라이브러리가 런타임에 구애받지 않게 될 수 있다. 그 때까지 사용하는 비동기 라이브러리가 특정 런타임에 따라 달라지는지 확인해야 합니다.

안정성이 최우선이라면, 저는 Tokio가 비동기식 std보다 더 성숙하고 그 위에 더 많은 라이브러리를 가지고 있기 때문에 Tokio를 추천하고 싶습니다. Rust 비동기식을 사용해 보는 사람은 여전히 비동기-std를 사용해 보는 것이 좋습니다. 누가 알겠어요, 아마 당신이 원하는 걸 찾을 수 있을 거예요.

## 3. 선물

`future-rs` 상자는 Rust에서 비동기식으로 사용할 수 있는 공유 기반 조각을 제공합니다. Tokio와 비동기 std 모두 공유 특성과 같은 것들을 위해 선물 상자의 일부를 사용합니다. 실제로 `std::future::미래는 원래 이 상자에서 가져온 것이며 다른 부품들은 언젠가 표준 라이브러리로 옮겨질 것이다.

이 상자에 들어 있는 것의 좋은 예는 스트림 모듈입니다. 스트림은 기본적으로 비동기식 반복기이며 이 모듈은 `Stream`과 `Stream`을 제공합니다.엑스트라 특성에는 반복기에 사용할 수 있는 기능과 유사한 결합기 기능이 포함되어 있다.

## 결론

내 생각에는 런타임 호환성에 대한 일부 우려에도 불구하고 비동기 Rust는 좋은 위치에 있다. Tokio와 비동기-std는 표준 라이브러리 유형에 대한 비동기 대안을 제공하는 범용 비동기 런타임입니다.

생산 어플리케이션의 경우, 완성도와 그 위에 제조된 상자 수 때문에 현재 Tokio를 추천하고 싶습니다. 배터리 포함 프레임워크를 찾는 사람들에게 러스트는 아직 좋은 선택이 아니다. 그러나 더 작고 모듈화된 부품으로 애플리케이션을 구축하는 것을 선호하는 사람들에게는 Tokio와 그 주변 생태계가 훌륭합니다.