---
layout: post
title: "클라이언트 렌더 대 서버 렌더 대 서버 없음"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/client-render-vs-server-render-vs-serverless.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/client-render-vs-server-render-vs-serverless.png?fit=730%2C487&ssl=1)

클라이언트 렌더, 서버 렌더 및 서버리스의 차이점은 무엇입니까?

클라이언트 렌더링은 서버가 소량의 코드를 사용자에게 전송하고 해당 코드가 사용자 컴퓨터 또는 전화기에 페이지를 작성하도록 하는 것을 의미합니다. 반면, 서버 렌더링은 이미 작성된 페이지를 전송하므로 사용자의 컴퓨터에서 전송된 페이지만 표시하면 됩니다.

또한 서버 유지 관리에 대한 부담을 덜어주는 세 번째 방법인 서버리스도 있습니다. 서버리스(Serverless)는 구글이나 아마존과 같은 제공자가 서버와 그 자원(예: RAM 및 CPU)을 처리하도록 하는 것을 의미한다.

## 클라이언트 렌더링, 서버 렌더링 및 서버리스 구현의 작동 방식

이 튜토리얼에서는 클라이언트 및 서버 렌더링을 모두 소량 구현하고 나중에 원하는 클라우드 서비스에 배포할 수 있는 서버 없는 프레임워크를 포함할 수 있는 방법을 보여 줍니다.

우리 프로젝트의 기본 구조는 다음과 같습니다.

```java
src/
  |-private // folder with the templates to be rendered by the server with handlebars
    |-layouts
      main.handlebars
    server-render.handlebars
  |-public // folder with the content that we will feed to the browser
    |-js
      client-render.js
    index.html
  handler.js // serverless function will be here
  package.json
  server.js // our Node.js server
  serverless.yml // configuration of the serverless server
```

#### ➡➡➡json의

```undefined
{
  "name": "client-server-serverless",
  "version": "1.0.0",
  "description": "Client vs server render vs serverless",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "server-debug": "nodemon --inspect server.js"
  },
  "author": "daspinola",
  "license": "MIT",
  "devDependencies": {
    "nodemon": "2.0.4",
    "serverless-offline": "6.8.0"
  },
  "dependencies": {
    "express": "4.17.1",
    "express-handlebars": "5.1.0",
    "handlebars": "4.7.6",
    "node-fetch": "2.6.1",
    "serverless": "2.4.0"
  }
}
```

npm 설치 잊지 마세요. 위에 언급된 다른 모든 파일은 아래 섹션에 필요에 따라 파일링될 것입니다.

## 클라이언트 렌더

목표는 클라이언트가 디브, 버튼 및 탐색의 모든 구성을 처리하도록 하여 서버 리소스를 최대한 자유롭고 빠르게 유지하는 것입니다.

그러기 위해 /actv에서 액세스할 때 HTML 파일만 반환하는 HTTP 서버를 생성하십시오.

#### 'server.js'

```js
const express = require('express')
const path = require('path')

const app = express()

app.use(express.static(path.join(__dirname, 'public')))

app.get('/', function(req, res) {
  res.sendFile(path.join(__dirname, 'public/client-render.html'))
})

app.listen(7000, function () {
  console.log(`Listening on port ${7000}!`)
})
```

HTML 파일에는 `공용` 폴더에서 찾을 수 있는 페이지를 생성하는 데 필요한 모든 리소스가 참조됩니다.

#### 공공/지수

```xml
<html>
  <head>
    <title>Client render</title>
  </head>
  <body>
    <script src="/js/client-render.js"></script>
  </body>
</html>
```

이 경우 사용자 브라우저에서 HTML 파일이 로드되는 즉시 `client-render.js`만 가져와야 합니다.

#### 'public/js/client-inclass.js'

