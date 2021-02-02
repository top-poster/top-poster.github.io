---
layout: post
title: "Rust와 TUI : Rust에서 명령 줄 인터페이스 구축
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/rust-tui-command-line-interface.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/rust-tui-command-line-interface.png?fit=730%2C487&ssl=1)

Rust는 크로스 컴파일을 잘 지원하는 저수준 시스템 프로그래밍 언어로, 명령 줄 응용 프로그램을 작성하기위한 주요 후보입니다.
 대표적인 예는 ripgrep, exa 및 bat와 같이 널리 사용되는 도구의 재 구현에서 GitUI, Spotify TUI, Bandwhich, KMon 및 Diskonaut와 같은 본격적인 터미널 UI 도구에 이르기까지 다양합니다.
 

Alacritty 및 Nushell을 사용하면 인기있는 셸 구현도 사용할 수 있습니다.
 꾸준히 성장하고있는이 과다한 도구에 대한 몇 가지 이유에는 RIR (Rewrite it in Rust) meme과 명령 줄 애플리케이션 Rust를 작성하기위한 환상적인 생태계가 포함됩니다.
 

CLI 용 Rust의 라이브러리 에코 시스템에 대해 이야기 할 때 특히 스테이플이며 앞서 언급 한 많은 도구에서 사용되는 Clap과 TUI를 언급하고 싶습니다.
 Clap은 환상적인 API와 사용 가능한 다양한 기능이 포함 된 명령 줄 파서로, 필요하지 않은 경우 더 빠른 컴파일 시간을 위해 많은 기능을 비활성화 할 수 있습니다.
 

## TUI 란 무엇입니까?
 

TUI는 기본적으로 터미널 사용자 인터페이스를 구축하기위한 프레임 워크입니다.
 터미널에 그리기위한 여러 "백엔드"를 지원합니다.
 이러한 백엔드는 올바른 문자 설정, 화면 지우기 등과 같은 터미널과 상호 작용하기위한 실제 논리를 대신하는 반면 TUI는 사용자 인터페이스를 구성하기위한 위젯 및 기타 도우미를 제공하는 상위 수준 인터페이스입니다.
 

이 튜토리얼에서는 Crossterm을 백엔드로 사용하는 TUI를 사용하여 간단한 터미널 애플리케이션을 구현하는 방법을 살펴 보겠습니다.
 렌더링 및 이벤트 처리 파이프 라인의 초기 설정 이후에는 Crossterm과 직접 상호 작용하지 않으므로 예제는 다른 TUI 백엔드에서도 최소한의 변경으로 작동해야합니다.
 

TUI의 작동 방식을 보여주기 위해 데이터 저장을 위해 로컬 JSON 파일을 사용하여 애완 동물을 관리하기위한 간단한 앱을 빌드합니다.
 완성 된 제품은 다음과 같습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/rust-tui-example-command-line-interface.jpeg?resize=720%2C411&ssl=1)

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/rust-tui-example-command-line-interface-finished.jpeg?resize=720%2C409&ssl=1)

첫 번째 이미지는 메뉴가있는 시작 화면을 보여줍니다.
 메뉴에서 강조 표시된 문자는 사용자가 작업을 실행하기 위해 눌러야하는 핫키를 보여줍니다.
 `p`를 선택하면 펫을 관리 할 수있는 두 번째 화면 (펫)으로 이동합니다.
 사용자는 목록을 탐색하고`a`를 사용하여 임의의 새 펫을 추가하고`d`를 사용하여 현재 선택된 펫을 삭제할 수 있습니다.
 `q`를 누르면 앱이 종료됩니다.
 

앱은 다소 간단하지만 TUI의 작동 방식과 이러한 애플리케이션의 기본 블록을 구축하는 방법을 보여주는 것으로 충분합니다.
 이 튜토리얼을 위해 작성한 Rust 명령 줄 예제 코드를 사용하면 편집을 통해이를 확장하고 새 애완 동물을 추가하기위한 양식을 추가 할 수 있습니다.
 

TUI를 사용하는 경우 응용 프로그램의 크기를 조정할 수 있으며 다른 UI 요소의 구성된 비율을 제자리에 유지하면서 반응 형으로 변경됩니다.
 이것은 기존 위젯을 사용할 때 TUI가하는 많은 일 중 하나입니다.
 

더 이상 고민하지 않고 뛰어 들자!
 

## Rust 앱 설정
 

따라 가기 위해 필요한 것은 최근 Rust 설치 (1.45+, 작성 당시 가장 최근 버전은 Rust 1.49.0)뿐입니다.
 

