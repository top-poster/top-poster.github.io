---
layout: post
title: "React Hooks vs. Redux : Hooks와 Context가 Redux를 대체합니까?
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/11/react-hooks-context-vs-redux.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/11/react-hooks-context-vs-redux.png?fit=730%2C487&ssl=1)

참고 :이 게시물에는 오래된 정보가 포함되어 있습니다.
 Redux Toolkit이 어떻게 상용구를 줄이고이 기사에 설명 된 다른 문제를 해결하는지 알아보십시오.
 

Redux는 과도한 양의 코드로 인해 코드베이스에 많은 복잡성을 유발합니다.
 

기껏해야 React 애플리케이션의 상태 관리를위한 불완전한 솔루션이됩니다.
 그러나 너무 많은 React 개발자가 다른 대안을 고려하지 않고 상태 관리를 위해 Redux를 기본으로 사용합니다.
 

이 Redux vs. Hooks 튜토리얼에서는 상태 관리를위한 React Context API를 소개하고 React Hooks와 Context가 Redux를 대체 할 수있는 이유를 설명합니다.
 

다음 내용을 다룹니다.
 

- React에서 상태 관리 도구가 필요한 이유
 
- React Hooks 및 Context로 전환해야하는 이유
 
- Redux는 무엇입니까?
 
- React Context API는 무엇입니까?
 
- React Hooks는 무엇입니까?
 
- `useContext` 후크
 
- `useReducer` 후크
 
- React Context에서`useReducer` Hook을 사용하는 방법
 

시각적 인 학습자라면 아래 동영상에서 React Context API를 설명하고 Redux를 React Hooks 및 Context로 대체해야하는 몇 가지 이유를 제공합니다.
 

## React에서 상태 관리 도구가 필요한 이유
 

일반적인 React에서 연결이 끊긴 구성 요소 간의 데이터를 처리하는 방법은 소품 드릴링을 사용하는 것입니다.
 예를 들어 최상위 구성 요소에서 다섯 번째 수준 구성 요소로 데이터를 전달하려는 경우 구성 요소가 액세스 할 수있는 전역 상태가 없기 때문에 데이터를 트리의 각 수준에서 소품으로 전달해야합니다.
 원하는 구성 요소에 도달 할 때까지.
 

그 결과 엄청난 양의 추가 코드를 작성하고 절대 사용하지 않을 구성 요소 속성을 제공하면 아키텍처 디자인에도 영향을 미칩니다.
 

이 문제를 해결하기 위해 우리는 모든 구성 요소가 아무리 깊게 중첩되어 있더라도 액세스 할 수있는 전역 상태를 제공하는 방법이 필요했습니다.
 이를 해결함으로써 Redux (애플리케이션 상태 관리를위한 오픈 소스 JavaScript 라이브러리)는 React 개발자를위한 솔루션이되었습니다.
 

## React Hooks 및 Context로 전환해야하는 이유
 

어느 정도까지 Redux는 React 애플리케이션의 상태 관리를 위해 작동하고 몇 가지 장점이 있지만 그 장황함으로 인해 선택하기가 정말 어렵고 애플리케이션에서 작동하는 데 필요한 많은 추가 코드로 인해 불필요한 복잡성이 많이 발생합니다.
 

반면에`useContext` API와 React Hooks를 사용하면 앱을 작동시키기 위해 외부 라이브러리를 설치하거나 파일과 폴더를 추가 할 필요가 없습니다.
 이것은 React 애플리케이션에서 전역 상태 관리를 처리하는 훨씬 더 간단하고 직접적인 방법입니다.
 

Redux, React Hooks 및 Context API를 자세히 살펴보고 작동 방식, 개발자가 이러한 도구를 사용할 때 직면하는 문제, React Hooks 및 Context를 사용하여 Redux와 관련된 일반적인 문제를 극복하는 데 어떻게 도움이되는지 살펴 보겠습니다.
 

## Redux는 무엇입니까?
 

Redux 문서에서는 일관되게 작동하고 다양한 환경에서 실행되며 테스트하기 쉬운 애플리케이션을 작성하는 데 도움이되는 JavaScript 애플리케이션의 예측 가능한 상태 컨테이너로 설명합니다.
 

소품 드릴링의 한 가지 단점은 최상위 구성 요소에서 데이터에 액세스하기 위해 상당한 양의 추가 코드를 작성해야한다는 것입니다.
 

Redux를 사용하면 전역 상태를 설정하는 데 필요한 모든 추가 코드가 훨씬 더 복잡해지기 때문에 이러한 단점이 더욱 심각해집니다.
 Redux가 작동하려면 액션, 리듀서, 스토어의 세 가지 주요 빌딩 부분이 필요합니다.
 

### 행위
 

Redux 저장소로 데이터를 보내는 데 사용되는 개체입니다.
 

