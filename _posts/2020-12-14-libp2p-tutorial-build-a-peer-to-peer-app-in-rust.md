---
layout: post
title: "libp2p 자습서: Rust에서 피어 투 피어 애플리케이션 구축"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/libp2p-rust-peer-to-peer.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/libp2p-rust-peer-to-peer.png?fit=730%2C487&ssl=1)

지난 몇 년 동안 블록체인 및 암호화폐를 둘러싼 과대광고로 인해 분산형 애플리케이션은 상당한 탄력을 받았다. 분산에 대한 관심이 급증하는 또 다른 요인은 데이터 개인 정보 보호와 독점 측면에서 웹의 대부분을 소규모 기업의 손에 맡기는 단점에 대한 인식이 커진 것이다.

어쨌든, 모든 암호화와 블록체인 기술을 떠나서라도 최근 탈중앙화 소프트웨어 분야에서 매우 흥미로운 발전이 있었다.

주목할 만한 예로는 IPFS, 새로운 분산 코딩 플랫폼 Radicle, 분산된 소셜 네트워크 Scutletbutt, 그리고 Mastodon과 같은 Fediverse 내의 더 많은 애플리케이션이 있다.

이 튜토리얼에서는 Rust와 다양한 언어의 성숙 단계에 존재하는 환상적인 `libp2p` 라이브러리를 사용하여 매우 단순한 피어 투 피어 애플리케이션을 구축하는 방법을 보여 줄 것이다.

간단한 명령줄 인터페이스를 갖춘 요리 레시피 앱을 구축하여 다음과 같은 작업을 수행할 수 있습니다.

- 레시피 만들기
- 레시피 게시
- 로컬 조리법 나열
- 네트워크에서 발견한 다른 피어 나열
- 지정된 피어의 게시된 레시피 나열
- 알고 있는 모든 피어의 모든 요리법 나열

러스트 300줄에서 이 모든 걸 할 거야 시작하겠습니다!

## 러스트 설치

따라서 최신 Rust 설치(1.47+)만 있으면 됩니다.

먼저 새 Rust 프로젝트를 만듭니다.

```bash
cargo new rust-p2p-example
cd rust-p2p-example
```

다음으로 `Cargo.toml` 파일을 편집하고 필요한 종속성을 추가합니다.

```undefined
[dependencies]
libp2p = { version = "0.31", features = ["tcp-tokio", "mdns-tokio"] }
tokio = { version = "0.3", features = ["io-util", "io-std", "stream", "macros", "rt", "rt-multi-thread", "fs", "time", "sync"] }
serde = {version = "=1.0", features = ["derive"] }
serde_json = "1.0"
once_cell = "1.5"
log = "0.4"
pretty_env_logger = "0.4"
```

위에서 언급했듯이, 우리는 피어 투 피어 네트워킹 부분에 `libp2p`를 사용할 것이다. 좀 더 구체적으로, 우리는 이것을 Tokio 비동기 런타임과 함께 사용할 것입니다. JSON 직렬화 및 역직렬화를 위해 Serde를 사용하고 로깅 및 상태 초기화를 위해 몇 개의 도우미 라이브러리를 사용합니다.

## libp2p란?

libp2p는 모듈화에 초점을 맞춘 피어투피어 애플리케이션 구축을 위한 프로토콜 집합이다.

자바스크립트, Go, Rust와 같은 여러 언어에 대한 라이브러리 구현이 있다. 이들 라이브러리는 모두 동일한 libp2p 규격을 구현하므로 Go와 함께 구축된 libp2p 클라이언트는 선택한 프로토콜 스택의 관점에서 호환되기만 하면 자바스크립트로 작성된 다른 클라이언트와 원활하게 상호 작용할 수 있다. 이러한 프로토콜은 기본 네트워크 전송 프로토콜에서 보안 계층 프로토콜 및 멀티플렉싱에 이르기까지 광범위한 범위를 포함한다.

우리는 이 게시물에서 libp2p의 세부 사항에 대해 깊이 파고들지는 않겠지만, 만약 여러분이 더 깊이 파고들기를 원한다면, 공식 libp2p의 문서들은 우리가 앞으로 만나게 될 다양한 개념들에 대한 아주 멋진 개요를 제공한다.

## libp2p의 작동 방식

libp2p가 동작하는 모습을 보려면 레시피 앱을 시작해보자. 먼저 필요한 몇 가지 상수와 유형을 정의합니다.