먼저 새로운 Rust 프로젝트를 만듭니다.
 

```bash
cargo new rust-cli-example
cd rust-cli-example
```

다음으로`Cargo.toml` 파일을 편집하고 필요한 종속성을 추가합니다.
 

```undefined
[dependencies]
crossterm = { version = "0.19", features = [ "serde" ] }
serde = {version = "1.0", features = ["derive"] }
serde_json = "1.0"
chrono = { version = "0.4", features = ["serde"] }
rand = { version = "0.7.3", default-features = false, features = ["std"] }
tui = { version = "0.14", default-features = false, features = ['crossterm', 'serde'] }
thiserror = "1.0"
```

Crossterm과 함께 TUI 라이브러리를 백엔드로 사용하는 것 외에도.
 또한 Serde를 사용하여 JSON을 처리하고 Chrono를 사용하여 애완 동물 생성 날짜를 처리하고 Rand를 사용하여 새로운 임의 데이터를 만듭니다.
 

위 구성에서 볼 수 있듯이 TUI에서`crossterm` 및`serde` 기능을 선택합니다.
 

기본적인 데이터 구조와 상수를 정의하는 것으로 시작하겠습니다.
 

## 데이터 구조 정의
 

먼저 로컬 "데이터베이스"JSON 파일에 대한 상수와 애완 동물이 어떻게 생겼는지에 대한 구조체를 정의하겠습니다.
 

```undefined
const DB_PATH: &str = "./data/db.json";

#[derive(Serialize, Deserialize, Clone)]
struct Pet {
    id: usize,
    name: String,
    category: String,
    age: usize,
    created_at: DateTime<Utc>,
}
```

데이터베이스 파일을 다룰 때 I / O 오류가 발생할 수 있습니다.
 가능한 모든 오류에 대해 UI 내에서 전체 오류 처리를 구현하는 것은 범위를 벗어 났지만 여전히 몇 가지 내부 오류 유형을 갖고 싶습니다.
 

```undefined
#[derive(Error, Debug)]
pub enum Error {
    #[error("error reading the DB file: {0}")]
    ReadDBError(#[from] io::Error),
    #[error("error parsing the DB file: {0}")]
    ParseDBError(#[from] serde_json::Error),
}
```

`db.json` 파일은`Pet` 구조체 목록의 JSON 표현입니다.
 

```undefined
[
    {
        "id": 1,
        "name": "Chip",
        "category": "cats",
        "age": 4,
        "created_at": "2020-09-01T12:00:00Z"
    },
    ...
]
```

입력 이벤트를위한 데이터 구조도 필요합니다.
 TUI 예제 저장소 내의 Crossterm 예제에서 사용한 것과 동일한 접근 방식을 사용합니다.
 

```cpp
enum Event<I> {
    Input(I),
    Tick,
}
```

이벤트는 사용자의 입력이거나 단순히 `틱`입니다.
 틱 속도 (예 : 200 밀리 초)를 정의하고 해당 틱 속도 내에서 입력 이벤트가 발생하지 않으면 `Tick`을 내 보냅니다.
 그렇지 않으면 입력이 방출됩니다.
 

마지막으로 메뉴 구조에 대한`열거 형`을 정의하여 응용 프로그램에서 현재 위치를 쉽게 확인할 수 있습니다.
 

```php
#[derive(Copy, Clone, Debug)]
enum MenuItem {
    Home,
    Pets,
}

impl From<MenuItem> for usize {
    fn from(input: MenuItem) -> usize {
        match input {
            MenuItem::Home => 0,
            MenuItem::Pets => 1,
        }
    }
}
```

현재 홈과 애완 동물의 두 페이지 만 있으며,이를 `usize`로 변환하는 방법을 구현했습니다.
 이를 통해 TUI의`Tabs` 구성 요소에있는 열거 형을 사용하여 메뉴에서 현재 선택된 탭을 강조 표시 할 수 있습니다.
 

이 초기 설정을 중단하고 TUI 및 Crossterm을 설정하여 화면에 사물을 렌더링하고 사용자 이벤트에 반응 할 수 있도록하겠습니다.
 

## 렌더링 및 입력
 

먼저 터미널을 `원시`(또는 비표준) 모드로 설정하면 사용자가 `Enter`가 입력에 반응 할 때까지 기다릴 필요가 없습니다.
 

```undefined
fn main() -> Result<(), Box<dyn std::error::Error>> {
    enable_raw_mode().expect("can run in raw mode");
...
```

