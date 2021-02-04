---
layout: post
title: "React Router와 함께 후크 사용
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/react-router-hooks.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-router-hooks.png?fit=730%2C487&ssl=1)

React Hooks는 React 16.8.0의 출시와 함께 소개되었습니다.
 후크를 사용하면 개발자는 클래스 구성 요소에 비해 더 적은 상용구로 더 깨끗한 구성 요소를 작성할 수 있습니다.
 많은 인기있는 React 패키지는 개발자가 기능 구성 요소에서 API를 활용할 수 있도록 Hook에 대한 지원을 추가하고 있습니다.
 

React Apps 용 라우팅 솔루션 인 React Router는 버전 5 릴리스에 Hooks 지원을 추가했습니다.
 이러한 React Router Hooks는 개발자에게 라우터 상태를 처리하는 새로운 방법을 제공합니다.
 

이 튜토리얼에서는 React Router와 함께 Hook을 사용하고 컴포넌트의 코드 라인을 최소화하는 방법을 보여줄 것입니다.
 

## 후크가 React Router와 함께 작동하는 방법
 

Hooks의 작동 방식을 보여주기 위해 React 프로젝트를 만들고 페이지를 설정합니다.
 

React 앱을 만들려면 다음 명령을 실행하십시오.
 

```undefined
npx create-react-app router-hooks-demo
```

`router-hooks-demo`는이 가이드의 앱 이름입니다.
 원하는대로 이름을 변경할 수 있습니다.
 

다음으로`react-router-dom` 패키지를 추가합니다.
 

```coffeescript
npm i react-router-dom --save
```

`react-router-dom` 패키지에서`BrowserRouter`,`Link`,`Route` 및`Switch` 구성 요소를 가져옵니다.
 이러한 구성 요소는 React 앱에서 클라이언트 측 라우팅을 구성하는 데 사용됩니다.
 

```coffeescript
import { BrowserRouter, Link, Route, Switch } from "react-router-dom";
```

이 데모에서는 홈페이지와 사용자 페이지라는 두 가지 경로 또는 페이지 만 제공됩니다.
 주로 사용자 페이지에서 작업하게됩니다.
 

```js
const User = () => {
  return <div>This is the user page</div>;
};

const Home = () => {
  return <div>This is the home page</div>;
};
```

`앱`구성 요소에서 탐색 메뉴 섹션을 만들고 `링크`구성 요소를 사용하여 페이지에 하이퍼 링크를 추가합니다.
 웹 페이지에서`Link` 구성 요소는`<a>`태그로 렌더링됩니다.
 

```xml
<nav>
  <div>
    <Link to="/">Home</Link>
  </div>
  <div>
    <Link to="/user/:id">User</Link>
  </div>
</nav>
```

다음으로`Switch` 및`Route` 구성 요소를 추가하고 모든 항목을`BrowserRouter` 구성 요소로 래핑합니다.
 

```xml
<BrowserRouter>
  <nav>
    <div>
      <Link to="/">Home</Link>
    </div>
    <div>
      <Link to="/user/:id">User</Link>
    </div>
  </nav>
  <Switch>
    <Route path="/user">
      <User />
    </Route>
    <Route path="/" exact>
      <Home />
    </Route>
  </Switch>
</BrowserRouter>
```

`BrowserRouter` 구성 요소는 클라이언트 측 라우팅 기능을 활성화하고 브라우저`history` API를 사용하여 라우팅 로직을 처리합니다.
 `Route`구성 요소는 경로 경로가 활성 URL과 일치 할 때 페이지 UI를 렌더링합니다.
 `Switch`구성 요소는 첫 번째 일치 경로 만 렌더링하도록 허용합니다.
 

동일한 URL과 일치하는 여러`Route` 구성 요소가있는 경우 모든 경로를 렌더링하게되며 이는 예상치 못한 결과입니다.
 URL에 대해 하나의 경로 만 렌더링해야합니다.
 

최종`App.js` 파일은 다음과 같습니다.
 

