---
layout: post
title: "빠른 미들웨어: 완전한 안내서"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/express-middlewares-complete-guide.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/express-middlewares-complete-guide.png?fit=730%2C487&ssl=1)

이 가이드에서는 Express.js 미들웨어를 사용하는 기본 사항에 대해 살펴봅니다. 처음부터 간단한 Express API를 생성한 다음 미들웨어를 추가하고 각 툴의 사용 방법을 시연합니다.

논의할 Express 미들웨어 도구는 초기 Express.js 앱 설정에 반드시 필요합니다. 이러한 구성 요소를 시작하는 방법을 보여드리며, 응용 프로그램의 고유한 요구 사항에 따라 추가로 구성할 수 있습니다.

다음 사항에 대해 살펴보겠습니다.

- Node.js란 무엇입니까?
- Express.js란 무엇입니까?
- Express 미들웨어란?
- 미들웨어는 어떻게 작동합니까?
- Express.js API 설정
- Express 미들웨어 사용
➡➡➡➡➡➡➡➡➡➡ ➡
헬멧
➡➡➡➡➡➡➡➡➡➡ ➡
급행 속도 제한
서빙 아이콘
- ➡➡➡➡➡➡➡➡➡➡ ➡
- 헬멧
- ➡➡➡➡➡➡➡➡➡➡ ➡
- 급행 속도 제한
- 서빙 아이콘

단순성을 위해 Express API 예에서는 끝점을 하나만 생성합니다. 전체 코드는 GitHub에서 사용할 수 있습니다.

## Node.js란 무엇입니까?

Node.js는 크롬의 V8 자바스크립트 엔진 위에 구축된 오픈 소스 자바스크립트 런타임 환경이다.

Node.js는 간단한 서버 생성과 같은 기본 작업을 처리할 수 있지만, 다른 경로에서 요청을 별도로 처리하거나 정적 파일을 제공하는 것과 같은 더 복잡한 작업은 더 어렵다.

## Express.js란 무엇입니까?

Express.js는 가장 대중적이고 널리 사용되는 노드 웹 프레임워크 중 하나이다. 사실 MERN, MEVN, MEAN Stack의 "E"는 "Express"의 약자이다.

공식 Express.js 설명서에 따르면, "Express는 Node.js를 위한 빠르고, 의견이 없으며, 미니멀리스트적인 웹 프레임워크이다." Express는 미니멀리스트이지만 매우 유연하여 Express.js와 함께 사용할 수 있는 다양한 미들웨어를 개발하여 생각할 수 있는 거의 모든 작업이나 문제를 해결합니다.

## Express 미들웨어란?

미들웨어는 요청-응답 주기 동안 실행되며 요청 객체(req)와 응답 객체(res)에 모두 액세스할 수 있는 기능을 포함하는 소프트웨어입니다. 미들웨어는 서버가 요청을 수신하는 시점과 응답을 보내는 시점 사이의 윈도우 동안 실행됩니다.

Express 미들웨어는 애플리케이션 레벨, 라우터 레벨 및 오류 처리 기능을 포함하며 내장 또는 타사로부터 내장될 수 있습니다. Express.js는 자체 기능이 제한적이기 때문에 Express 앱은 주로 여러 미들웨어 함수 호출로 구성되어 있다.

Express.js용 미들웨어를 직접 작성할 수 있지만 대부분의 개발자는 일반적인 작업에 기본 제공 및 타사 도구를 사용하고 구성합니다. 이 가이드에서는 가장 인기 있는 Express 미들웨어 5개를 사용하는 방법을 설명합니다. 먼저 미들웨어가 앱 내에서 어떻게 작동하는지 간략하게 설명합니다.

## 미들웨어는 어떻게 작동합니까?

미들웨어가 어떻게 작동하는지 이해하기 위해, 당신이 고객들이 그들 자신의 레몬을 가지고 와서 레모네이드를 만드는 레모네이드 판매대를 소유하고 있다고 상상해보라. 레몬의 기원과 신선도를 평가하고, 어떤 아편 레몬을 버리고, 마지막으로 레모네이드를 만드는 일을 담당합니다.

작업량을 줄이기 위해 레몬이 유기적으로 자라고 유해한 화학물질이 없는지 확인하기 위해 직원을 고용합니다. 이 비유에서 래리는 여러분과 여러분의 고객의 레몬 사이에서 기능하는 미들웨어입니다.