다음으로, 입력 핸들러와 렌더링 루프간에 통신 할`mpsc` (다중 생산자, 단일 소비자) 채널을 설정합니다.
 

```undefined
    let (tx, rx) = mpsc::channel();
    let tick_rate = Duration::from_millis(200);
    thread::spawn(move || {
        let mut last_tick = Instant::now();
        loop {
            let timeout = tick_rate
                .checked_sub(last_tick.elapsed())
                .unwrap_or_else(|| Duration::from_secs(0));

            if event::poll(timeout).expect("poll works") {
                if let CEvent::Key(key) = event::read().expect("can read events") {
                    tx.send(Event::Input(key)).expect("can send events");
                }
            }

            if last_tick.elapsed() >= tick_rate {
                if let Ok(_) = tx.send(Event::Tick) {
                    last_tick = Instant::now();
                }
            }
        }
    });
```

채널을 만든 후 앞서 언급 한 틱 속도를 정의하고 스레드를 생성합니다.
 여기에서 입력 루프를 수행합니다.
 이 스 니펫은 TUI 및 Crossterm을 사용하여 입력 루프를 설정하는 데 권장되는 방법입니다.
 

아이디어는 다음 틱 (`timeout`)을 계산 한 다음`event :: poll`을 사용하여 이벤트가 발생할 때까지 기다린 다음 이벤트가있는 경우 사용자가 누른 키로 채널을 통해 해당 입력 이벤트를 전송하는 것입니다.
 

이 타임 아웃 내에 사용자 이벤트가 발생하지 않으면 단순히 `Tick`이벤트를 보내고 처음부터 시작합니다.
 `tick_rate`를 사용하면 애플리케이션의 응답 성을 조정할 수 있습니다.
 그러나 너무 낮게 설정하면이 루프가 많이 실행되고 리소스가 소모됩니다.
 

이 로직은 애플리케이션을 렌더링하기 위해 메인 스레드가 필요하기 때문에 다른 스레드에서 생성됩니다.
 이렇게하면 입력 루프가 렌더링을 차단하지 않습니다.
 

`Crossterm`백엔드로 TUI `Terminal`을 설정하는 데 필요한 몇 가지 단계가 있습니다.
 

```cpp
    let stdout = io::stdout();
    let backend = CrosstermBackend::new(stdout);
    let mut terminal = Terminal::new(backend)?;
    terminal.clear()?;
```

우리는`stdout`을 사용하여`CrosstermBackend`를 정의하고 TUI`Terminal`에서 사용하여 처음에는 지우고 모든 것이 작동하는지 암시 적으로 확인했습니다.
 이 중 하나라도 실패하면 당황하고 응용 프로그램이 중지됩니다.
 이벤트 렌더링에 대한 변경 사항을 얻지 못했기 때문에 여기서 반응 할 좋은 방법이 없습니다.
 

이것으로 입력 루프와 그릴 수있는 터미널을 만드는 데 필요한 상용구 설정을 마칩니다.
 

첫 번째 UI 요소 인 탭 기반 메뉴를 만들어 보겠습니다!
 

## TUI에서 위젯 렌더링
 

메뉴를 만들기 전에 매 반복마다`terminal.draw ()`를 호출하는 렌더링 루프를 구현해야합니다.
 

```undefined
    loop {
        terminal.draw(|rect| {
            let size = rect.size();
            let chunks = Layout::default()
                .direction(Direction::Vertical)
                .margin(2)
                .constraints(
                    [
                        Constraint::Length(3),
                        Constraint::Min(2),
                        Constraint::Length(3),
                    ]
                    .as_ref(),
                )
                .split(size);
...
```

우리는`Rect`를받는 클로저와 함께`draw` 함수를 제공합니다.
 이것은 위젯이 렌더링되어야하는 위치를 정의하는 데 사용되는 TUI에서 사용되는 사각형의 레이아웃 기본 요소입니다.
 

다음 단계는 레이아웃의 `청크`를 정의하는 것입니다.
 세 개의 상자가있는 전통적인 수직 레이아웃이 있습니다.
 

- 메뉴
 
- 함유량
 
- 보행인
 

TUI의 `Layout`을 사용하여 방향과 몇 가지 제약을 설정하여이를 정의 할 수 있습니다.
 이러한 제약은 레이아웃의 다른 부분이 어떻게 구성되어야하는지 정의합니다.
 이 예에서는 메뉴 부분을 길이 3 (기본적으로 3 줄), 중간 콘텐츠 부분은 최소 2, 바닥 글 3으로 정의합니다. 즉,이 앱을 전체적으로 실행하면
 화면에서 메뉴와 바닥 글은 항상 세 줄 높이에서 일정하게 유지되는 반면 콘텐츠 부분은 나머지 크기로 커집니다.
 