```php
const STORAGE_FILE_PATH: &str = "./recipes.json";

type Result<T> = std::result::Result<T, Box<dyn std::error::Error + Send + Sync + 'static>>;

static KEYS: Lazy<identity::Keypair> = Lazy::new(|| identity::Keypair::generate_ed25519());
static PEER_ID: Lazy<PeerId> = Lazy::new(|| PeerId::from(KEYS.public()));
static TOPIC: Lazy<Topic> = Lazy::new(|| Topic::new("recipes"));
```

Seagate는 앱이 실행 파일과 동일한 폴더에 있을 것으로 예상하는 간단한 JSON 파일인 `recipes.json`에 로컬 레시피를 저장할 것입니다. 또한 임의 오류를 전파할 수 있는 `결과`에 대한 도우미 유형을 정의합니다.

그런 다음 `once_cell::게으름뱅이, 게으르게 몇 가지를 초기화하다. 무엇보다도, 우리는 이것을 공개 키에서 파생된 키 쌍과 이른바 `피어 아이디`를 생성하는 데 사용한다. 우리는 또한 libp2p의 또 다른 핵심 개념인 `토픽`을 만든다.

이게 다 무슨 뜻이죠? 간단히 말해서, `PeerId`는 전체 피어 투 피어 네트워크 내의 특정 피어에 대한 고유 식별자일 뿐이다. 우리는 그것의 고유성을 보장하기 위해 키 쌍에서 그것을 도출한다. 또한 키 쌍을 통해 다른 네트워크와의 안전하게 통신할 수 있으므로 아무도 우리를 가장할 수 없습니다.

반면 주제는 libp2p의 펍/서브 인터페이스를 구현한 Floodsub의 개념이다. 주제란 예를 들어 펍/서브 네트워크에서 트래픽의 일부만 듣기 위해 우리가 `구독`하고 메시지를 보낼 수 있는 것이다.

레시피에는 몇 가지 유형도 필요합니다.

```cpp
type Recipes = Vec<Recipe>;

#[derive(Debug, Serialize, Deserialize)]
struct Recipe {
    id: usize,
    name: String,
    ingredients: String,
    instructions: String,
    public: bool,
}
```

또한 전달할 메시지의 몇 가지 유형은 다음과 같습니다.

```undefined
#[derive(Debug, Serialize, Deserialize)]
enum ListMode {
    ALL,
    One(String),
}

#[derive(Debug, Serialize, Deserialize)]
struct ListRequest {
    mode: ListMode,
}

#[derive(Debug, Serialize, Deserialize)]
struct ListResponse {
    mode: ListMode,
    data: Recipes,
    receiver: String,
}

