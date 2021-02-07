---
layout: post
title: "클래스 대신 React Hooks를 사용해야 하는 이유"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/reacthooksoverclasses.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/reacthooksoverclasses.png?fit=730%2C487&ssl=1)

이 게시물에서는 React 버전 16에 소개된 React Hooks를 사용하는 기능 구성 요소와 클래스 구성 요소에서 이를 사용해야 하는 이유에 대해 알아보겠습니다. 이 게시물은 수업에 익숙한 React 개발자와 어떤 것을 사용할지 고민하는 새로운 React 개발자에게 적합합니다.

## 리액트 훅이란?

공식 문서에 따르면, Hooks는 이전에 클래스 구성 요소에서 접근할 수 있는 모든 전원을 기능 구성 요소로 가져와 기능 구성 요소에서 사용할 수 있도록 하였다. 이제 React Hooks를 사용하면 클래스 구성 외부에서 React의 상태 및 기타 기능을 사용할 수 있습니다.

```js
import React, { useState } from 'react';
function Example() {
  // Declare a new state variable, which we'll call "count"
const [count, setCount] = useState(0);
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

useState는 React 기능 구성 요소에서 사용할 수 있는 후크 중 하나입니다. 훅스는 지난해 출시됐지만 파격적인 변화가 없는 등 몇 가지 부가적인 장점을 앞세워 출하했다. 즉, 완전히 역호환성이며 사용할 수 있는 옵션입니다. 즉, 원하는 경우 클래스를 고수하거나 후크를 사용해 볼 수 있습니다.

## 수업 시간보다 후크를 고려해야 하는 이유

Hooks의 도입으로 이제 기능 구성 요소 내에서 수업 중에 일반적으로 수행하는 모든 작업을 수행할 수 있습니다. 이것은 여러분이 배울 개념과 쓸 줄의 코드가 적기 때문에 게임 체인지입니다. 작년 이전에는 리액트 개발자들이 이미 수업을 이용했기 때문에 한동안 사용하던 것을 새로운 것에 맡기기가 쉽지 않을 수 있지만, 후크는 선택 사항이기 때문에, 당신은 어떠한 블로백 없이 그것을 시도해 볼 수 있다.

## 기능 구성 요소의 라이프사이클 방법

리액션 개발자들은 클래스 구성 요소를 사용하면 실제로 수명 주기 방법을 피할 수 없다는 것을 알고 있는데, `componentDidMount`는 가장 인기 있는 방법 중 하나이다. 첫 번째 구성요소 렌더링을 실행한 후 실행됩니다. 이 LogRocket 블로그에 대한 헤더를 반환할 클래스 구성 요소를 생성하겠습니다.

```js
class NewComponent extends React.Component {
 componentDidMount() {
   console.log("The first render has executed!");
 }

 render() {
   return <div> <h3>You are reading a LogRocket article</h3> </div>;
 }
}
```

이제 이 작업을 실행한 후 이미 마운트된 이 구성 요소를 제거하려면 `구성 요소 WillUnmount` 라이프사이클을 포함해야 합니다. 이제 React Hooks 덕분에 기능 구성 요소 내에서 이와 동일한 라이프사이클 방법을 사용할 수 있습니다. 이는 혁신적인 결과라고 생각합니다.

```js
const FunComponent = () => {
 React.useEffect(() => {
   console.log("The first render has executed!");
 }, []);
 return <div> <h3>You are reading a LogRocket article</h3> </div>;
};
```

위의 이 코드 블록은 `UseEffect`라는 리액트 후크를 사용하며 빈 어레이 두 번째 인수를 사용하여 위에서 정의한 클래스 구성 요소와 동일한 출력을 반환합니다.

## 기능 구성 요소의 상태 관리

이 질문은 Hooks가 기능 구성 요소에서 라이프사이클 방법처럼 작동할 수 있다는 것을 확인한 후 물어본 다음 질문이며, 이제 Hooks 덕분에 기능 구성 요소에서 상태를 처리할 수 있습니다. 100에서 100까지 카운트다운하는 데 사용되는 간단한 클릭자 클래스에서 상태를 구현하려면 다음과 같이 해야 합니다.

```js
class NewComponent extends React.Component {
 constructor(props) {
   super(props);
   this.state = {
     counter: 100
   };
 }

