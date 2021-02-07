---
layout: post
title: "기본 소품 대응: 전체 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2018/08/complete-guide-default-props-react.jpeg"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/08/complete-guide-default-props-react.jpeg?fit=730%2C486&ssl=1)

React는 서버, 웹, 모바일, 데스크톱 등의 다양한 플랫폼에서 실행할 수 있는 확장 가능한 애플리케이션을 구축하기 위한 매우 강력한 구성요소 기반 JavaScript 프레임워크입니다. 오늘날 이러한 플랫폼에서 실행되는 수천 개의 애플리케이션은 React를 기반으로 구축됩니다.

React의 놀라운 기능으로는 린 프레임워크, 가상 DOM, JSX 지원, 코드 재사용이 있습니다. 이 문서에서 반응에 대해 자세히 알아볼 수 있습니다.

이 가이드에서는 매우 기본적인 수준에서 반응 구성 요소의 기본 소품 설정에 대해 알아야 할 모든 사항을 공개합니다. 그것은 주로 리액트 프레임워크의 신입들을 위한 것이다. 따라서, 리액션에 대한 기본적인 지식이 필요하다.

그러나 React를 꽤 오랫동안 사용해 온 개발자가 여전히 이 가이드의 일부에 대해 통찰력이 있다고 생각할 수 있습니다.

이 안내서의 스크린샷에는 몇 가지 기본 Bootboot 4 CSS 스타일링과 함께 렌더링된 뷰가 나와 있습니다. 유사한 결과를 얻으려면 몇 가지 추가 부트스트랩 스타일을 사용하여 코드 조각들을 실행해야 합니다.

## 반응 구성 요소란 무엇입니까?

반응 앱은 일반적으로 응용 프로그램의 UI를 구성하는 몇 가지 독립 구성 요소로 구성됩니다. 반응 구성 요소는 반응 응용 프로그램의 구성 요소입니다.

리액트 컴포넌트(Retact component)는 소품으로 알려진 임의의 입력의 객체를 가져와서 UI에서 렌더링해야 하는 것을 설명하는 리액트 요소를 반환하는 자바스크립트 함수이다.

```js
// Simple React Component
function ReactHeader(props) {
  return <h1>React {props.version} Documentation</h1>
}
```

이 코드 조각은 지정된 React 버전의 설명서에서 제목을 포함하는 `<h1> 요소를 렌더링하는 매우 간단한 `ReactHeader` 구성 요소를 정의합니다. JSX(JavaScript XML) 구문을 사용하여 선언적 방식으로 구성 요소의 DOM 요소 계층을 생성합니다. 여기서 반응과 함께 JSX를 사용하는 방법에 대해 자세히 알아볼 수 있습니다.

JSX가 없으면 이전 코드 조각은 다음과 같이 작성됩니다.

```js
// Simple React Component (without JSX)
function ReactHeader(props) {
  return React.createElement('h1', null, `React ${props.version} Documentation`);
}
```

React를 사용하는 데 JSX가 필요하지 않습니다. 예를 들어, 컴파일 형식 없이 React를 사용하려는 경우 JSX를 사용할 수 없습니다.

실제로 React 구성 요소의 모든 JSX는 구성 요소가 렌더링되기 전에 해당 `createElement` 등가로 컴파일됩니다. 그러나 이 가이드에서는 가능한 모든 코드 조각에 JSX가 사용됩니다.

이전 코드 조각에서, ReactHeader 구성 요소에서 `version` proput을 그것에 전달해야 한다는 것은 꽤 명백하다.

ReactHeader` 구성 요소는 다음과 같이 DOM(임의 ``요소 내부)에서 렌더링할 수 있습니다.

```js
// Render a React Component
ReactDOM.render(, document.getElementById('root'));
```

여기에서 리액트 헤더를 버전 16으로 설정한 상태로 렌더링했습니다. 현재 리액트헤더 구성 요소에는 다음 스크린샷과 같이 모든 것이 제대로 작동하는 것으로 보인다.