enum EventType {
    Response(ListResponse),
    Input(String),
}
```

그 레시피는 꽤 간단하다. ID, 이름, 재료, 실행 지침 등이 있습니다. 또 공유하고자 하는 레시피와 보관하고자 하는 레시피를 구분할 수 있도록 퍼블릭 플래그를 추가했다.

처음에 언급한 것처럼 다른 피어에서 목록을 가져올 수 있는 방법은 두 가지가 있습니다. 모두 또는 하나에서 목록을 가져올 수 있으며, 이 방법은 `목록 모드` 열거형으로 표시됩니다.

요청 목록 및 응답 목록 유형은 이 유형과 해당 유형을 사용하여 보낸 날짜에 대한 래퍼일 뿐입니다.

`이벤트 유형`은 다른 피어의 응답과 우리 자신의 입력을 구별한다. 왜 이 차이가 관련이 있는지 나중에 알아보겠습니다.

### libp2p 클라이언트 생성

피어 투 피어 네트워크 내에서 피어를 설정하는 `메인` 함수 작성을 시작해 보겠습니다.

```php
#[tokio::main]
async fn main() {
    pretty_env_logger::init();

    info!("Peer Id: {}", PEER_ID.clone());
    let (response_sender, mut response_rcv) = mpsc::unbounded_channel();

    let auth_keys = Keypair::<X25519Spec>::new()
        .into_authentic(&KEYS)
        .expect("can create auth keys");
```

우리는 로깅을 초기화하고 비동기 `채널`을 만들어 애플리케이션의 다른 부분 간에 통신한다. 나중에 이 채널을 사용하여 `libp2p` 네트워크 스택의 응답을 응용 프로그램에 다시 전송하여 처리하도록 하겠습니다.

또한 네트워크 내의 트래픽을 보호하는 데 사용할 노이즈 암호화 프로토콜에 대한 인증 키를 생성합니다. 이를 위해, 우리는 새로운 키 쌍을 만들고 `into_trigent` 함수를 사용하여 ID 키로 서명한다.

다음 단계는 중요하며 `libp2p`의 핵심 개념인 이른바 Transport.autificate(NoiseConfig::xx(authkeys) 생성과 관련이 있습니다.인증() // XX 핸드셰이크 패턴으로, IX도 존재하며 IK - 현재 XX만 다른 libp2pimpls와 interop을 제공합니다.multiplex(mplex:MplexConfig::new() .boxed(;)입니다.

```php
    let transp = TokioTcpConfig::new()
        .upgrade(upgrade::Version::V1)
        .authenticate(NoiseConfig::xx(auth_keys).into_authenticated())
        .multiplex(mplex::MplexConfig::new())
        .boxed();
```

전송은 피어 간의 연결 지향 통신을 가능하게 하는 네트워크 프로토콜 집합입니다. 또한 하나의 애플리케이션 내에서 여러 전송(예: TCP/IP 및 Websockets)을 동시에 사용할 수도 있습니다. 즉, 여러 사용 사례에 대해 UDP를 동시에 사용할 수도 있습니다.

이 예에서는 Tokio의 비동기 TCP를 사용하여 TCP를 기준으로 사용합니다. TCP 연결이 설정되면, 안전한 통신을 위해 `노이즈`를 사용하도록 `업그레이드`하겠습니다. 웹 기반 예로는 HTTP 위에 TLS를 사용하여 보안 연결을 만드는 것이 있습니다.

우리는 세 가지 옵션 중 하나인 `NoiseConfig:xx` 핸드셰이크 패턴을 사용합니다. 왜냐하면 이 핸드셰이크 패턴이 다른 `libp2p` 응용 프로그램과 상호 운용이 보장되는 유일한 옵션이기 때문입니다.

libp2p의 좋은 점은 Rust 클라이언트를 쓸 수 있고 다른 클라이언트가 JavaScript 클라이언트를 쓸 수 있다는 것이며, 프로토콜이 두 버전의 라이브러리에 구현되어 있는 한 그들은 여전히 쉽게 통신할 수 있다는 것이다.

마지막에는 전송을 멀티플렉싱하여 동일한 전송에서 여러 하위 스트림 또는 연결을 멀티플렉싱할 수 있다.

휴, 꽤 이론적이네요! 그러나 이 모든 것은 libp2p 문서에서 찾을 수 있다. 이는 피어 투 피어 전송을 생성하는 여러 가지 방법 중 하나에 불과합니다.

다음 개념은 `네트워크 행동`이다. 이 부분은 `libp2p` 내에서 네트워크 및 모든 피어의 논리를 실제로 정의하는 부분(예: 수신 이벤트와 전송할 이벤트)입니다.

```php
    let mut behaviour = RecipeBehaviour {
        floodsub: Floodsub::new(PEER_ID.clone()),
        mdns: TokioMdns::new().expect("can create mdns"),
        response_sender,
    };

    behaviour.floodsub.subscribe(TOPIC.clone());
```

이 경우 위에서 언급한 바와 같이 FloodSub 프로토콜을 사용하여 이벤트를 처리할 것입니다. 또한 로컬 네트워크에서 다른 피어를 검색하기 위한 프로토콜인 mDNS를 사용할 것입니다. 또한 송신자 부분을 여기에 넣어 이벤트를 애플리케이션의 주요 부분으로 다시 전파하는 데 사용할 수 있도록 하겠습니다.

이전에 만든 FloodSub 주제는 현재 우리의 행동으로부터 구독되고 있습니다. 이것은 우리가 이벤트를 받을 것이고 그 주제에 대한 이벤트를 보낼 수 있다는 것을 의미합니다.

libp2p 설정이 거의 완료되었습니다. 우리에게 필요한 마지막 개념은 `워마`이다.

```php
    let mut swarm = SwarmBuilder::new(transp, behaviour, PEER_ID.clone())
        .executor(Box::new(|fut| {
            tokio::spawn(fut);
        }))
        .build();
```

"Swarm"은 전송을 사용하여 생성된 연결을 관리하고 우리가 생성한 네트워크 동작을 실행하며 이벤트를 트리거하고 수신하며 외부에서 이를 확인할 수 있는 방법을 제공합니다.

전송, 동작 및 피어 ID를 사용하여 "Swarm"을 생성합니다. 실행자 부분은 단순히 "Swarm"에게 "Tokio" 런타임으로 내부적으로 실행하라고 하지만 우리는 다른 비동기 런타임도 사용할 수 있다.

이제 우리의 `전쟁`을 시작하는 일만 남았다.

```cpp
    Swarm::listen_on(
        &mut swarm,
        "/ip4/0.0.0.0/tcp/0"
            .parse()
            .expect("can get a local socket"),
    )
    .expect("swarm can be started");
```

예를 들어 TCP 서버를 시작하는 것과 마찬가지로 로컬 IP를 사용하여 간단히 `listen_on`이라고 불러 OS에서 포트를 결정하도록 합니다. 이것이 우리의 모든 설정으로 `전쟁`을 시작하겠지만, 우리는 아직 어떤 논리도 정의하지 않았다.

사용자 입력 처리부터 시작하겠습니다.

## 'libp2p' 입력 처리

사용자 입력의 경우 기존 STDIN을 그대로 사용합니다. 따라서 `Swarm::listen_on` 호출 전에 다음을 추가합니다.

```php
    let mut stdin = tokio::io::BufReader::new(tokio::io::stdin()).lines();
```

이것은 STDIN에 비동기식 판독기를 정의했고, 이 판독기는 스트림을 라인별로 읽습니다. 그래서 Enter 키를 누르면 새로운 메시지가 나타납니다.

다음 부분은 위에서 정의한 SDDIN과 Warm, 그리고 응답 채널에서 이벤트를 들을 수 있는 이벤트 루프를 만드는 것이다.

```php
    loop {
        let evt = {
            tokio::select! {
                line = stdin.next_line() => Some(EventType::Input(line.expect("can get line").expect("can read line from stdin"))),
                event = swarm.next() => {
                    info!("Unhandled Swarm Event: {:?}", event);
                    None
                },
                response = response_rcv.recv() => Some(EventType::Response(response.expect("response exists"))),
            }
        };
        ...
    }
}
```

우리는 Tokio의 `선택` 매크로를 사용하여 몇 가지 비동기 프로세스를 기다렸다가 첫 번째 프로세스를 마칩니다. 우리는 `스워` 행사를 아무 것도 하지 않는다. 이러한 것들은 나중에 보게 될 우리의 `레시피 행동` 안에서 다루어지지만, 우리는 `스워`를 앞으로 나아가기 위해 `스워.다음()`을 불러야 한다.

`…` 대신 몇 가지 이벤트 처리 논리를 추가해 보겠습니다.

```php
        if let Some(event) = evt {
            match event {
                EventType::Response(resp) => {
                   ...
                }
                EventType::Input(line) => match line.as_str() {
                    "ls p" => handle_list_peers(&mut swarm).await,
                    cmd if cmd.starts_with("ls r") => handle_list_recipes(cmd, &mut swarm).await,
                    cmd if cmd.starts_with("create r") => handle_create_recipe(cmd).await,
                    cmd if cmd.starts_with("publish r") => handle_publish_recipe(cmd).await,
                    _ => error!("unknown command"),
                },
            }
        }