일반적으로 두 가지 속성, 즉 작업이 수행하는 작업을 설명하는 유형 속성과 앱 상태에서 변경해야하는 정보를 포함하는 페이로드 속성이 있습니다.
 

```js
// action.js
const reduxAction = payload => {
  return {
    type: 'action description',
    payload
  }
};

export default reduxAction;
```

`유형`은 일반적으로 단어가 밑줄로 구분 된 대문자로되어 있습니다.
 예 :`SIGNUP_USER` 또는`DELETE_USER_DATA`.
 

### 감속기
 

이들은 액션 동작을 구현하는 순수 함수입니다.
 현재 애플리케이션 상태를 가져 와서 작업을 수행 한 다음 새 상태를 반환합니다.
 

```js
const reducer = (state, action) => {
  const { type, payload } = action;
  switch(type){
    case "action type":
      return {
        ["action description"]: payload
      };
    default:
      return state;
  }
};

export default reducer;
```

### 저장
 

스토어는 애플리케이션의 상태가있는 곳입니다.
 Redux 애플리케이션에는 하나의 저장소 만 있습니다.
 

```js
import { createStore } from 'redux'

const store = createStore(componentName);
```

애플리케이션은 하나의 Redux 저장소 만 가질 수 있기 때문에 모든 구성 요소에 대해 단일 루트 감속기를 만들려면 Redux의`combineReducers` 메서드가 필요합니다.
 

Redux를 설정하는 데 필요한 긴 프로세스와 상당한 양의 코드로 작업 할 여러 구성 요소가있을 때 코드베이스가 어떻게 보일지 상상해보십시오.
 

Redux가 상태 관리 문제를 해결하더라도 사용하는 데 시간이 많이 걸리고 학습 곡선이 어렵고 애플리케이션에 완전히 새로운 복잡성 계층을 도입합니다.
 

다행히도 React Context API가이 문제를 해결합니다.
 React Hooks와 결합하면 설정 시간이 덜 걸리고 학습 곡선이 더 쉽고 최소한의 코드 만 필요한 상태 관리 솔루션이 있습니다.
 

## React Context API는 무엇입니까?
 

새로운 Context API는 React 16.3과 함께 제공됩니다.
 React Context를 사용하면 현재 인증 된 사용자, 테마 또는 선호하는 언어와 같은 React 구성 요소 트리에 대해 "전역"으로 간주 될 수있는 데이터를 공유 할 수 있습니다.
 

React 문서에서 컨텍스트를 설명하는 방법은 다음과 같습니다.
 

> 컨텍스트는 모든 레벨에서 수동으로 소품을 전달하지 않고도 구성 요소 트리를 통해 데이터를 전달할 수있는 방법을 제공합니다.
 

React Context API는 직접 연결되지 않은 여러 구성 요소에서 상태를 관리하는 React의 방법입니다.
 

컨텍스트를 만들기 위해 React의`createContext` 메서드를 사용합니다.이 메서드는 기본값에 대한 매개 변수를받습니다.
 

```js
import React from 'react';

const newContext = React.createContext({ color: 'black' });
```

`createContext` 메소드는`Provider` 및`Consumer` 구성 요소가있는 객체를 반환합니다.
 

```cpp
const { Provider, Consumer } = newContext;
```

`Provider` 구성 요소는 구성 요소 계층 구조 내에 얼마나 깊이 중첩되어 있든 관계없이 모든 하위 구성 요소에서 상태를 사용할 수 있도록합니다.
 `Provider` 구성 요소는`value` prop을받습니다.
 여기에서 현재 가치를 전달할 수 있습니다.
 

```xml
<Provider value={color: 'blue'}>
  {children}
</Provider>
```

이름에서 알 수 있듯이`Consumer`는 소품 드릴링없이`Provider`의 데이터를 사용합니다.
 

```xml
<Consumer>
  {value => <span>{value}</span>}
</Consumer>
```

Hooks가 없으면 Context API는 Redux와 비교할 때별로 보이지 않을 수 있지만`useReducer` Hook과 결합하면 상태 관리 문제를 마침내 해결하는 솔루션이 있습니다.
 

## React Hooks는 무엇입니까?
 

후크는 기본 코드에서 사용자 지정 코드를 실행할 수 있도록하는 함수 유형입니다.
 React에서 Hooks는 핵심 기능에 "연결"할 수있는 특수 기능입니다.
 

React Hooks는 기능적 구성 요소에서 상태 관리를 쉽게 처리 할 수 있도록하여 클래스 기반 구성 요소 작성에 대한 대안을 제공합니다.
 

### `useContext` 후크
 

아시다시피 React Context API를 설명 할 때 콘텐츠를`Consumer` 구성 요소로 래핑 한 다음 상태에 액세스 (또는 소비) 할 수 있도록 함수를 자식으로 전달해야했습니다.
 

이로 인해 불필요한 구성 요소 중첩이 발생하고 코드의 복잡성이 증가합니다.
 

