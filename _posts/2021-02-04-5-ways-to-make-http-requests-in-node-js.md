---
layout: post
title: "Node.js에서 HTTP 요청을 만드는 5가지 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/02/Screen-Shot-2021-02-02-at-2.27.58-PM.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/Screen-Shot-2021-02-02-at-2.27.58-PM.png?fit=730%2C486&ssl=1)

## 도입

Node.js에서 HTTP 요청을 하는 방법은 여러 가지가 있습니다. 우리는 물론 표준 HTTP/HTTPS 모듈을 사용하여 그렇게 할 수도 있고, 우리의 삶을 훨씬 더 쉽게 만드는 npm 패키지 중 하나를 사용할 수도 있습니다.

이 게시물에서는 Node.js 설치와 함께 기본 제공되는 네이티브 HTTPS 모듈의 코드 예와 Axios, Got, SuperAgent 및 node-fetch와 같은 npm 패키지를 살펴보겠습니다. 자, 시작하자!

### 전제조건

설명과 코드를 자세히 살펴보기 전에 다음은 원격 모의 JSON API 호출을 포함하는 Node.js 코드로 손을 더럽히는 몇 가지 전제 조건입니다.

- 시스템에서 Node.js가 실행 중이어야 합니다(도커 컨테이너로 사용). 모든 예제는 활성 LTS인 Node.js 14.x를 사용하여 실행됩니다.
- npm init와 같은 npm 명령에 익숙하며, 프로젝트에 npm install --save <module-name을 사용하여 npm 패키지를 설치할 수 있습니다.
- 명령줄에서 `node <filename`으로 JavaScript 파일을 실행하여 출력 예제를 확인할 수 있습니다.
- 콜백, 약속 및 비동기/대기 기능에 익숙합니다.

기본 사항이지만 더 진행하기 전에 확인받아서 좋습니다 🙂

### 사용할 예제

JSON Placeholder mock API에서 데이터를 호출하여 모든 HTTP 클라이언트 옵션으로 GET 요청을 예시하겠습니다. 그것은 우리에게 10명의 사용자 데이터를 되돌려 보낼 것입니다. 우리는 각 사용자의 이름과 사용자 ID를 출력할 것입니다.

모든 코드는 별도의 풀 요청으로 표시됩니다. GitHub의 이 오픈 소스 저장소에서 수집된 모든 코드 예를 볼 수 있습니다. 첫 번째 예는 콜백 기반이고, 다음 두 가지는 약속 기반이며, 마지막 두 가지는 비동기/대기 기능을 사용합니다.

## Node.js의 HTTP 요청에 대한 클라이언트 옵션

플레이스홀더 API에 GET HTTP 호출을 할 수 있는 5가지 옵션을 살펴보겠습니다. Node.js는 많은 HTTP(S) 관련 작업을 수행할 수 있는 모듈들을 내장하고 있는데, 그 중 하나가 HTTP 호출이다. Node.js로 기본 제공되는 기본 HTTP(S) 옵션을 첫 번째 예로 살펴보겠습니다.

### 표준 노드.js HTTP(S) 모듈

Node.js는 표준 라이브러리에 HTTP 및 HTTPS 모듈과 함께 제공됩니다. 예를 들어 HTTPS URL이므로 HTTPS 모듈을 사용하여 GET 통화를 수행합니다. 다음은 코드 예제입니다.

```coffeescript
const https = require('https');

https.get('https://jsonplaceholder.typicode.com/users', res => {
  let data = [];
  const headerDate = res.headers && res.headers.date ? res.headers.date : 'no response date';
  console.log('Status Code:', res.statusCode);
  console.log('Date in Response header:', headerDate);

  res.on('data', chunk => {
    data.push(chunk);
  });

  res.on('end', () => {
    console.log('Response ended: ');
    const users = JSON.parse(Buffer.concat(data).toString());

    for(user of users) {
      console.log(`Got user with id: ${user.id}, name: ${user.name}`);
    }
  });
}).on('error', err => {
  console.log('Error: ', err.message);
});
```

코드를 살펴봅시다. 먼저 Node.js 설치 시 사용할 수 있는 `https` 표준 노드 모듈이 필요하다. 패키지는 필요 없습니다.json 파일 또는 npm install --save를 사용하여 실행할 수 있습니다.

그런 다음 "res" 변수에 입력한 응답을 제공하는 콜백이 있는 "get" 방식으로 JSON 자리 표시자 URL을 호출합니다.