이러한 제약은 강력한 시스템이며 백분율 및 비율을 설정하는 옵션도 있습니다. 나중에 살펴 보겠습니다.
 마지막으로 레이아웃을 레이아웃 청크로 분할합니다.
 

레이아웃에서 가장 간단한 UI 구성 요소는 가짜 저작권이있는 정적 바닥 글이므로 위젯을 만들고 렌더링하는 방법을 알아보기 위해 시작해 보겠습니다.
 

```php
            let copyright = Paragraph::new("pet-CLI 2020 - all rights reserved")
                .style(Style::default().fg(Color::LightCyan))
                .alignment(Alignment::Center)
                .block(
                    Block::default()
                        .borders(Borders::ALL)
                        .style(Style::default().fg(Color::White))
                        .title("Copyright")
                        .border_type(BorderType::Plain),
                );

            rect.render_widget(copyright, chunks[2]);
```

우리는 TUI에 존재하는 많은 위젯 중 하나 인`Paragraph` 위젯을 사용합니다.
 하드 코딩 된 텍스트로 단락을 정의하고, 기본 스타일에`.fg`를 사용하여 다른 전경색을 사용하여 스타일을 설정하고, 정렬을 가운데로 설정 한 다음 블록을 정의합니다.
 

이 블록은 "기본"위젯이기 때문에 중요합니다. 즉, 렌더링 할 다른 모든 위젯에서 사용할 수 있습니다.
 `블록`은 상자 내부에서 렌더링하는 콘텐츠 주위에 제목과 테두리 (선택 사항)를 넣을 수있는 영역을 정의합니다.
 

이 경우에는 3 개의 상자로 구성된 멋진 세로 레이아웃을 만들기 위해 전체 테두리가있는 "저작권"이라는 제목의 상자를 만들었습니다.
 

그런 다음`draw` 메소드의`rect`를 사용하고`render_widget`을 호출하여 단락을 레이아웃의 세 번째 부분 (하단 부분) 인`chunks [2]`로 렌더링했습니다.
 

이것이 TUI에서 위젯을 렌더링하는 모든 것입니다.
 아주 간단 하죠?
 

### 탭 기반 메뉴 만들기
 

이제 마침내 탭 기반 메뉴를 다룰 수 있습니다.
 다행히 TUI에는 `탭`위젯이 기본 제공되며이를 작동시키기 위해 많은 작업을 수행 할 필요가 없습니다.
 활성화 된 메뉴 항목에 관한 상태를 관리하려면 렌더링 루프 앞에 두 줄을 추가해야합니다.
 

```bash
...
    let menu_titles = vec!["Home", "Pets", "Add", "Delete", "Quit"];
    let mut active_menu_item = MenuItem::Home;
...
loop {
    terminal.draw(|rect| {
...
```

첫 번째 줄에서는 하드 코딩 된 메뉴 제목을 정의하고`active_menu_item`은 현재 선택된 메뉴 항목을 저장하고 처음에는`Home`으로 설정합니다.
 

하지만 페이지가 두 개뿐이므로 `Home`또는 `Pets`로만 설정되지만 기본 최상위 라우팅에이 접근 방식을 어떻게 사용할 수 있는지 상상할 수 있습니다.
 

메뉴를 렌더링하려면 먼저 메뉴 레이블을 유지하기위한`Span` (예, HTML`<span>`태그와 같은) 요소 목록을 만든 다음`Tabs` 위젯에 넣어야합니다.
 

```undefined
            let menu = menu_titles
                .iter()
                .map(|t| {
                    let (first, rest) = t.split_at(1);
                    Spans::from(vec![
                        Span::styled(
                            first,
                            Style::default()
                                .fg(Color::Yellow)
                                .add_modifier(Modifier::UNDERLINED),
                        ),
                        Span::styled(rest, Style::default().fg(Color::White)),
                    ])
                })
                .collect();

            let tabs = Tabs::new(menu)
                .select(active_menu_item.into())
                .block(Block::default().title("Menu").borders(Borders::ALL))
                .style(Style::default().fg(Color::White))
                .highlight_style(Style::default().fg(Color::Yellow))
                .divider(Span::raw("|"));

            rect.render_widget(tabs, chunks[0]);
```

