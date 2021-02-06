---
layout: post
title: "반응 스타일링: 반응 앱을 스타일링하는 3가지 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2017/12/react-styling.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2017/12/react-styling.png?fit=730%2C487&ssl=1)

리액트 튜토리얼에 스타일링이 별로 없을 수도 있지만, 이 튜토리얼이 가장 좋은 튜토리얼입니다. 왜요, 물어봐요? 먼저, React 구성 요소를 스타일링하는 세 가지 방법을 검토하겠습니다. 둘째, 내가 그렇게 말하니까 (그리고 편집자는 내가 예술적 자유를 누릴 수 있다고 말한다)

당신이 이 글을 읽으면서 당신이 여기 와서 리액션 컴포넌트 스타일링에 대해 알게 된 것을 알 수 있습니다. 당신이 이 글을 다 읽으면, 나는 당신이 그것을 어떻게 하는지 알 수 있을 것이라고 장담한다.

## 무엇을 기대해야 하는가?

이 기사에서는 React 구성 요소를 스타일링할 수 있는 다양한 방법을 살펴보겠습니다. 지금부터 살펴보는 스타일링 방법은 다음과 같습니다.

- 인라인 스타일링
- 스타일리쉬한 요소.
- CSS 모듈

우리는 이러한 각 방법을 설명하기 위해 해야 할 애플리케이션의 일부인 구성 요소를 사용할 것이다.

반응을 처음 접한 경우 시작할 공식 문서를 확인할 수 있습니다.

## 대응 프로그램 설정

응용 프로그램을 설정하려면 응용 프로그램 만들기-응답-앱을 사용할 수 있습니다. 리액트 프로젝트를 시작하는 가장 쉬운 방법입니다. 그러나 CRA 데모는 기사의 범위를 벗어났기 때문에 이를 생략하고 신뢰할 수 있는 실행 응용 프로그램을 스타일링합니다.

첫 번째 방법부터 시작하겠습니다.

## 방법 #1: 반응 성분의 인라인 스타일링

기본 HTML에 익숙하다면 CSS를 인라인으로 추가할 수 있다는 것을 알게 될 것입니다. 이것은 반응에서도 비슷하다.

렌더링하려는 React 구성 요소에 인라인 스타일을 추가할 수 있습니다. 이러한 스타일은 속성으로 작성되고 요소에 전달됩니다. 인라인 스타일을 사용하여 구성 요소의 일부를 스타일링해 보겠습니다.

```coffeescript
class ToDoApp extends React.Component {
  // ...
  render() {
    return (
      <div style={ backgroundColor: "#44014C", width: "300px", minHeight: "200px"}>
        <h2 style={ padding: "10px 20px", textAlign: "center", color: "white"}>ToDo</h2>
        <div>
          <Input onChange={this.handleChange} />
          <p>{this.state.error}</p>
          <ToDoList value={this.state.display} />
        </div>
      </div>
    )
  }
}
```

그래서 우리는 가장 바깥쪽의 div와 h2에 인라인 스타일을 추가했다. 여기에 이것에 대해 주의해야 할 몇 가지 사항이 있습니다.

먼저, 두 개의 곱슬곱슬한 괄호가 있습니다. 우리가 렌더링하는 것은 JSX로 작성되며, JSX에서 사용되는 순수한 자바스크립트 식을 위해서는 그것들은 곱슬곱슬한 괄호 안에 포함되어야 한다.

첫 번째 컬리 브래킷은 JSX에 JavaScript를 주입한다. 내부 꼬불꼬불한 괄호는 객체 리터럴을 작성합니다. 스타일은 요소에 객체 리터럴로 전달됩니다.

> 💡 JSX는 JavaScript에 XML 구문을 추가하는 전처리 단계입니다. JSX 없이도 React를 사용할 수 있지만, JSX는 React를 훨씬 더 우아하게 만듭니다. XML과 마찬가지로 JSX 태그에는 태그 이름, 속성 및 하위 태그가 있습니다.

다음으로 주의해야 할 점은 속성이 쉼표로 구분된다는 것입니다. 이것은 우리가 지나가고 있는 것이 물건이기 때문이다. 자바스크립트 속성이기 때문에 속성은 camelCase로 작성되고 대시로 구분되지 않습니다.