 render() {
   return (
     <>
       <p>counter: {this.state.counter} counts left</p>
       <button onClick={() => this.setState({ counter: this.state.counter - 1 })}>
         Click
       </button>
     <>
   );
 }
}
```

이제 여러분은 상태 저장 논리를 위한 초특급 소품들을 가지고 있어야만 한다는 것을 알고 있습니다. 이제 기능적인 요소들로 이것을 성취하기 위해서는 다음과 같이 보입니다.

```js
const FunComponent = () => {
 const [counter, setCounter] = React.useState(100);

 return (
   <>
     <p>{counter} more counts to go!</p>
     <button onClick={() => setCounter(counter - 1)}>Click</button>
   <>
 );
};
```

useState Hook은 기능 구성 요소에서 상태 저장 로직을 처리하는 데 사용되며 구문도 보기처럼 매우 쉽습니다. 다음은 클래스 대신 기능 구성 요소를 작성할 수 있는 몇 가지 다른 이유입니다.

## 코드 줄 수 감소

따라서 기능 구성 요소를 사용하면 클래스 구성 요소의 등가선에 비해 평균적으로 적은 수의 코드 행을 쓸 수 있습니다. 다음은 클래스 구성 요소가 있는 작은 머리글을 포함하는 디바이스를 반환하는 작은 예입니다.

```js
import React, { Component } from "react";
class NewComponent extends Component {
 render() {
   return <div> <h3>You are reading a LogRocket article</h3> </div>;
 }
}
```

기능 구성 요소의 경우 다음과 같습니다.

```js
import React from "react";

const FunComponent = () => {
 return <div> <h3>You are reading a LogRocket article</h3> </div>;
};
```

## 더 쉬운 학습 곡선

위의 예에서는 클래스 구성 요소보다 기능 구성 요소에서 배우거나 이해해야 할 개념이 더 적다는 것을 알 수 있습니다. 함수 컴포넌트는 JSX만 반환하는 간단한 함수이지만 클래스 컴포넌트의 측면에서는 렌더링 방법과 `Ract.component`를 확장하기 위해 어떻게 사용되는지 이해해야 합니다.

## "이것"을 다시 사용할 필요가 없습니다.

이것은 잘 논의된 사항이지만, Retact 팀조차도 자바스크립트에서의 클래스 사용이 때때로 개발자와 심지어 기계에게 혼란스러울 수 있다는 것을 확인시켜준다. 헷갈리는 것 중 하나는 속성 전달이나 언어별 동작 방식 등에 이것을 사용하는 것이다. 예를 들어, 제목을 가진 머리글 구성 요소가 있다고 가정해 보십시오.

```xml
<Heading title="blog" />
```

이 제목 속성을 전달하면 기능 구성 요소와 클래스 구성 요소 모두에 대해 매우 고유한 구문이 있습니다. 클래스 구성 요소의 경우 코드 블록은 다음과 같습니다.

```js
class NewComponent extends React.Component {
  render() {
    const {title} = this.props;
    return <h3>You are reading a LogRocket {title}</h3>;
 }
}
```

우리는 "당신이 읽고 있는 로그로켓 블로그"를 렌더링 시 출력하기 위해 "this.props"에 바인딩해야 했지만, 동일한 결과를 얻기 위해 기능 구성요소를 사용하면서 이 문제에 대해 걱정할 필요는 없습니다.

```js
const FunComponent = ({ title }) => {
 return <h3>You are reading a LogRocket {title}</h3>;
};
```

## 결론

이 게시물은 React Hooks의 개요와 클래스 구성 요소 대신 기능 구성 요소에 사용해야 하는 이유에 대한 설명입니다. React Hooks에 대해 자세히 알아보려면 공식 설명서를 참조하고 Hooks를 통해 수업에서 기능 구성 요소로 앱을 리팩터링하는 방법을 배우고 싶다면 이 훌륭한 기사를 확인하십시오. 나는 네가 도움이 되고, 행복한 해킹을 발견하길 바래!