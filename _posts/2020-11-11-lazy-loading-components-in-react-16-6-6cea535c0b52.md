---
layout: post
title: "느리게 로드되는 반응 구성 요소"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2018/10/react-lazy-loading.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2018/10/react-lazy-loading.png?fit=730%2C487&ssl=1)

프런트엔드 개발의 세계는 끊임없이 발전하고 있으며, 사람들은 매일 점점 더 복잡하고 강력한 앱과 소프트웨어를 만듭니다. 자연스럽게, 이는 대량의 코드 번들로 이어졌고, 이는 앱이 로딩하는 데 걸리는 시간을 획기적으로 증가시키고 사용자 경험에 부정적인 영향을 미칠 수 있다. 게으른 짐은 바로 그 때 들어옵니다.

이 튜토리얼에서는 React.js에서 게으른 로드가 어떻게 작동하는지 보여주고, `react.lazy`와 `React`를 사용하여 코드 분할 및 게으른 로드를 적용하는 방법을 시연한다.서스펜스, 그리고 데모 리액션 앱을 구축하여 이러한 개념을 실제로 볼 수 있습니다.

다음 사항에 대해 자세히 설명합니다.

- 게으른 짐은 무엇인가?
- 반응에서 느린 로드를 사용하는 방법
- 반응에서의 코드 분할
- 반응의 동적 가져오기
- React.lazy() 사용
- 리액트 사용.서스펜스
- 반응 구성 요소의 명명된 내보내기
- 반응에서의 경로 기반 게으른 부하
- 대응에서 서버측 코드 분할

## 게으른 짐은 무엇인가?

게으른 로딩은 웹과 모바일 앱을 최적화하기 위한 디자인 패턴이다. 게으른 로딩의 개념은 간단하다: 사용자 인터페이스에 중요한 객체를 먼저 초기화하고 중요하지 않은 항목은 나중에 조용히 렌더링한다.

웹 사이트를 방문하거나 앱을 사용할 때 사용 가능한 콘텐츠가 모두 표시되지 않을 가능성이 높습니다. 앱을 탐색하고 사용하는 방법에 따라 특정 구성 요소가 필요하지 않을 수 있으며 불필요한 항목을 로드하는 데 시간과 컴퓨팅 리소스가 소요됩니다. 느리게 로드하면 필요 시 요소를 렌더링하여 애플리케이션의 효율성을 높이고 사용자 환경을 개선할 수 있습니다.

## 반응에서 느린 로드를 사용하는 방법

React는 코드 분할과 느린 로드를 React 구성 요소에 적용하기 매우 쉬운 두 가지 기능을 가지고 있다: React.lazy()와 React.서스펜스

React.lazy()는 동적 가져오기를 일반 구성 요소로 렌더링할 수 있는 기능입니다. 동적 가져오기는 코드 분할의 한 가지 방식이며, 이는 게으른 로드의 핵심입니다. 리액트 16.6의 핵심 기능인 리액트.레이지()는 리액트 로드 가능과 같은 타사 라이브러리를 사용할 필요가 없다.

`Ract.Suspense`를 사용하면 아래 트리의 구성 요소가 아직 렌더링할 준비가 되지 않은 경우 로드 표시기를 지정할 수 있습니다.

React.lazy와 React를 보기 전에.서스펜스에서는 코드 분할 및 동적 가져오기 개념을 신속하게 검토하고, 어떻게 동작하는지 설명하며, 리액션에서 어떻게 게으른 로드를 촉진하는지 분석해보자.

### 반응에서의 코드 분할

ES 모듈, Babel과 같은 Transpiler, Webpack 및 Browserify와 같은 번들러가 등장함에 따라 이제 JavaScript 애플리케이션을 완벽한 모듈형 패턴으로 작성할 수 있어 유지관리가 용이합니다. 일반적으로 각 모듈을 가져와 번들이라는 단일 파일로 병합한 다음 번들이 웹 페이지에 포함되어 전체 앱을 로드합니다. 앱이 커질수록 번들 크기가 커지고 결국 페이지 로드 시간에 영향을 미칩니다.

코드 분할(Code-spliting)은 동적으로 로드될 수 있는 코드 번들을 여러 번들로 나누는 과정이다. 이렇게 하면 실제로 앱의 코드 양을 줄이지 않고도 크기가 큰 번들과 관련된 성능 문제를 방지할 수 있습니다.

### 반응의 동적 가져오기