```

이벤트가 있을 경우 대응 이벤트인지 입력 이벤트인지 매칭합니다. 일단 입력 이벤트만 살펴보겠습니다.

몇 가지 옵션이 있습니다. 다음 명령을 지원합니다.

- lsp는 알려진 모든 피어를 나열합니다.
- lsr은 지역 요리법을 나열한다.
- lsr {peerId}은(는) 특정 피어에서 게시된 레시피를 나열합니다.
- lsrall은 알려진 모든 피어의 게시된 레시피를 나열합니다.
- {criter}자지정된 레시피를 게시합니다.
- creator {reecipeName}|{reecipe재료}|{레시피`지침`은 지정된 데이터와 증가된 ID로 새로운 레시피를 생성합니다.

이 경우 피어의 모든 레시피를 나열하면 피어에 대한 레시피 요청을 보내고 응답할 때까지 기다렸다가 결과를 표시할 수 있습니다. 피어 투 피어 네트워크에서는 일부 피어가 지구 반대편에 있을 수 있기 때문에 시간이 좀 걸릴 수 있으며, 이러한 피어가 모두 우리에게 응답할지조차 알 수 없습니다. 예를 들어 요청을 HTTP 서버로 보내는 것과는 상당히 다릅니다.

먼저 피어를 나열하는 논리에 대해 살펴보겠습니다.

```js
async fn handle_list_peers(swarm: &mut Swarm<RecipeBehaviour>) {
    info!("Discovered Peers:");
    let nodes = swarm.mdns.discovered_nodes();
    let mut unique_peers = HashSet::new();
    for peer in nodes {
        unique_peers.insert(peer);
    }
    unique_peers.iter().for_each(|p| info!("{}", p));
}
```

이 경우 mDNS를 사용하여 검색된 모든 노드를 제공하고 해당 노드를 반복 및 표시할 수 있습니다. 쉬워요.

다음에 목록 명령을 실행하기 전에 레시피를 만들고 게시하는 과정을 살펴보겠습니다.

```bash
async fn handle_create_recipe(cmd: &str) {
    if let Some(rest) = cmd.strip_prefix("create r") {
        let elements: Vec<&str> = rest.split("|").collect();
        if elements.len() < 3 {
            info!("too few arguments - Format: name|ingredients|instructions");
        } else {
            let name = elements.get(0).expect("name is there");
            let ingredients = elements.get(1).expect("ingredients is there");
            let instructions = elements.get(2).expect("instructions is there");
            if let Err(e) = create_new_recipe(name, ingredients, instructions).await {
                error!("error creating recipe: {}", e);
            };
        }
    }
}