다음으로, 우리는 `데이터`를 빈 배열로 초기화하고, 그 후에 응답자 헤더에서 상태 코드와 날짜를 기록한다. 그런 다음 데이터를 얻을 때마다 데이터 어레이에 청크를 푸시합니다.

그런 다음 응답 끝에서 어레이 데이터를 연결하여 문자열로 변경하고 JSON을 구문 분석하여 10명의 사용자 목록을 개체 배열로 가져옵니다. 결과적으로, 우리는 10명의 사용자를 순환시키고 사용자 개체의 ID와 이름을 한 번에 하나씩 기록한다.

여기서 한 가지 주의해야 할 점은 요청에 오류가 있는 경우 오류 메시지가 콘솔에 기록됩니다. 위의 코드는 참조용으로 풀 요청으로 사용할 수 있습니다.

HTTPS는 표준 Node.js 모듈이기 때문에 `패키지`가 필요하지 않다.json — Node.js 프로젝트 중 일부에 대해 이 말을 하고 싶습니다.

파일의 이름이 `native-https.js`인 경우 `node native-https.js`로 코드를 실행할 수 있습니다. 다음과 같은 출력이 표시되어야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/Output-of-node-native-code.png?resize=730%2C290&ssl=1)

동일한 방법을 사용하여 이 게시물의 다른 모든 예제를 실행할 수 있습니다. 상태 코드, 응답 헤더의 날짜, 응답 본문의 사용자 ID 및 이름과 유사한 출력이 표시됩니다.

다음 차례는 Axiosnpm 패키지입니다. 이를 위해서는 `패키지`가 필요합니다.json의 파일 방법을 볼 시간이다.

### 악시오스

Axios는 매우 인기 있는 약속 기반 요청 라이브러리이다. 브라우저와 Node.js에서 모두 사용할 수 있는 HTTP 클라이언트이다. 또한 요청 및 응답 데이터 가로채기와 같은 편리한 기능과 요청 및 응답 데이터를 JSON으로 자동 변환하는 기능도 포함되어 있습니다.

다음 명령을 사용하여 Axios를 설치할 수 있습니다.

```undefined
npm install --save axios
```

그러면 그걸 사용할 수 있어요. Axios를 사용하여 모의 사용자를 JSON API로 호출하는 예를 살펴보겠습니다.

```js
const axios = require('axios');

axios.get('https://jsonplaceholder.typicode.com/users')
  .then(res => {
    const headerDate = res.headers && res.headers.date ? res.headers.date : 'no response date';
    console.log('Status Code:', res.status);
    console.log('Date in Response header:', headerDate);

    const users = res.data;

    for(user of users) {
      console.log(`Got user with id: ${user.id}, name: ${user.name}`);
    }
  })
  .catch(err => {
    console.log('Error: ', err.message);
  });
```

보시는 것처럼, 여기에서는 이전 예보다 코드가 더 적습니다. 콜백 지향과 달리 약속 기반이기 때문에 원할 경우 이 코드를 비동기/대기 형식으로 쉽게 전환할 수 있습니다.

코드 예제에서 어떤 작업을 수행하는지 설명하겠습니다. 먼저, 우리는 `axios` 라이브러리를 요구한 다음 JSON 자리 표시자 사용자를 `axios.get`(약속 기반)으로 API라고 부른다.

우리는 `그때` 방법을 사용하여 약속이 해결되었을 때 결과를 얻고 응답 객체를 `res` 변수로 얻습니다. `그때` 방법에서는 응답 헤더에서 상태 코드와 날짜를 기록합니다.

우리는 Axios의 자동 변환 덕분에 JSON 데이터를 `res.data`로 쉽게 배열할 수 있다. 결과적으로, 우리는 ID와 이름을 기록하면서 사용자들을 순환한다. 오류가 발생할 경우 콘솔에 오류 메시지를 기록합니다. 코드 예제는 풀 요청으로도 액세스할 수 있습니다.

다음으로, 우리는 또 다른 인기 있고 기능이 풍부한 도서관인 Got을 살펴볼 것이다.

### 얻었다

Got은 Node.js에 대해 널리 사용되는 또 다른 HTTP 요청 라이브러리입니다. 그것은 "Node.js를 위한 인간 친화적이고 강력한 HTTP 요청 라이브러리"라고 주장한다. 또한 약속 기반 API를 제공하며 HTTP/2 지원 및 페이지 지정 API는 Got의 USP이다. 현재 Got은 Node.js에서 가장 인기 있는 HTTP 클라이언트 라이브러리로, 매주 1,900만 건 이상의 다운로드가 가능하다.

다음 명령으로 Got을 설치할 수 있습니다.

