---
layout: post
title: "Rust:letter 및 imap에 대한 이메일 상자"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/email-crates-for-rust-lettre-and-imap.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/email-crates-for-rust-lettre-and-imap.png?fit=730%2C487&ssl=1)

요즘 대부분의 웹 애플리케이션은 어떤 식으로든 이메일과 상호 작용해야 하므로 좋은 언어 지원이 필수적입니다. Rust는 확실히 이메일 작업에 대한 기본적인 지원이 있지만, 아직 모든 사용 사례를 커버할 수 있다고는 말할 수 없습니다. 생태계가 매우 작으며 현재 비동기/대기 지원을 제대로 받지 못하고 있습니다.

그렇긴 하지만, 사용 가능한 솔루션을 만지작거리며 어떻게 작동하는지 확인하는 것은 유용한 연습입니다. 이 튜토리얼에서는 전자 메일을 보내고 Rust의 IMAP 서버에 연결하는 방법에 대해 설명합니다.

## 편지를 사용하여 전자 메일 만들기

가장 인기 있는 이메일 상자는 `레트레`다. 현재 안정적인 최신 문자 릴리즈(0.9)에는 비동기 API가 포함되어 있지 않지만, 버전 0.10에는 tokio와 sync-std를 통한 비동기화를 지원하는 alpha 릴리즈도 있다.

먼저 비동기 없이 현재 버전을 확인한 다음 알파 릴리스를 사용하여 비동기 구현에 코드를 포트합니다.

Cargo.toml에 다음을 추가하여 시작하십시오.

```undefined
[dependencies]
lettre = "0.9"
lettre_email = "0.9"
```

0.9 버전에서는 lettre_email을 사용하여 이메일을 작성하고, lettre를 사용하여 이메일을 전송해야 하므로 둘 다 지정해야 합니다.

기본 전자 메일을 만드는 것부터 시작합니다.

```undefined
use lettre_email::EmailBuilder;

fn main() {
    let email = EmailBuilder::new()
        .to("hello@example.com")
        .from("me@hello.com")
        .subject("Example subject")
        .text("Hello, world!")
        .build()
        .unwrap();
}
```

작성기 패턴을 사용하여 전자 메일을 나타내는 데이터 구조를 만듭니다. 이름과 주소를 지정하려면 다음과 같은 튜플을 사용할 수 있습니다.

```undefined
let email = EmailBuilder::new()
    .to(("hello@example.com", "Alice Smith"))
    .from(("me@hello.com", "Bob Smith"))
    .subject("Example subject")
    .text("Hello, world!")
    .build()
    .unwrap();
```

HTML 본문을 추가하는 것은 빌더에서 해당 메소드를 호출하는 것만큼 간단합니다.

```undefined
let email = EmailBuilder::new()
    .to("hello@example.com")
    .from("me@hello.com")
    .subject("Example subject")
    .html("<h1>Hello, world!</h1>")
    .build()
    .unwrap();
```

이메일과 함께 첨부 파일을 보내고 싶다고 가정해 보겠습니다. 다음은 이메일에 로컬 파일을 첨부하는 방법입니다.

```php
use lettre_email::mime;
use lettre_email::EmailBuilder;
use std::path::Path;

fn main() {
    let email = EmailBuilder::new()
        .to("hello@example.com")
        .from("me@hello.com")
        .subject("Example subject")
        .html("<h1>Hello, world!</h1>")
        .attachment_from_file(
            &Path::new("path/to/file.pdf"), // Path to file on disk
            Some("Cookie-recipe.pdf"),      // Filename to use in the email
            &mime::APPLICATION_PDF,
        )
        .unwrap()
        .build()
        .unwrap();
}
```

또는 `attachment(...)` builder 방법을 사용하여 디스크의 파일 대신 메모리에서 직접 바이트 벡터를 전송할 수 있다.

제가 언급하지 않은 몇 가지 작성기 방법이 더 있습니다. 따라서 이메일로 다른 작업을 수행하려면 `lettre_mail:: EmailBuilder`입니다.

## 편지와 함께 전자 메일 보내기

레터를 사용하면 `레터:이전에 작성한 이메일을 전송하기 위한 특성입니다.

가장 쉽게 출발할 수 있는 교통수단은 스텁 트랜스포트다. 이름에서 알 수 있듯이, 이것은 단지 스텁일 뿐이지 실제 운송은 아닙니다. 이것은 테스트에 가장 유용합니다.

```php
use lettre::stub::StubTransport;
use lettre::Transport;
use lettre_email::EmailBuilder;