지금 당신은 수익을 올리고 있습니다. 그래서 당신은 두 명의 다른 직원인 컬리와 모이를 고용합니다. 래리는 레몬의 원산지를 확인하고 유기적으로 재배된 레몬을 컬리에게 건네줍니다. 컬리는 썩은 레몬을 버리고 좋은 레몬을 모이에게 건네줍니다. 모는 그들의 신선함을 확인하고 신선한 레몬을 당신에게 건네줍니다.

이제 여러분은 레모네이드를 만들고 여러분의 이익을 늘리는 데 집중할 수 있습니다.

당신의 HTTP 요청대로 레몬을 생각하고 당신의 레모네이드 스탠드를 서버로 생각하세요. 승인 또는 거부하기 전에 HTTP 요청과 마찬가지로 레몬의 원본을 확인합니다. 신뢰할 수 있는 오리진의 모든 요청이 양호한 것은 아니므로 필터링해야 합니다. 래리, 컬리, 모이 등 귀사의 직원들은 귀사의 레모네이드 프로그램을 위한 미들웨어와 같습니다. 미들웨어가 요청이 잘못된 것으로 판단하는 경우, 미들웨어는 요청-응답 주기를 종료할 수 있습니다.

일단 요청이 앱에 있는 모든 미들웨어를 통과하면 컨트롤러 기능에 도달합니다. 즉, 당사의 예에서는 사용자(또는 보다 구체적으로 레모네이드를 만드는 행위)입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/middleware-infographic.png?resize=730%2C411&ssl=1)

이것은 분명히 단순한 예일 뿐입니다. 실제 시나리오에서는 여러 미들웨어를 사용하여 사용자 로깅과 같은 단일 태스크를 수행해야 할 수 있습니다.

## Express.js API 설정

Express.js 미들웨어를 사용하는 방법을 시연하기 위해 단일 엔드포인트를 사용하여 간단한 Express API를 생성하겠습니다.

터미널에서 다음 명령을 실행합니다.

```bash
mkdir express-api
cd express-api
npm init -y
```

마지막 명령을 실행하면 `패키지`가 생성됩니다.프로젝트의 루트 디렉터리에 있는 json` 파일입니다. 다음과 같은 모양이 됩니다.

```undefined
{
"name": "express-api",
"version": "1.0.0",
"description": "",
"main": "index.js",
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1"
},
"keywords": [],
"author": "",
"license": "MIT"
}
```

다음 명령을 실행하여 `express`를 설치하십시오.

```coffeescript
npm install express
```

`index.js` 파일 생성:

```css
touch index.js
```

다음을 `index.js`에 추가하여 단순 Express API를 생성하십시오.

```coffeescript
const express = require("express");
const app = express();

// Port
const port = 3000;

app.get("/", (req, res) => {
  res.json({
    message: "Hello Stranger! How are you?",
  });
});

// Listen
app.listen(port, () => {
  console.log(`Listening on port: ${port}`);
});
```

프로젝트의 루트 디렉터리에서 다음 명령을 실행하여 `노데몬 없음`을 Devidentity로 설치하십시오. 이것은 훌륭한 지역 개발 도구이다.

```coffeescript
npm install -D nodemon
```

`nodemon`을 사용하면 Express.js 서버를 수동으로 재시작할 필요가 없습니다. `nodemon`은 파일 변경을 감지하고 서버를 자동으로 재시작합니다.

패키지에서 "스크립트"를 수정합니다.json은 다음과 같습니다.

```bash
"scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
```

네 소포.json은 다음과 같이 보여야 한다.

```undefined
{
  "name": "express-api",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "keywords": [],
  "author": "Ashutosh K Singh",
  "license": "MIT",
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.6"
  }
}
```

다음 명령을 실행하여 Express 서버를 시작합니다.

```coffeescript
npm run dev
```

서버가 시작되면 터미널에 다음 메시지가 표시됩니다.

```css
[nodemon] 2.0.5
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node index.js`
Listening on port: 3000
```

`http://localhost:3000`으로 이동하면 API에서 다음과 같은 응답을 볼 수 있습니다.

```undefined
{
  "message": "Hello Stranger! How are you?"
}
```

## Express 미들웨어 사용

