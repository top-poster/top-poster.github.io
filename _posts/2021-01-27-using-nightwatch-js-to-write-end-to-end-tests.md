---
layout: post
title: "Nightwatch.js를 사용하여 종단 간 테스트 작성
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/nightwatch-end-testing.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/nightwatch-end-testing.png?fit=730%2C487&ssl=1)

이 블로그 게시물에서는 사용자에게 엔드 투 엔드 테스트를 소개하고 개발자에게 인기있는 엔드 투 엔드 테스트 프레임 워크 인 Nightwatch.js를 소개합니다.
 이를 설정하고 내부에서 작동하는 방식을 설명하고 circleCI와 같은 지속적 통합 파이프 라인과 통합하여 제품을 더 빠르게 배송하는 데 어떻게 도움이되는지 자세히 보여 드리겠습니다.
 

## 종단 간 테스트 란 무엇입니까?
 

엔드 투 엔드 테스트는 사용자의 관점에서 애플리케이션을 테스트하므로 애플리케이션의 비즈니스 사용 사례가 얼마나 잘 충족되고 있는지 확인할 수 있습니다.
 

이 프로세스에는 Nightwatch와 같은 브라우저 기반 자동화 테스트 도구와 Cypress를 사용하여 브라우저에서 애플리케이션을 테스트하는 것이 포함됩니다.
 그런 다음 테스트 결과를 기반으로 어설 션을 만들 수 있습니다.
 

엔드-투-엔드 테스트에는 프런트 엔드 구성 요소와 빌딩 블록에서 백엔드 기능 및 기능에 이르기까지 전체적으로 애플리케이션의 모든 기능을 테스트하는 것이 포함됩니다.
 응용 프로그램의 목표를 달성하기 위해 서로 얼마나 잘 통합되는지 테스트합니다.
 

## Nightwatch 소개
 

Nightwatch는 Node.js로 작성된 엔드-투-엔드 테스트 라이브러리로, W3C 웹 드라이버 API를 활용하여 웹 브라우저와 상호 작용하고, 명령을 제공하고, 주어진 명령을 기반으로 어설 션을 수행합니다.
 

## 왜 Nightwatch인가?
 

Nightwatch는 다음과 같은 다양한 혜택을 즉시 제공합니다.
 

- 깨끗한 구문
 
- 내장 테스트 러너
 
- 지속적인 통합
 
- 확장하기 쉬움 (이 기능에 대해 Nightwatch를 사용합니다. 테스트를위한 사용자 지정 명령과 어설 션을 작성할 수 있기 때문입니다)
 

## 종단 간 테스트를 위해 Nightwatch 사용
 

웹 드라이버는 웹 브라우저 자동화를위한 범용 라이브러리이며 모든 브라우저에는 특정 웹 드라이버 서버 구현이 있습니다.
 Nightwatch는 HTTP 호출을 통해 편안한 API를 통해 브라우저 웹 드라이버 서버와 통신합니다.
 이러한 프로토콜은 W3C Webdriver 사양에 의해 정의됩니다.
 

Nightwatch는 명령 또는 어설 션을 수행하기 위해 최소 두 개의 요청을 웹 드라이버 서버에 보냅니다.
 

- CSS 선택기 또는 X 경로를 사용하여 요소 찾기 요청
 
- 주어진 요소에 대한 실제 명령 또는 어설 션 수행 요청
 

Nightwatch에 대한 이러한 접근 방식은 하나의 브라우저와 상호 작용하고 자동화 된 테스트를 수행합니다.
 

또는 Nightwatch는 Selenium Server (Selenium Grid)를 사용하여 브라우저를 병렬로 테스트 할 것입니다.
 Grid를 사용하면 여러 브라우저로 세션을 만들고 병렬로 테스트하여 웹 앱이 다양한 브라우저와 호환되는지 확인할 수 있습니다.
 

## Selenium 서버는 어떻게 작동합니까?
 

Selenium 서버는 Nightwatch 테스트 스크립트 (WebDriver API로 작성)와 브라우저 드라이버 (WebDriver 프로토콜에 의해 제어 됨) 사이의 프록시 역할을합니다.
 