```js
import React from "react";
import { BrowserRouter, Link, Route, Switch } from "react-router-dom";

const User = () => {
  return <div>This is the user page</div>;
};

const Home = () => {
  return <div>This is the home page</div>;
};

export default function App() {
  return (
    <div className="App">
      <BrowserRouter>
        <nav>
          <div>
            <Link to="/">Home</Link>
          </div>
          <div>
            <Link to="/user/:id">User</Link>
          </div>
        </nav>
        <Switch>
          <Route path="/user/:id">
            <User />
          </Route>
          <Route path="/" exact>
            <Home />
          </Route>
        </Switch>
      </BrowserRouter>
    </div>
  );
}
```

## React Router Hooks가 유용한 이유
 

페이지 구성 요소 내에서 URL의 현재 경로 이름에 액세스해야한다고 가정 해 보겠습니다.
 Hooks 이전에는 페이지 구성 요소를`Route` 구성 요소의`component` prop에 전달해야합니다.
 그런 다음`Route` 구성 요소는`match`,`location` 및`history`와 같은 route props를 삽입합니다.
 

괜찮지 만 컴포넌트를 읽기가 더 어려워지고 프로젝트를 유지할 때 소품이 어떻게 주입되는지 이해하기 어렵습니다.
 React Router 작성자는 Hooks 지원을 추가하여 페이지 구성 요소가`Route` 구성 요소의 구성 요소 소품으로 페이지 구성 요소를 전달하지 않고도`history`,`location` 및`match` 객체에 액세스 할 수 있도록했습니다.
 

```js
// Route with component prop
<Route path="/user/:id" component={User} />;

// User component
const User = (props) => {
  const location = props.location;

  console.log(location.pathname);

  return <div>This is the user page</div>;
};
```

## 라우터 후크 반응
 

라우팅을 위해 후크가 추가 된 이유를 이해 했으므로 이제 후크가 작동하는지 살펴 보겠습니다.
 

### `useParams`
 

`useParams` 후크는 동적 URL에서 전달 된 매개 변수의 키-값 쌍을 포함하는 객체를 반환합니다.
 

예를 들어 URL에서 `id`를 매개 변수로 허용하는 `User`페이지 구성 요소가 있다고 가정 해 보겠습니다.
 

```js
<Route path="/user/:id">
  <User />
</Route>
```

`react-router-dom` 패키지에서`useParam` 후크를 가져옵니다.
 

```coffeescript
import { useParams } from "react-router-dom";
```

이제 아래와 같이 후크를 사용할 수 있습니다.
 

```js
const User = () => {
  const params = useParams();

  console.log(params);

  return (
    // ...
  )
}
```

따라서 사용자 URL (`/ user / 3`)에서 ID로`3`을 전달하면`useParam ()`은 다음과 같은 객체를 반환합니다.
 

```css
{
  id: 3,
}
```

이`id` 매개 변수를 사용하여 페이지에 표시하거나 서버에서 사용자 정보를 가져올 수 있습니다.
 

```js
import {
  // ...
  useParams,
} from "react-router-dom";

// /user/:id
const User = () => {
  const params = useParams();

  return (
    <div>
      <div>This is the user page</div>
      <div>current user Id - {params.id}</div>
    </div>
  );
};
```

### `useHistory`
 

`useHistory` 후크는 라우터가 내부적으로 경로 변경을 처리하는 데 사용하는`history` 객체를 반환합니다.
 `history` 객체는 세션 히스토리를 관리하는 데 사용됩니다.
 

예를 들어`history.push ()`메서드를 사용하여 기록 스택에 새 항목을 추가하고 현재 경로에서 사용자를 탐색 할 수 있습니다.
 

```js
import {
  // ...
  useHistory,
} from "react-router-dom";

const User = () => {
  const history = useHistory();
  const params = useParams();

  const handleBack = () => {
    history.goBack();
  };

  const handleNavigation = () => {
    history.push("/user/5");
  };

  return (
    <div>
      <div>This is the user page</div>
      <div>current user Id - {params.id}</div>
      <div>
        <button onClick={handleBack}>Go Back</button>
      </div>
      <div>
        <button onClick={handleNavigation}>Go To Different User</button>
      </div>
    </div>
  );
};
```