코드를 분할하는 한 가지 방법은 가져오기() 구문을 활용하는 동적 가져오기를 사용하는 것입니다. 모듈을 로드하기 위해 `import()`를 호출하는 것은 JavaScript Promise에 따라 다릅니다. 따라서 로드된 모듈로 이행되거나 모듈을 로드할 수 없는 경우 거부된 약속을 반환합니다.

다음은 웹 팩과 함께 제공된 앱의 모듈을 동적으로 가져오는 방법입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/dynamic-import-webpack-1.png?resize=720%2C226&ssl=1)

웹 팩은 이 구문을 보면 `순간` 라이브러리에 대해 별도의 번들 파일을 동적으로 생성해야 합니다.

React 앱의 경우 "create-react-app" 또는 "Next.js"와 같은 보일러 플레이트를 사용하는 경우 동적 `import()를 사용한 코드 분할이 즉시 수행됩니다. 그러나 사용자 지정 웹 팩 설정을 사용하는 경우 코드 분할 설정에 대한 웹 팩 가이드를 확인해야 합니다. Babel transling의 경우 동적 `import()를 올바르게 구문 분석하려면 `babel-plugin-syntax-dynamic-import` 플러그인이 필요합니다.

그럼 이제 리액트.레이지()와 리액트(Ract)를 사용하는 방법을 살펴보겠습니다.서스펜스

## React.lazy() 사용

React.lazy()를 사용하면 동적 import()를 사용하여 로드되지만 일반 구성 요소처럼 렌더링되는 구성 요소를 쉽게 만들 수 있습니다. 이렇게 하면 구성 요소가 렌더링될 때 구성 요소가 포함된 번들이 자동으로 로드됩니다.

React.lazy()는 구성 요소를 로드하기 위해 import()를 호출하여 약속을 반환해야 하는 함수로 간주한다. 반환된 약속은 React 구성 요소를 포함하는 기본 내보내기를 사용하는 모듈로 결정됩니다.

React.lazy()를 사용하면 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/react.lazy_-1.png?resize=720%2C534&ssl=1)

## 리액트 사용.서스펜스

React.lazy()를 사용하여 생성한 구성 요소는 렌더링해야 할 때만 로드됩니다. 따라서 로드 표시기와 같이 게으른 구성 요소가 로드되는 동안 일종의 자리 표시자 콘텐츠를 표시해야 합니다. 이것이 바로 `반응`이다.서스펜스는 을 위해 고안되었다.

`액트 서스펜스`는 게으른 부품을 포장하는 부품이다. 여러 계층 수준에서 단일 `Suspense` 구성 요소로 여러 개의 게으른 구성 요소를 래핑할 수 있습니다.

`Suspense` 구성 요소는 모든 게으른 구성 요소가 로드되는 동안 사용자가 원하는 리액트 요소를 자리 표시자 콘텐츠로 받아들이는 `fallback` 프로포트를 사용합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/react.suspense-1.png?resize=720%2C362&ssl=1)

게으른 구성 요소가 로드되지 않을 경우 사용자 환경을 개선하기 위해 게으른 구성 요소 위의 아무 곳에나 오류 경계를 배치할 수 있습니다.

저는 코드 샌드박스에 `Ract.lazy()와 `Suspense`를 사용하여 구성 요소를 느리게 로드하는 것을 시연하기 위해 매우 간단한 데모를 만들었습니다.

미니어처 앱의 코드는 다음과 같습니다.

```js
import React, { Suspense } from "react";
import Loader from "./components/Loader";
import Header from "./components/Header";
import ErrorBoundary from "./components/ErrorBoundary";

const Calendar = React.lazy(() => {
  return new Promise(resolve => setTimeout(resolve, 5 * 1000)).then(
    () =>
      Math.floor(Math.random() * 10) >= 4
        ? import("./components/Calendar")
        : Promise.reject(new Error())
  );
});

export default function CalendarComponent() {
  return (
    <div>
      <ErrorBoundary>
        <Header>Calendar</Header>

        <Suspense fallback={<Loader />}>
          <Calendar />
        </Suspense>
      </ErrorBoundary>
    </div>
  );
}
```

여기서는 게으른 `캘린더` 구성 요소의 예비 컨텐츠로 사용할 간단한 `로더` 구성 요소를 만들었습니다. 또한 게으른 `Calendar` 구성 요소가 로드되지 않을 때 메시지를 표시하기 위해 오류 경계도 만들었습니다.

나는 5초의 지연을 시뮬레이션하겠다는 또 다른 약속으로 게으른 달력 수입을 포장했다. 캘린더 구성 요소가 로드되지 않을 가능성을 높이기 위해 캘린더 구성 요소를 가져오거나 거절하는 약속을 반환하는 조건도 사용했습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/react.lazy-calendar-import-1.png?resize=720%2C276&ssl=1)

다음 애니메이션은 `Ract.lazy() 및 `Ract`를 사용하여 렌더링할 때 구성 요소의 모양을 보여 주는 데모를 보여 줍니다.서스펜스

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/10/react-lazy-load-demo.gif?resize=720%2C720&ssl=1)

