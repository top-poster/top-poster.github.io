---
layout: post
title: "Fastify로 초고속 API 구축 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/fastify-API-server.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/fastify-API-server.png?fit=730%2C487&ssl=1)

Fastify는 강력한 플러그인 아키텍처와 최소한의 오버헤드로 백엔드 웹 개발을 위한 프레임워크입니다. 주로 Hapi와 Express에서 영감을 받았으며 Node.js에서 실행되는 가장 빠른 웹 프레임워크 중 하나입니다.

Fastify v3.0은 최신 버전이며 2020년 7월 초부터 사용할 수 있습니다. 버전 3에는 다음과 같은 몇 가지 예외적인 새로운 기능이 있다.

- Fastify에서 Express 애플리케이션 실행
- 로깅 직렬화의 변경 사항
- 스키마 대체 변경 사항
- 더 나은 TypeScript 지원

## Fastify의 기능 개요

이제 Fastify의 최신 버전 업데이트 기능에 대해 알아보았으니 Fastify의 가장 중요한 기능에 대한 간략한 목록을 살펴보도록 하겠습니다.

코드 복잡성에 따라 덜 복잡한 비즈니스 로직을 위해 초당 약 30,000건의 요청을 처리할 수 있습니다.

후크, 플러그인 및 장식기로 완벽하게 확장 가능

내부적으로 Fastify는 JSON 스키마를 경로 검증 및 출력 직렬화에 사용할 수 있는 고성능 함수로 컴파일합니다.

Pino는 보다 빠른 로깅을 위해 사용되는 매우 비용 효율적인 로거입니다.

이 프레임워크는 표현력이 뛰어나고 쉽게 시작할 수 있습니다. 또한, 개발자는 성능이나 보안의 저하 없이 소규모 프로젝트를 더 큰 성능의 애플리케이션으로 신속하게 확장할 수 있습니다.

TypeScript 유형 선언 파일은 TypeScript 커뮤니티에 대한 지원을 유지합니다.

## 알아야 할 5가지 중요한 Fastify 플러그인

Fastify가 제공하는 방대한 기능 외에도 강력한 플러그인 아키텍처도 갖추고 있습니다. 모든 개발자는 Fastify와 함께 작동하는 플러그인을 빌드하여 API 프로젝트를 부트스트래핑하기 위한 빠른 구성 요소를 만들 수 있습니다.

외부 개발자가 개발한 플러그인은 `커뮤니티 플러그인`의 범주에 속하며, 패스트파이 팀도 `코어 플러그인`으로 분류하는 자체 플러그인을 일부 유지하고 있다. 그러나 커뮤니티 플러그인은 Fastify의 모범 사례를 준수해야 합니다.

핵심 플러그인을 사용하는 이점은 Fastify 팀이 이러한 플러그인을 적극적으로 유지하지만 커뮤니티 플러그인이 유지되지 않을 수 있다는 점을 유념해야 한다는 것이다.

다음은 몇 가지 중요한 Fastify 플러그인입니다.

- fastify-auth: API 경로에 인증 로직을 빠르게 주입할 수 있도록 Fastify 팀에서 개발한 인증 플러그인
- fastify-cors: 크로스 오리진 요청은 모든 애플리케이션에 중요하며, fastify-cors는 별도로 CORS 패키지를 설치할 필요 없이 이를 관리할 수 있도록 도와줍니다.
- 고정 장치: 이 플러그인은 표준 JSON 웹 토큰으로 응용 프로그램을 장식합니다. Fastify-jwt는 내부적으로 json 웹 토큰 패키지를 사용합니다.
- fastify-nextjs: 다음은 서버 측에 사전 렌더링 웹 사이트를 구축하기 위한 Retact 프레임워크입니다. 이 플러그인을 사용하면 Fastify에서도 동일한 작업을 수행할 수 있습니다.
- fastify-redis: 이렇게 하면 Fastify 응용 프로그램이 서버 전체에서 동일한 Redis 연결을 공유할 수 있습니다.

그리고 그것은 심지어 포괄적인 목록도 아닙니다. Fastify에는 선택할 수 있는 광범위한 플러그인이 있습니다.

## Fastify vs Koa vs Express