위의 코드 블록에서 두 개의 버튼이 `사용자`페이지 구성 요소에 추가되었습니다. 하나는 사용자를 이전 페이지로 이동하는 버튼이고 다른 하나는 사용자를 다른 페이지로 이동하는 버튼입니다.
 `history.goBack ()`메소드는 사용자를 이전 페이지로 이동하고 내역 스택에서 한 항목 뒤로 이동합니다.
 

### `useLocation`
 

`useLocation` 후크를 사용하면 활성 URL을 나타내는`location` 객체에 액세스 할 수 있습니다.
 `location`개체의 값은 사용자가 새 URL로 이동할 때마다 변경됩니다.
`useLocation`후크는 URL이 변경 될 때마다 이벤트를 트리거해야 할 때 편리 할 수 있습니다.
 

사용자 프로필 페이지의 조회수를 추적해야한다는 점을 고려하세요.
 React와 함께 제공되는`useEffect` Hook을 사용하여`location` 객체의 변경 사항을 감지 할 수 있습니다.
 

```js
import {
  // ...
  useLocation,
} from "react-router-dom";

const User = () => {
  const history = useHistory();
  const params = useParams();
  const location = useLocation();

  useEffect(() => {
    console.log(location.pathname);
    // Send request to your server to increment page view count
  }, [location]);

  const handleBack = () => {
    history.goBack();
  };

  const handleNavigation = () => {
    history.push("/user/5");
  };

  return (
    <div>
      <div>This is the user page</div>
      <div>current user Id - {params.id}</div>
      <div>
        <button onClick={handleBack}>Go Back</button>
      </div>
      <div>
        <button onClick={handleNavigation}>Go To Different User</button>
      </div>
    </div>
  );
};
```

### `useRouteMatch`
 

`useRouteMatch` 후크는`Route` 구성 요소가 작동하는 방식과 유사하게 활성 URL을 지정된 경로와 일치시킵니다.
 매우 흥미로운 후크입니다.
 불필요한`Route` 구성 요소를 제거하고`match` 객체에 액세스 할 수 있습니다.
 

이제 사용자 페이지의 경로 구성 요소에서 단순히`/ user`로 경로를 변경할 수 있습니다.
 

```js
<Route path="/user/">
  <User />
</Route>
```

그런 다음 아래와 같이`routeMatch` 후크를 사용합니다.
 

```js
import {
  // ...
  useRouteMatch,
} from "react-router-dom";

const User = () => {
  const history = useHistory();

  const routeMatch = useRouteMatch("/user/:id");
  const location = useLocation();

  useEffect(() => {
    console.log(location);
  }, [location]);

  const handleBack = () => {
    history.goBack();
  };

  const handleNavigation = () => {
    history.push("/user/5");
  };

  return (
    <div>
      <div>This is the user page</div>
      {routeMatch ? (
        <div>user Id - {routeMatch.params.id}</div>
      ) : (
        <div>You are viewing your profile</div>
      )}

      <div>
        <button onClick={handleBack}>Go Back</button>
      </div>
      <div>
        <button onClick={handleNavigation}>Go To Different User</button>
      </div>
    </div>
  );
};
```

`useParams` 후크는 Route 구성 요소에 지정된 경로 매개 변수가 없기 때문에 빈 객체를 반환하지 않습니다.
 대신 이제`routeMatch` 객체의 매개 변수에 액세스 할 수 있습니다.
 

## 결론
 

후크는 React 생태계에 큰 도움이됩니다.
 이제 후크로 라우팅을 처리하는 방법에 대한 코드를 확인 했으므로 이제 후크가 제공하는 기능을 최대한 활용할 준비가되었습니다.
 

다음 시간까지 안전하게 React Hooks로 코딩하세요!
 