## 반응 구성 요소의 명명된 내보내기

현재 React.lazy()는 React 구성 요소에 대해 명명된 내보내기를 지원하지 않습니다. React 구성 요소를 포함하는 명명된 내보내기를 사용하려면 별도의 중간 모듈에서 기본 내보내기로 다시 내보내야 합니다.

예를 들어 모듈에 `기타 구성 요소`를 명명된 내보내기로 사용하고 `Ract.lazy()를 사용하여 `기타 구성 요소`를 로드하려고 한다고 가정합시다. 기본 내보내기로 `기타 구성 요소`를 다시 내보내기 위한 중간 모듈을 생성합니다.

`Components.js`:

`OtherComponent.js`:

이제 `Ract.lazy()`를 사용하여 중간 모듈에서 `기타 구성 요소`를 로드할 수 있습니다.

## 반응에서의 경로 기반 게으른 부하

리액트.레이지. 리액트.서스펜스`를 사용하면 외부 패키지를 사용하지 않고도 경로 기반 코드 분할을 수행할 수 있습니다. 앱의 경로 구성 요소를 게으른 구성 요소로 변환하고 모든 경로를 `Suspense` 구성 요소로 포장하기만 하면 됩니다.

다음 코드 조각은 도달 라우터 라이브러리를 사용하는 경로 기반 코드 분할을 보여 줍니다.

```coffeescript
import React, { Suspense } from 'react';
import { Router } from '@reach/router';
import Loading from './Loading';

const Home = React.lazy(() => import('./Home'));
const Dashboard = React.lazy(() => import('./Dashboard'));
const Overview = React.lazy(() => import('./Overview'));
const History = React.lazy(() => import('./History'));
const NotFound = React.lazy(() => import('./NotFound'));

function App() {
  return (
    <div>
      <Suspense fallback={<Loading />}>
        <Router>
          <Home path="/" />
          <Dashboard path="dashboard">
            <Overview path="/" />
            <History path="/history" />
          </Dashboard>
          <NotFound default />
        </Router>
      </Suspense>
    </div>
  )
}
```

## 대응에서 서버측 코드 분할

서버측 렌더링에는 아직 React.lazy()와 Suspense(서스펜스)를 사용할 수 없습니다. 서버 측 코드 분할의 경우 `react-loadable`을 사용해야 합니다.

코드 분할 반응 구성 요소에 대한 한 가지 접근 방식은 경로 기반 코드 분할이라고 하며, 이 접근 방식은 느린 부하 경로 구성 요소에 동적 `가져오기()`를 적용해야 한다. `react-loadable`은 동적 `import() 구문을 활용하여 Retact 구성 요소를 약속과 함께 로드하기 위한 고차 성분(HOC)을 제공한다.

`MyComponent`라고 하는 다음 React 구성 요소를 고려하십시오.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/server-side-rendering-react-lazy-load-1.png?resize=720%2C334&ssl=1)

여기서 `기타 구성요소`는 `내 구성요소`가 렌더링될 때까지 필요하지 않습니다. 하지만 우리는 정적으로 기타 구성요소를 가져오기 때문에 마이 컴포넌트와 함께 번들로 제공됩니다.

"Ract-loadable"을 사용하여 "MyComponent"를 렌더링할 때까지 "OtherComponent" 로드를 지연시켜 코드를 별도의 번들로 분할할 수 있습니다.

react-loadable을 사용하여 로드된 `other Component`는 다음과 같습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/react-loadable-example-1.png?resize=720%2C476&ssl=1)

구성 요소는 동적 `import() 구문을 사용하여 가져오고 옵션 개체의 `loader` 속성에 할당됩니다.

또한 "loadable"은 "loading" 속성을 사용하여 실제 구성 요소가 로드되기를 기다리는 동안 렌더링되는 폴백 구성 요소를 지정합니다.

## 결론

리액트.레이지() 리액트()로.서스펜스, 코드 분할 및 느리게 로드되는 리액트 구성 요소는 그 어느 때보다도 간단합니다. 이러한 기능을 통해 React 앱의 성능을 더욱 빠르게 하고 전반적인 사용자 환경을 개선할 수 있습니다.