하드 코딩 된 메뉴 레이블을 반복하고 각 레이블에 대해 첫 번째 문자에서 문자열을 분할합니다.
 이렇게하면 첫 번째 문자의 스타일을 다르게 지정하여 다른 색상과 밑줄을 지정하여 사용자에게이 메뉴 항목을 활성화하기 위해 입력해야하는 문자임을 나타냅니다.
 

이 연산의 결과는 단순히 `Span`목록 인 `Spans`요소입니다.
 이것은 선택적으로 스타일이 지정된 여러 텍스트 조각에 지나지 않습니다.
 그런 다음 이러한 범위는 `탭`위젯에 배치됩니다.
 

`active_menu_item`과 함께`.select ()`를 호출하고 일반 스타일과 다른 하이라이트 스타일을 설정합니다.
 즉, 메뉴 항목이 선택되면 완전히 노란색으로 표시되고 선택되지 않은 항목에는 첫 번째 문자 만 노란색으로 표시되고 나머지는 흰색으로 표시됩니다.
 또한 분할을 정의하고 스타일을 일관되게 유지하기 위해 테두리와 제목이있는 기본 블록을 다시 설정합니다.
 

저작권과 마찬가지로`chunks [0]`를 사용하여 탭 메뉴를 레이아웃의 첫 번째 부분으로 렌더링합니다.
 

이제 세 가지 레이아웃 요소 중 두 가지가 완료되었습니다.
 그러나 여기서는 `active_menu_item`을 요소가 활성화되어야하는 저장소로 사용하여 상태 저장 요소를 정의했습니다.
 이것이 어떻게 바뀌나요?
 

이 미스터리를 해결하기 위해 다음 입력 처리를 살펴 보겠습니다.
 

## TUI에서 입력 처리
 

렌더링`loop {}`내에서`terminal.draw ()`호출이 완료된 후 입력 처리라는 또 다른 코드를 추가합니다.
 

즉, 항상 현재 상태를 먼저 렌더링 한 다음 새로운 입력에 반응합니다.
 처음에 설정 한 `채널`의 수신단에서 입력을 기다리면됩니다.
 

```php
        match rx.recv()? {
            Event::Input(event) => match event.code {
                KeyCode::Char('q') => {
                    disable_raw_mode()?;
                    terminal.show_cursor()?;
                    break;
                }
                KeyCode::Char('h') => active_menu_item = MenuItem::Home,
                KeyCode::Char('p') => active_menu_item = MenuItem::Pets,
                _ => {}
            },
            Event::Tick => {}
        }
} // end of render loop
```

이벤트가 들어 오면 들어오는 키 코드를 일치시킵니다.
 사용자가 `q`를 누르면 앱을 닫고 싶다는 뜻입니다. 즉, 정리하고 싶다는 뜻이므로받은 것과 같은 상태로 단말기를 돌려줍니다.
 실제 응용 프로그램에서이 정리는 치명적인 오류에 대해서도 발생해야합니다.
 그렇지 않으면 터미널이 원시 모드로 유지되고 엉망으로 보입니다.
 

원시 모드를 비활성화하고 커서를 다시 표시하므로 사용자는 앱을 시작하기 전과 같이 "정상"터미널 상태에 있어야합니다.
 

`h`가 있으면 활성 메뉴를 `Home`으로 설정하고 `p`인 경우 `Pets`로 설정합니다.
 이것이 우리가 필요로하는 모든 라우팅 로직입니다.
 

또한`terminal.draw` 클로저 내에서이 라우팅에 대한 로직을 추가해야합니다.
 

```php
            match active_menu_item {
                MenuItem::Home => rect.render_widget(render_home(), chunks[1]),
                MenuItem::Pets => {
                  ...
                }
            }
...

fn render_home<'a>() -> Paragraph<'a> {
    let home = Paragraph::new(vec![
        Spans::from(vec![Span::raw("")]),
        Spans::from(vec![Span::raw("Welcome")]),
        Spans::from(vec![Span::raw("")]),
        Spans::from(vec![Span::raw("to")]),
        Spans::from(vec![Span::raw("")]),
        Spans::from(vec![Span::styled(
            "pet-CLI",
            Style::default().fg(Color::LightBlue),
        )]),
        Spans::from(vec![Span::raw("")]),
        Spans::from(vec![Span::raw("Press 'p' to access pets, 'a' to add random new pets and 'd' to delete the currently selected pet.")]),
    ])
    .alignment(Alignment::Center)
    .block(
        Block::default()
            .borders(Borders::ALL)
            .style(Style::default().fg(Color::White))
            .title("Home")
            .border_type(BorderType::Plain),
    );
    home
}
```