```undefined
npm install --save got
```

다음은 Got을 사용하여 모의 API에서 사용자를 가져오는 간단한 예입니다.

```js
const got = require('got');

got.get('https://jsonplaceholder.typicode.com/users', {responseType: 'json'})
  .then(res => {
    const headerDate = res.headers && res.headers.date ? res.headers.date : 'no response date';
    console.log('Status Code:', res.statusCode);
    console.log('Date in Response header:', headerDate);

    const users = res.body;
    for(user of users) {
      console.log(`Got user with id: ${user.id}, name: ${user.name}`);
    }
  })
  .catch(err => {
    console.log('Error: ', err.message);
  });
```

이 코드 예는 Axios와 매우 유사하지만 두 가지 주요 차이점이 있습니다.

- {{ 응답}을(를) 넘겨야 했습니다.응답이 JSON 형식임을 나타내는 두 번째 매개 변수로 `json`}`을 입력합니다.
- 상태 코드 헤더의 이름이 `statusCode`가 아니라 `statusCode`입니다.

다른 것들은 기본적으로 악시오스와의 이전 요청과 동일하게 유지되었다. 이러한 예는 이 풀 요청에서도 확인할 수 있습니다.

다음은 슈퍼 에이전트를 살펴보겠습니다.

### 슈퍼 에이전트

SuperAgent by VisionMedia는 2011년 4월에 출시된 가장 오래된 Node.js 요청 패키지 중 하나이다. Node.js를 위한 강력한 HTTP 라이브러리인 SuperAgent는 그 자체를 "작고 점진적이며 클라이언트측 HTTP 요청 라이브러리 및 동일한 API를 가진 Node.js 모듈로 브랜드화하여 많은 고급 HTTP 클라이언트 기능을 지원한다. 콜백 기반 API와 약속 기반 API를 모두 제공합니다. 약속 기반 API에서는 비동기/대기 기능을 사용하는 것이 그 위에 구문론적인 설탕에 불과하다.

또한 SuperAgent는 캐시 없음에서 HTTP 타이밍 측정에 이르기까지 다양한 플러그인을 제공합니다.

다음 명령을 사용하여 SuperAgent를 설치할 수 있습니다.

```undefined
npm install --save superagent
```

SuperAgent의 예제 사용자 API 호출에 대해 살펴보겠습니다. 다양한 기능을 제공하기 위해, 약속 기반 예제와 비교하여 즉시 호출된 기능 표현식(IIFE)과 함께 이 그림에 비동기/대기 기능을 사용합니다.

```js
const superagent = require('superagent');

(async () => {
  try {
    const res = await superagent.get('https://jsonplaceholder.typicode.com/users');
    const headerDate = res.headers && res.headers.date ? res.headers.date : 'no response date';
    console.log('Status Code:', res.statusCode);
    console.log('Date in Response header:', headerDate);

    const users = res.body;
    for(user of users) {
      console.log(`Got user with id: ${user.id}, name: ${user.name}`);
    }
  } catch (err) {
    console.log(err.message); //can be console.error
  }
})();
```

Super Agent를 통해 요청을 어떻게 수행했는지 자세히 살펴보겠습니다. 우리는 테스트 HTTP GET 호출을 위해 `superagent` 라이브러리가 필요했습니다. 우리는 다음 시점에서 언급한 바와 같이 wait를 사용하기 위해 iIFE를 sync로 시작했다.

다음으로 try 블록에서 wait로 superagent.get을 불렀는데, 이것은 약속을 해결하고 우리의 모의 사용자 API에 HTTP 호출의 결과를 우리에게 줄 것이다. 그런 다음 res 변수에서 res에서 날짜를 선택합니다.헤더 및 콘솔에 기록된 상태 및 날짜입니다.

그 후 응답 본문을 `사용자` 상수로 설정하고 10명의 사용자 배열을 순환하여 각 사용자의 이름과 ID를 출력하였다. 따라서 `catch` 블록이 있으며, `try` 블록의 어느 곳에서든 오류가 발생하면 해당 블록이 캡처되고 오류 메시지가 콘솔에 기록됩니다.

SuperAgent는 성숙하고 전투적인 테스트를 거쳤기 때문에 상당히 안정적입니다. 또한 SuperTest를 통해 SuperAgent 호출을 테스트할 수도 있습니다. SuperTest는 자체적으로 매우 편리한 라이브러리입니다. 위의 예와 같이 SuperAgent 코드를 풀 요청으로 사용할 수 있습니다.

이제 노드-페치를 살펴보겠습니다.

### 노드로 된

노드-페치(Node-fetch)는 Node.js에 대한 HTTP 요청 라이브러리로, 2020년 12월 첫 주에 pnpm 추세와 같이 2천만 번 이상 다운로드되었다.

즉, "노드-페치"는 Petch API(창)를 가져오는 경량 모듈입니다.fetch`)를 Node.js로" 브라우저 기반 `창`과의 정합성이 특징이다.fetch 및 네이티브 약속 및 비동기식 함수

