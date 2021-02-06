---
layout: post
title: "아주 작은 http vs. 속달: 어느 것이 더 나아요?"
author: "CSS Dev"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2021/01/feature-image.jpg"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/feature-image.jpg?fit=730%2C383&ssl=1)

## 도입

tinyhttp는 TypeScript로 작성되고 네이티브 ESM에 컴파일된 현대의 Express 같은 웹 프레임워크이다.

Express는 웹 및 모바일 애플리케이션을 위한 강력한 기능 집합을 제공하는 최소의 유연한 Node.js 웹 애플리케이션 프레임워크이다.

다음은 작은 http가 가지고 있는 가장 중요한 기능의 간단한 목록입니다.

- Express보다 2.5배 빠름
- 전체 Express 미들웨어 지원
- 비동기 미들웨어 지원
- 네이티브 ESM 및 공통JS 지원
- 기존 종속성 없이 JavaScript 자체만 사용 가능
- 상자에 입력 안 함

### 기본 비교

### 벤치마크

> 참고: 벤치마크는 완전히 정확하지 않고 실행마다 기계마다 다릅니다. 절대값 대신 비율을 비교해야 합니다.

결론: 작은 http(추가 `req` / res` 확장 없이)는 Express보다 약 1.9배 더 빠릅니다.

### 설치

패키지 관리자를 사용하여 작은 http 및 Express를 설치할 수 있습니다. 나는 시연에 pm을 사용할 것이다.

- tinyhttp: `npmi @http/app`
- Express: `npm install Express`

### 안녕 세계

익스프레스와 작은 http의 앱 구조는 꽤 비슷하다. Express를 알고 있으면 작은 http도 알 수 있습니다.

#### 작은 http

```coffeescript
import { App } from '@tinyhttp/app'
const app = new App()
const PORT = 3000

app
  .get('/', (_, res) => void res.send('<h1>Hello World</h1>'))
  .listen(PORT, () => console.log(`Started on http://localhost:${PORT}!`))
```

#### 익스프레스

```coffeescript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello world');
});

app.listen(3000, () => console.log('listening on port 3000'));
```

당신은 아마도 우리가 `require` 대신 ESM 수입품을 사용한다는 것을 알아차렸을 것이다. 작은 http는 네이티브 ESM과 함께 사용하도록 권장된다. 작은 http는 익스프레스보다 훨씬 작으면서도 ESM과 Common.js 모듈 시스템으로 컴파일된다.

Node.js에서 `import` / `export` 구문을 함께 사용할 수 있습니다. 노드 ESM 패키지를 설정하려면 패키지에 "type": "module"을 넣으십시오.json 파일은 다음과 같습니다.

```undefined
{
  "type": "module"
}
```

다른 옵션은 `.mjs` 확장을 사용하는 것입니다. 그런 다음 "type" 필드를 package.json에 넣을 필요가 없습니다. 자세한 내용은 ECMAcript ModulesNode.js 설명서를 참조하십시오.

대부분의 인기 있는 익스프레스 미들웨어는 구식 모듈도 사용하기 때문에, 작은 http는 로거, 세션 등과 같은 인기 있는 제품의 재작성/리메이크 세트를 제공한다.

## 라우팅

작은 http와 Express에서 몇 가지 기본 라우팅을 처리합시다. Express에는 req 및 res 개체에 많은 도우미 기능이 포함되어 있습니다. tinyhttp는 res.send, res.download, res.redirect 등의 방법으로 Express API를 완벽하게 구현한다.

#### 작은 http

```coffeescript
import { App } from '@tinyhttp/app'
import { once } from 'events'

const app = new App()
const PORT = 3000

app.get('/', (req, res) => {
    res.send('Sent a GET!')
})

app.post('/', async (req, res) => {
    // Nothing complex here, we just listen to 'data' event and return the data as a promise to a `data` variable
    const data = await once(req, 'data').then(d => d.toString())

    // And then we send it
    res.end(`Sent some data: ${data}`)
})

app.listen(PORT, () => console.log(`Started on http://localhost:${PORT}!`))
```

#### 익스프레스

```js
var express = require('express');
var app = express();

app.get('/', function(req, res){
   res.send("Send a GET!");
});

app.post('/', function(req, res){
   res.send("hello'!\n");
});

app.listen(3000);
```

## 결론

작은 http는 빠르고 가볍다. 오늘부터 백엔드 애플리케이션에 사용할 수 있습니다. 작은 http 저장소에는 MongoDB와 GraphQL 통합을 포함한 많은 예가 포함되어 있습니다.