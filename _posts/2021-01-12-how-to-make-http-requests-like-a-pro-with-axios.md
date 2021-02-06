---
layout: post
title: "Axios를 사용하여 HTTP 요청을 만드는 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/07/axios-http-requests.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/axios-http-requests.png?fit=730%2C487&ssl=1)

편집자 참고: 이 Axios 자습서는 2021년 1월 26일에 마지막으로 업데이트되었습니다.

Axios는 브라우저에서 제공하는 XMLHttpRequest 인터페이스를 기반으로 하는 클라이언트 HTTP API이다.

이 튜토리얼에서는 Axios를 사용하여 HTTP 요청을 만드는 방법(axios.post을 사용하여 Axios POST 요청을 만드는 방법)을 비롯하여 여러 요청을 동시에 보내는 방법 등을 명확한 예와 함께 보여 준다.

다음 사항에 대해 자세히 설명합니다.

- 왜 악시오스를 사용하죠?
- Axios 설치
- Axios POST 요청을 만드는 방법
- Axios HTTP 요청에 대한 단축키 방법
- axios.post은 무엇을 반환하는가?
- `axios`를 사용해서.여러 가지 요청을 보내기 위해 모두
- Axios를 사용하여 사용자 지정 헤더 전송
- Axios가 포함된 POST JSON
- 요청 및 응답 변환
- 요청 및 응답 가로채기
- XSRF에 대한 보호를 위한 클라이언트 측 지원
- POST 요청 진행 모니터링
- 요청 취소 중
- 인기 있는 악시오스 도서관
- 브라우저 지원

시각 학습자라면 아래 비디오 튜토리얼을 참조하십시오.

## 왜 악시오스를 사용하죠?

프런트엔드 프로그램이 서버와 통신하는 가장 일반적인 방법은 HTTP 프로토콜을 사용하는 것입니다. 리소스를 가져오고 HTTP 요청을 만들 수 있는 Fetch API 및 `XMLHtpRequest` 인터페이스에 익숙할 것입니다.

자바스크립트 라이브러리를 사용하는 경우 클라이언트 HTTP API와 함께 제공될 가능성이 높습니다. 예를 들어, jQuery의 `$ajax()` 함수는 프런트엔드 개발자들에게 특히 인기가 높았습니다. 그러나 개발자들이 네이티브 API를 선호하여 이러한 라이브러리에서 벗어나면서, 그 공백을 메우기 위해 전용 HTTP 클라이언트가 등장하였다.

Feetch와 마찬가지로 Axios도 약속 기반이다. 그러나 보다 강력하고 유연한 기능 세트를 제공합니다.

기본 Fetch API에 비해 Axios를 사용하면 다음과 같은 이점이 있습니다.

- 요청 및 응답 가로채기
- 효율적인 오류 처리
- XSRF에 대한 보호
- 업로드 진행률 지원
- 응답 시간 초과
- 요청을 취소할 수 있는 기능
- 이전 브라우저 지원
- 자동 JSON 데이터 변환

## Axios 설치

다음을 사용하여 Axios를 설치할 수 있습니다.

- npm:
$npm 설치 축
- Bower 패키지 관리자:
$bower 설치 축
- 또는 컨텐츠 제공 네트워크:
<script src="https://unpkg.com/axios/dist/axios.min.js"/script

## Axios POST 요청을 만드는 방법

HTTP 요청을 만드는 것은 구성 개체를 Axios 함수에 전달하는 것만큼 쉽습니다. Axios를 사용하여 지정된 엔드포인트에 데이터를 "게시"하고 이벤트를 트리거하는 POST 요청을 할 수 있습니다.

Axios에서 HTTP POST 요청을 수행하려면 `axios.post`으로 문의하십시오.

Axios에서 POST 요청을 만들려면 서비스 끝점의 URI와 서버로 전송하려는 속성이 포함된 개체의 두 가지 매개 변수가 필요합니다.

간단한 Axois POST 요청의 경우 개체에는 `url` 속성이 있어야 합니다. 메서드가 제공되지 않으면 `GET`가 기본값으로 사용됩니다.

간단한 Axios POST 예를 살펴보겠습니다.

```undefined
// send a POST request
axios({
  method: 'post',
  url: '/login',
  data: {
    firstName: 'Finn',
    lastName: 'Williams'
  }
});
```

