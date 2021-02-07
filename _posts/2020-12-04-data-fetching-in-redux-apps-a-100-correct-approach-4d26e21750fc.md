---
layout: post
title: "데이터 가져오기에 Redux 앱 및 Axios 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2018/08/react-redux-data-fetching.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2018/08/react-redux-data-fetching.png?fit=730%2C487&ssl=1)

많은 개발자가 알고 있듯이, 상태 관리는 강력한 애플리케이션을 구축하는 동안 해결해야 하는 많은 문제 중 하나가 될 수 있습니다. 그것은 특히 고객측에서 악몽으로 빠르게 성장할 수 있다.

Redux가 시행하는 데이터의 단방향 흐름을 통해 이벤트가 애플리케이션의 상태를 어떻게 변화시키는지 쉽게 이해할 수 있습니다. 좋아요! 하지만 가장 흔한 부작용인 네트워크 요청과 같은 부작용을 다루는 건 어떨까요?

## Redux란 무엇인가?

Redux는 상태 컨테이너이며 UI 프레임워크의 주요 문제 중 하나인 상태 관리를 해결하는 훌륭한 도구입니다. 하지만 그 자체로는, 즉석에서 해결책을 제시하지 못합니다. 다행히 지역사회는 문제를 해결하기 위해 많은 수의 도서관을 유지하고 있다.

자, 이 중 어느 것이 당신의 프로젝트에 적합한가?

진실은 이 솔루션들이 각기 다른 접근법, 활용 사례, 정신 모델을 염두에 두고 구축되었기 때문에 모두 장단점을 가지고 있다는 것이다. 이 블로그에서는 가능한 모든 접근 방식에 대해 논의하지는 않겠지만, 가장 일반적인 패턴 중 몇 가지를 간단한 응용 프로그램으로 살펴보겠습니다.

## 중간 게시물 사용 예제

위의 응용 프로그램 스크린샷을 보십시오. 그것은 거의 틀림없이 매우 간단하다. 왼쪽에는 여러 개의 텍스트와 중간 박수 아이콘이 있습니다. 이 앱은 GitHubrepo를 잡을 수 있습니다.

Medium clap은 클릭할 수 있습니다. 미디엄 클랩 클론을 만든 이유는 다음과 같습니다.

이 간단한 응용 프로그램에서도 서버에서 데이터를 가져와야 합니다. 필요한 보기를 표시하는 데 필요한 JSON 페이로드의 모양은 다음과 같을 수 있습니다.

```undefined
{
  "numberOfRecommends": 1900,
  "title": "My First Fake Medium Post",
  "subtitle": "and why it makes no intelligible sense",
  "paragraphs": [
    {
      "text": "This is supposed to be an intelligible post about something intelligible."
    },
    {
      "text": "Uh, sorry there’s nothing here."
    },
    {
      "text": "It’s just a fake post."
    },
    {
      "text": "Love it?"
    },
    {
      "text": "I bet you do!"
    }
  ]
}
```

앱의 구조는 정말 간단하다. `조항`과 `박수`라는 두 가지 주요 구성 요소가 있다.

components/Article.js에서 아티클 구성 요소는 제목, 자막, 문단 소품을 포함하는 상태 비저장 기능 구성 요소입니다. 렌더링된 구성 요소는 다음과 같습니다.

```js
const Article = ({ title, subtitle, paragraphs }) => {
  return (
    <StyledArticle>
      <h1>{title}</h1>
      <h4>{subtitle}</h4>
      {paragraphs.map(paragraph => <p>{paragraph.text}</p>)}
    </StyledArticle>
  );
};
```

여기서 `Styled 아티클`은 CSS-in-JS 솔루션 `Styled-components`를 통해 스타일링되는 일반 div 요소이다.

당신이 어떤 CSS-in-JS 솔루션에 익숙한지는 중요하지 않다. 스타일 아티클은 good old CSS로 대체될 수 있다.

논쟁을 시작하지 말고 그것을 끝내자. 😂

Medium clap 구성 요소는 `components/Clap.js` 내에서 내보내집니다. 그 코드는 조금 더 관여되어 있으며 이 기사의 범위를 벗어난다. 하지만, 여러분은 제가 어떻게 미디엄 클랩을 만들었는지 읽어보실 수 있습니다. 5분짜리 읽기입니다.