fn main() {
    let email = EmailBuilder::new()
        .to("hello@example.com")
        .from("me@hello.com")
        .subject("Example subject")
        .html("<h1>Hello, world!</h1>")
        .build()
        .unwrap();

    let mut mailer = StubTransport::new_positive();

    let result = mailer.send(email.into());

    println!("{:?}", result);
}
```

stub transport를 만들기 위해 new_positive()를 사용했기 때문에 send(...) 방식은 항상 성공할 것이다. 강제로 전송하지 않으려면 설명서의 `Stub Transport::new(...)`를 참조하십시오.

이것을 실제 이메일 전송에 적응시키려면, 우리는 단지 "메일 발송자"를 실제 전송으로 바꾸기만 하면 된다. 이 예에서는 `SmtpTransport`를 사용합니다.

SMTP 전송은 스텁보다 구성 옵션이 많기 때문에 `SmtpClient`라는 빌더를 사용합니다.

```php
use lettre::smtp::authentication::Credentials;
use lettre::{SmtpClient, Transport};
use lettre_email::EmailBuilder;

fn main() {
    let email = EmailBuilder::new()
        .to("hello@example.com")
        .from("me@hello.com")
        .subject("Example subject")
        .html("<h1>Hello, world!</h1>")
        .build()
        .unwrap();

    let mut mailer = SmtpClient::new_simple("smtp.hello.com")
        .unwrap()
        .credentials(Credentials::new("username".into(), "password".into()))
        .transport();

    let result = mailer.send(email.into());

    println!("{:?}", result);
}
```

이렇게 하면 실제로 연결하는 SMTP 서버를 통해 이메일이 전송됩니다. 여기서, 우리는 제공된 도메인을 사용하여 암호화된 전송을 빌드하여 TLS 인증서를 검증하는 `new_simple(...)it`를 사용하여 클라이언트를 생성했다. 이것이 `Smtp운송`을 만드는 데 권장되는 방법이다. 다행스럽게도, 그게 가장 쉬워요. 그 후, 우리는 몇 가지 자격 증명을 통과하고 스텁과 같은 이메일을 보냈습니다.

## 비동기/대기로의 포트 지정

만약 당신이 출혈의 가장자리에 사는 것이 행복하다면, 당신은 비동기식 지원을 위해 알파 버전의 편지를 사용해 보는 것이 좋을 것이다. 또한 0.10 버전의 레터에는 기능 플래그 아래에 이메일 빌더가 포함되어 있으므로 더 이상 `lettre_email`을 사용할 필요가 없습니다.

먼저 `Cargo.toml` 종속성을 변경하십시오.

```undefined
[dependencies]
lettre = { version = "0.10.0-alpha.2", features = ["builder", "tokio02-native-tls"] }
tokio = { version = "0.2", features = ["macros"] }
```

e-메일 작성 및 토키오 런타임 지원을 위한 선택적 기능을 활성화했습니다.

이제 전자 메일을 만들고 보내는 코드는 다음과 같습니다.

```undefined
use lettre::transport::smtp::authentication::Credentials;
use lettre::{AsyncSmtpTransport, Message, Tokio02Connector, Tokio02Transport};

#[tokio::main]
async fn main() {
    env_logger::init();

    let email = Message::builder()
        .to("hello@example.com".parse().unwrap())
        .from("me@hello.com".parse().unwrap())
        .subject("Example subject")
        .body("Hello, world!")
        .unwrap();

    let mailer = AsyncSmtpTransport::<Tokio02Connector>::relay("smtp.hello.com")
        .unwrap()
        .credentials(Credentials::new("username".into(), "password".into()))
        .build();

    let result = mailer.send(email).await;

    println!("{:?}", result);
}
```

여기서 가장 큰 변화는 우리가 `letter_email:: "letter:"가 포함된 EmailBuilder:메시지 및 `letter::SmtpClient(letter::)를 사용하여AsyncSmtpTransport`입니다. 이 버전의 다른 변경 사항에 주의하십시오. 예를 들어, 이제 메시지 작성기 함수에서 `to(...)` 및 `from(...)`에 주소를 전달하기 전에 주소를 구문 분석해야 합니다.

또한, 문자 0.10은 알파 릴리스이며 0.9의 모든 기능을 지원하지 않습니다. 대부분의 애플리케이션은 비동기화 없이 괜찮으니, 이 버전이 안정될 때까지 우리가 앞에서 본 동기식 버전을 자유롭게 고수하세요.

## IMAP

많은 응용 프로그램에서 전자 메일을 보내는 것이 더 중요하지만 들어오는 전자 메일을 모니터링하고 상호 작용해야 할 수도 있습니다. 러스트에서 지금 가장 좋은 방법은 imap 상자를 사용하는 것이다. 현재 버전의 레터와 마찬가지로 이 상자는 비동기 API를 제공하지 않습니다.

시작하려면 이러한 종속성을 가진 새 프로젝트를 만드십시오.

```undefined
[dependencies]
imap = "2"
native-tls = "0.2"
```