async fn handle_publish_recipe(cmd: &str) {
    if let Some(rest) = cmd.strip_prefix("publish r") {
        match rest.trim().parse::<usize>() {
            Ok(id) => {
                if let Err(e) = publish_recipe(id).await {
                    info!("error publishing recipe with id {}, {}", id, e)
                } else {
                    info!("Published Recipe with id: {}", id);
                }
            }
            Err(e) => error!("invalid id: {}, {}", rest.trim(), e),
        };
    }
}
```

두 경우 모두 문자열을 구문 분석하여 `|`로 구분된 데이터 또는 `게시`의 경우 지정된 Recipe ID로 이동하여 지정된 입력이 유효하지 않을 경우 오류를 기록해야 한다.

생성`의 경우 주어진 데이터를 사용하여 `create_new_reecipe` 도우미 함수를 호출한다. 간단한 로컬 JSON 스토리지와 상호 작용하여 레시피를 제공하는 데 필요한 모든 도우미 기능을 확인해 보겠습니다.

```js
async fn create_new_recipe(name: &str, ingredients: &str, instructions: &str) -> Result<()> {
    let mut local_recipes = read_local_recipes().await?;
    let new_id = match local_recipes.iter().max_by_key(|r| r.id) {
        Some(v) => v.id + 1,
        None => 0,
    };
    local_recipes.push(Recipe {
        id: new_id,
        name: name.to_owned(),
        ingredients: ingredients.to_owned(),
        instructions: instructions.to_owned(),
        public: false,
    });
    write_local_recipes(&local_recipes).await?;

    info!("Created recipe:");
    info!("Name: {}", name);
    info!("Ingredients: {}", ingredients);
    info!("Instructions:: {}", instructions);

    Ok(())
}

async fn publish_recipe(id: usize) -> Result<()> {
    let mut local_recipes = read_local_recipes().await?;
    local_recipes
        .iter_mut()
        .filter(|r| r.id == id)
        .for_each(|r| r.public = true);
    write_local_recipes(&local_recipes).await?;
    Ok(())
}

async fn read_local_recipes() -> Result<Recipes> {
    let content = fs::read(STORAGE_FILE_PATH).await?;
    let result = serde_json::from_slice(&content)?;
    Ok(result)
}

async fn write_local_recipes(recipes: &Recipes) -> Result<()> {
    let json = serde_json::to_string(&recipes)?;
    fs::write(STORAGE_FILE_PATH, &json).await?;
    Ok(())
}
```

가장 기본적인 구성 요소는 read_local_reecipes와 write_local_reecipes로, 스토리지 파일에서 또는 파일로 레시피를 읽고, 역직렬화 또는 직렬화하여 쓰는 것이다.

publish_reecipe 도우미는 파일에서 모든 레시피를 가져오고 ID가 지정된 레시피를 찾아 공개 플래그를 true로 설정합니다.