위의 코드에서 우리는 우리가 스타일링한 요소들에 몇 가지 속성을 추가했습니다. 하지만, 우리가 요소에 점점 더 많은 스타일을 추가해야 한다고 생각해보세요. 여기가 바로 인라인 방식이 깔끔해 보이지 않아 고장이 나는 곳이다.

하지만 이 일에는 방법이 있다. 객체 변수를 만들어 요소에 전달할 수 있습니다. 그럼 그렇게 합시다.

### 스타일 객체 변수 생성

JavaScript 객체를 생성하는 것과 동일한 방식으로 스타일 객체 변수를 생성합니다. 그런 다음 이 개체가 스타일 지정하려는 요소의 스타일 속성으로 전달됩니다.

따라서 이전 예에서와 같이 인라인 방식으로 스타일을 추가하는 대신 객체 변수를 전달합니다.

```js
const TodoComponent = {
  width: "300px",
  margin: "30px auto",
  backgroundColor: "#44014C",
  minHeight: "200px",
  boxSizing: "border-box"
}

const Header = {
  padding: "10px 20px",
  textAlign: "center",
  color: "white",
  fontSize: "22px"
}

const ErrorMessage = {
  color: "white",
  fontSize: "13px"
}

class ToDoApp extends React.Component {
  // ...
  render() {
    return (
      <div style={TodoComponent}>
        <h2 style={Header}>ToDo</h2>
        <div>
          <Input onChange={this.handleChange} />
          <p style = {ErrorMessage}>{this.state.error}</p>
          <ToDoList value={this.state.display} />
        </div>
      </div>
    )
  }
}
```

위의 코드에서 우리는 `ToDoComponent`, `Header`, `ErrorMessage`의 세 가지 객체 변수를 생성했다. 그런 다음 이러한 변수를 직접 입력하는 대신 요소에 전달합니다.

> 💡 이 변수들은 객체 자체이기 때문에 우리는 요소에서 이중 곱슬 브래킷을 사용할 필요가 없었습니다.

개체 속성을 보면 컴파일 중에 camelCase가 대시 구분 CSS 속성으로 변환됩니다. 예를 들어 다음과 같습니다.

```undefined
backgroundColor: "#44014C",
minHeight: "200px",
boxSizing: "border-box"
```

일반 CSS에서는 다음과 같이 작성됩니다.

```undefined
background-color: #44014C;
min-height: 200px;
box-sizing: border-box;
```

> ⚠는 camelCase to dash로 구분된 문자열 변경 사항을 속성 값이 아닌 속성 이름에만 적용합니다.

변수를 속성 값으로 전달할 수 있습니다. 이렇게 할 수 있습니다.

```cpp
const spacing = "10px 20px";
const Header = {
margin: spacing,
padding: spacing
// ...
}
```

대부분의 JavaScript 환경에서는 전역 개체 변수를 생성하는 것이 나쁠 수 있지만 React에서는 괜찮습니다. 파일을 가져오지 않으면 다른 파일에 표시되지 않으므로 충돌 없이 동일한 이름으로도 개체 변수를 얼마든지 만들 수 있습니다.

### 여러 반응 구성 요소 간에 스타일 공유

스타일 개체와 구성 요소는 동일한 파일에 있을 필요가 없습니다. 스타일에 대해 별도의 .js 파일을 생성하고 이러한 스타일을 내보낸 다음 사용할 구성 요소로 가져올 수 있습니다. 이렇게 하면 여러 구성 요소에서 스타일을 재사용할 수 있습니다. 우리 부품을 위해 이렇게 합시다.

먼저 `style.js`라는 별도의 .js 파일을 생성하겠습니다. 그런 다음 다음 스타일을 추가합니다.

```cpp
const TodoComponent = {
  width: "300px",
  margin: "30px auto",
  backgroundColor: "#44014C",
  minHeight: "200px",
  boxSizing: "border-box"
}

const Header = {
  padding: "10px 20px",
  textAlign: "center",
  color: "white",
  fontSize: "22px"
}

const ErrorMessage = {
  color: "white",
  fontSize: "13px"
}

const styles = {
  TodoComponent: TodoComponent,
  Header: Header,
  ErrorMessage: ErrorMessage
}
```