`active_menu_item`과 일치하고`Home` 인 경우 사용자에게 현재 위치를 알려주고 앱과 상호 작용하는 방법에 대한 지침을 제공하는 기본 환영 메시지를 렌더링합니다.
 

이것이 입력 처리 측면에서 지금 필요한 전부입니다.
 이것을 실행한다면, 우리는 이미 탭을 가지고 놀 수 있습니다.
 그러나 우리는 애완 동물 관리 애플리케이션의 실제 고기 인 애완 동물 취급을 놓치고 있습니다.
 

## TUI에서 상태 저장 위젯 만들기
 

좋습니다. 첫 번째 단계는 JSON 파일에서 애완 동물을로드하고 왼쪽에 애완 동물 이름 목록을 표시하여 선택한 애완 동물에 대한 애완 동물 세부 정보 (기본값은 첫 번째)를 오른쪽에 표시하는 것입니다.
 

여기에서는 약간의 면책 조항으로,이 자습서의 제한된 범위로 인해이 응용 프로그램은 오류를 제대로 처리하지 못합니다.
 파일 읽기에 실패하면 유용한 오류를 표시하는 대신 앱이 충돌합니다.
 기본적으로 = 오류 처리는 다른 UI 응용 프로그램에서와 동일한 방식으로 작동합니다. 오류를 분기하고, 예를 들어 사용자에게 문제를 해결하기위한 힌트 또는 행동 유도와 함께 유용한 오류 `Paragraph`를 표시합니다.
 

이제 이전 섹션의`active_menu_item` 일치에서 누락 된 부분을 살펴 보겠습니다.
 

```php
            match active_menu_item {
                MenuItem::Home => rect.render_widget(render_home(), chunks[1]),
                MenuItem::Pets => {
                    let pets_chunks = Layout::default()
                        .direction(Direction::Horizontal)
                        .constraints(
                            [Constraint::Percentage(20), Constraint::Percentage(80)].as_ref(),
                        )
                        .split(chunks[1]);
                    let (left, right) = render_pets(&pet_list_state);
                    rect.render_stateful_widget(left, pets_chunks[0], &mut pet_list_state);
                    rect.render_widget(right, pets_chunks[1]);
                }
            }
```

`애완 동물`페이지에있는 경우 새 레이아웃을 만듭니다.
 이번에는 목록보기와 자세히보기의 두 요소를 나란히 표시하려고하므로 가로 레이아웃이 필요합니다.
 이 경우 목록보기가 화면의 약 20 %를 차지하고 나머지는 세부 정보 테이블에 남겨 둡니다.
 

`.split`에서는 rect 크기 대신`chunks [1]`을 사용합니다.
 즉,`chunks [1]`(중간`Content` 레이아웃 청크) 영역을 설명하는 사각형을 두 개의 수평 뷰로 분할합니다.
 그런 다음`render_pets`를 호출하여 렌더링 할 왼쪽 및 오른쪽 부분을 반환하고 해당`pets_chunks`로 간단히 렌더링합니다.
 

하지만 잠깐,이게 뭐야?
 목록보기를 위해`render_stateful_widget`을 호출합니다.
 이것은 무엇을 의미합니까?
 

완전하고 환상적인 프레임 워크 인 TUI는 상태 저장 위젯을 생성 할 수 있습니다.
 `List` 위젯은 상태 저장이 가능한 위젯 중 하나입니다.
 이것이 작동하려면 렌더링 루프 전에 먼저`ListState`를 만들어야합니다.
 

```coffeescript
    let mut pet_list_state = ListState::default();
    pet_list_state.select(Some(0));

loop {
...
```

애완 동물 목록 상태를 초기화하고 기본적으로 첫 번째 항목을 선택합니다.
 이`pet_list_state`도`render_pets` 함수에 전달하는 것입니다.
 이 함수는 다음에 대한 모든 논리를 수행합니다.
 

- 데이터베이스 파일에서 애완 동물 가져 오기
 
- 목록 항목으로 변환
 
- 애완 동물 목록 만들기
 
- 현재 선택된 애완 동물 찾기
 
- 선택한 애완 동물의 데이터로 테이블 렌더링
 
- 두 위젯 모두 반환
 

상당히 많으므로 코드가 약간 길지만 나중에 살펴 보겠습니다.
 