서버는 Nightwatch의 스크립트에서 드라이버로 명령을 전달하고 드라이버의 응답을 어설 션이 만들어지는 스크립트로 반환합니다.
 Selenium Server는 서로 다른 언어로 여러 스크립트를 처리 할 수 있으며 서로 다른 버전 및 구현에서 여러 브라우저를 시작하고 관리 할 수 있습니다.
 

선호도와 사용 사례에 따라 셀레늄 서버를 로컬 또는 원격 (Sawce Labs 또는 BrowserStack을 통해) 또는 외부 인프라로 호스팅 할 수 있습니다.
 

## 로컬에서 Nightwatch를 설정하는 방법
 

먼저`create-react application-npx create-react-app my-app`을 사용하여 새로운 반응 애플리케이션을 부트 스트랩합니다.
 

이제 원하는 브라우저 웹 드라이버를 설치하십시오.
 이 기사에서는 Chrome 및 FireFox에서 테스트를 실행하므로 두 드라이버를 모두 설치하겠습니다.
 

```undefined
- npm install geckodriver --save-dev
- npm install chromedriver --save-dev
```

Selenium Server를 로컬로 설치하십시오.
 

```undefined
- npm install selenium-server --save-dev
```

이제 Selenium 서버 (Selenium-server-standalone-4.0.0-alpha-2.zip)를 다운로드 해 보겠습니다.
 

파일 압축을 풀고 프로젝트 루트에 bin 폴더를 만들고`selenium-server-4.0.0-alpha-2.jar`을이 폴더로 이동합니다.
 

다음으로 Java를 다운로드하십시오.
 Selenium Server는 자바 앱이므로 컴퓨터에 자바를 설치해야합니다.
 `java -version`을 실행하여 이미 설치되어 있는지 확인하십시오.
 

이제 Node Selenium Server 패키지 Selenium-server를 설치하겠습니다.
 이 패키지는 이전에 저장된 Selenium Server 바이너리 / 실행 파일의 경로를 포함하는 경로 문자열을 내 보냅니다.
 

프로젝트의 루트에`nightwatch.conf.js` 파일을 만들고 다음 구성을 추가합니다.
 

```coffeescript
module.exports = {
    src_folders: ['e2e'],
    test_settings: {
      default: {
        selenium_port: 4444,
        selenium_host: "localhost",
        silent: true,
        screenshots: {
          enabled: false,
          path: "",
        },
        globals: {
          abortOnAssertionFailure: false,
          waitForConditionPollInterval: 300,
          waitForConditionTimeout: 10000,
          retryAssertionTimeout: 5000,
        },
        selenium : {
          start_process : true,
          server_path: require("selenium-server").path
        }
      },
      selenium: {
        selenium: {
          start_process: true,
          port: 4444,
          server_path: require("selenium-server").path,
          cli_args: {
            "webdriver.chrome.driver": require("chromedriver").path,
            "webdriver.gecko.driver": require("geckodriver").path
          },
        },
        webdriver: {
          start_process: false,
        },
      },
      "chrome": {
        extends: "selenium",
        desiredCapabilities: {
          browserName: "chrome",
          chromeOptions: {
            w3c: false,
          },
        },
      },
  
      "firefox": {
        extends: "selenium",
        desiredCapabilities: {
          browserName: "firefox",
        },
      },
    },
  }
```

`src_folders :`
 

여기서 우리는 테스트를 저장할 기본 경로를`e2e`라는 폴더로 지정했습니다.
 이 속성은 배열입니다. 즉, 테스트 실행기가 실행할 테스트를 찾는 여러 항목을 허용 할 수 있습니다.
 

주요 섹션을 살펴 보겠습니다.
 

### Nightwatch의 테스트 설정
 

`test_settings`
 

테스트 환경을 정의하는 객체입니다.
 아래와 같이 기본 개체로 구성됩니다.
 

`기본값`
 

test_settings 아래의이 속성은 다른 환경이 상속되는 환경을 정의합니다.
 여기서 우리는 Selenium Server를 로컬과 포트로 정의했습니다.
 다음은 위 파일에서 기본값으로 만든 구성입니다.
 

```css
default: {
        selenium_port: 4444,
        selenium_host: "localhost",
        silent: true,
        screenshots: {
          enabled: true,
          path: "tests_output",
        },
        globals: {
          abortOnAssertionFailure: false,
          waitForConditionPollInterval: 300,
          waitForConditionTimeout: 10000,
          retryAssertionTimeout: 5000,
        },
        selenium : {
          start_process : true,
          server_path: require("selenium-server").path
        }
      }
```

