---
layout: post
title: "실행 및 Node.js와 함께 기능 플래그 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/using-feture-flags-unleash-node.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/using-feture-flags-unleash-node.png?fit=730%2C487&ssl=1)

## 피쳐 플래그란 무엇입니까?

프로그래밍에서 플래그(Flag)는 프로그램이 값에 따라 경로를 실행하도록 하는 변수이다.

아래 예제를 살펴보십시오.

```cpp
if (flag === true) {
  // feature 1
} else {
  // feature 2
}
```

플래그는 일반적으로 서버가 시작될 때 호출되는 환경 변수이며, 개발 및 프로덕션 환경 간의 로그에서 상세 수준 설정, 응용 프로그램의 기본 언어 설정 등과 같은 작업을 수행하는 데 사용됩니다. 이러한 변수를 전환하려면 파일 또는 데이터베이스를 변경하고 서버를 재시작하여 새 설정을 적용해야 합니다.

기능 플래그는 언제든지 전환할 수 있는 변수이며 변경 내용은 다시 시작할 필요 없이 프로그램에 반영됩니다.

## 피쳐 플래그 사용 시기

그림 표시: 배경을 임의의 색으로 변경하는 단추인 새 기능을 방금 웹 사이트에서 시작했습니다. 테스트 및 설치를 완료했으며 사용자의 경험을 기다릴 수 없습니다.

시간이 지나면 사용자는 기능과 상호 작용하여 배경 대신 텍스트 색상이 변경되는 것을 발견합니다. 재앙이 닥쳤으니 이제 수리될 때까지 기능을 되돌려야 합니다. 이 경우 일반적으로 코드를 적용하고 기능을 제거하기 위해 긴급 재시작한 후 다른 코드가 수정 사항을 적용하게 됩니다.

다시 시작은 새로운 기능이나 수정 사항이 추가되었을 때만 수행되며, 특히 긴급하지 않은 것이 좋습니다. 여기서 피쳐 플래그를 사용할 수 있습니다. 기능 플래그를 사용하면 결함 있는 기능을 즉시 비활성화하여 기능을 되돌리고 다시 시작하여 제거할 수 있습니다.

## 피쳐 플래그의 작동 방식

기능 플래그를 표시하기 위해 앞서 언급한 예: 배경색 대신 텍스트 색상을 전환하는 단추가 있는 웹 사이트를 사용합니다.

### 프로젝트 구조

```java
src/
  |-public // folder with the content that we will feed to the browser
    |-js
      new-feature.js
    index.html    
  package.json
  server.js // our Node.js server
```

#### ➡➡➡json의

```undefined
{
  "name": "feature-flags-101",
  "version": "1.0.0",
  "description": "Setup of feature flags with unleash",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "server-debug": "nodemon --inspect server.js"
  },
  "author": "daspinola",
  "license": "MIT",
  "devDependencies": {
    "nodemon": "2.0.4"
  },
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

#### 'server.js'

```js
const express = require('express')
const path = require('path')
const app = express()

app.use(express.static(path.join(__dirname, 'public')))

app.get('/', function(req, res) {
  res.sendFile(path.join(__dirname, 'public/index.html'))
})

app.listen(7000, function () {
  console.log(`Listening on port ${7000}!`)
})
```

#### '공용'/'인덱스'

```xml
<html>
  <head>
    <title>Feature flags 101</title>
  </head>
  <body>
    <div>
      <span id="span-welcome-text">My awesome website has a new button!!!</span>
      <button id="button-switch-background-color">Background Color</button>
    </div>
    <script src="/js/new-feature.js"></script>
  </body>
</html>
```

#### '공용'/'js'/'신규'

```js
document.addEventListener('DOMContentLoaded', init, false);

function init() {
  const buttonSwitchColor = document.getElementById('button-switch-background-color')

  buttonSwitchColor.addEventListener('click', () => {
    const body = document.querySelector('body')
    body.style.color = getRandomColor()
  })

  // Code from: https://stackoverflow.com/questions/1484506/random-color-generator
  function getRandomColor() {
    var letters = '0123456789ABCDEF'
    var color = '#'
    for (var i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)]
    }
    return color
  }
}
```

`패키지`에서요.json은 에몬이 없으므로 변경 후 빠른 HTTP 서버 설정을 위해 언제든지 서버 재시작을 수행할 수 있습니다(서버.js).

index.contract`에는 결함이 있는 버튼이 있는 웹사이트가 포함되어 있습니다. 여기서 `new-target.js`는 색상 변경 기능이 정의됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/buggy-color-change-functionality.gif?resize=409%2C406&ssl=1)

## 기능 플래그를 실행과 함께 사용