위의 코드에서는 각 스타일 객체를 개별적으로 내보내도록 선택할 수 있지만, 이는 해당 객체를 개별적으로 가져오는 것을 의미하기도 합니다. 파일에 여러 가지 스타일 개체가 있으면 지루해질 수 있습니다.

따라서 모든 스타일을 포함하는 개체를 만드는 것이 합리적입니다. 이 개체는 개체를 사용할 구성 요소로 한 번 내보내고 가져옵니다. 자, 그렇게 합시다.

```xml
import React, { Component } from "react";

// Import the styles
import {styles} from "./styles";

class ToDoApp extends React.Component {
  // ...
  render() {
    return (
      <div style={styles.TodoComponent}>
        <h2 style={styles.Header}>ToDo</h2>
        <div>
          <Input onChange={this.handleChange} />
          <p style = {styles.ErrorMessage}>{this.state.error}</p>
          <ToDoList value={this.state.display} />
        </div>
      </div>
    )
  }
}
```

4호선은 스타일 객체를 가져오는 곳입니다. 이 개체는 React 앱의 구성 요소를 스타일링하는 데 사용되며 다른 JavaScript 개체와 마찬가지로 사용됩니다.

여기서 짚고 넘어가야 할 점은 스타일을 여러 가지 구성요소에서 사용하고 재사용할 수 있다는 것입니다. 스타일을 가져와 스타일 속성에 추가하면 됩니다.

물론 인라인 스타일링을 사용하면 안 되는 상황도 있고, 그 두 가지 방법이 나올 수 있습니다.

## 방법 #2: 스타일 구성 요소

스타일링 컴포넌트로, 우리는 자바스크립트 파일에 실제 CSS를 쓸 수 있다. 즉, 미디어 쿼리, 의사 선택기, 중첩 등과 같은 CSS의 모든 기능을 JavaScript에서 사용할 수 있습니다.

스타일 구성 요소는 ES6의 태그가 지정된 템플릿 리터럴을 사용하여 구성 요소를 스타일링합니다. 이 기능을 사용하면 구성 요소와 스타일 간의 매핑이 제거됩니다. 즉, 스타일을 정의할 때 실제로 스타일이 연결된 일반 반응 구성 요소를 만듭니다.

스타일 구성 요소를 사용하여 스타일이 포함된 재사용 가능한 구성 요소를 생성할 수 있습니다. 그것은 창조하고 사용하는 것이 꽤 흥미롭다. 예를 들어 설명하는 것이 우리에게 많은 도움이 될 것입니다.

먼저 이 프로그램을 설치해야 하므로 Retact 앱 디렉토리에서 다음을 실행하십시오.

```shell
$ npm install --save styled-components
```

이제 작업관리 앱으로 돌아가서 컴포넌트가 스타일링된 컴포넌트를 사용하도록 합시다. 먼저 `스타일 구성 요소` 패키지를 가져오겠습니다.

```coffeescript
import styled from 'styled-components';
```

바로 사용할 수 있어요. 먼저 스타일링된 구성 요소를 생성한 다음 이를 어떻게 사용할지 확인합니다.

```undefined
const TodoComponent = styled.div`
background-color: #44014C;
width: 300px;
min-height: 200px;
margin: 30px auto;
box-sizing: border-box;
`;
```

위에서 우리는 다른 React 구성 요소와 동일하게 사용할 수 있는 구성 요소를 만들었습니다. 하지만, 우리는 자바스크립트 파일에서 순수 CSS를 사용하고 있다는 것을 알아두세요. 다음으로 이 구성 요소를 사용해 보겠습니다.

```xml
class ToDoApp extends React.Component {
  // ...
  render() {
    return (
      <TodoComponent>
        <h2>ToDo</h2>
        <div>
          <Input onChange={this.handleChange} />
          <p>{this.state.error}</p>
          <ToDoList value={this.state.display} />
        </div>>
      </TodoComponent>
    )
  }
}
```