위의 코드를 더 자세히 설명하겠습니다.
 

- `selenium_port` : Selenium이 연결을 수락하는 포트 번호
 
- `selenium_host` : Selenium 서버가 연결을 수락하는 호스트 IP 주소
 
- `screenshots` :이 구성을 통해 스크린 샷을 활성화하고 save screenshot 명령을 사용하여 요청할 때마다 저장할 테스트의 기본 디렉토리 스크린 샷을 설정할 수 있습니다.
 
- `globals` : 테스트 유연성을 보장하는 데 필요한 전역 구성입니다.
 몇 가지를 강조하겠습니다.
`abortOnAssertionFailure` :이 컨트롤은 어설 션이 실패 할 때 테스트 실행을 중단하고 나머지는 건너 뜁니다.
`waitForConditionTimeout` :`waitFor 명령`의 기본 폴링 간격 (500ms)을 덮어 씁니다.
`waitForConditionTimeout` :`waitFor` 명령 및 기본`waitFor` 값의 기본 시간 초과를 덮어 씁니다.
`retryAssertionTimeout` : 테스트 실행기가 포기하기 전에 시간 초과에 도달 할 때까지 실패한 어설 션을 자동으로 재 시도합니다.
 
- `abortOnAssertionFailure` :이 컨트롤은 어설 션이 실패 할 때 테스트 실행을 중단하고 나머지는 건너 뜁니다.
 
- `waitForConditionTimeout` :`waitFor 명령`의 기본 폴링 간격 (500ms)을 덮어 씁니다.
 
- `waitForConditionTimeout` :`waitFor` 명령 및 기본`waitFor` 값의 기본 시간 초과를 덮어 씁니다.
 
- `retryAssertionTimeout` : 테스트 실행기가 포기하기 전에 시간 초과에 도달 할 때까지 실패한 어설 션을 자동으로 재 시도합니다.
 
- `selenium` : Selenium 서버 관련 구성 옵션을 포함하는 객체입니다.
 Selenium을 사용하지 않는 경우 대신 webdriver 옵션을 설정해야합니다.
 
- `server_path` : 이전에 다운로드 한 Selenium 드라이버가 참조되는 경로입니다.
 따라서 selenium_server 노드 패키지가 필요합니다.
 

### Selenium 서버 설정
 

이제 Selenium 서버 설정에 대해 설명하겠습니다.
 

```css
selenium: {
        selenium: {
          start_process: true,
          port: 4444,
          server_path: require("selenium-server").path,
          cli_args: {
            "webdriver.chrome.driver": require("chromedriver").path,
            "webdriver.gecko.driver": require("geckodriver").path
          },
        },
        webdriver: {
          start_process: false,
        },
      }
```

더 세분화하려면 :
 

- `start_process` : 셀레늄 프로세스를 자동으로 관리할지 여부를 결정합니다.
`Server_path` : 서버 경로의 위치입니다. 여기서는`selenium_server` 노드 경로를 사용하여 서버 경로를 확인했습니다.
`Port` : Selenium Server가 수신하고 Nightwatch가 연결되는 포트를 나타냅니다.
`Cli_args` : 여기에는 Selenium 프로세스에 전달할 CLI 인수 목록이 포함됩니다. 위에서 테스트를 실행하려는 브라우저 유형에 따라 다른 드라이버를 지정합니다. 이것이 우리가 이전에 다음을 설치 한 이유입니다.
-npm geckodriver 설치 --save-dev
-npm install chromedriver --save-dev
Firefox와 Chrome에서 병렬로 테스트를 실행하고 싶기 때문에이 두 가지만 사용하기로 결정했습니다.
위의 패키지는 노드 (Linux, macOs, Windows)로 식별되는 설치된 플랫폼에 대해 각각의 브라우저 드라이버를 가져와 실행하도록 설정되었습니다.
webdriver : webdriver는 백그라운드에서 하위 프로세스로 실행되며 자동으로 시작 및 중지됩니다. 웹 드라이버와 상호 작용하기 위해 Selenium 서버를 사용하고 있기 때문에이 구성을 비활성화합니다.
 