```undefined
fn read_db() -> Result<Vec<Pet>, Error> {
    let db_content = fs::read_to_string(DB_PATH)?;
    let parsed: Vec<Pet> = serde_json::from_str(&db_content)?;
    Ok(parsed)
}

fn render_pets<'a>(pet_list_state: &ListState) -> (List<'a>, Table<'a>) {
    let pets = Block::default()
        .borders(Borders::ALL)
        .style(Style::default().fg(Color::White))
        .title("Pets")
        .border_type(BorderType::Plain);

    let pet_list = read_db().expect("can fetch pet list");
    let items: Vec<_> = pet_list
        .iter()
        .map(|pet| {
            ListItem::new(Spans::from(vec![Span::styled(
                pet.name.clone(),
                Style::default(),
            )]))
        })
        .collect();

    let selected_pet = pet_list
        .get(
            pet_list_state
                .selected()
                .expect("there is always a selected pet"),
        )
        .expect("exists")
        .clone();

    let list = List::new(items).block(pets).highlight_style(
        Style::default()
            .bg(Color::Yellow)
            .fg(Color::Black)
            .add_modifier(Modifier::BOLD),
    );

    let pet_detail = Table::new(vec![Row::new(vec![
        Cell::from(Span::raw(selected_pet.id.to_string())),
        Cell::from(Span::raw(selected_pet.name)),
        Cell::from(Span::raw(selected_pet.category)),
        Cell::from(Span::raw(selected_pet.age.to_string())),
        Cell::from(Span::raw(selected_pet.created_at.to_string())),
    ])])
    .header(Row::new(vec![
        Cell::from(Span::styled(
            "ID",
            Style::default().add_modifier(Modifier::BOLD),
        )),
        Cell::from(Span::styled(
            "Name",
            Style::default().add_modifier(Modifier::BOLD),
        )),
        Cell::from(Span::styled(
            "Category",
            Style::default().add_modifier(Modifier::BOLD),
        )),
        Cell::from(Span::styled(
            "Age",
            Style::default().add_modifier(Modifier::BOLD),
        )),
        Cell::from(Span::styled(
            "Created At",
            Style::default().add_modifier(Modifier::BOLD),
        )),
    ]))
    .block(
        Block::default()
            .borders(Borders::ALL)
            .style(Style::default().fg(Color::White))
            .title("Detail")
            .border_type(BorderType::Plain),
    )
    .widths(&[
        Constraint::Percentage(5),
        Constraint::Percentage(20),
        Constraint::Percentage(20),
        Constraint::Percentage(5),
        Constraint::Percentage(20),
    ]);

    (list, pet_detail)
}
```

먼저 JSON 파일을 읽고 Pets의 `Vec`로 파싱하는`read_db` 함수를 정의합니다.
 

그런 다음`List`와`Table` (둘 다 TUI 위젯)의 튜플을 반환하는`render_pets` 내에서 먼저 목록 뷰에 대한 주변`pets` 블록을 정의합니다.
 

애완 동물 데이터를 가져온 후 애완 동물 이름을 `ListItems`로 변환합니다.
 그런 다음`pet_list_state`를 기준으로 목록에서 선택한 애완 동물을 찾으려고합니다.
 이것이 실패하거나 애완 동물이 없으면 위에서 언급 한 것처럼 의미있는 오류 처리가 없기 때문에이 간단한 버전에서 앱이 충돌합니다.
 

선택한 애완 동물이 있으면 목록 항목으로 `목록`위젯을 만들어 강조 표시된 스타일을 정의하여 현재 선택된 애완 동물을 볼 수 있습니다.
 

마지막으로, 우리는 pet 구조체의 열 이름에 하드 코딩 된 헤더를 설정하는`pet_details` 테이블을 생성합니다.
 또한 문자열로 변환 된 각 애완 동물의 데이터를 포함하는`행`목록도 정의합니다.
 

이 테이블을 `Details`라는 제목과 테두리가있는 기본 블록으로 렌더링하고 백분율을 사용하여 5 개 열의 상대적 너비를 정의합니다.
 따라서이 테이블은 크기 조정시 반응 적으로 작동합니다.
 

이것이 애완 동물을위한 전체 렌더링 로직입니다.
 추가해야 할 것은 무작위로 새로운 애완 동물을 추가하고 선택한 애완 동물을 삭제하여 앱에 더 많은 상호 작용을 제공하는 기능입니다.
 

먼저 애완 동물 추가 및 삭제를위한 두 가지 도우미 함수를 추가합니다.
 