이제 간단한 API를 설정했으므로 상위 Express.js 미들웨어 툴 중 5개와 사용 방법을 자세히 살펴보겠습니다. 각 미들웨어 조각, 미들웨어의 기능 및 Express API를 사용하여 설정하고 사용하는 방법에 대해 설명합니다.

### ➡➡➡➡➡➡➡➡➡➡ ➡

morgan은 각 API 요청에 대한 로그를 생성하는 Node.js용 HTTP 요청 로거 미들웨어입니다. 가장 좋은 점은 미리 정의된 형식을 사용하거나 필요에 따라 새 형식을 만들 수 있다는 것입니다.

morgan을 설치하려면 다음 명령을 실행합니다.

```coffeescript
npm install morgan
```

`➡`에는 사용할 수 있는 여러 가지 미리 정의된 형식이 포함되어 있습니다. 많은 개발자들은 표준 아파치 공통 로그 출력인 common을 사용하는 것을 선호한다.

다음과 같이 `index.js`를 수정합니다.

```js
const express = require("express");
const morgan = require("morgan")

const app = express();

// Middlewares
app.use(morgan("common"))


// Port
const port = 3000;

app.get("/", (req, res) => {
  res.json({
    message: "Hello Stranger! How are you?",
  });
});

// Listen
app.listen(port, ()=>{
    console.log(`Listening on port: ${port}`)
})
```

이제 끝이야! `http://localhost:3000`으로 이동하면 서버가 실행 중인 터미널에 `morgan`에서 이와 같은 로그가 생성됩니다.

```php
::ffff:127.0.0.1 - - [14/Oct/2020:09:21:16 +0000] "GET / HTTP/1.1" 304 -
```

다음은 동일한 내용의 스크린샷입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/screenshot-showing-morgan-log.png?resize=730%2C351&ssl=1)

미들웨어 도구는 정의하는 순서에 따라 실행됩니다.

### 헬멧

헬멧은 다양한 HTTP 헤더를 설정하여 Express.js 앱을 보호하는 보안 미들웨어입니다.

Helmet의 작동 방식을 더 잘 이해하려면 `http://localhost:3000/`로 이동하여 Chrome의 `CTRL + Shift + J` 또는 Firefox의 `CTRL + Shift + K`를 눌러 콘솔을 엽니다. 이제 네트워크 탭을 클릭합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/network-tab.png?resize=730%2C123&ssl=1)

네트워크 탭이 비어 있는 경우 네트워크 탭이 열린 상태에서 페이지를 다시 로드하면 항목이 가득 찬 것을 볼 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/network-tab-filling-up.png?resize=730%2C95&ssl=1)

지금은 `파비콘` 요청을 무시해도 됩니다. 나중에 얘기하도록 하겠습니다.

GET/` 요청 데이터를 클릭합니다. 응답 헤더 섹션에 초점을 맞춥니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/response-headers-dropdown.png?resize=730%2C379&ssl=1)

취약성은 보이지 않을 수 있지만, 현재 API의 상태에서는 공격자와 해커가 쉽게 이용할 수 있습니다. 특히 `X-Powered-By: Express` 필드는 앱이 Express.js를 실행하고 있는 세계로 브로드캐스트합니다.

헬멧은 잘 알려진 취약성 및 공격으로부터 앱을 보호하는 11개의 작은 미들웨어 모음입니다.

다음 명령을 실행하여 `헬멧`을 설치합니다.

```undefined
npm install --save helmet
```

다음과 같이 `index.js` 파일을 업데이트하여 `helmet` 미들웨어를 포함합니다.

```php
const express = require("express");
const morgan = require("morgan")
const helmet = require("helmet");
const app = express();


// Middlewares
app.use(morgan("common"))
app.use(helmet());
// Port
const port = 3000;

app.get("/", (req, res) => {
  res.json({
    message: "Hello Stranger! How are you?",
  });
});
// Listen
app.listen(port, () => {
  console.log(`Listening on port: ${port}`);
});
```

다시 `http://localhost:3000/`page 새로 고침 페이지로 이동하여 개발자 도구의 네트워크 탭 아래에 있는 응답 헤더 섹션을 엽니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/response-headers-under-network.png?resize=730%2C439&ssl=1)

이번에 볼 수 있듯이 응답 헤더에 새로운 항목이 있으며 `X-Powered-By: Express` 필드가 사라졌습니다.