Clap 구성 요소와 아티클 구성 요소가 모두 설치된 상태에서 App 구성 요소는 containers/App.js에서 볼 수 있듯이 두 구성 요소를 모두 구성합니다.

```js
class App extends Component {
  state = {};
  render() {
    return (
      <StyledApp>
        <aside>
          <Clap />
        </aside>
        <main>
          <Article />
        </main>
      </StyledApp>
    );
  }
}
```

다시, 당신은 StyledApp을 일반 div로 대체하고 CSS를 통해 스타일링을 할 수 있다.

자, 이제 이 기사의 핵심을 살펴봅시다.

## 데이터 가져오기를 위한 다양한 대체 솔루션

이제 Redux 앱에서 데이터를 가져오도록 선택할 수 있는 몇 가지 방법과 장단점을 살펴보겠습니다.

가장 인기 있는 옵션은 단연 `dux-thunk`와 `dux-saga`다.

준비됐나요

### 칙칙한 칙칙한 칙칙한 칙칙한 칙칙한 칙칙한 칙칙한 칙칙함

기억해야 할 가장 중요한 사항 중 하나는 모든 타사 라이브러리에 학습 곡선과 잠재적인 확장성 문제가 있다는 것입니다.

레드삭스의 부작용 관리를 위한 모든 커뮤니티 라이브러리들 중에서, `드렉스 씽크`와 `드렉스 프러미`와 같은 작업을 하는 라이브러리가 가장 쉽게 시작할 수 있다.

전제는 간단하다.

### 중복 제거 및 중복 약속으로 데이터 가져오기

dedux-thunk의 경우 객체를 "생성"하지 않고 함수를 반환하는 작업 생성자를 작성합니다. 이 함수는 Redux에서 getState와 디스패치 함수를 통과한다.

가짜 미디어 앱이 어떻게 `dux-thunk` 라이브러리를 활용할 수 있는지 살펴보자.

먼저 `redux-thunk` 라이브러리를 설치합니다.

```undefined
yarn add redux-thunk
```

도서관이 예상대로 작동하려면 미들웨어로 적용해야 한다.

`store/index.js`의 경우:

```js
import { createStore, applyMiddleware } from "redux";
import thunk from "redux-thunk";
const store = createStore(rootReducer, applyMiddleware(thunk));
```

위의 코드 블록의 첫 번째 줄은 redux에서 CreateStore와 ApplyMiddleware 기능을 가져옵니다.

2호선은 씽크(dedex-thunk)에서 씽크(thunk)를 수입한다.

3호선은 `스토어`를 만들지만 미들웨어가 적용됐다.

이제 실제 네트워크 요청을 할 준비가 되었습니다.

네트워크 요청을 위해 `axios` 라이브러리를 사용하겠지만 원하는 HTTP 클라이언트로 바꾸십시오.

중복 씽크를 사용하면 네트워크 요청을 쉽게 시작할 수 있습니다. 다음과 같은 작업 작성자를 작성합니다(즉, 함수를 반환하는 작업 작성자).

```js
export function fetchArticleDetails() {
  return function(dispatch) {
    return axios.get("https://api.myjson.com/bins/19dtxc")
      .then(({ data }) => {
      dispatch(setArticleDetails(data));
    });
  };
}
```

`App` 구성 요소를 마운트하면 다음 작업 생성자를 발송합니다.

```coffeescript
componentDidMount() {
    this.props.fetchArticleDetails();
 }
```

그리고 그게 다야. 여기서 키 라인만 강조 표시하므로 전체 코드 차이를 확인하십시오.

이를 통해 기사의 세부 정보를 가져와 앱에 표시했습니다.

이 접근법이 정확히 무엇이 잘못되었는가?

만약 여러분이 아주 작은 어플리케이션을 만들고 있다면, `redux-thunk`는 문제를 해결해주며, 아마도 그것은 어울리기 가장 쉬울 것이다.

하지만, 사용 편의성은 비용이 듭니다. 세 가지 단점을 생각해 봅시다.

#### 1. 모든 작업 작성자는 오류를 처리하고 헤더를 설정하는 반복적인 기능을 가집니다.

다음은 앞에서 작성한 액션 크리에이터입니다.

```js
export function fetchArticleDetails() {
   return function(dispatch) {
     return axios.get("https://api.myjson.com/bins/19dtxc")
      .then(({ data }) => {
         dispatch(setArticleDetails(data));
     });
   };
}
```