레시피를 만들 때, 우리는 또한 파일에서 모든 레시피를 가져오고, 마지막에 새로운 레시피를 추가하고, 전체 데이터를 다시 쓰면서 파일을 덮어씁니다. 이것은 매우 효율적이지는 않지만, 간단하고 효과가 있습니다.

## 'libp2p'를 사용하여

다음에 `list` 명령을 살펴보고 다른 피어에 메시지를 보내는 방법에 대해 알아보겠습니다.

list 명령에는 세 가지 경우가 있습니다.

```php
async fn handle_list_recipes(cmd: &str, swarm: &mut Swarm<RecipeBehaviour>) {
    let rest = cmd.strip_prefix("ls r ");
    match rest {
        Some("all") => {
            let req = ListRequest {
                mode: ListMode::ALL,
            };
            let json = serde_json::to_string(&req).expect("can jsonify request");
            swarm.floodsub.publish(TOPIC.clone(), json.as_bytes());
        }
        Some(recipes_peer_id) => {
            let req = ListRequest {
                mode: ListMode::One(recipes_peer_id.to_owned()),
            };
            let json = serde_json::to_string(&req).expect("can jsonify request");
            swarm.floodsub.publish(TOPIC.clone(), json.as_bytes());
        }
        None => {
            match read_local_recipes().await {
                Ok(v) => {
                    info!("Local Recipes ({})", v.len());
                    v.iter().for_each(|r| info!("{:?}", r));
                }
                Err(e) => error!("error fetching local recipes: {}", e),
            };
        }
    };
}
```

수신 명령을 구문 분석하여 `lsr` 부분을 제거하고 남은 부분을 확인합니다. 명령에 다른 내용이 없으면 이전 섹션에 정의된 도우미를 사용하여 로컬 레시피를 가져와 인쇄할 수 있습니다.