## 반응에서 기본 소품 사용

그러면 버전 소품이 전달되지 않으면 어떻게 될까요?

```js
// Render the ReactHeader Component
ReactDOM.render(, document.getElementById('root'));
```

아마 짐작이 옳았을 거예요. ReactHeader 구성 요소가 `version` propert 없이 렌더링되면 다음과 같은 상황이 발생합니다.

`버전` 소품이 통과되지 않았기 때문에 `props`에 대한 언급이 있다.구성 요소의 버전은 `➡`이므로 위의 스크린샷입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/0_REmjO3vBUL7tHrSD.png?resize=730%2C144&ssl=1)

이 문제를 해결할 수 있는 한 가지 방법은 조건부 렌더링을 적용하는 것입니다. 다음 조각에 표시된 것처럼 필요한 소품이 전달되지 않거나 유효하지 않을 때마다 구성 요소가 렌더링되지 않도록 방지할 수 있습니다.

```js
// Simple React Component
function ReactHeader(props) {
  return (
    Number.isFinite(props.version)
      ? <h1>React {props.version} Documentation</h1>
      : null
  );
}
```

이 문제를 해결할 수 있는 또 다른 방법은 구성 요소의 기본 소품을 설정하는 것입니다. React 문서에 따르면, "defaultProps"는 클래스의 기본 소품을 설정하기 위해 구성요소 클래스 자체의 속성으로 정의할 수 있습니다.

기본적으로 구성 요소를 약간 조정하여 `version` prop가 전달되지 않을 때마다 기본값을 사용할 수 있습니다.

여기 있습니다:

```js
// With JSX
function ReactHeader(props) {
  return <h1>React {props.version || 16} Documentation</h1>
}

// OR
// Without JSX
function ReactHeader(props) {
  return React.createElement('h1', null, `React ${props.version || 16} Documentation`);
}
```