이와 같은 미들웨어를 비활성화하도록 `헬멧() 기능을 구성할 수도 있습니다.

```php
// This disables the `referrerPolicy` middleware but keeps the rest.
app.use(
    helmet({
        referrerPolicy: false,
    })
  );
```

### ➡➡➡➡➡➡➡➡➡➡ ➡

CORS는 교차 오리진 자원 공유를 나타냅니다. Express.js 앱에서 CORS를 활성화하고 구성하는 데 사용됩니다.

포트 3000에서 Retactfrontend가 실행되고 포트 8000에서 Express 백엔드 서버가 실행되는 풀 스택 앱을 상상해보십시오. 클라이언트(예: Reactfrontend)에서 백엔드 Express 서버로 요청이 오지만 Express 서버가 아닌 다른 오리진에서 요청이 왔기 때문에 요청이 실패할 가능성이 높습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/cors-error.png?resize=730%2C143&ssl=1)

다른 오리진이라도 서버에 요청을 수락하도록 지시해야 합니다. 여기서 `코어`가 나옵니다.

다음 명령을 실행하여 `cors`를 설치하십시오.

```undefined
npm install --save cors
```

다음과 같이 `index.js`를 업데이트합니다.

```php
const express = require("express");
const morgan = require("morgan")
const helmet = require("helmet");
const cors = require("cors");
const app = express();


// Middlewares
app.use(morgan("common"))
app.use(helmet());
app.use(cors())

// Port
const port = 3000;

app.get("/", (req, res) => {
  res.json({
    message: "Hello Stranger! How are you?",
  });
});
// Listen
app.listen(port, () => {
  console.log(`Listening on port: ${port}`);
});
```

위의 코드 `app.use(cors()`는 모든 오리진의 요청을 허용하지만, 오리진의 요청을 수락하려는 공용 API가 없는 한 보안 취약성에 대한 앱이 열릴 수 있습니다.

React 및 Express가 포함된 풀 스택 앱의 위의 예를 살펴보겠습니다. 오리진의 요청을 허용하는 대신 허용된 도메인의 화이트리스트를 만들고 요청이 화이트리스트 도메인의 것인지 확인할 수 있습니다.

```js
// whitelist
const whitelist = ['http://localhost:3000', 'http://localhost:3001']
const corsOptions = {
  origin: function (origin, callback) {
    if (whitelist.indexOf(origin) !== -1) {
      callback(null, true)
    } else {
      callback(new Error('Not allowed by CORS'))
    }
  }
}
app.use(cors(corsOptions));
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/whitelisted-domain.png?resize=730%2C230&ssl=1)

MDN 웹 문서에서 CORS에 대한 자세한 내용을 볼 수 있습니다.

### 급행 속도 제한

ExpressRateLimit는 이름이 암시하듯이 동일한 IP 주소에서 반복되는 API 요청을 제한하는 Express.js의 기본 속도 제한 미들웨어입니다.

`특급 속도 제한`을 설치하는 방법은 다음과 같습니다.

```undefined
npm install --save express-rate-limit
```

다음으로 이 미들웨어를 index.js로 가져오고 limiter라는 변수를 만들어 express-rate-limit을 구성한다.

```js
const rateLimit = require("express-rate-limit");

const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100 // limit each IP to 100 requests per windowMs
  });
```

위의 코드는 각 IP 주소를 15분 동안 100개의 요청으로 제한합니다.

다음과 같이 `index.js`를 업데이트합니다.

```php
const express = require("express");
const morgan = require("morgan")
const helmet = require("helmet");
const cors = require("cors");
const rateLimit = require("express-rate-limit");

const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100 // limit each IP to 100 requests per windowMs
  });

const app = express();


// Middlewares
app.use(morgan("common"))
app.use(helmet());
app.use(cors())
app.use(limiter); //  apply to all requests

// Port
const port = 3000;

app.get("/", (req, res) => {
  res.json({
    message: "Hello Stranger! How are you?",
  });
});
// Listen
app.listen(port, () => {
  console.log(`Listening on port: ${port}`);
});
```

`특급 속도 제한`을 사용하는 방법을 더 잘 표시하려면 다음과 같이 `제한`을 변경하십시오.

```cpp
const limiter = rateLimit({
    windowMs: 60 * 1000, // 1 minute
    max: 2, // limit each IP to 2 requests per windowMs
    message: "Too many accounts created from this IP, please try again after a minute"
  });
```