물론 각 프레임워크에는 장단점이 있지만, 각 프레임워크에는 적용 분야도 있습니다. 다양한 틀을 비교하는 것은 쉽지 않습니다. 하지만, 저는 당신이 프레임워크를 선택할 때 관련 평가 기준을 선택하려고 노력했습니다.

### 속도 비교

다음은 StackShare.io의 속도 비교 개요입니다.

Express: Express는 초당 최소 요청량을 처리합니다. 이 벤치마크는 Express가 초당 15,978건의 요청을 처리할 수 있음을 증명합니다.
코아: 코아가 익스프레스보다 더 나은 선택이야. 또한 54,848개의 요청을 초당 처리하는 경량 프레임워크입니다.
Fastify: Fastify는 초당 78,956건의 요청으로 최고의 벤치마크 결과를 얻었습니다.

### 플러그인 생태계

앞 절에서 논의한 바와 같이, Fastify는 이 세 가지 중 유일하게 광범위한 플러그인을 가지고 있는 웹 프레임워크이며, 큰 차이를 만든다. 개발자들은 애플리케이션을 구축하기 위해 여러 프레임워크나 패키지에 의존할 필요가 없기 때문에, 개발자들에게 큰 플러스가 됩니다. Fastify는 원스톱 솔루션이 됩니다.

### TypeScript 지원

Fastify는 TypeScript 지원을 즉시 사용할 수 있는 유일한 프레임워크입니다. Node.js 버전에 따라 `@types/node`를 설치해야 할 수도 있습니다.

## Fastify 3.0을 사용하여 첫 번째 서버 생성

그리고 이제, 신나는 부분! 이 튜토리얼에서는 Fastify를 사용하여 첫 번째 서버를 구축하는 과정을 안내하며, 다음 사항에 대해 설명합니다.

- 설치
- 첫 번째 서버 실행
- API에 경로 추가
- Fastify 플러그인 사용
- 데이터 검증 기능 추가

준비됐어? 시작하자.

## 1. 설치 및 요구 사항

먼저 다음을 사용하여 새 npm 프로젝트를 시작합니다.

```coffeescript
npm init -y
```

다음으로, 프로젝트에 Fastify 종속성을 추가하겠습니다.

npm 사용:

```coffeescript
npm i fastify --save
```

실 사용:

```undefined
yarn add fastify
```

시스템에 최신 Node.js 버전이 설치되어 있는지 확인하십시오. nvm(Node Version Manager)을 사용하여 서로 다른 Node.js 버전 간에 빠르게 전환할 수 있습니다. cURL 또는 Postman과 같은 요청을 보낼 도구도 필요합니다.

## 2. server.js 생성

다음으로, 프로젝트의 루트에 `server.js`라는 새 파일을 만들어 보겠습니다. 다음 코드를 `server.js` 파일에 추가합니다.

```coffeescript
const fastify = require('fastify')({ logger: true })

//Add routes here, discussed in further steps

//@Server
fastify.listen(5000, (err) => {
  if (err) {
    console.log(err)
    process.exit(1)
  } else {
    console.log(`Server running, navigate to  https://localhost:5000`)
  }
})
```

보시는 것처럼 수신 기능은 포트 `5000`에서 서버를 시작합니다. 또한 오류 개체를 포함할 수 있는 하나의 인수를 허용하는 콜백도 수락합니다. 이것은 Fastify API를 실행하기 위한 가장 기본적인 서버 설정입니다.

이 기본 설정을 시도하려면 `node` 명령을 사용하여 다음과 같이 `server.js` 파일을 실행할 수 있습니다.

```css
node server.js
```

이렇게 하면 서버가 http://localhost:5000 주소로 시작됩니다. 주소로 이동하려고 하면 아직 경로를 정의하지 않았기 때문에 이 경로가 존재하지 않는다는 오류가 표시됩니다. 이제 간단한 CRUD 경로를 추가해야 합니다.

## 3. CRUD 경로 추가

응용 프로그램에 몇 가지 기본 CRUD 경로를 추가하겠습니다. 먼저 GET 경로를 추가하십시오.

### 3.1 GET 경로

우리가 타입 어레이인 스택 객체를 가지고 있다고 상상해 보세요. 먼저 이 배열의 내용을 검색할 GET 경로를 추가하고자 합니다. 이를 위해 Fastify 개체를 사용하여 get 경로를 정의할 수 있습니다. 첫 번째 인수는 경로를 연결할 경로를 수락하고 두 번째 인수는 고객에게 회신을 보내는 콜백을 수락합니다.

```coffeescript
const stack = []

