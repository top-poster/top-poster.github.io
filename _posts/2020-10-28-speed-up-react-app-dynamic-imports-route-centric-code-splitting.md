---
layout: post
title: "동적 가져오기 및 경로 중심 코드 분할을 통해 Return 앱의 속도 향상"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/react-code-splitting-nocdn.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/react-code-splitting-nocdn.png?fit=730%2C487&ssl=1)

성능 최적화는 모든 개발자가 다루는 중요한 소프트웨어 개발 이정표입니다. 우수한 코드 작성, 기능 추가, 장기화된 디버깅 섹션 지속, 그리고 마지막으로 NAT은 가장 좋아하는 호스팅 서비스를 선택하고 애플리케이션을 클라우드에 배포합니다.

그러나 애플리케이션을 호스팅하고 탐색하려고 하면 로드 시간이 많이 걸리는 것을 즉시 알 수 있습니다. 즉, 애플리케이션의 속도가 엄청나게 느립니다. 이제 성능 최적화 이정표에 도달했습니다.

로컬 호스트에서 개발하는 동안 성능 문제는 거의 발생하지 않지만, 생산과 개발의 차이가 있기 때문입니다.

로컬 서버에서 개발하는 동안 모든 파일은 컴퓨터 포트에서 호스팅됩니다. React에서 포트는 기본적으로 `3000`으로 설정됩니다. 로컬 서버를 사용하는 동안에는 인터넷 연결이 중요하지 않으므로 모든 파일과 JavaScript 번들을 매우 빠르게 다운로드할 수 있습니다.

그러나, 특히 고속 인터넷이 없는 곳에서는, 우리가 라이브로 접속하면 대용량 파일과 JavaScript 번들을 다운로드하는 것이 큰 문제가 됩니다. React와 함께 사용할 수 있는 몇 가지 성능 최적화 기법과 요령이 있습니다. 이 기사에서는 경로 중심 코드 분할을 사용하여 성능을 향상시키는 방법에 대해 알아보겠습니다.

## 코드 분할의 이점

create-react-app을 사용하면 코드 분할 및 청킹이 가능합니다. 청크를 사용하면 코드를 하나의 파일로 패키지된 관련 청크 그룹인 번들로 구분할 수 있습니다. create-react-app, Gatsby.js 및 Next.js와 같은 도구들은 웹 팩을 사용하여 응용 프로그램을 번들로 묶는다. 웹 팩과 같은 번들러는 모든 응용 프로그램 파일을 가져와 단일 번들로 병합합니다.

이 작업의 몇 가지 이점은 다음과 같습니다.

- 사용자의 브라우저가 다른 HTTP 요청 없이 원활하게 탐색할 수 있도록 전체 앱을 한 번 다운로드할 수 있습니다.
- 브라우저는 모두 번들에 있으므로 다른 파일을 요구하거나 가져올 필요가 없습니다.

번들은 종종 도움이 되지만, 앱이 커질수록 앱 번들이 엄청나게 커질 수 있기 때문에 앱 로딩 시간에 부메랑을 일으킬 수 있다.

웹 개발자는 필요에 따라 파일을 느리게 로드하고 Retact 애플리케이션의 성능을 향상시킬 수 있으므로 웹 개발자는 대형 번들을 더 작은 번들로 분할합니다.

다음은 React 앱의 프로덕션 빌드에 대한 조각입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/yBSPW03w.png?resize=730%2C121&ssl=1)

빌드/정적/js 및 빌드/정적/css 디렉토리에서 빌드 스크립트 npm run build 또는 yarn build를 각각 실행하여 프로덕션 빌드를 생성할 수 있습니다.

이미지를 통해 파일이 서로 다른 청크로 분할되는 것을 알 수 있다. create-react-app는 `SplitChunksPlugin` 웹 팩 플러그인을 통해 이를 달성한다. 위에 표시된 코드를 분해해 보겠습니다.