`useContext` Hook은 일을 훨씬 더 멋지고 간단하게 만듭니다.
 상태에 액세스하려면 생성 된 `컨텍스트`를 인수로 사용하여 상태를 호출하면됩니다.
 

```undefined
const newContext = React.createContext({ color: 'black' });

const value = useContext(newContext);

console.log(value); // this will return { color: 'black' }
```

이제 콘텐츠를`Consumer` 구성 요소로 래핑하는 대신`value` 변수를 통해 간단히 상태에 액세스 할 수 있습니다.
 

### `useReducer` 후크
 

`useReducer` 후크는 React 16.8과 함께 제공됩니다.
 자바 스크립트의`reduce ()`메서드와 마찬가지로`useReducer` 후크는 두 값 (리듀서 함수 및 초기 상태)을 인수로받은 다음 새 상태를 반환합니다.
 

```js
const [state, dispatch] = useReducer((state, action) => {
  const { type } = action;
  switch(action) {
    case 'action description':
      const newState = // do something with the action
      return newState;
    default:
      throw new Error()
  }
}, []);
```

위 블록에서 우리는 우리의 상태와 그에 상응하는 메소드 인`dispatch`를 정의하여 처리했습니다.
 `dispatch` 메서드를 호출하면`useReducer ()`후크는 메서드가 action 인수에서받는`type`에 따라 작업을 수행합니다.
 

```js
...
return (
  <button onClick={() =>
    dispatch({ type: 'action type'})}>
  </button>
)
```

## React Context에서`useReducer` Hook을 사용하는 방법
 

### 우리 가게 설정
 

이제 Context API와`useReducer` 후크가 개별적으로 작동하는 방식을 알았으므로 애플리케이션에 이상적인 글로벌 상태 관리 솔루션을 얻기 위해 결합 할 때 어떤 일이 발생하는지 살펴 보겠습니다.
 `store.js` 파일에 전역 상태를 생성합니다.
 

```js
// store.js
import React, {createContext, useReducer} from 'react';

const initialState = {};
const store = createContext(initialState);
const { Provider } = store;

const StateProvider = ( { children } ) => {
  const [state, dispatch] = useReducer((state, action) => {
    switch(action.type) {
      case 'action description':
        const newState = // do something with the action
        return newState;
      default:
        throw new Error();
    };
  }, initialState);

  return <Provider value={ state, dispatch }>{children}</Provider>;
};

export { store, StateProvider }
```

`store.js` 파일에서는 앞서 설명한`React`의`createContext ()`메서드를 사용하여 새 컨텍스트를 생성했습니다.
 

`createContext ()`메소드는`Provider` 및`Consumer` 구성 요소가있는 객체를 반환합니다.
 이번에는 `Provider`컴포넌트 만 사용하고 상태에 액세스해야 할 때 `useContext`후크를 사용합니다.
 

`StateProvider`에서`useReducer` 후크를 어떻게 사용했는지 주목하세요.
 상태를 조작해야 할 때`dispatch` 메소드를 호출하고 원하는`type`을 인수로 사용하여 객체를 전달합니다.
 

`StateProvider`에서는`useReducer` Hook에서`value` prop과`state` 및`dispatch`를 사용하여`Provider` 구성 요소를 반환했습니다.
 

### 전 세계적으로 상태에 액세스
 

전역 적으로 상태에 액세스하려면`ReactDOM.render ()`함수에서 렌더링하기 전에`<App />`구성 요소를`StoreProvider`에 래핑해야합니다.
 

```js
// root index.js file
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { StateProvider } from './store.js';

const app = (
  <StateProvider>
    <App />
  </StateProvider>
);

ReactDOM.render(app, document.getElementById('root'));
```

이제 우리 스토어 `컨텍스트`는 컴포넌트 트리의 모든 컴포넌트에서 액세스 할 수 있습니다.
 이를 위해`react`에서`useContext` 후크를 가져오고`. / store.js` 파일에서`store`를 가져옵니다.
 

```js
// exampleComponent.js
import React, { useContext } from 'react';
import { store } from './store.js';

const ExampleComponent = () => {
  const globalState = useContext(store);
  console.log(globalState); // this will return { color: red }
};
```

### 상태에서 데이터 추가 및 제거
 

우리는 글로벌 상태에 어떻게 접근 할 수 있는지 보았습니다.
 

상태에서 데이터를 추가하고 삭제하려면 `store`컨텍스트에서 `dispatch`메소드가 필요합니다.
 `dispatch` 메서드를 호출하고`type` (`StateProvider` 구성 요소에 정의 된 작업 설명)을 매개 변수로 사용하여 객체를 전달하기 만하면됩니다.
 

```js
// exampleComponent.js
import React, { useContext } from 'react';
import { store } from './store.js';

const ExampleComponent = () => {
  const globalState = useContext(store);
  const { dispatch } = globalState;

  dispatch({ type: 'action description' })
};
```