- `start_process` : 셀레늄 프로세스를 자동으로 관리할지 여부를 결정합니다.
 
- `Server_path` : 서버 경로의 위치입니다.
 여기서는`selenium_server` 노드 경로를 사용하여 서버 경로를 확인했습니다.
 
- `Port` : Selenium Server가 수신하고 Nightwatch가 연결되는 포트를 나타냅니다.
 
- `Cli_args` : 여기에는 Selenium 프로세스에 전달할 CLI 인수 목록이 포함됩니다.
 위에서 테스트를 실행하려는 브라우저 유형에 따라 다른 드라이버를 지정합니다.
 이것이 우리가 이전에 다음을 설치 한 이유입니다.
-npm geckodriver 설치 --save-dev
-npm install chromedriver --save-dev
Firefox와 Chrome에서 병렬로 테스트를 실행하고 싶기 때문에이 두 가지만 사용하기로 결정했습니다.
위의 패키지는 노드 (Linux, macOs, Windows)로 식별되는 설치된 플랫폼에 대해 각각의 브라우저 드라이버를 가져와 실행하도록 설정되었습니다.
 
- webdriver : webdriver는 백그라운드에서 하위 프로세스로 실행되며 자동으로 시작 및 중지됩니다.
 웹 드라이버와 상호 작용하기 위해 Selenium 서버를 사용하고 있기 때문에이 구성을 비활성화합니다.
 

## 웹 브라우저 설정 구성
 

다음 코드는 Selenium 서버에서 관리하는 웹 브라우저 (Firefox 및 Chrome)에 대한 개별 구성으로 구성됩니다.
 

```css
 "chrome": {
        extends: "selenium",
        desiredCapabilities: {
          browserName: "chrome",
          chromeOptions: {
            w3c: false,
          },
        },
      },
  
      "firefox": {
        extends: "selenium",
        desiredCapabilities: {
          browserName: "firefox",
        },
      },
    },
  }
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/folder-structure-example.png?resize=490%2C710&ssl=1)

이 지침을 따르면 위와 유사한 폴더 구조가 생깁니다.
 

## 종단 간 테스트 작성
 

### 응용 프로그램 구조 만들기
 

React에서 데모 앱을 만들었 기 때문에 end-to-end 테스트를 실제로 볼 수 있습니다.
 이 데모 앱 서버는이 블로그 게시물의 소스 코드에서 볼 수 있듯이 JSON SERVER를 사용하는 가짜 REST API로 구축되었습니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/subscription-successful-alert-1.gif?resize=730%2C319&ssl=1)

구독 버튼 중 하나를 클릭하면 가짜 API에 새 구독을 주문하라는 요청이 생성됩니다.
 성공하면 알림 팝업이 표시됩니다.
 

성공하면 "구독 성공.
 발사!”
 

실패하면 "지금 구독 할 수 없습니다. 다시 시도하십시오!"라는 경고가 표시됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/subscription-failure-alert-1.gif?resize=730%2C319&ssl=1)

### React 애플리케이션에 대한 종단 간 테스트 작성
 

`E2E`테스트는 프로젝트의 E2E 폴더에서 찾을 수 있습니다.
 다음은 구독 기능에 대한 테스트입니다.
 

```js
module.exports = {
    "SUBSCRIBE_USER": function(browser) {
     // the aside element css selector
        const alertElement = "div[role=alert]";
        browser
        .url("http://localhost:3005/")
        .waitForElementVisible("body")
        .click('#subscribe2')
        .waitForElementVisible(alertElement)
        .assert.containsText(alertElement, "Subscription successful. Fire on!")
        .saveScreenshot('subscription.png');
    }
}
```

이 게시물의 목표는 Nightwatch에서 종단 간 테스트를 작성하는 세부 사항을 가르치는 것이 아니라 위의 테스트에 대해 설명 할 것입니다.이 테스트는 엄청나게 적용 가능한 사용 사례입니다.
 

```undefined
 browser
        .url("http://localhost:3005/")