대부분의 응용 프로그램에서는 GET, POST 등의 여러 가지 방법으로 여러 가지 요청을 수행해야 합니다.

추천 아티클이라는 다른 액션 크리에이터가 있다고 가정합니다. 이렇게 보일 수도 있습니다.

```php
export function recommendArticle (id,  amountOfRecommends) {
  return  function (dispatch) {
    return axios.post("https://api.myjson.com/bins/19dtxc, {
       id,
       amountOfRecommends
  });
})
```

그리고 사용자 프로필을 가져오려면요?

```js
export function fetchUserProfile() {
    return function(dispatch) {
      return axios.get("https://api.myjson.com/bins/19dtxc")
       .then(({ data }) => {
          dispatch(setUserProfile(data));
        });
    };
 }
```

반복되는 기능이 많은 것을 보는 데는 오래 걸리지 않습니다. 오류를 가져오려면 모든 작업 작성자에 `캐치` 블록을 추가해야 합니다.

#### 2. 비동기 생성자가 많아지면 테스트도 어려워집니다.

비동기식은 일반적으로 테스트하기가 더 어렵습니다. 테스트하는 것이 불가능하거나 어려운 것이 아니라, 테스트하는 것을 상당히 어렵게 만들 뿐입니다.

작업 생성자를 가능한 상태 비저장 상태로 유지하고 간단한 기능을 사용하면 디버그 및 테스트하기가 쉬워집니다.

응용프로그램에 포함시키는 작업 작성자가 많아질수록, 테스트는 더 어려워집니다.

#### 3. 서버 통신 전략 변경은 더욱 어려워집니다.

만약 새로운 개발자가 와서 팀이 xaxios에서 다른 http 클라이언트로 옮겨야 한다고 결정한다면 어떻게 될까? 이제 다른 (다중) 액션 크리에이터를 바꾸셔야 합니다.

그렇게 쉽진 않지, 그렇지?

### 중복 사가 및 중복 관측 가능 사용

이런 것들은 중복이나 중복약속보다 약간 더 복잡하다.

중복과 중복은 확실히 배율이 높지만 학습 곡선이 필요하다. sagas와 RxJS와 같은 개념은 학습되어야 하며, 팀에서 일하는 엔지니어들이 얼마나 많은 경험을 가지고 있는지에 따라 이것은 어려운 일일 수 있다.

따라서 중복 씽크(dux-thunk)와 중복 약속(dux-promise)이 프로젝트에 너무 단순하고, 중복 사가(dux-saga)와 중복 관측(dux-observable)이 팀으로부터 추상화하고자 하는 복잡성의 층을 도입한다면, 당신은 어디로 눈을 돌립니까?

사용자 정의 미들웨어!

중복 씽크, 중복약속, 중복사가 등 대부분의 솔루션은 미들웨어를 후드 아래 사용한다. 만들 수 없는 이유가 무엇입니까?

방금 "왜 바퀴를 다시 만들어요?"라고 말씀하셨나요?

#### 이게 완벽한 해결책인가요?

바퀴를 다시 만드는 것은 완전히 나쁜 것처럼 들리지만, 기회를 주세요.

많은 기업이 이미 필요에 맞게 맞춤형 솔루션을 구축하고 있습니다. 사실, 많은 오픈 소스 프로젝트들이 그렇게 시작되었습니다.

그렇다면 이 맞춤형 솔루션에서 기대하는 것은 무엇일까요?

- 중앙 집중식 솔루션, 즉 하나의 모듈.
- GET, POST, DELETE, PUT 등 다양한 HTTP 처리 가능
- 사용자 지정 헤더 설정을 처리할 수 있음
- 일부 외부 로깅 서비스로 전송하거나 권한 부여 오류를 처리하는 등의 사용자 정의 오류 처리를 지원합니다.
- onSuccessful 및 onFailure 콜백 허용
- 로드 상태를 처리하기 위한 레이블 지원

다시 말해, 특정 요구에 따라 더 큰 목록이 있을 수 있습니다.

이제, 제가 당신을 적절한 출발점으로 안내하겠습니다. 특정 사용 사례에 맞게 조정할 수 있습니다.

Redux 미들웨어는 항상 다음과 같이 시작합니다.

```js
const apiMiddleware = ({dispatch}) => next => action => {
  next (action)
}
```