http://localhost:3000/으로 이동하여 페이지를 서너 번 새로 고칩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/refreshing-page.gif?resize=600%2C325&ssl=1)

모건은 아직 유효하기 때문에 터미널에서 로그를 볼 수 있습니다.

```bash
::1 - - [14/Nov/2020:08:15:58 +0000] "GET / HTTP/1.1" 304 -
::1 - - [14/Nov/2020:08:15:59 +0000] "GET / HTTP/1.1" 304 -
::1 - - [14/Nov/2020:08:15:59 +0000] "GET / HTTP/1.1" 429 71
```

429 상태 코드는 지정된 시간 동안 사용자가 너무 많은 요청을 보냈음을 나타냅니다("속도 제한").

또한 특정 요청에 적용하거나 모든 요청에 적용되지 않도록 `특급 속도 제한`을 구성할 수도 있습니다.

```php
//  apply to all requests
app.use(limiter); 

// only apply to requests that begin with /api/
app.use("/api/", limiter);
```

다음은 확인할 만한 몇 가지 요율 제한 미들웨어입니다.

- 금리인하형
- ➡➡➡➡
- 금리인하

### 서빙 아이콘

서빙 아이콘(serve-conticon)은 미들웨어 서빙에 가장 적합한 아이콘이다. 헬멧 섹션에서 네트워크 탭을 열었을 때 실패한 즐겨찾기 요청을 기억할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/failed-favicon-request.png?resize=730%2C85&ssl=1)

즐겨찾기 아이콘은 주소 표시줄의 페이지 제목 왼쪽에 나타나는 작은 아이콘입니다. 예를 들어, 다음은 LogRocket 블로그의 즐겨찾기 아이콘입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/logrocket-favicon.png?resize=632%2C172&ssl=1)

serve-favicon을 설치하려면:

```coffeescript
npm install serve-favicon
```

또한 프로젝트의 루트 디렉터리에 즐겨찾기 파일이 필요합니다. GitHub에서 샘플 favicon을 잡을 수 있습니다.

다음과 같이 `index.js` 파일을 업데이트합니다.

```php
const express = require("express");
const morgan = require("morgan")
const helmet = require("helmet");
const cors = require("cors");
const rateLimit = require("express-rate-limit");
var favicon = require('serve-favicon')


const limiter = rateLimit({
    windowMs: 15 *60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per windowMs
    message: "Too many accounts created from this IP, please try again after a minute"
  });

const app = express();

// Serve Favicon
app.use(favicon('favicon.ico'))

// Middlewares
app.use(morgan("common"))
app.use(helmet());
app.use(cors())
app.use(limiter); //  apply to all requests

// Port
const port = 3000;

app.get("/", (req, res) => {
  res.json({
    message: "Hello Stranger! How are you?",
  });
});
// Listen
app.listen(port, () => {
  console.log(`Listening on port: ${port}`);
});
```

즐겨찾기 아이콘이 `public` 폴더에 있으면 `path`를 사용할 수 있습니다.

```php
var path = require('path')
 ...

// Serve Favicon
app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')))
...
```

`http://localhost:3000/`로 이동하면 페이지에 즐겨찾기 아이콘이 나타납니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/favicon.png?resize=730%2C157&ssl=1)

이제 네트워크 탭을 열고 페이지를 다시 로드합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/refreshed-network-page-showing-favicon.png?resize=730%2C287&ssl=1)

serve-hypicon은 또한 디스크 액세스를 줄임으로써 성능을 향상시키기 위해 메모리에서 favicon을 캐시한다.

기본적으로 serve-favicon은 1년 동안 favicon을 캐시한다.

```undefined
Cache-Control: public, max-age=31536000
```

favicon에서 maxAge의 속성을 가진 옵션 객체를 전달하여 캐시-컨트롤을 변경할 수도 있습니다.

```php
// Serve Favicon
app.use(
  favicon("favicon.ico", {
    maxAge: 500 * 60 * 60 * 24 * 1000,
  })
);
```

## 결론

이 기사에서는 5개의 Express.js 미들웨어를 사용하는 방법에 대해 논의하였다. 물론 Express API의 다른 미들웨어를 탐색하고 싶으실 수도 있지만, 본 가이드에서 살펴본 툴은 거의 모든 애플리케이션에서 사용할 수 있으며 Express 미들웨어가 API를 향상시키기 위해 할 수 있는 훌륭한 스냅샷을 제공합니다.