```

위의 코드는 브라우저에서 http : // localhost : 3005 /로 이동합니다.
 

```bash
.waitForElementVisible("body")
```

`waitForElementVisible` 명령은 다른 명령이나 어설 션을 수행하기 전에 요소가 페이지에 표시 될 때까지 5000ms를 기다립니다.
 이 경우 요소는 페이지의 본문입니다.
 

```bash
.click('#subscribe2')
```

click 명령을 사용하면 인수로 전달 된 CSS 선택기를 클릭 할 수 있으므로 ID가`# subscribe2` 인 "standard"구독 버튼을 클릭합니다.
 

```css
.assert.containsText(alertElement, "Subscription successful. Fire on!")
```

`assert.containsText`는 허용하는 CSS 선택기에 예상 텍스트가 포함되어 있는지 확인합니다.
 

```bash
.saveScreenshot('subscription.png');
```

마지막으로 Nightwatch 구성에 정의 된대로 test_output 폴더에 이름으로 테스트 스크린 샷을 저장합니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/screenshot-to-save.png?resize=730%2C121&ssl=1)

프로젝트 디렉터리에서 다음 명령을 실행하여 다음을 수행합니다.
 

먼저 가짜 서버를 시작합니다.
 

```coffeescript
npm run server:start
```

그런 다음 React 앱을 시작합니다.
 

```coffeescript
npm run start
```

마지막으로 E2E 테스트를 실행합니다.
 

```coffeescript
npm run e2e
```

또한 다음 명령을 실행하여 위 테스트의 모든 단계를 수행 할 수 있습니다.
 

```coffeescript
npm run e2e-test
```

위의 명령은 다음과 같습니다.
 

```coffeescript
concurrently -k --success first \”npm run start\” \”npm run server:start\” \”npm run e2e\””
```

이 명령을 동시에 실행해야하므로 패키지를 동시에 추가했습니다.
 

또한 명령 중 하나가 실패하거나 종료 될 때 다른 모든 명령 프로세스를 종료해야합니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/terminate-commands-1.gif?resize=730%2C257&ssl=1)

### End-to-end 테스트를 CircleCI에 연결
 

이제 테스트가 로컬에서 실행된다는 것을 확인 했으므로 개발 시간을 가속화하기 위해 CI 파이프 라인에서 실행되는지 확인해야합니다.
 

아래는이 테스트를위한 CircleCI 구성 파일입니다.
 

## 지속적인 통합 및 개발을 위해 CircleCI 사용
 

프로젝트에 대해 CircleCI를 설정하는 것으로 시작하겠습니다.
 

이 작업을 완료 한 후 루트 디렉토리에서`.circleci / config.yml 파일`을 편집하고 다음을 붙여 넣습니다.
 

```bash
version: 2.1
orbs:
  browser-tools: circleci/browser-tools@1.1.1
jobs:
  build:
    docker:
      - image: 'cimg/node:15.0.1-browsers'
        auth:
          username: $DOCKERHUB_USER
          password: $DOCKERHUB_PASSWORD
    working_directory: ~/repo

    steps:
      - checkout
      - browser-tools/install-browser-tools

      - run: npm install

      # run tests!
      - run: npm test
      - run: npm run e2e-test
```

우리는 CircleCI에서 실행되는 도커 컨테이너에서 테스트하기 위해 브라우저에 필요한 안정적인 도커 이미지를 제공하는 CircleCI 편의 이미지를 사용했습니다.
 

```bash
      - image: 'cimg/node:15.0.1-browsers'
```

또한 브라우저 테스트를 위해 다양한 브라우저와 도구를 설치하는 CircleCI orbs를 활용했습니다.
 여기에는 Chrome, FireFox, ChromeDriver 및 GeckoDriver가 포함됩니다.
 

```undefined
orbs:
  browser-tools: circleci/browser-tools@1.1.1
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/npm-run-e2e-test-1.gif?resize=730%2C508&ssl=1)

## 결론
 

이 기사에서는 엔드-투-엔드 테스트의 작동 방식, 브라우저 호환성을 보장하기 위해 다양한 브라우저에서 병렬로 테스트하는 것과 같은 엔드-투-엔드 테스트를 처리하기위한 다양한 접근 방식 및 Selenium 서버 그리드를 활용하는 방법에 대해 논의했습니다.
 

또한 로컬로 설정하고 CI / CD 파이프 라인에 연결하는 방법도 배웠습니다.
 즐거운 코딩 되세요!
 