그런 다음 IMAP 서버에 연결합니다.

```undefined
use native_tls::TlsConnector;

fn main() {
    let domain = "imap.example.com";
    let tls = TlsConnector::builder().build().unwrap();
    let client = imap::connect((domain, 993), domain, &tls).unwrap();
}
```

우리는 TLS 인증서를 검증하기 위한 도메인으로 연결되는 주소로서 도메인을 한 번 더 전달해야 합니다.

클라이언트가 인증되지 않았으니 다음에 로그인합시다.

```undefined
let mut imap_session = client.login("hello@example.com", "password").unwrap();
```

이제 `immap:세션 "원격 우편함과 상호 작용하는 데 사용할 수 있습니다.

예를 들어, 받은 편지함의 처음 다섯 개의 전자 메일 본문을 읽어 보겠습니다.

```bash
imap_session.select("INBOX").unwrap();

let messages = imap_session.fetch("1,2,3,4,5", "RFC822").unwrap();

for message in messages.iter() {
    if let Some(body) = message.body() {
        println!("{}", std::str::from_utf8(body).unwrap());
    } else {
        println!("Message didn't have a body!");
    }
}

imap_session.logout().unwrap();
```

먼저 작업할 사서함을 선택합니다. 이 경우 "INBOX"를 선택합니다. 그런 다음 받은 편지함에 메시지 번호 1에서 5로 가져옵니다. 가져오기 방법에서 볼 수 있는 "RFC822"는 전자 메일 본문의 형식을 나타냅니다. 일단 우리가 그들에게 반복하는 메시지가 포함된 응답을 받으면, 우리는 각 본문을 추출하여 인쇄합니다. 마지막으로 로그아웃으로 세션을 종료합니다.

메시지에서 얻을 수 있는 다른 항목에 대한 설명은 `Fetch` 유형을 참조하십시오.

이메일을 읽는 것 외에도 우편함을 관리하는 방법에 대해 알아보겠습니다.

생성 및 삭제하기

```css
imap_session.create("work").unwrap();
imap_session.delete("work").unwrap();
```

`을(를) 생성하려고 하면 생성에 실패합니다.받은 편지함" 또는 이미 있는 편지함입니다. 마찬가지로 ``을(를) 삭제하려고 하면 삭제가 실패합니다.받은 편지함" 또는 존재하지 않는 사서함입니다.

마지막으로, 사서함에서 변경 사항을 모니터링하는 방법에 대해 알아보겠습니다.

```css
imap_session.select("INBOX").unwrap();
imap_session.idle().unwrap().wait();
```

여기서 유휴()는 기다릴 수 있는 핸들()을 반환합니다. 선택한 편지함이 변경될 때까지 차단됩니다. `wait_keepalive()를 사용하여 keepalive로 대기하고 `wait_with_timeout(Duration)`을 사용하여 시간 초과로 블록할 수도 있습니다. 변경 사항이 감지될 때까지 차단하는 기능은 더 많은 리소스를 소비하는 느린 루프에서 폴링을 방지하는 데 도움이 되기 때문에 유용합니다.

내 생각에는 imap은 머리 감싸기 쉬운 API가 좋은 것 같아. 세션 문서를 자세히 읽어보면 이 상자가 무엇을 할 수 있는지 잘 알 수 있을 것입니다.

이러한 예제에 사용된 대부분의 방법은 네트워크 요청을 하므로 실패할 수 있으며 실패할 수도 있습니다. 단순성을 위해 언랩()을 자유자재로 사용했지만, 실제 적용 시 에러는 확실히 처리하셔야 합니다.

## 결론

전반적으로, Rust의 이메일 지원은 아직 눈에 띄지 않습니다. 비동기/대기 없이 전자 메일을 보내는 것은 최상의 지원을 제공하므로 필요한 경우 이미 사용해도 됩니다. 수신 전자 메일을 처리하려면 immap 상자를 사용하는 것이 가장 좋습니다.

이 기사에서는 POP3에 대해 언급하지 않았으며, 정당한 이유 때문에 Rust의 POP3 지원은 사실상 존재하지 않습니다.

이메일에 비동기/대기 기능이 절대적으로 필요한 애플리케이션의 경우 Rust를 아직 사용하지 않습니다. 이러한 상자를 다른 비동기 애플리케이션으로 슬롯화하려는 경우 비동기 런타임에서 제공하는 스레드 풀 구현을 사용할 수 있습니다. 예를 들어 `tokio::task::spawn_blocking(...)을(를) 참조하십시오.

Rust의 웹 에코시스템 대부분은 이미 매우 견고하기 때문에 시간이 지남에 따라 이메일 스토리가 개선될 것으로 확신합니다. 그 때까지는 전자 메일 사용량이 많은 응용 프로그램에 다른 언어를 사용하는 것이 좋습니다.