```js
document.addEventListener('DOMContentLoaded', init, false);

async function init() {
  const body = document.querySelector('body')
  const welcomeDiv = document.createElement('div')
  const hourDiv = document.createElement('div')
  const dateButton = document.createElement('button')

  dateButton.innerHTML = 'Date'
  welcomeDiv.innerHTML = `Welcome to the client render version, this text was added on your browser.`

  body.appendChild(welcomeDiv)
  body.appendChild(dateButton)

  dateButton.addEventListener('click', () => {
    const date = new Date()
    hourDiv.innerHTML = `It's now ${date}`
    body.appendChild(hourDiv)
  })
}
```

사용자가 브라우저에서 해당 파일을 다운로드하면 즉석에서 페이지를 작성하기 시작합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/client-render-example.png?resize=720%2C326&ssl=1)

이 시나리오에서는 `/` 라우트(이 시나리오에서는 로컬 호스트)에 대한 요청이 이루어지며, 브라우저에 의해 `index.html` 파일이 로드되고 리소스 `client-ender.js`가 종속성으로 발견됩니다. 브라우저가 해당 파일을 가져오도록 요청합니다. 로드되면 페이지가 작성됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/client-render-page-after-events.png?resize=472%2C76&ssl=1)

Date(날짜) 버튼을 누르면 개발자 도구의 Network(네트워크) 탭에 새 요청이 표시되지 않고 브라우저의 날짜가 검색됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/client-render-page-no-flash.gif?resize=537%2C87&ssl=1)

서버 날짜를 가져오려면 요청을 하고 텍스트를 업데이트해야 합니다. 그럼에도 불구하고, 페이지는 서버가 아닌 텍스트를 요청하고 업데이트하는 `client-render.js`이기 때문에 다시 로드되지 않습니다.

클라이언트 렌더 사용에 대한 장단점을 자세히 살펴보겠습니다.

### 프로스

- 서버에서 사용하는 리소스 수 감소
- 페이지가 로드되면 탐색 속도가 매우 빠릅니다.
- 페이지를 다시 로드할 필요 없음

### 콘스

- 렌더링 시간은 클라이언트 브라우저 및 시스템에 따라 크게 다름
- 자바스크립트 페이로드가 많고 서버에 대한 요청 수가 많기 때문에 속도가 느립니다.
- JavaScript가 비활성화된 경우 웹 사이트가 전혀 로드되지 않을 수 있음

## 서버 렌더

이제 서버가 페이지 렌더링을 처리하고 전체 결과를 사용자의 브라우저에 반환하도록 한다고 가정해 보겠습니다.

예를 단순화하기 위해 클라이언트 렌더 부분을 제거했습니다. `server.js`를 아래 경로로 바꾸거나 새 경로를 아래 경로로 추가할 수 있습니다.

#### 'server.js'

```js
const express = require('express')
const exphbs = require('express-handlebars')
const path = require('path')
const app = express()

app.engine('handlebars', exphbs());

app.set('views', path.join(__dirname, 'private'))
app.set('view engine', 'handlebars');

app.get('/', function(req, res) {
  const welcomeText = 'Welcome to the server render version, this text was added on the server'
  const date = req.query.date === 'true'
    ? new Date()
    : undefined

  res.render('server-render', { welcomeText, date })
})

app.listen(7000, function () {
  console.log(`Listening on port ${7000}!`)
})
```

이것은 아직 다른 HTTP 서버이지만, 클라이언트에 렌더링할 JavaScript가 포함된 HTML 파일을 보내는 대신 이번에는 핸들 바를 사용하여 렌더링하고 전체 결과를 클라이언트로 다시 보냅니다.

#### '개인용/개인용/메인용 술집'

```xml
<html>
  <head>
    <title>Server render</title>
  </head>
  <body>
    {{ body }}
  </body>
</html>
```

#### '개인용/서버용.인스턴스 바'

```xml
<div> { welcomeText } </div>

<form action="/server-render" method="get" target="_self">
  <input type="hidden" name="date" value="true" /> 
  <button type="submit">Date</button>
</form>