사용자 지정 API 미들웨어에 대한 본격적인 코드는 다음과 같습니다. 처음에는 많아 보일 수 있지만, 제가 짧은 시간 안에 모든 대목을 설명해 드리겠습니다.

여기 있습니다:

```js
import axios from "axios";
import { API } from "../actions/types";
import { accessDenied, apiError, apiStart, apiEnd } from "../actions/api";


const apiMiddleware = ({ dispatch }) => next => action => {
  next(action);

  if (action.type !== API) return;

  const {
    url,
    method,
    data,
    accessToken,
    onSuccess,
    onFailure,
    label,
    headers
  } = action.payload;
  
  const dataOrParams = ["GET", "DELETE"].includes(method) ? "params" : "data";
  
  
  // axios default configs
  axios.defaults.baseURL = process.env.REACT_APP_BASE_URL || "";
  axios.defaults.headers.common["Content-Type"]="application/json";
  axios.defaults.headers.common["Authorization"] = `Bearer${token}`;


  if (label) {
    dispatch(apiStart(label));
  }

  axios
    .request({
      url,
      method,
      headers,
      [dataOrParams]: data
    })
    .then(({ data }) => {
      dispatch(onSuccess(data));
    })
    .catch(error => {
      dispatch(apiError(error));
      dispatch(onFailure(error));

      if (error.response && error.response.status === 403) {
        dispatch(accessDenied(window.location.pathname));
      }
    })
   .finally(() => {
      if (label) {
        dispatch(apiEnd(label));
      }
   });
};

export default apiMiddleware;
```

깃허브에서 손에 넣을 수 있는 100줄에 불과한 코드로 추리하기 쉬운 흐름의 맞춤형 솔루션을 갖췄다.

각 행에 대해 설명하기로 약속했으므로 먼저 미들웨어의 작동 방식을 간략히 살펴보겠습니다.

우선, 몇 가지 중요한 수입품을 만들고, 그 사용법을 곧 알게 될 것입니다.

#### 1. 미들웨어 설정

중복 미들웨어에 필요한 일반적인 설정입니다. 예:

```js
const apiMiddleware = ({ dispatch }) => next => action => {}
```

#### 2. 관련 없는 행동 유형 해제

```bash
if (action.type !== API) return;
```

위의 조건은 `API` 유형의 작업을 제외한 모든 작업이 네트워크 요청을 트리거하지 못하도록 하는 데 중요합니다.

#### 3. 작업 페이로드에서 중요 변수 추출

```cpp
const {
    url,
    method,
    data,
    onSuccess,
    onFailure,
    label,
  } = action.payload;
```

성공적인 요청을 하기 위해서는 작업 페이로드에서 다음을 추출해야 합니다.

url은 히트할 끝점을, method는 요청의 HTTP 방식을, data는 서버나 쿼리 매개 변수(GET 또는 DELETE 요청의 경우), onSuccessful, onFailure는 성공 또는 실패 시 디스패치할 작업 생성자를 나타내며 레이블을 나타냅니다. 요청의 문자열 표시입니다.

실제 예제에서 사용되는 이러한 기능을 잠시 후에 확인할 수 있습니다.

#### 4. HTTP 방법 처리

```cpp
const dataOrParams = ["GET", "DELETE"].includes(method) ? "params" : "data";
```

이 솔루션은 axios를 사용하기 때문에 대부분의 HTTP 클라이언트는 이렇게 동작하기 때문에 GET와 DELETE 방법은 params를 사용하는 반면 다른 방법은 데이터를 서버로 전송해야 할 수 있다.

따라서 data OrParams 변수에는 요청 방법에 따라 params 또는 data 값 중 하나가 포함됩니다.

웹에서 개발한 경험이 있다면 이상하지 않아야 합니다.

#### 5. 글로벌 처리

```js
// axios default configs
  axios.defaults.baseURL = process.env.REACT_APP_BASE_URL || "";
  axios.defaults.headers.common["Content-Type"]="application/json";
  axios.defaults.headers.common["Authorization"] = `Bearer${token}`;
```

대부분의 정상 응용 프로그램에는 일부 권한 부여 계층, `baseUrl` 및 일부 기본 헤더가 있습니다. 기술적으로 모든 API 클라이언트는 모든 요청에 대해 일부 기본값을 사용할 가능성이 높습니다.