`all` 키워드가 발생하면 `ListMode:ALL을 설정하여 JSON에 직렬화하고, Warm에 있는 FloodSub 인스턴스를 사용하여 앞서 언급한 Topic에 게시합니다.

명령에서 피어 ID가 발견될 경우에도 동일한 현상이 발생합니다. 이 경우 "ListMode:피어 ID를 가진 하나의 모드입니다. 유효한 피어 ID인지 또는 발견한 피어 ID인지 확인할 수 있습니다. 간단히 말해 들어줄 사람이 없으면 아무 일도 일어나지 않습니다.

네트워크에 메시지를 보내려면 이 정도만 하면 됩니다. 이제 문제는, 이 메시지들은 어떻게 되는가 하는 것입니다. 그들은 어디에서 취급됩니까?

피어투피어 애플리케이션의 경우, 우리는 이벤트의 발신자(Sender)와 수신자(Receiver)이기 때문에 구현 시 송신 이벤트와 수신 이벤트를 모두 처리해야 한다는 점을 기억하십시오.

## libp2p로 메시지에 응답

이것이 마침내 우리의 `레시피 행동`이 나오는 부분이다. 정의해 보겠습니다.

```css
#[derive(NetworkBehaviour)]
struct RecipeBehaviour {
    floodsub: Floodsub,
    mdns: TokioMdns,
    #[behaviour(ignore)]
    response_sender: mpsc::UnboundedSender<ListResponse>,
}
```

동작 자체는 단순한 구조이지만 libp2p의 네트워크 행동 파생 매크로를 사용하기 때문에 모든 특성 기능을 직접 구현할 필요는 없다.

이 파생 매크로는 `행동(무시)`으로 주석이 달린 구조체의 모든 구성원에 대해 `네트워크 행동` 특성의 기능을 구현한다. 우리의 채널은 우리의 행동과 직접 관련이 없기 때문에 여기서 무시된다.

남은 것은 `플로우 서브이벤트`와 `Mdns이벤트`에 대한 `inject_event` 기능을 모두 구현하는 것이다.

mDNS부터 시작하겠습니다.

```php
impl NetworkBehaviourEventProcess<MdnsEvent> for RecipeBehaviour {
    fn inject_event(&mut self, event: MdnsEvent) {
        match event {
            MdnsEvent::Discovered(discovered_list) => {
                for (peer, _addr) in discovered_list {
                    self.floodsub.add_node_to_partial_view(peer);
                }
            }
            MdnsEvent::Expired(expired_list) => {
                for (peer, _addr) in expired_list {
                    if !self.mdns.has_node(&peer) {
                        self.floodsub.remove_node_from_partial_view(&peer);
                    }
                }
            }
        }
    }
}
```

이 처리기에 대한 이벤트가 들어오면 inject_event 함수가 호출됩니다. mDNS 쪽에서는 검색됨과 만료됨이라는 두 가지 이벤트만 있는데, 이는 네트워크에서 새로운 피어가 보이거나 기존 피어가 사라졌을 때 발생한다. 두 경우 모두 메시지를 전파할 노드 목록인 FloodSub "partial view"에 추가하거나 제거한다.

펍/서브 이벤트에 대한 inject_event는 좀 더 복잡하다. 수신되는 ListRequest와 List Response 페이로드에 모두 대응해야 한다. List Request를 보내면 요청을 받은 피어가 로컬에서 게시된 레시피를 가져온 다음 다시 보낼 수 있는 방법이 필요합니다.

해당 파일을 요청 피어로 다시 보내는 유일한 방법은 네트워크에 게시하는 것입니다. 펍/서브만이 실제로 우리가 가지고 있는 유일한 메커니즘이기 때문에, 우리는 들어오는 요청과 들어오는 응답에 모두 대응할 필요가 있다.

어떻게 작동하는지 살펴보겠습니다.

```php
impl NetworkBehaviourEventProcess<FloodsubEvent> for RecipeBehaviour {
    fn inject_event(&mut self, event: FloodsubEvent) {
        match event {
            FloodsubEvent::Message(msg) => {
                if let Ok(resp) = serde_json::from_slice::<ListResponse>(&msg.data) {
                    if resp.receiver == PEER_ID.to_string() {
                        info!("Response from {}:", msg.source);
                        resp.data.iter().for_each(|r| info!("{:?}", r));
                    }
                } else if let Ok(req) = serde_json::from_slice::<ListRequest>(&msg.data) {
                    match req.mode {
                        ListMode::ALL => {
                            info!("Received ALL req: {:?} from {:?}", req, msg.source);
                            respond_with_public_recipes(
                                self.response_sender.clone(),
                                msg.source.to_string(),
                            );
                        }
                        ListMode::One(ref peer_id) => {
                            if peer_id == &PEER_ID.to_string() {
                                info!("Received req: {:?} from {:?}", req, msg.source);
                                respond_with_public_recipes(
                                    self.response_sender.clone(),
                                    msg.source.to_string(),
                                );
                            }
                        }
                    }
                }
            }
            _ => (),
        }
    }
}
```

수신 메시지와 일치하여 요청 또는 응답에 대한 역직렬화를 시도합니다. 응답의 경우 msg를 사용하여 수신되는 발신자의 피어 ID로 응답을 인쇄하기만 하면 된다.출처를 밝히다 수신 요청이 들어오면 ALL과 One 사례를 구분할 필요가 있다.

"One"의 경우 지정된 피어 ID가 우리와 동일한지, 즉 요청이 실제로 우리를 위한 것인지 여부를 확인합니다. 만약 그렇다면, 우리는 출판된 요리법을 돌려주는데, 이것은 `ALL`의 경우에도 우리의 반응이다.

두 경우 모두 `response_with_public_recipes` 도우미를 호출한다.

```php
fn respond_with_public_recipes(sender: mpsc::UnboundedSender<ListResponse>, receiver: String) {
    tokio::spawn(async move {
        match read_local_recipes().await {
            Ok(recipes) => {
                let resp = ListResponse {
                    mode: ListMode::ALL,
                    receiver,
                    data: recipes.into_iter().filter(|r| r.public).collect(),
                };
                if let Err(e) = sender.send(resp) {
                    error!("error sending response via channel, {}", e);
                }
            }
            Err(e) => error!("error fetching local recipes to answer ALL request, {}", e),
        }
    });
}
```

이 도우미 방법에서는 Tokio의 from을 사용하여 모든 로컬 레시피를 읽고 데이터에서 `ListResponse`를 생성한 후 이 데이터를 `channel_sender`를 통해 이벤트 루프로 전송하여 다음과 같이 처리한다.

```php
                EventType::Response(resp) => {
                    let json = serde_json::to_string(&resp).expect("can jsonify response");
                    swarm.floodsub.publish(TOPIC.clone(), json.as_bytes());
                }