이는 jQuery의 $ajax 기능으로 작업한 사람들에게 친숙해 보일 것이다. 이 코드는 Axios가 키/값 쌍의 객체를 데이터로 `/로그인`에 POST 요청을 전송하도록 지시하는 것이다. Axios는 자동으로 데이터를 JSON으로 변환하여 요청 본문으로 전송합니다.

## Axios HTTP 요청에 대한 단축키 방법

Axios는 또한 다른 유형의 요청을 수행하기 위한 일련의 짧은 방법을 제공한다. 방법은 다음과 같습니다.

- `hbsos.request(config)`
- ➡os.get(url[, config])`
- ➡os.delete(url[, config])`
- ➡os.head(url[, config])`
- `httpsos.options(url[, config])`
- `axios.post(url[, data[, config]])`
- ➡os.put(url[, data[, config])`
- ➡os.➡(url[, data[, config])`

예를 들어, 다음 코드는 이전 예제가 어떻게 `axios.post`을 사용하여 작성될 수 있는지를 보여준다.

```bash
axios.post('/login', {
  firstName: 'Finn',
  lastName: 'Williams'
});
```

## axios.post은 무엇을 반환하는가?

HTTP POST 요청이 이루어지면 Axios는 백엔드 서비스의 응답에 따라 이행되거나 거부된 약속을 반환합니다.

결과를 처리하기 위해 다음과 같은 `then()` 방법을 사용할 수 있습니다.

```coffeescript
axios.post('/login', {
  firstName: 'Finn',
  lastName: 'Williams'
})
.then((response) => {
  console.log(response);
}, (error) => {
  console.log(error);
});
```

약속이 이행되면 그 때()의 첫 번째 주장이 불리고, 약속이 거부되면 두 번째 주장이 불리게 된다. 설명서에 따르면 이행 값은 다음 정보를 포함하는 개체입니다.

```undefined
{
  // `data` is the response that was provided by the server
  data: {},
 
  // `status` is the HTTP status code from the server response
  status: 200,
 
  // `statusText` is the HTTP status message from the server response
  statusText: 'OK',
 
  // `headers` the headers that the server responded with
  // All header names are lower cased
  headers: {},
 
  // `config` is the config that was provided to `axios` for the request
  config: {},
 
  // `request` is the request that generated this response
  // It is the last ClientRequest instance in node.js (in redirects)
  // and an XMLHttpRequest instance the browser
  request: {}
}
```

예를 들어 GitHub API에서 데이터를 요청할 때 다음과 같이 응답합니다.

```coffeescript
axios.get('https://api.github.com/users/mapbox')
  .then((response) => {
    console.log(response.data);
    console.log(response.status);
    console.log(response.statusText);
    console.log(response.headers);
    console.log(response.config);
  });

// logs:
// => {login: "mapbox", id: 600935, node_id: "MDEyOk9yZ2FuaXphdGlvbjYwMDkzNQ==", avatar_url: "https://avatars1.githubusercontent.com/u/600935?v=4", gravatar_id: "", …}
// => 200
// => OK
// => {x-ratelimit-limit: "60", x-github-media-type: "github.v3", x-ratelimit-remaining: "60", last-modified: "Wed, 01 Aug 2018 02:50:03 GMT", etag: "W/"3062389570cc468e0b474db27046e8c9"", …}
// => {adapter: ƒ, transformRequest: {…}, transformResponse: {…}, timeout: 0, xsrfCookieName: "XSRF-TOKEN", …}
```

## 'axios'를 사용해서.여러 가지 요청을 보내기 위해 모두

Axios의 더 흥미로운 기능 중 하나는 일련의 인수를 `axios.all() 방법`에 전달하여 병렬로 여러 요청을 할 수 있는 기능이다. 이 메서드는 배열로 전달된 모든 인수가 해결된 경우에만 해결되는 단일 약속 개체를 반환합니다.

여기 `axios`를 어떻게 사용하는 간단한 예가 있습니다.all`을(를) 사용하여 동시 HTTP 요청을 할 수 있습니다.

```js
// execute simultaneous requests 
axios.all([
  axios.get('https://api.github.com/users/mapbox'),
  axios.get('https://api.github.com/users/phantomjs')
])
.then(responseArr => {
  //this will be executed only when all requests are complete
  console.log('Date created: ', responseArr[0].data.created_at);
  console.log('Date created: ', responseArr[1].data.created_at);
});

// logs:
// => Date created:  2011-02-04T19:02:13Z
// => Date created:  2017-04-03T17:25:46Z
```

이 코드는 GitHub API에 두 번 요청을 한 다음 각 응답의 `created_at` 속성의 값을 콘솔에 기록합니다. 만약 어떤 주장이든 거절한다면, 그 약속은 거절하는 첫 번째 약속의 이유와 함께 즉시 거절할 것이라는 것을 명심하세요.

Axios는 편의를 위해 `axios.spread()`라는 메소드를 제공하여 반응 배열의 속성을 별도의 변수에 할당합니다. 이 방법을 사용하는 방법은 다음과 같습니다.

```js
axios.all([
  axios.get('https://api.github.com/users/mapbox'),
  axios.get('https://api.github.com/users/phantomjs')
])
.then(axios.spread((user1, user2) => {
  console.log('Date created: ', user1.data.created_at);
  console.log('Date created: ', user2.data.created_at);
}));

// logs:
// => Date created:  2011-02-04T19:02:13Z
// => Date created:  2017-04-03T17:25:46Z
```

이 코드의 출력은 이전 예제와 동일합니다. 유일한 차이점은 `axios.spread()` 메서드가 응답 어레이의 값을 언팩하는 데 사용된다는 것입니다.

## Axios를 사용하여 사용자 지정 헤더 전송

Axios와 함께 사용자 지정 헤더를 보내는 것은 매우 간단합니다. 머리글이 포함된 개체를 마지막 인수로 전달하기만 하면 됩니다. 예를 들어:

```undefined
const options = {
  headers: {'X-Custom-Header': 'value'}
};

axios.post('/save', { a: 10 }, options);
```

## Axios가 포함된 POST JSON

Axios는 두 번째 매개 변수로 `axios.post` 함수에 전달되면 JavaScript 객체를 JSON으로 자동 직렬화한다. 따라서 POST 본문을 JSON에 직렬화할 필요가 없습니다.

Axios는 또한 `콘텐츠 타입` 헤더를 `애플리케이션/json`으로 설정한다. 이렇게 하면 웹 프레임워크가 데이터를 자동으로 구문 분석할 수 있습니다.

사전 직렬화된 JSON 문자열을 JSON으로 `axios.post`에 보내려면 `콘텐츠 유형` 헤더가 설정되어 있는지 확인해야 합니다.

## 요청 및 응답 변환

Axios는 기본적으로 요청과 응답을 자동으로 JSON으로 변환하지만 기본 동작을 재정의하고 다른 변환 메커니즘을 정의할 수도 있습니다. 이 기능은 XML 또는 CSV와 같은 특정 데이터 형식만 허용하는 API를 사용할 때 특히 유용합니다.

서버로 보내기 전에 요청 데이터를 변경하려면 구성 개체에서 `transformRequest` 속성을 설정하십시오. 이 방법은 PUT, POST, Patch 요청 방법에만 적용됩니다.

Axios에서 `transformRequest`를 사용하는 예는 다음과 같습니다.

```js
const options = {
  method: 'post',
  url: '/login',
  data: {
    firstName: 'Finn',
    lastName: 'Williams'
  },
  transformRequest: [(data, headers) => {
    // transform the data

    return data;
  }]
};

// send the request
axios(options);
```

데이터를 "then() 또는 "catch()"로 전달하기 전에 수정하려면 "transformResponse" 속성을 설정할 수 있습니다.

```js
const options = {
  method: 'post',
  url: '/login',
  data: {
    firstName: 'Finn',
    lastName: 'Williams'
  },
  transformResponse: [(data) => {
    // transform the response

    return data;
  }]
};

// send the request
axios(options);
```

## 요청 및 응답 가로채기

HTTP 가로채기는 Axios의 인기 있는 기능입니다. 이 기능을 사용하면 프로그램에서 서버로 또는 그 반대로 HTTP 요청을 검사하고 변경할 수 있으며, 이는 로깅 및 인증과 같은 다양한 암시적 태스크에 매우 유용합니다.

얼핏 보면, 인터셉터는 변환과 매우 흡사하지만, 한 가지 중요한 방식으로는 다르다. 즉, 데이터와 헤더만 인수로 수신하는 변환과는 달리, 인터셉터는 전체 응답 객체 또는 요청 구성을 수신한다.

Axios에서 다음과 같이 요청 인터셉터를 선언할 수 있습니다.

```js
// declare a request interceptor
axios.interceptors.request.use(config => {
  // perform a task before the request is sent
  console.log('Request was sent');

  return config;
}, error => {
  // handle the error
  return Promise.reject(error);
});

// sent a GET request
axios.get('https://api.github.com/users/mapbox')
  .then(response => {
    console.log(response.data.created_at);
  });
```

이 코드는 요청을 보낼 때마다 콘솔에 메시지를 기록한 다음 서버에서 응답을 받을 때까지 기다렸다가 GitHub에서 계정이 생성된 시간을 콘솔에 인쇄합니다. 인터셉터를 사용하면 더 이상 각 HTTP 요청에 대한 태스크를 별도로 구현할 필요가 없습니다.

Axios는 또한 응답 인터셉터를 제공하므로 응용프로그램으로 돌아가는 서버의 응답을 변환할 수 있습니다.

```js
// declare a response interceptor
axios.interceptors.response.use((response) => {
  // do something with the response data
  console.log('Response was received');

  return response;
}, error => {
  // handle the response error
  return Promise.reject(error);
});

// sent a GET request
axios.get('https://api.github.com/users/mapbox')
  .then(response => {
    console.log(response.data.created_at);
  });
```

## XSRF에 대한 보호를 위한 클라이언트 측 지원

사이트 간 요청 위조(XSRF)는 공격자가 합법적이고 신뢰할 수 있는 사용자로 위장하여 앱과 사용자 브라우저 간의 상호 작용에 영향을 미치는 웹 호스팅된 앱을 공격하는 방법이다. XMLHttpRequest 등 여러 가지 방법으로 공격을 실행할 수 있다.

다행히 Axios는 요청을 할 때 추가 인증 데이터를 포함할 수 있도록 하여 XSRF로부터 보호하도록 설계되었습니다. 이렇게 하면 서버가 인증되지 않은 위치에서 요청을 검색할 수 있습니다. Axios를 사용하여 이 작업을 수행할 수 있는 방법은 다음과 같습니다.

```undefined
const options = {
  method: 'post',
  url: '/login',
  xsrfCookieName: 'XSRF-TOKEN',
  xsrfHeaderName: 'X-XSRF-TOKEN',
};

// send the request
axios(options);
```

## POST 요청 진행 모니터링

Axios의 또 다른 흥미로운 기능은 요청 진행 상황을 모니터링하는 기능입니다. 특히 대용량 파일을 다운로드하거나 업로드할 때 유용합니다. Axios 설명서에 제공된 예제를 통해 이러한 작업을 수행할 수 있는 방법에 대해 잘 알 수 있습니다. 하지만 우리는 단순함과 스타일을 위해 이 튜토리얼에서 Axios Progress Bar 모듈을 사용할 것입니다.

이 모듈을 사용하려면 먼저 관련 스타일 및 스크립트를 포함해야 합니다.

```js
<link rel="stylesheet" type="text/css" href="https://cdn.rawgit.com/rikmms/progress-bar-4-axios/0a3acf92/dist/nprogress.css" />

<script src="https://cdn.rawgit.com/rikmms/progress-bar-4-axios/0a3acf92/dist/index.js"></script>
```

그러면 다음과 같은 진행률 표시줄을 구현할 수 있습니다.

```js
loadProgressBar()

const url = 'https://media.giphy.com/media/C6JQPEUsZUyVq/giphy.gif';

function downloadFile(url) {
  axios.get(url)
  .then(response => {
    console.log(response)
  })
  .catch(error => {
    console.log(error)
  })
}

downloadFile(url);
```

진행률 표시줄의 기본 스타일을 변경하려면 다음 스타일 규칙을 재정의할 수 있습니다.

```css
#nprogress .bar {
    background: red !important;
}

#nprogress .peg {
    box-shadow: 0 0 10px red, 0 0 5px red !important;
}

#nprogress .spinner-icon {
    border-top-color: red !important;
    border-left-color: red !important;
}
```

## 요청 취소 중

경우에 따라서는 결과에 더 이상 신경 쓰지 않고 이미 전송된 요청을 취소하고자 할 수 있습니다. 이 작업은 취소 토큰을 사용하여 수행할 수 있습니다. 요청 취소 기능은 버전 1.5에서 Axios에 추가되었으며 취소 가능한 약속 제안을 기반으로 합니다. 다음은 간단한 예입니다.

```js
const source = axios.CancelToken.source();

axios.get('https://media.giphy.com/media/C6JQPEUsZUyVq/giphy.gif', {
  cancelToken: source.token
}).catch(thrown => {
  if (axios.isCancel(thrown)) {
    console.log(thrown.message);
  } else {
    // handle error
  }
});

// cancel the request (the message parameter is optional)
source.cancel('Request canceled.');
```

아래와 같이 실행자 기능을 `CancelToken` 생성자에게 전달하여 취소 토큰을 생성할 수도 있습니다.

```js
const CancelToken = axios.CancelToken;
let cancel;

axios.get('https://media.giphy.com/media/C6JQPEUsZUyVq/giphy.gif', {
  // specify a cancel token
  cancelToken: new CancelToken(c => {
    // this function will receive a cancel function as a parameter
    cancel = c;
  })
}).catch(thrown => {
  if (axios.isCancel(thrown)) {
    console.log(thrown.message);
  } else {
    // handle error
  }
});

// cancel the request
cancel('Request canceled.');
```

## 인기 있는 악시오스 도서관

악시오스의 개발자들 사이의 인기 상승은 그것의 기능을 확장하는 제3자 도서관의 풍부한 선택으로 이어졌다. 테스터에서 로거에 이르기까지 Axios를 사용할 때 필요한 거의 모든 추가 기능을 위한 라이브러리가 있습니다. 다음은 현재 사용 가능한 인기 있는 라이브러리입니다.

- axios-contract: 📼 JavaScript에서 요청 기록 및 재생
- axios-response-contract: 응답을 기록하는 📣 Axios 인터셉터
- axios-filengers-filgers: ⛵ips Axios 요청 메서드 재정의 플러그인
- axios-확장: 스로틀 및 캐시 GET 요청 기능을 포함한 🍱 Axios 확장 lib
- axios-api-versioning: Axios에 관리하기 쉬운 API 버전 관리 추가
- axios-cache-cash: Axios를 사용할 때 GET 요청을 캐시할 수 있도록 지원
- axios-supper jar-support: Axios에 강력한 쿠키 지원 추가
- 반응성-변환성: Axios용 사용자 정의 반응 후크
- Moxios: 모의 Axios 테스트 요청
- 리덕스-코너: Redux-Saga 추가 기능을 통해 AJAX 요청 처리 간소화
- axios-fetch: Axios 클라이언트가 지원하는 웹 API Fetch 구현
- 축음기: Axios 요청을 콘솔에서 curl 명령으로 기록
- axios-actions: 엔드포인트를 호출 가능한 재사용 가능한 서비스로 번들
- Mocha-axios: Axios를 사용하는 Mocha에 대한 HTTP 주장
- axios-contrace-contrace: 요청을 쉽게 모의실험할 수 있는 Axios 어댑터
- axios-debug-log: 디버그 라이브러리를 사용한 로깅 요청 및 응답의 Axios 인터셉터
- Redux-axios-middleware: Axios HTTP 클라이언트로 데이터를 가져오는 Redux 미들웨어
- 축음기: Axios 기반 수퍼 테스트: nNode.js 요청 처리기를 Axios 어댑터로 변환하여 Node.js 서버 유닛 테스트에 사용

## 브라우저 지원

브라우저 지원에 관한 한 Axios는 매우 신뢰할 수 있다. IE 11과 같은 오래된 브라우저도 Axios와 잘 호환된다.

## 마무리하기

악시오스가 개발자들 사이에서 인기가 많은 데는 그만한 이유가 있다: 그것은 유용한 기능들로 가득 차 있다. 이 게시물에서는 Axios의 몇 가지 주요 기능을 자세히 살펴보고 이를 실제로 사용하는 방법을 배웠습니다. 하지만 악시오에는 아직 우리가 논의하지 않은 많은 측면이 있다. 자세한 내용은 Axios GitHub 페이지를 참조하십시오.

Axios 사용에 대한 몇 가지 팁이 있습니까? 댓글로 알려주세요.