Axios 개체의 일부 속성을 설정하여 이 작업을 수행합니다. 당신이 선택한 어떤 고객도 같은 일을 할 수 있을지는 의문입니다.

#### 6. 적재 상태 처리

```bash
if (label) {
    dispatch(apiStart(label));
}
```

레이블은 특정 네트워크 요청 작업을 식별하기 위한 문자열입니다. 마치 액션의 타입처럼.

레이블이 있으면 미들웨어는 apiStart 작업 생성자를 발송합니다.

apiStart 액션 크리에이터는 다음과 같습니다.

```js
export const apiStart = label => ({
  type: API_START,
  payload: label
});
```

작업 유형은 `API_START`입니다.

이제 감산기 내에서 이 작업 유형을 처리하여 요청이 언제 시작되는지 알 수 있습니다. 잠시 후에 예를 보여드리겠습니다.

또한 네트워크 요청에 성공하거나 실패한 경우 `API_END` 작업도 전송됩니다. 요청이 언제 시작되고 언제 종료되는지 정확히 알 수 있으므로 로드 상태를 처리하는 데 적합합니다.

잠시 후에 예를 보여드리겠습니다.

#### 7. 실제 네트워크 요청, 오류 처리 및 콜백 호출

```coffeescript
axios
    .request({
      url: `${BASE_URL}${url}`,
      method,
      headers,
      [dataOrParams]: data
    })
    .then(({ data }) => {
      dispatch(onSuccess(data));
    })
    .catch(error => {
      dispatch(apiError(error));
      dispatch(onFailure(error));
      
     if (error.response && error.response.status === 403) {
        dispatch(accessDenied(window.location.pathname));
      }
    })
    .finally(() => { if (label) { dispatch(apiEnd(label)); } });
```

그것은 보기만큼 복잡하지 않다.

operos.request는 객체 구성이 전달된 상태에서 네트워크 요청을 하는 역할을 합니다. 이러한 변수는 앞서 작업 페이로드에서 추출한 변수입니다.

당시 블록에서 보듯 요청이 성공하면 apiEnd 액션 크리에이터를 파견한다.

다음과 같습니다.

```js
export const apiEnd = label => ({
  type: API_END,
  payload: label
});
```

감산기에서 이 소리를 듣고 요청이 종료된 대로 로드 상태를 제거할 수 있습니다.

그런 다음 "onSuccess" 콜백을 보냅니다.

`onSuccessful` 콜백은 네트워크 요청이 성공한 후 배포하려는 모든 작업을 반환합니다. 가져온 데이터를 Redux 저장소에 저장하는 등 네트워크 요청이 성공한 후 작업을 디스패치하는 경우가 거의 대부분 있습니다.

`catch` 블록에 표시된 것처럼 오류가 발생하면 `apiEnd` 작업 생성자도 실행 중지하고 실패한 오류가 있는 `apiError` 작업 생성자를 발송합니다.

```js
export const apiError = error => ({
  type: API_ERROR,
  error
});
```

이 작업 유형을 수신하고 외부 로깅 서비스에 오류가 발생하는지 확인하는 다른 미들웨어가 있을 수 있습니다.

오류 시 콜백도 발송합니다. 사용자에게 시각적 피드백을 보여줘야 할 경우에 대비해서입니다. 토스트 알림에도 사용할 수 있습니다.

마지막으로 인증 오류를 처리하는 예를 보여드렸습니다.

```coffeescript
if (error.response && error.response.status === 403) {
     dispatch(accessDenied(window.location.pathname));
  }
```

이 예에서는 사용자가 있었던 위치를 나타내는 `액세스 거부` 액션 생성자를 보냅니다.

그러면 나는 다른 미들웨어에서 이 `액세스 거부` 조치를 처리할 수 있습니다.

이것들을 다른 미들웨어에서 다룰 필요는 없습니다. 이러한 작업은 동일한 코드 블록 내에서 수행할 수 있지만 신중하게 추상화하기 위해 이러한 문제를 분리하는 것이 프로젝트에 더 적합할 수 있습니다.

바로 그거야!

### 실행 중인 사용자 지정 미들웨어

이제 이 맞춤형 미들웨어를 사용하기 위해 가짜 미디어 애플리케이션을 리팩터링하겠습니다. 변경 사항은 다음 미들웨어를 포함시키는 것입니다.

```js
import apiMiddleware from "../middleware/api";
const store = createStore(rootReducer, applyMiddleware(apiMiddleware));
```