위의 코드에서는 다른 HTML 요소를 사용할 것처럼 5번 라인에서 만든 스타일 구성 요소인 `TodoComponent`를 사용했습니다. 유일한 차이점은 미리 정의된 스타일이 함께 제공된다는 것입니다.

구성 요소의 다른 부분에서도 동일한 작업을 수행할 수 있습니다.

```js
import styled from 'styled-components';

const TagComponent = styled.div`
  background-color: #44014C;
  width: 300px;
  min-height: 200px;
  margin: 30px auto;
  box-sizing: border-box;
`;

const Header = styled.h2`
  padding: 10px 20px;
  text-align: center;
  color: white;
  fontSize: 22px
`;

const ErrorMessage = styled.p`
  color: white;
  font-size: 13px;
`;

class ToDoApp extends React.Component {
  // ...
  render() {
    return (
      <TagComponent>
        <Header>ToDo</Header>
        <div>
          <Input onChange={this.handleChange} />
          <ErrorMessage>{this.state.error}</ErrorMessage>
          <ToDoList value={this.state.display} />
        </div>
      </TagComponent>
    )
  }
}
```

스타일링된 구성 요소와 해당 구성 요소의 사용 방법에 대한 자세한 내용은 여기서 공식 설명서를 참조하십시오.

이제 React에서 세 번째이자 마지막 스타일링 방법에 대해 알아보겠습니다.

## 방법 #3: CSS 모듈

CSS Module은 기본적으로 모든 클래스 이름과 애니메이션 이름의 범위가 로컬에서 지정되는 CSS 파일입니다. 로컬에서 범위 지정 단어를 기록해 두십시오. 그것을 조금 분해해 봅시다.

CSS 클래스 이름 및 애니메이션 이름은 기본적으로 전역 범위로 지정됩니다. 이 경우 특히 큰 스타일시트에서 충돌이 발생할 수 있으며, 한 스타일이 다른 스타일보다 우선할 수 있습니다. 이것이 CSS 모듈이 해결하는 문제입니다. CSS 클래스는 CSS 클래스가 사용되는 구성 요소 내에서만 사용할 수 있습니다.

CSS 모듈은 기본적으로 컴파일된 .css 파일입니다. 컴파일하면 두 개의 출력이 생성됩니다. 하나는 CSS인데, CSS의 수정된 버전으로 클래스 이름이 바뀐 것이다. 다른 하나는 원래 CSS 이름을 이름이 변경된 이름으로 매핑하는 JavaScript 개체입니다.

어떻게 작동하는지 예를 들어 보겠습니다. 하지만, 만약 여러분이 더 깊이 파고 싶다면, 그것에 대한 이 기사를 확인해 보세요. 네, 샘플 오류 메시지에 대한 CSS 클래스를 모듈에 생성하겠습니다. 저희 모듈의 이름은 `style.css`입니다.

```css
.error-message {
color: red;
font-size: 16px;
}
```

이를 통해 다음과 같은 결과를 얻을 수 있습니다.

```css
.error-message_jhys {
color: red;
font-size: 16px;
}
```

추가된 jhys는 이 클래스를 고유하게 식별하는 데 사용되는 샘플 키(본인 추가)에 불과합니다. 앞에서 말한 것처럼 JS 개체를 생성하며, 이 개체를 Retact 파일로 가져와 사용할 수 있습니다.

```css
{
    error-message: error-message_jhys
}
```

이제 이 기능을 어떻게 사용할 수 있는지 살펴보겠습니다.

```js
import styles from './styles.css';
class Message extends React.Component {
     // ... 
    render() {
        return (
I am an error message
) } }
```

CSS 모듈의 주요 목적은 CSS 클래스를 로컬 범위로 만들고 이름 지정 시 충돌을 피하는 것입니다.

## 결론

반응 구성 요소를 스타일링하는 세 가지 방법이 있습니다. 일반적으로 모든 방법이 유용하며 프로젝트 크기에 따라 어느 것이든 사용할 수 있습니다.

리액트에서 스타일링에 대해 한두 가지 배웠기를 바랍니다. 질문이나 피드백이 있으면 아래에 의견을 남겨주세요.