- `메인.[해시].chunk.css`는 애플리케이션에 필요한 모든 CSS 코드를 나타냅니다. JavaScript에서 스타일 구성 요소 같은 것을 사용하여 CSS를 작성해도 CSS로 컴파일됩니다.
- `숫자`.[➡➡.➡.js`는 응용 프로그램에서 사용되는 모든 라이브러리를 나타냅니다. 기본적으로 `node_modules` 폴더에서 가져온 모든 벤더 코드입니다.
- `메인.[해시].chunk.js는 `App.js`, `Contact.js`, `About.js` 등의 모든 애플리케이션 파일입니다. 리액트 응용 프로그램에 대해 작성한 모든 코드를 나타냅니다.
- `런타임-메인`[discent.js`는 응용 프로그램을 로드하고 실행하는 데 사용되는 작은 웹 팩 런타임 로직을 나타냅니다. 내용은 기본적으로 `build/index.html` 파일에 있습니다.

하지만, 우리의 생산 빌드는 최적화되어 있지만, 여전히 개선의 여지가 있습니다.

아래 이미지를 고려하십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/u7TMq3gZ-e1603726308465.png?resize=730%2C567&ssl=1)

프로덕션 빌드를 생성하고 애플리케이션을 그대로 배포할 수 있지만 위의 이미지는 애플리케이션을 더욱 최적화할 수 있음을 보여 줍니다.

사진에서 우리는 `메인`을 볼 수 있습니다.[hash.chunk.js`에는 당사의 모든 응용 프로그램 파일이 들어 있으며 크기가 1000KB입니다. 또한 사용자가 로그인 페이지를 방문할 때 브라우저에서 전체 1000KB 청크가 다운로드되는 것을 볼 수 있습니다. 이 청크에는 사용자에게 필요하지 않을 수 있는 코드가 포함되어 있습니다. 따라서 로그인 페이지가 2KB인 경우 사용자는 2KB 페이지를 보려면 1000KB 청크를 로드해야 합니다.

왜냐하면 `메인`이기 때문이다.[hash]chunk.js의 크기는 애플리케이션이 커질수록 증가하며, 더 큰 앱은 크기가 1000KB를 초과할 수 있습니다. 즉, 앱 로드 시간이 크게 증가할 수 있으며, 사용자가 인터넷 속도가 낮으면 성능이 훨씬 더 느려질 수 있습니다. 이것이 우리가 추가적인 최적화가 필요한 이유입니다.

이에 대한 해결책은 `메인`을 나누는 것이다.[mbs.js]는 사용자가 페이지를 방문할 때 필요한 코드 청크만 다운로드하도록 보장하는 더 작은 청크입니다. 이 예에서는 사용자의 브라우저가 로그인 청크만 로드해야 합니다.

이를 통해 앱 초기 로드 중에 사용자가 다운로드하는 코드 수를 대폭 줄이고 응용 프로그램의 성능을 향상시킬 수 있습니다. 다음 섹션에서 코드 분할을 구현하는 방법을 살펴보겠습니다.

## 경로 중심 코드 분할 구현

코드 분할을 구현하려면 JavaScript와 React:의 기능을 결합합니다.

### 1. 동적수입

이것은 거의 약속처럼 우리의 파일을 가져오는 현대 자바스크립트 기능입니다.

이전:

```coffeescript
import Login from "Pages/Login.js";
import Home from "Pages/Home.js";
import About from "Pages/About.js";
import Contact from "Pages/Contact.js";
import Blog from "Pages/Blog.js";
import Shop from "Pages/Shop.js";
```

위의 코드 조각은 정적 가져오기를 사용하여 파일을 가져옵니다. 웹 팩은 이 구문을 발견하면 모든 파일을 함께 묶습니다. 왜냐하면 우리는 그것들을 함께 정적으로 포함시키고 싶기 때문입니다.

이후:

```js
const module = await import('/modules/myCustomModule.js');
```

동기식인 정적 가져오기와 달리 동적 가져오기는 비동기식입니다. 이를 통해 모듈 및 파일을 온디맨드 방식으로 가져올 수 있습니다.

웹 팩이 이 구문을 발견하면 즉시 응용 프로그램을 코드 분할하기 시작합니다.

### 2.'반응.게으름()'

이 반응 구성 요소는 다른 함수를 인수로 사용하는 함수입니다. 이 인수는 동적 가져오기를 호출하고 약속을 반환합니다. React.lazy()는 이 약속을 처리하며 기본 export React 구성 요소를 포함하는 모듈을 반환할 것으로 예상한다.

이전:

```coffeescript
import Login from "Pages/Login.js";
```

이후:

```coffeescript
import React, {lazy} from "react";
const Login = lazy(()=> import("Pages/Login"));
```

이제 로그인 페이지가 게으르게 로드되어 `Login.js` 청크가 렌더링된 경우에만 로드됩니다.

### 3. 반응해.서스펜스()

React.Suspense()를 사용하면 구성 요소가 로드될 때까지 렌더링 작업을 조건부로 일시 중단할 수 있습니다. 리액트 요소를 받아들이는 폴백(fallback) 소품이 있다. 반응 요소는 JSX 조각이거나 전체 구성 요소일 수 있습니다.

동적 가져오기를 사용하는 페이지를 방문할 때 앱이 모듈을 로드하는 동안 빈 화면이 나타날 수 있습니다. 동적 가져오기가 비동기적이기 때문에 사용자가 오류를 얻을 수도 있습니다. 사용자가 인터넷 연결이 느린 경우 이 가능성이 증가합니다.

이 문제를 해결하기 위해 React.lazy()와 React.suspense()가 함께 사용된다.

반응하는 동안.서스펜스는 모든 종속성이 게으르게 로드될 때까지 구성 요소의 렌더링을 중단하며 폴백 소품으로 전달되는 리액트 요소도 폴백 UI로 표시합니다.

아래 코드를 고려하십시오.

```js
import React, { lazy, Suspense } from 'react';

const Hero = lazy(() => import('./Components/Hero'));
const Service = lazy(() => import('./Component/Service'));

const Home = () => {
  return (
    <div>
      <Suspense fallback={<div>Page is Loading...</div>}>
        <section>
          <Hero /> 
          <Service />
        </section>
      </Suspense>
    </div>
  );
}
```

여기서는 영웅과 서비스 구성요소를 게으르게 로드합니다. 이것들은 `홈 컴포넌트`의 의존성이다. 그것은 완전한 홈페이지를 보여주기 위해 그들이 필요하다.

우리는 `서스펜스 컴포넌트`를 사용하여 의존성이 게으르게 로드될 때까지 `홈 컴포넌트`의 렌더링을 일시 중단시켜 사용자가 홈페이지로 이동할 때 오류나 빈 페이지가 생기지 않도록 한다.

이제 구성 요소를 느리게 로드하는 경우 사용자는 아래의 폴백 UI를 사용합니다.

```xml
<div>Page is Loading...</div>
```

### 4. 반응기

react-router-dom 라이브러리는 경로 수준 코드 분할을 지원합니다. 경로 레벨에서 청크를 다운로드할 수 있습니다. 따라서, 우리는 경로 레벨에서 분할 코드를 만들 것입니다. 이것은 매우 유용합니다.

아래 코드를 고려하십시오.

```coffeescript
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const Shop = lazy(() => import('./routes/Shop'));

const App = () => {
    return ( 
    <Router>
      <Suspense fallback={<div>Page is Loading...</div>}>
        <Switch>
          <Route exact path="/" component={Shop}/>
          <Route path="/shop" component={Shop}/>
        </Switch>
      </Suspense>
    </Router>
  )
};
```

이 코드 샘플에서 우리는 `react-router-router` 라이브러리를 사용하여 경로를 설정하고, `Home`과 `Shop` 구성 요소는 느리게 로드된다. 모든 `서스펜스` 코드가 모든 경로를 어떻게 캡슐화하는지 주목하십시오. 이렇게 하면 요청된 구성 요소가 느리게 로드되는 동안 폴백 UI가 사용자에게 렌더링됩니다.

우리의 설정 때문에 웹 팩은 우리의 코드를 미리 정리한다. 따라서 사용자는 요청 시 페이지를 렌더링하는 데 필요한 청크만 수신합니다. 예를 들어, 사용자가 홈 페이지를 방문할 때 사용자는 `Home.js` 청크를 수신하고, 사용자가 상점 페이지를 방문할 때 `Shop.js` 청크를 볼 수 있습니다.

따라서, 우리는 앱의 코드 양을 줄이지 않고도 애플리케이션의 초기 로드 시간을 크게 줄였습니다.

## 결론

이 기사에서는 경로 중심 코드 분할이 무엇인지, 왜 사용이 도움이 되는지에 대해 설명했습니다. 우리는 또한 더 나은 성능의 리액트 애플리케이션을 만들기 위해 동적 수입품인 `Ract.lazy()the`와 `Suspense`를 활용하는 것에 대해서도 논의하였다.