여기서 논리 OR(`||) 연산자는 `버전` 받침이 통과되지 않을 때마다 폴백 값을 설정하는 데 사용됩니다. `version` 소품에 대해 기본값 `16`이 설정되었습니다. 이러한 변화로 모든 것이 예상대로 작동합니다.

본 가이드에서는 다양한 맛의 React 구성 요소에 대한 기본 소품을 설정하는 다양한 방법에 대해 설명합니다.

- React.createClass() API 사용
- 클래스 구성 요소
- 기능 구성 요소
- 고차 구성 요소 사용

## React에서 'Ract.createClass()' 사용

React에서 클래스는 구성 요소 내부에서 상태를 유지해야 하거나 구성 요소의 수명 주기 방법을 활용하려는 경우 상태 저장 구성 요소를 구축하는 데 가장 적합합니다.

React가 처음 출시되었을 때, 자바스크립트에서는 클래스가 실제로 존재하지는 않았다. 따라서 자바스크립트에서 클래스를 만들 방법이 없었다.

그러나 React는 클래스 유사 구성 요소를 생성하기 위한 React.createClass() API와 함께 제공합니다. 시간이 지남에 따라, 이 API는 더 이상 사용되지 않게 되었고 마침내 ES6 클래스를 선호하여 React에서 제거되었다.

`15.5.0` 이전의 React 버전을 사용하는 경우 다음과 같이 `React.createClass()` API를 사용하여 간단한 React 구성 요소를 생성할 수 있습니다.

```js
import React from 'react';

/**
 * ThemedButton Component
 * Using React.createClass()
 *
 * Renders a Bootstrap themed button element.
 */
 
const ThemedButton = React.createClass({

  // Component display name
  displayName: 'ThemedButton',
  
  // render() method
  render() {
    const { theme, label, ...props } = this.props;
    return { label }
  }
  
});
```

이 코드 조각은 `Ract.createClass() API를 사용하여 매우 간단한 `MemeButton` 구성 요소를 생성합니다. 이 구성 요소는 전달된 소품을 기반으로 부트스트랩 테마 버튼을 렌더링합니다.

또한 버튼을 올바르게 렌더링하려면 테마 소자와 레이블 소자가 전달되어야 합니다.

이제 Return 앱에서 다음과 같이 테마 버튼 세트를 렌더링할 수 있습니다.

```coffeescript
import React from 'react';
import ReactDOM from 'react-dom';

// [...ThemedButton component here]

function App(props) {
  return (
    <div>
      <ThemedButton theme="danger" label="Delete Item" />
      <ThemedButton theme="primary" label="Create Item" />
      <ThemedButton theme="success" label="Update Item" />
      <ThemedButton theme="warning" label="Add to Cart" />
      <ThemedButton />
    </div>
  );
}

const rootElement = document.getElementById('root');
ReactDOM.render(<App />, rootElement);
```

테마 버튼 구성 요소는 앱에서 다섯 번 렌더링되었습니다. 다섯 번째, `테마 버튼`은 어떤 소품도 통과되지 않는다. 다음은 앱의 모양에 대한 스크린샷입니다.

위의 스크린샷에서 테마와 라벨 소품 없이 렌더링되기 때문에 다섯 번째 버튼이 시각적으로 보기에 표시되지 않는다는 것을 알 수 있다. 이에 따라 테마 버튼 구성 요소에 대한 기본 소품을 설정할 필요가 있다.

React.createClass() API를 사용하여 만든 구성 요소의 경우 객체 리터럴에 getDefaultProps라는 메서드를 추가하여 기본 소품을 설정할 수 있습니다.

GetDefaultProps() 메서드는 구성 요소의 기본 소품 집합을 나타내는 개체를 반환해야 합니다. 여기 있습니다:

```js
const ThemedButton = React.createClass({

  // Component display name
  displayName: 'ThemedButton',
  
  // render() method
  render() {
    const { theme, label, ...props } = this.props;
    return <button className={`btn btn-${theme}`} {...props}>{ label }</button>
  },
  
  // Set default props
  getDefaultProps() {
    return {
      theme: "secondary",
      label: "Button Text"
    };
  }
  
})
```

이 코드 조각에서 `MeedButton` 구성 요소에 대한 기본 소품이 설정되었습니다. 테마는 전달되지 않으면 보조로 기본 설정되며 레이블은 버튼 텍스트로 기본 설정됨

기본 소품을 설정하면 앱이 다음과 같은 스크린샷으로 표시됩니다.

## 클래스 구성 요소 사용

최신 버전의 React에서는 ES6 클래스 구문을 활용하여 클래스 구성 요소를 생성할 수 있습니다. 이것이 ES6 클래스 구문을 사용하는 테마 버튼 구성 요소의 모습입니다.

```js
import React, { Component } from 'react';

class ThemedButton extends Component {

  // render() method
  render() {
    const { theme, label, ...props } = this.props;
    return <button className={`btn btn-${theme}`} {...props}>{ label }</button>
  }

}
```

ES6 클래스 구문을 사용하여 만든 React 구성 요소의 경우 구성 요소 클래스에 `defaultProps`라는 정적 속성을 추가하여 기본 소품을 설정할 수 있습니다.

기본Props 정적 속성은 구성 요소의 기본 소품을 나타내는 개체로 설정해야 합니다.

이 작업은 다음 코드 조각에 표시된 것처럼 클래스 본문 외부에 있는 구성 요소 클래스 자체에서 `defaultProps`를 정의하여 수행할 수 있습니다.

```java
class ThemedButton extends React.Component {
  render() {
    // ...implement render method
  }
}

// Set default props
ThemedButton.defaultProps = {
  theme: "secondary",
  label: "Button Text"
};
```

ECMA 스크립트 사양에 정적 클래스 속성 및 메서드를 추가하면 다음 조각에 표시된 것처럼 `defaultProps`를 지정할 수 있습니다.

```java
class ThemedButton extends React.Component {
  render() {
    // ...implement render method
  }
  
  // Set default props
  static defaultProps = {
    theme: "secondary",
    label: "Button Text"
  }
}
```

## 반응의 기능 구성 요소

반응에서 함수 구문은 상태나 라이프사이클을 추적하지 않고 요소를 렌더링하는 구성 요소에 적합합니다. 이러한 구성 요소를 일반적으로 기능 구성 요소 또는 상태 비저장 기능 구성 요소라고 합니다.

상태 비저장 기능 구성 요소로 다시 기록될 때 `MeedButton` 구성 요소는 다음과 같습니다.

```js
import React from 'react';

function ThemedButton(props) {
  const { theme, label, ...restProps } = props;
  return <button className={`btn btn-${theme}`} {...restProps}>{ label }</button>
}
```

클래스 구성요소와 마찬가지로, 구성요소 기능 자체에 `defaultProps`라는 정적 속성을 추가하여 기능 구성요소에 기본 소품을 설정할 수 있습니다.

```js
function ThemedButton(props) {
  // ...render component
}

// Set default props
ThemedButton.defaultProps = {
  theme: "secondary",
  label: "Button Text"
};
```

또는 ES6 객체 파괴 구문을 사용하여 기본값이 있는 기능 구성 요소의 소품을 파괴할 수 있습니다. ES6 파괴에 대한 자세한 내용은 이 기사를 참조하십시오.

다음은 `테마 버튼` 구성 요소의 구조화된 소품입니다.

```js
import React from 'react';

// METHOD 1:
// Default Props with destructuring
function ThemedButton(props) {
  const { theme = 'secondary', label = 'Button Text', ...restProps } = props;
  return <button className={`btn btn-${theme}`} {...restProps}>{ label }</button>
}

// METHOD 2:
// More compact destructured props
function ThemedButton({ theme = 'secondary', label = 'Button Text', ...restProps }) {
  return <button className={`btn btn-${theme}`} {...restProps}>{ label }</button>
}
```

## 고차 구성 요소 사용

반응에서 HOC(High-order component)는 기본적으로 반응 성분을 인수로 받아들여 다른 반응 성분(대개 원본의 향상 요소)을 반환하는 함수이다.

고차 구성 요소는 구성 요소 구성에 매우 유용하며 React 구성 요소와 함께 사용할 수 있는 고차 구성 요소(재구성 중인 매우 유명한 구성 요소)를 제공하는 패키지가 많이 있습니다.

Recompose는 React 구성 요소와 함께 사용할 수 있는 고차 구성 요소의 풍부한 모음입니다. 그것은 리액트를 위한 로다쉬에 가깝다. 이 참조에서 Recompose를 통해 제공되는 고차 구성 요소 및 API에 대해 자세히 알아볼 수 있습니다.

이제 다음 명령을 실행하여 프로젝트의 종속성으로 Recompose를 설치할 수 있습니다.

```undefined
npm install recompose --save
```

Recompose는 전달된 모든 React 구성 요소에 지정된 기본 소품을 설정하고 수정된 React 구성 요소를 반환하는 고차 구성 요소를 반환하는 `defaultProps` 함수를 내보냅니다.

다음은 Recompose에서 기본 Props 고차 구성 요소를 사용하여 `MemeButton` 구성 요소를 다시 쓰는 방법입니다.

```js
import React from 'react';
import { defaultProps } from 'recompose';

// React Component
function ThemedButton(props) {
  const { theme, label, ...restProps } = props;
  return <button className={`btn btn-${theme}`} {...restProps}>{ label }</button>
}

// Default Props HOC
const withDefaultProps = defaultProps({
  theme: "secondary",
  label: "Button Text"
});

// Enhanced Component with default props
ThemedButton = withDefaultProps(ThemedButton);
```

## 결론

기본 소품을 사용하면 React 구성 요소를 크게 개선할 수 있습니다. 본 가이드에서는 React 응용 프로그램에서 제공하는 다양한 맛의 React 구성 요소에 대한 기본 소품을 설정하는 몇 가지 방법에 대해 다룹니다.