//@Routes
fastify.get('/getStack', (request, reply) => {
  reply.send(stack)
})
```

### 3.2 POST 경로

다음으로 POST 경로를 사용하여 스택 어레이에 항목을 추가해 보겠습니다. 이렇게 하면 우리는 요청과 함께 데이터를 보낼 수 있습니다. 여기서는 사용자가 `항목`이라는 매개 변수가 하나 있는 JSON 개체를 보낼 것으로 예상한다. 우리는 이 `항목`을 스택 배열에 넣습니다. 이제 POST 요청과 함께 전송되는 데이터를 포함하는 콜백 `request`의 첫 번째 인수를 사용할 수 있습니다.

```coffeescript
fastify.post('/addItem', (request, reply) => {
    const item = request.body.item
    stack.push(item)
    reply.send(stack)
})
```

PUT, DELETE, HEAD, PATCH 및 OPTIONS와 같은 다른 경로 방법에도 동일한 원칙이 적용됩니다. 경로 옵션에 대한 자세한 내용은 Fastify 설명서를 참조하십시오.

### 3.3 최종 라우팅 코드

최종 라우팅 코드는 다음과 같아야 합니다.

```coffeescript
const fastify = require('fastify')({ logger: true })

const stack = []

//@Routes
fastify.get('/getStack', (request, reply) => {
  reply.send(stack)
})

fastify.post('/addItem', (request, reply) => {
    const item = request.body.item
    stack.push(item)
    reply.send(stack)
})

//@Server
fastify.listen(5000, (err) => {
  if (err) {
    console.log(err)
    process.exit(1)
  } else {
    console.log(`Server running, navigate to  https://localhost:5000`)
  }
})
```

이제 우리가 만든 코드를 시험해 봅시다. 먼저 `nodeserver.js`로 서버를 시작합니다. 그런 다음 빈 어레이 개체를 반환해야 하는 다음 경로 http://localhost:5000/getStack을 방문하십시오.

cURL을 이용해서 스택에 아이템을 추가하자. 스택에 애플을 추가하고 싶다. 따라서, 나는 키 `항목`과 값 `apple`이 있는 JSON 객체를 보냅니다.

```undefined
curl --header "Content-Type: application/json" --request POST --data '{"item": "apple"}' http://localhost:5000/addItem
```

http://localhost:5000/getStack을 다시 방문하면 스택 배열이 `apple` 항목으로 채워지는 것을 알 수 있습니다.

다 됐나요? 플러그인 추가!

## 4. Fastify API에 플러그인 추가

Fastify 플러그인을 추가하고 사용하는 것이 얼마나 쉬운지 입증하기 위해 Fastify 경로를 설치하면 Fastify 인스턴스로 등록된 모든 경로의 맵을 검색할 수 있습니다.

먼저 CLI에서 Fastify-route 종속성을 설치합니다.

```coffeescript
npm i fastify-routes
```

플러그인을 설치한 후 경로를 등록하기 전에 플러그인을 포함하여 플러그인을 등록합니다.

다음은 `fastify-routes` 플러그인을 포함하는 `server.js` 파일의 조각입니다. 또한 플러그인을 사용하여 등록된 모든 경로를 반환하는 방법을 보여주는 `console.log` 문을 추가했습니다.

```coffeescript
const fastify = require('fastify')({ logger: true })
fastify.register(require('fastify-routes')) // Add and register plugin

const stack = []

//@Routes
fastify.get('/getStack', (request, reply) => {
    reply.send(stack)
})

fastify.post('/addItem', (request, reply) => {
    const item = request.body.item
    stack.push(item)
    reply.send(stack)
})