그런 다음 아티클 세부 정보 가져오기 액션을 편집하여 일반 개체를 반환합니다.

```js
export function fetchArticleDetails() {
  return {
    type: API,
    payload: {
      url: "https://api.myjson.com/bins/19dtxc",
      method: "GET",
      data: null,
      onSuccess: setArticleDetails,
      onFailure: () => {
        console.log("Error occured loading articles");
      },
      label: FETCH_ARTICLE_DETAILS
    }
 };
}

function setArticleDetails(data) {
  return {
    type: SET_ARTICLE_DETAILS,
    payload: data
  };
}
```

`FetchItectDetails`의 페이로드에 미들웨어에 필요한 모든 정보가 어떻게 포함되어 있는지 주목하십시오.

그러나 작은 문제점이 있습니다.

작업 생성자를 한 번 넘어서면 한 번에 페이로드 개체를 쓰는 것이 번거로워집니다. 특히 일부 값이 `null`이거나 일부 기본값을 사용하는 경우.

쉽게 작업 개체의 생성을 `apiAction`이라는 새 작업 생성자로 추상화할 수 있습니다.

```coffeescript
function apiAction({
  url = "",
  method = "GET",
  data = null,
  onSuccess = () => {},
  onFailure = () => {},
  label = ""
}) {
  return {
    type: API,
    payload: {
      url,
      method,
      data,
      onSuccess,
      onFailure,
      label
    }
  };
}
```

ES6 기본 매개 변수를 사용하여 `apiAction`에 이미 설정된 일부 기본값이 있는 방법을 확인하십시오.

이제 `문서 세부 정보 가져오기`에서 다음을 수행할 수 있습니다.

```js
function fetchArticleDetails() {
  return apiAction({
    url: "https://api.myjson.com/bins/19dtxc",
    onSuccess: setArticleDetails,
    onFailure:() => {console.log("Error occured loading articles")},
    label: FETCH_ARTICLE_DETAILS
  });
}
```

일부 ES6에서는 이러한 작업이 더욱 간단해질 수 있습니다.

```coffeescript
const fetchArticleDetails = () => apiAction({
   url: "https://api.myjson.com/bins/19dtxc",
   onSuccess: setArticleDetails,
   onFailure: () => {console.log("Error occured loading articles")},
   label: FETCH_ARTICLE_DETAILS
});
```

훨씬 더 간단해!

그리고 결과는 똑같습니다. 작동 중인 애플리케이션입니다!

라벨이 로드 상태에 어떻게 유용할 수 있는지 확인하기 위해, 저는 `API_START`와 `API_END` 작업 유형을 감소기 내에서 처리하겠습니다.

```bash
case API_START:
  if (action.payload === FETCH_ARTICLE_DETAILS) {
     return {
        ...state,
        isLoadingData: true
     };
}

case API_END:
   if (action.payload === FETCH_ARTICLE_DETAILS) {
      return {
        ...state,
        isLoadingData: false
      };
 }
```

이제 상태 개체에 `API_START`와 `API_END` 작업 유형을 모두 기준으로 `isLoadingData` 플래그를 설정합니다.

이를 바탕으로 앱 구성 요소 내에서 로드 상태를 설정할 수 있습니다.

결과는 다음과 같습니다.

됐다!

여기서 공유한 사용자 지정 미들웨어는 응용 프로그램의 좋은 시작점 역할을 할 뿐입니다. 이것이 정확한 상황에 적합한지 평가합니다. 특정 사용 사례에 따라 몇 가지 수정이 필요할 수 있습니다.

무슨 가치가 있든 간에, 저는 이 결정을 후회하지 않고 상당히 큰 프로젝트의 출발점으로 삼았습니다.

## 결론

Redux 앱에서 네트워크 요청을 작성하기 전에 사용 가능한 다양한 옵션을 사용해 보시기 바랍니다.

안타깝게도, 성장된 응용 프로그램을 위한 전략을 선택한 후에 리팩터링을 하는 것은 어려워집니다.

결국 팀, 애플리케이션, 시간, 그리고 궁극적으로는 당신 혼자만의 선택을 할 수 있습니다.

GitHub에서 코드 저장소를 확인하는 것을 잊지 마십시오. 이 기사에 영감을 준 https://leanpub.com/redux-book, 덕분입니다.

나중에 보자!