이제 결함이 있는 웹 사이트를 구축했으므로 기능 플래그에 동일한 버기 기능이 어떻게 도입될 수 있었는지 살펴보겠습니다.

해제는 플래그를 쉽게 켜고 끌 수 있는 UI를 만들 수 있는 기능 토글 시스템입니다.

### 실행 설정

Lunset을 설정하려면 다음 세 가지가 필요합니다.

- Postgres, 여기서 Flease는 기능 플래그 상태를 저장합니다.
- 실행 중지 서버 - 플래그 상태를 쿼리할 UI 및 끝점을 제공합니다.
- 실행 중지-클라이언트 노드, 실행 서버에 연결하여 플래그 상태를 알 수 있는 라이브러리

Postgres를 가질 수 있는 한 가지 방법은 Docker와 함께 실행하는 것이다.

```undefined
docker run --name unleash-postgres -e POSTGRES_PASSWORD=pass -d -p 5432:5432 postgres
```

실행 중이면 Postbird에 연결하고 "unrelease" 데이터베이스를 작성할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/create-unleash-server.png?resize=369%2C171&ssl=1)

이제 npmi resollease-server를 실행하고 프로젝트에 새 파일을 만들어 released-server를 설치해야 할 때이다.

#### 'sublic-server.js'

```js
const unleash = require('unleash-server');

unleash
  .start({
    databaseUrl: 'postgres://postgres:pass@localhost:5432/unleash',
    port: 4242,
  })
  .then(unleash => {
    console.log(
      `Unleash started on http://localhost:${unleash.app.get('port')}`,
    );
  });
```

#### ➡➡➡json의

```undefined
{
  "name": "feature-flags-101",
  "version": "1.0.0",
  "description": "Setup of feature flags with unleash",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "server-debug": "nodemon --inspect server.js",
    "unleash-server": "node unleash-server.js"
  },
  "author": "daspinola",
  "license": "MIT",
  "devDependencies": {
    "nodemon": "2.0.4"
  },
  "dependencies": {
    "express": "^4.17.1",
    "unleash-server": "^3.5.4"
  }
}
```

브라우저에서 npm runnise-server 명령을 실행하면 localhost:4242에 UI가 있어야 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/ui-add-feature-flags-1.png?resize=720%2C365&ssl=1)

이제 개발하려는 기능을 하나 만들어 보겠습니다. 예를 들어:

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/create-new-feature-toggle-1.png?resize=720%2C274&ssl=1)

Postgres가 설치되어 실행 중인 releas-server가 실행되면 이제 서버에 연결하여 `npm install releasure-client`를 사용하여 기능 플래그의 상태를 확인하고 다음과 같이 서버를 변경할 수 있습니다.

#### 'server.js'

```php
const express = require('express')
const path = require('path')

// The unleash client makes requests every 10 seconds or so for updates on the status of the flags so there is no need to restart the server if they change
const { initialize, isEnabled } = require('unleash-client');
const instance = initialize({
    url: 'http://localhost:4242/api/',
    appName: 'feature-flags',
    instanceId: 'feature-flags-101',
});

const app = express()
app.use(express.static(path.join(__dirname, 'public')))

app.get('/', function(req, res) {
  res.sendFile(path.join(__dirname, 'public/index.html'))
})

// This is the endpoint we're going to call in our front end to check if a flag is available or not
app.get('/feature-flag/:name?', function(req, res) {
  const flagName = req.params.name
  res.send({
    flagName,
    isEnabled: isEnabled(flagName)
  })
})

// We add this way so our server only starts when the flags are available
instance.on('ready', () => {
  app.listen(7000, function () {
    console.log(`Listening on port ${7000}!`)
  })
})
```

#### 'public'/'js'/'new-spartic.js'

```js
document.addEventListener('DOMContentLoaded', init, false);

async function init() {
  const body = document.querySelector('body')
  const spanWelcomeText = document.getElementById('span-welcome-text')
  const showNewButton = await fetch('/feature-flag/random-background-color-button')
    .then(response => response.json())

  if (showNewButton.isEnabled) {
    const newButton = document.createElement('button')

    newButton.id = 'button-switch-background-color'
    newButton.innerHTML = 'Background Color'
    newButton.addEventListener('click', () => {
      body.style.color = getRandomColor()
    })
    body.appendChild(newButton)

    spanWelcomeText.innerHTML = 'My awesome website has a new button!!!'

    // Taken from the useful reply in: https://stackoverflow.com/questions/1484506/random-color-generator
    function getRandomColor() {
      var letters = '0123456789ABCDEF'
      var color = '#'
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)]
      }
      return color
    }
  }
}
```

#### '공용'/'인덱스'

```xml
<html>
  <head>
    <title>Feature flags 101</title>
  </head>
  <body>
    <div>
      <span id="span-welcome-text">My awesome website will have a new button!!!</span>
    </div>
    <script src="/js/new-feature.js"></script>
  </body>