//@Server
fastify.listen(5000, (err) => {
    console.log(fastify.routes) // Log all registered routes
    if (err) {
        console.log(err)
        process.exit(1)
    } else {
        console.log(`Server running, navigate to  https://localhost:5000`)
    }
})
```

이제 `nodeserver.js`로 서버를 시작하면 CLI가 등록된 모든 경로를 인쇄합니다.

이 플러그인은 서버에 플러그인을 추가하는 것이 얼마나 쉬운지 보여줍니다. 초기화할 필요가 없습니다. 서버를 구성하는 `fastify` 개체는 등록된 모든 플러그인의 상위 역할도 하며, 이 `fastify` 개체에서 직접 호출할 수 있습니다.

마지막으로 기본 데이터 유효성 검사를 서버에 추가하겠습니다.

## 5. 데이터 유효성 검사 추가

이 튜토리얼의 마지막 요소로 데이터 유효성 검사를 경로에 추가해 보겠습니다. 특히 이전에 생성한 POST 경로에 대한 유효성 검사를 추가하려고 합니다. 본문 객체에 `항목` 속성이 포함되어 있고 데이터 유형이 `문자열` 유형과 일치해야 하는지 확인하겠습니다.

다행히도, Fastify를 사용하면 경로에 대한 검증 스키마를 정의할 수 있습니다. 다음은 `항목` 속성이 있고 `문자열`이 포함되어 있는지 확인하는 예입니다. 또한 Fastify 서버에 `additional Properties: false` 설정을 사용하여 본문 개체에 대한 추가 속성을 허용하지 않는다고 말합니다.

요청이 성공했을 때 응답을 설명하는 응답 속성을 정의할 수도 있습니다.

데이터 유효성 검사 옵션을 추가한 후의 전체 코드입니다. pOST 경로에 두 번째 인수로 `itemValidation`을 추가하는 것을 잊지 마십시오.

```coffeescript
const fastify = require('fastify')({ logger: true })
fastify.register(require('fastify-routes'))

const itemValidation = { // Define validation
    schema: {
        body: {
            type: 'object',
            additionalProperties: false,
            required: [
                'item'
            ],
            properties: {
                item: { type: 'string' }
            }
        },
        response: {
            201: {
                type: 'object',
                properties: {
                    item: { type: 'string' }
                }
            }
        }
    }
}

const stack = []

//@Routes
fastify.get('/getStack', (request, reply) => {
    reply.send(stack)
})

fastify.post('/addItem', itemValidation, (request, reply) => { // Add validation options to POST route
    const item = request.body.item
    stack.push(item)
    reply.send(stack)
})

//@Server
fastify.listen(5000, (err) => {
    console.log(fastify.routes)
    if (err) {
        console.log(err)
        process.exit(1)
    } else {
        console.log(`Server running, navigate to  https://localhost:5000`)
    }
})
```

같은 요청을 서버에 보내고 `apple` 항목을 추가하여 코드를 다시 테스트해 봅시다. 이 요청을 성공적으로 실행해야 합니다.

```undefined
curl --header "Content-Type: application/json" --request POST --data '{"item": "apple"}' http://localhost:5000/addItem
```

다음으로, 서버가 요청을 거부하는지 테스트할 수 있도록 빈 객체가 포함된 항목을 보내 봅니다. 다음 요청을 서버에 전송하여 데이터 유효성 검사 구현을 확인합니다.

```undefined
curl --header "Content-Type: application/json" --request POST --data '{"item": {}' http://localhost:5000/addItem
```

서버에서 다음 오류 메시지를 보내야 합니다.

```undefined
{"statusCode":400,"error":"Bad Request","message":"body.item should be string"}
```

다 됐나요? 축하합니다! 첫 번째 Fastify API 서버를 성공적으로 완료했습니다.

## 결론

데이터 검증을 구현하고 플러그인을 추가하는 동안 Fastify를 사용하여 간단한 CRUD API를 구축하셨기를 바랍니다.

더 많은 플러그인이 존재하므로 Fastify 플러그인 에코시스템을 통해 사용 가능한 플러그인이 무엇인지 확인하십시오. 플러그인은 API를 신속하게 부트스트랩하여 처음부터 API를 빌드할 필요가 없도록 하는 데 유용합니다.

Fastify가 제공하는 다음 개념을 언제든지 확인해 보십시오.

- 데이터 직렬화
- 서버에 HTTP2를 사용하는 방법
- 서버의 특정 이벤트를 수신하기 위한 후크 추가
- 사용자 정의 미들웨어가 필요하십니까? 그냥 추가해!

바로 그거예요, 여러분! 전체 코드는 내 GitHub 저장소에서 찾을 수 있습니다.