```php
fn add_random_pet_to_db() -> Result<Vec<Pet>, Error> {
    let mut rng = rand::thread_rng();
    let db_content = fs::read_to_string(DB_PATH)?;
    let mut parsed: Vec<Pet> = serde_json::from_str(&db_content)?;
    let catsdogs = match rng.gen_range(0, 1) {
        0 => "cats",
        _ => "dogs",
    };

    let random_pet = Pet {
        id: rng.gen_range(0, 9999999),
        name: rng.sample_iter(Alphanumeric).take(10).collect(),
        category: catsdogs.to_owned(),
        age: rng.gen_range(1, 15),
        created_at: Utc::now(),
    };

    parsed.push(random_pet);
    fs::write(DB_PATH, &serde_json::to_vec(&parsed)?)?;
    Ok(parsed)
}

fn remove_pet_at_index(pet_list_state: &mut ListState) -> Result<(), Error> {
    if let Some(selected) = pet_list_state.selected() {
        let db_content = fs::read_to_string(DB_PATH)?;
        let mut parsed: Vec<Pet> = serde_json::from_str(&db_content)?;
        parsed.remove(selected);
        fs::write(DB_PATH, &serde_json::to_vec(&parsed)?)?;
        pet_list_state.select(Some(selected - 1));
    }
    Ok(())
}
```

두 경우 모두 애완 동물 목록을로드하고 조작 한 다음 파일에 다시 씁니다.
 `remove_pet_at_index`의 경우`pet_list_state`도 줄여야하므로 목록의 마지막 애완 동물이면 자동으로 이전 애완 동물로 이동합니다.
 

마지막으로 `Up`및 `Down`을 사용하여 애완 동물 목록을 탐색하기위한 입력 처리기를 추가하고 애완 동물을 추가 및 삭제하기 위해 `a`및 `d`를 사용해야합니다.
 

```js
                KeyCode::Char('a') => {
                    add_random_pet_to_db().expect("can add new random pet");
                }
                KeyCode::Char('d') => {
                    remove_pet_at_index(&mut pet_list_state).expect("can remove pet");
                }
                KeyCode::Down => {
                    if let Some(selected) = pet_list_state.selected() {
                        let amount_pets = read_db().expect("can fetch pet list").len();
                        if selected >= amount_pets - 1 {
                            pet_list_state.select(Some(0));
                        } else {
                            pet_list_state.select(Some(selected + 1));
                        }
                    }
                }
                KeyCode::Up => {
                    if let Some(selected) = pet_list_state.selected() {
                        let amount_pets = read_db().expect("can fetch pet list").len();
                        if selected > 0 {
                            pet_list_state.select(Some(selected - 1));
                        } else {
                            pet_list_state.select(Some(amount_pets - 1));
                        }
                    }
                }
```

`Up`및 `Down`케이스에서 목록이 언더 플로되거나 오버플로되지 않도록해야합니다.
 이 간단한 예제에서는이 키를 누를 때마다 애완 동물 목록을 읽습니다. 이는 매우 비효율적이지만 충분히 간단합니다.
 예를 들어 실제 앱에서는 현재 애완 동물의 양을 공유 메모리에 저장할 수 있지만이 자습서에서는이 순진한 방법으로 충분합니다.
 

어쨌든, 우리는 단순히`up`의 경우`pet_list_state` 선택을 늘리거나 줄여서 사용자가`Up`과`Down`을 사용하여 애완 동물 목록을 스크롤 할 수있게합니다.
 `cargo run`을 사용하여 애플리케이션을 시작하는 경우 다른 키 바인딩을 테스트하고 `a`를 사용하여 임의의 애완 동물을 추가 한 다음, `d`를 사용하여 탐색하고 다시 삭제할 수 있습니다.
 

전체 예제 코드는 GitHub에서 찾을 수 있습니다.
 

## 결론
 

이 튜토리얼의 소개에서 언급했듯이, 터미널 애플리케이션을 만들기위한 Rust의 생태계는 환상적입니다.이 포스트가 TUI 및 Crossterm과 같은 강력한 라이브러리로 할 수있는 일을 엿볼 수 있도록 도움이 되었기를 바랍니다.
 

저는 명령 줄 응용 프로그램의 열렬한 팬입니다.
 일반적으로 빠르고 가볍고 최소한이며 Vim 사용자로서 저는 키보드를 사용하여 응용 프로그램을 탐색하고 상호 작용하는 것을 두려워하지 않습니다.
 터미널 UI의 또 다른 큰 장점은 SSH에 연결된 원격 서버 나 복구 / 디버그 시나리오와 같은 헤드리스 환경에서도 사용할 수 있다는 것입니다.
 

Rust와 풍부한 에코 시스템은 명령 줄 유틸리티를 쉽게 구축 할 수있게 해주 며, 앞으로 사람들이이 영역에서 무엇을 구축하고 게시 할 것인지 기대됩니다.
 