</html>
```

이 업데이트된 코드의 기본 아이디어는 서버가 플래그가 사용 가능하다고 응답한 경우에만 버튼이 작성된다는 것입니다. 그렇지 않으면 UI가 손상된 기능 대신 사용자에게 표시될 수 있습니다.

클라이언트가 새 기능이 손상되었다고 보고하면 실행 UI를 열고 기능을 사용하지 않도록 설정해야 합니다. 10초 후 페이지를 새로 고치면 클라이언트는 손상된 버튼을 더 이상 볼 수 없습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/feature-flag-disabled-unleash-1.png?resize=720%2C184&ssl=1)

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/fallback-behavior.png?resize=358%2C84&ssl=1)

릴리스 설치 방법에 대한 자세한 내용은 공식 설명서를 참조하십시오.

## 버그 수정 중

사용자에 대해 비활성화된 기능 플래그 문제를 보면 `body.style.color = getRandomColor()`가 실제로는 `body.style.backgroundColor = getRandomColor()여야 합니다.

기능을 수정, 테스트 및 재배치한 후 기능 플래그를 활성화할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/flag-activated-unleash-ui-1.png?resize=720%2C218&ssl=1)

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/background-color-change-feature.gif?resize=337%2C239&ssl=1)

## 모드

플래그가 있으면 변경 내용이 사용자 기반으로 전파되는 방식을 제어할 수 있습니다.

Lunset에서 가장 유용한 세 가지 모드는 다음과 같습니다.

- 기본 전략(모든 사람에게 이 플래그 적용)
- 활성 세션을 가진 특정 비율의 사용자에게 활성화하려는 기능을 보여주는 `gradual RolloutSessionId`
- `gradual RolloutRandom`은 사용자가 페이지에 액세스할 때 임의로 활성화됩니다. 이 기능은 기능 플래그 아래에 있는 것(예: API의 새 버전의 끝점)이 사용자에게 표시되지 않는 경우에 특히 유용합니다.

공식 문서에서 사용 가능한 모든 선택사항을 확인할 수 있습니다.

## 피쳐 플래그를 사용해야 합니까?

마무리하기 전에 Node.js의 기능 플래그 사용에 대한 몇 가지 장단점을 살펴보겠습니다.

### 프로스

- 번거로운 기능을 롤백하기 위해 응용 프로그램을 중지할 필요가 없습니다.
- 일단 설정되면 기능 플래그를 프로젝트에 쉽게 추가할 수 있습니다.
- 피쳐 플래그를 사용하여 작은 그룹 간의 피쳐를 테스트하고 피드백을 수집할 수 있습니다.

### 콘스

- 피쳐 플래그가 안정적인 것으로 확인된 후에는 피쳐 플래그를 제거해야 합니다. 프로젝트가 데드 코드 또는 플래그를 누적하지 않도록 항상 제거 날짜를 계획해야 합니다.
- 올바르게 설정되지 않으면 기능 플래그가 잘못된 사용자 환경을 만들 수 있습니다. 예를 들어 플래그가 중지되기 전에 로드되어 더 이상 필요하지 않은 필드를 사용자가 채우고 몇 초 후에 사라집니다.
- 기존 기능의 실험 버전 배포와 관련하여 기존 코드와 새 코드를 언제든지 트리거할 수 있도록 유지하므로 두 배의 코드를 전송할 수 있습니다.

## 최종 발언

기능 플래그를 적절하게 사용하면 응용 프로그램의 새 릴리스 시작과 관련된 많은 스트레스를 제거할 수 있습니다. 문제가 발생하면 버튼을 클릭하여 원래 상태로 복원하고 최소한의 다운타임으로 문제를 해결할 수 있는 시간만 주면 됩니다.

또한 피쳐 플래그를 사용하면 메트릭을 사용하여 이전 피쳐와 새 피쳐를 비교할 때 뛰어난 통찰력을 얻을 수 있습니다. 이 예제를 사용하려면 사용자 지정 배경색을 선택하면 사용자가 페이지에 계속 표시되는 경우 앱에서 해당 기능을 유지하는 것을 고려해야 합니다. 시간이 지난 후에는 이 코드를 제거하는 것을 잊지 마십시오. 불필요한 코드 번들을 원하는 사람은 없습니다.

이 튜토리얼에 사용된 모든 코드는 내 GitHub 페이지에서 액세스할 수 있습니다.