{#if date}
<div>It's now { date }</div>
{/if}
```

서버측 렌더링을 사용할 때 보다 쉽게 사용할 수 있도록 HTML에 변수, 조건 및 루프를 포함할 수 있는 보기 엔진을 지정할 수 있습니다.

이 예에서 엔진은 핸들바이며 클라이언트가 경로를 요청할 때 위의 최종 결과는 HTML입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/server-render-example.png?resize=720%2C295&ssl=1)

또한 단일 요청에서 유일한 텍스트를 검색하기 때문에 전송된 리소스는 클라이언트 렌더 상대보다 3배 더 적었습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/server-render-example-page-load.png?resize=483%2C64&ssl=1)

이 구현에는 클라이언트 렌더링 예와 비교하여 고려해야 할 두 가지가 있습니다.

- 검색된 날짜는 클라이언트 브라우저가 아닌 서버에서 가져옵니다.
- 날짜 버튼을 누르면 전체 페이지가 다시 로드됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/server-render-new-requests.png?resize=670%2C191&ssl=1)

### 프로스

- 빠른 초기 페이지 로드
- 서버가 모든 로드를 사용하기 때문에 서로 다른 장치 간의 일관된 렌더링 시간
- SEO 점수 향상

### 콘스

- 모든 작업이 새 요청이므로 서버에서 사용하는 리소스 증가
- 내비게이션 재로드 필요
- 사용 중인 기술 스택에 따라 설정이 더 까다로울 수 있음

## 서버리스

위에서 설명한 두 가지 방법을 모두 서버 없는 아키텍처에 적용할 수 있습니다. 즉, 일반 HTTP 서버와 마찬가지로 서버리스 기능 내에서 실행되는 클라이언트 또는 서버 렌더링을 사용하여 페이지를 생성할 수 있습니다.

애플리케이션 내에서 자주 발생하지 않는 개별 기능에서 서버리스가 가장 유리하기 때문에 전체 웹 사이트에서 이러한 접근 방식은 큰 비용을 초래할 수 있습니다.

다음은 서버 없는 서버를 실행하고 제공자에 배포할 필요 없이 로컬로 할당된 기능을 호출하는 방법입니다.

#### '서버리스.yml'

```undefined
service: client-server-serverless
frameworkVersion: '2'
provider:
  name: aws
  runtime: nodejs12.x
functions:
  serverDate:
    handler: handler.serverDate
    events:
      - http:
         path: serverDate
         method: get
         cors: true
plugins:
  - serverless-offline
```

로컬에서 테스트를 수행할 수 있는 `서버리스 오프라인` 플러그인 외에도 관심 있는 것은 트리거할 수 있는 기능을 지정해야 하는 `기능`뿐이다.

이 구성은 라우트를 생성하는 역할을 합니다. 이 경우 `/serverDate`가 되며, 이 날짜는 `handler.js` 파일에 정의되어야 합니다.

#### ➡.js

```js
module.exports.serverDate = async event => {
  const serverDate = new Date()
  return {
    statusCode: 200,
    body: JSON.stringify({
      serverDate
    }),
  };
};
```

서버를 실행하려면 `npx sls offline start`와 `localhost:3000/dev/serverDate`에서 해당 기능을 사용할 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/serverless-date-object.png?resize=340%2C39&ssl=1)

이와 같은 요청이 서버리스 기능으로 수행되면 해당 요청 기간 동안 요금이 부과됩니다(제공업체마다 청구 매개변수가 다릅니다. 로컬 서버리스 서버의 콘솔에서 수행된 견적을 보면 서버리스에서 기능을 실행하는 데 비용이 얼마나 들는지 알 수 있습니다.

아래는 서버측 렌더 예제에서 호출되는 서버리스 함수의 예입니다.

#### 'server.js'

```js
const express = require('express')
const exphbs = require('express-handlebars')
const fetch = require('node-fetch')

const path = require('path')

const app = express()

app.engine('handlebars', exphbs());

app.set('views', path.join(__dirname, 'private'))
app.set('view engine', 'handlebars');

app.get('/', function(req, res) {
  const welcomeText = 'Welcome to the server render version, this text was added on the server'
  const date = req.query.date === 'true'
    ? new Date()
    : undefined
  const serverlessResponse = await fetch('http://localhost:3000/dev/serverDate')
    .then(res => res.json())
  res.render('server-render', { welcomeText, date, serverlessResponse: serverlessResponse.serverDate })
})

app.listen(7000, function () {
  console.log(`Listening on port ${7000}!`)
})
```

#### '개인용/서버용.인스턴스 바'

```xml
<div> { welcomeText }. </div>

<div>Serverless function server date: { serverlessResponse }</div>

<form action="/server-render" method="get" target="_self">
  <input type="hidden" name="date" value="true" /> 
  <button type="submit">Date</button>
</form>

{#if date}
<div>It's now { date }</div>
{/if}
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/serverless-update-page.png?resize=473%2C72&ssl=1)

### 프로스

- 사용량에 따라 자동으로 확장
- 실행 중인 기능이 일반 서버의 전체 용량을 사용하지 않는 경우 비용 절감
- 서버 유지보수가 필요 없음

### 콘스

- 가격은 요청 수와 사용량에 따라 결정되며, 비용이 매우 빠르게 들 수 있습니다.
- 한동안 호출되지 않은 엔드포인트에 요청이 들어오면 해당 기능을 "부팅"해야 합니다. 시간이 좀 걸립니다. — 작업에 따라 가치가 있을 수 있는 보통 밀리초
- 구현이 다양한 경향이 있기 때문에 공급자(AWS, Google 등)와 분리하기가 더 어렵습니다.

## 결론

공학 분야의 대부분의 주제와 마찬가지로, 어떤 길을 선택할지 결정하는 데 도움이 되는 마법의 공식은 없습니다. 일반적으로 하이브리드 접근 방식이 적절합니다.

예를 들어, 클라이언트 측에서 후속 페이지가 렌더링되는 동안 서버측 렌더링 속도를 이용하여 초기 페이지를 서버에 렌더링할 수 있습니다.

마찬가지로, 추적 페이지나 전자 메일 전송과 같은 일회성 기능은 서버 없는 아키텍처와 잘 어울려야 합니다.

내 GitHub에서 이 기사에서 참조된 모든 코드에 액세스할 수 있습니다.