```

응답 이벤트를 통해 "내부"가 전송되면 JSON에 직렬화하여 네트워크로 전송합니다.

## 'libp2p'로 테스트

이상으로 구현을 마치겠습니다. 이제 시험해보자.

구현이 작동하는지 확인하려면 다음 명령을 사용하여 여러 터미널에서 응용 프로그램을 시작하십시오.

```undefined
RUST_LOG=info cargo run
```

응용 프로그램에서 시작하는 디렉터리에 `reecipes.json`이라는 파일이 있어야 합니다.

응용 프로그램이 시작되면 피어 ID를 인쇄하는 다음과 같은 로그가 표시됩니다.

```undefined
INFO  rust_peer_to_peer_example > Peer Id: 12D3KooWDc1FDabQzpntvZRWeDZUL351gJRy3F4E8VN5Gx2pBCU2
```

이제 이벤트 루프를 시작하려면 Enter 키를 눌러야 합니다.

lsp를 입력하면 검색된 피어 목록이 표시됩니다.

```undefined
ls p
 INFO  rust_peer_to_peer_example > Discovered Peers:
 INFO  rust_peer_to_peer_example > 12D3KooWCK6X7mFk9HeWw69WF1ueWa3XmphZ2Mu7ZHvEECj5rrhG
 INFO  rust_peer_to_peer_example > 12D3KooWLGN85pv5XTDALGX5M6tRgQtUGMWXWasWQD6oJjMcEENA
```

`lsr`를 사용하면 다음과 같은 현지 요리법을 얻을 수 있습니다.

```css
ls r
 INFO  rust_peer_to_peer_example > Local Recipes (3)
 INFO  rust_peer_to_peer_example > Recipe { id: 0, name: " Coffee", ingredients: "Coffee", instructions: "Make Coffee", public: true }
 INFO  rust_peer_to_peer_example > Recipe { id: 1, name: " Tea", ingredients: "Tea, Water", instructions: "Boil Water, add tea", public: false }
 INFO  rust_peer_to_peer_example > Recipe { id: 2, name: " Carrot Cake", ingredients: "Carrots, Cake", instructions: "Make Carrot Cake", public: true }
```

lsrall을 호출하면 다른 피어에 요청을 전송하고 해당 레시피를 반환합니다.

```css
ls r all
 INFO  rust_peer_to_peer_example > Response from 12D3KooWCK6X7mFk9HeWw69WF1ueWa3XmphZ2Mu7ZHvEECj5rrhG:
 INFO  rust_peer_to_peer_example > Recipe { id: 0, name: " Coffee", ingredients: "Coffee", instructions: "Make Coffee", public: true }
 INFO  rust_peer_to_peer_example > Recipe { id: 2, name: " Carrot Cake", ingredients: "Carrots, Cake", instructions: "Make Carrot Cake", public: true }
```

피어 ID가 `lsr`인 경우에도 마찬가지입니다.

```css
ls r 12D3KooWCK6X7mFk9HeWw69WF1ueWa3XmphZ2Mu7ZHvEECj5rrhG
 INFO  rust_peer_to_peer_example > Response from 12D3KooWCK6X7mFk9HeWw69WF1ueWa3XmphZ2Mu7ZHvEECj5rrhG:
 INFO  rust_peer_to_peer_example > Recipe { id: 0, name: " Coffee", ingredients: "Coffee", instructions: "Make Coffee", public: true }
 INFO  rust_peer_to_peer_example > Recipe { id: 2, name: " Carrot Cake", ingredients: "Carrots, Cake", instructions: "Make Carrot Cake", public: true }
```

됐다! 같은 네트워크에 있는 엄청난 양의 클라이언트에서도 이 기능을 사용해 볼 수 있습니다.

전체 예제 코드는 GitHub에서 찾을 수 있습니다.

## 결론

이 게시물에서는 Rust와 libp2p를 사용하여 소규모 분산형 네트워크 애플리케이션을 구축하는 방법에 대해 다루었다.

웹 배경을 사용하는 경우, 많은 네트워킹 개념이 다소 익숙하겠지만 피어 투 피어 애플리케이션을 구축하려면 근본적으로 다른 설계 및 구축 방식이 필요합니다.

libp2p 라이브러리는 상당히 성숙했으며, 암호화 분야 내에서 Rust의 인기로 인해 강력한 분산형 애플리케이션을 구축하기 위한 새로운 풍부한 라이브러리 생태계가 존재한다.