아래 명령을 사용하여 노드-페치를 설치할 수 있습니다.

```undefined
npm install --save node-fetch
```

다음으로, 노드 페치가 어떻게 우리의 모의 사용자 API를 호출하는 데 사용될 수 있는지 살펴보자. 이 예에서는 또한 비동기/대기 기능을 사용하여 작업을 간소화합니다.

```js
const fetch = require('node-fetch');

(async () => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/users');
    const headerDate = res.headers && res.headers.get('date') ? res.headers.get('date') : 'no response date';
    console.log('Status Code:', res.status);
    console.log('Date in Response header:', headerDate);

    const users = await res.json();
    for(user of users) {
      console.log(`Got user with id: ${user.id}, name: ${user.name}`);
    }
  } catch (err) {
    console.log(err.message); //can be console.error
  }
})();
```

여기서 비동기/대기 기능과 함께 SuperAgent를 사용하는 사례와 비교하여 몇 가지 차이점을 살펴보겠습니다.

- `fetch`는 명시적인 GET 방법이 필요하지 않았다. HTTP 동사는 두 번째 매개 변수인 개체에서 `method` 키로 전송될 수 있다. 예를 들어 `{method: `GET`}`
- 또 다른 차이점은 헤더가 헤더 값을 가져오는 `get` 메서드를 가진 개체라는 점이다. 우리는 res를 불렀다.hesters.get`date`) `날짜 응답 헤더 값을 가져옵니다.
- 최종적인 차이는 `waiteres.json()으로 시신을 JSON으로 받겠다는 약속을 풀 필요가 있다는 점이다. 약간의 추가 작업처럼 보였지만, 브라우저 Fetch API 응답은 그렇게 작동합니다.

위의 모든 예와 마찬가지로, 이 코드는 참조에 대한 풀 요청으로도 액세스할 수 있습니다.

이제 방금 살펴본 네 개의 라이브러리를 비교해 보겠습니다.

## 노드 HTTP 요청 방법의 빠른 비교

HTTP/HTTPS 표준 노드 모듈을 제외하고 Node.js의 다른 4개의 HTTP 클라이언트 라이브러리를 모두 npm 패키지로 사용할 수 있습니다. 다음은 지난 6개월 동안의 다운로드 통계를 npm 동향을 통해 간략히 보여 줍니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/NPM-trends-download-statistics.png?resize=730%2C438&ssl=1)

주간 다운로드에서는 노드-페치가 가장 인기가 높았고, 지난 6개월 동안 슈퍼에이전트가 가장 인기가 없었다. Got GitHubrepo의 비교 표 덕분에 경쟁업체들 사이에서 인기를 더 잘 알 수 있도록 몇 가지 다른 측정 기준을 자세히 살펴보도록 하겠습니다.

위의 표에서 노드-페치가 가장 많이 다운로드된 패키지입니다. SuperAgent의 설치 크기는 1.70MB로 가장 크며, Axios의 GitHub 스타는 80.55K로 가장 많다.

## 결론

저는 꽤 오래 전에 슈퍼에이전트를 광범위하게 사용했습니다. 그 후, 악시오스로 옮겼습니다. 긴 피처리스트로 Got에게 가까운 시일 내에 도전해보고 싶습니다. 노드-페치가 유망해 보이고 설치 크기가 작아 보이지만, 적어도 나에게는 API가 충분히 사용자 친화적인지 잘 모르겠다.

내가 Requestnpm 패키지를 언급하지 않았다는 걸 눈치챌 수 있을 거야. Request는 여전히 매우 인기가 있지만(매주 2,236만 다운로드) 2020년 2월 11일부로 완전히 더 이상 사용되지 않기 때문에 더 이상 사용되지 않는 라이브러리를 사용할 필요가 없다.

이 모든 도서관들은 주로 같은 일을 합니다. 여러분이 어떤 브랜드의 커피를 선호하는지와 마찬가지로, 결국 여러분은 여전히 커피를 마시고 있습니다. 사용 사례에 따라 현명하게 선택하고, 최대한의 이익을 위해 적절한 절충안을 만드십시오.