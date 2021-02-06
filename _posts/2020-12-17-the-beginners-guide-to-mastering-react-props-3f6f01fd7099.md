---
layout: post
title: "React에서 소품을 구성 요소로 전달하는 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/05/beginners-guide-mastering-react-props.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/05/beginners-guide-mastering-react-props.png?fit=1024%2C682&ssl=1)

React를 사용하여 웹 애플리케이션을 개발하는 방법을 배우다 보면 소품이라는 개념을 알게 될 수밖에 없습니다. 소품이 작동하는 방식을 이해하는 것은 리액트를 마스터하는 데 필수적이지만 개념을 완전히 파악하는 것은 쉬운 일이 아니다.

### 반응하는 소품: 소개

소품은 "속성"을 의미하며, 반응 응용 프로그램에서 한 반응 구성 요소에서 다른 반응 구성 요소로 데이터를 보내는 데 사용됩니다. 아래 예제 코드를 살펴보겠습니다. 여기서는 문자열을 렌더링하는 단일 React 구성 요소를 제공합니다.

```js
import React, { Component } from "react";
import ReactDOM from "react-dom";

class App extends Component {
  render(){
    return <div>Hello, World!</div>
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```

이제 `App` 구성 요소에 소품을 추가하는 방법은 다음과 같습니다. `RactDOM.render`의 앱 구성 요소에 대한 호출 바로 옆에 임의 속성을 입력하고 값을 할당합니다. name 속성을 생성해 Nathan으로 지정합니다.

```js
import React, { Component } from "react";
import ReactDOM from "react-dom";

class App extends Component {
  render(){
    return <div>Hello, World!</div>
  }
}

ReactDOM.render(<App name="Nathan" />, document.getElementById("root"));
```

이를 통해 앱 구성 요소에는 이제 이름이라는 소품이 들어 있는데, 이를 이용해 수업 시간에 호출할 수 있다. 인사말씀 드리겠습니다.

```js
import React, { Component } from "react";
import ReactDOM from "react-dom";

class App extends Component {
  render(){
    return <div>Hello, {this.props.name}!</div>
  }
}

ReactDOM.render(<App name="Nathan" />, document.getElementById("root"));
```

이것이 바로 소품의 기본입니다. 즉, 구성 요소를 호출할 때 생각할 수 있는 모든 데이터를 구성 요소로 전송할 수 있습니다. 두 개 이상의 성분이 있는 경우 데이터를 전달할 수 있습니다. 다음은 두 가지 구성 요소의 다른 예입니다.

위의 코드에서 알 수 있듯이, 일반적인 자바스크립트 `함수`를 호출할 때 인수를 전달하듯이 구성요소를 호출할 때 구성요소를 추가하여 소품을 전달할 수 있습니다. 그리고 기능 얘기가 나와서 말인데, 리액트에서는 `기능`을 사용해서도 구성 요소를 만들 수 있기 때문에, 다음에는 기능 구성 요소에서 소품이 어떻게 작동하는지 알아보겠습니다.

![image](https://i1.wp.com/cdn-images-1.medium.com/max/2400/1*wV7zU6J05BL3bphzMlB2rA.png?ssl=1)

### 기능 구성 요소의 소품

함수 구성 요소에서 구성 요소는 일반 함수 인수와 똑같이 소품을 수신합니다. 함수 구성 요소는 구성 요소 호출에 설명된 속성을 가진 `props` 개체를 수신합니다.

```js
import React from "react";
import ReactDOM from "react-dom";

function App() {
  return <Greeting name="Nathan" age={27} occupation="Software Developer" />;
}

function Greeting(props) {
  return (
    <p>
      Hello! I'm {props.name}, a {props.age} years old {props.occupation}.
      Pleased to meet you!
    </p>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

이 예에서는 여러 개의 소품을 한 번에 전달하는 것 외에도 `나이` 소품이 숫자 데이터 유형임을 확인할 수 있습니다. 이것은 `숫자`, `부울` 또는 `개체`와 같이 자바스크립트에서 사용할 수 있는 모든 유형의 데이터를 소품으로 전달할 수 있음을 보여준다. 이렇게 하면 상위 레벨의 구성요소가 하위 구성요소로 데이터를 전송할 수 있는 하향식 접근 방식을 사용하여 데이터를 전송할 수 있습니다.

### 소품 및 상태와 함께 코드 재사용

소품을 사용하면 더 많은 리액트 코드를 재사용하고 반복하지 않도록 할 수 있습니다. 이 예제의 경우, 동일한 `인사` 구성 요소를 여러 다른 사람들에게 재사용할 수 있습니다.

```php
import React from "react";
import ReactDOM from "react-dom";

function App() {
  return (
    <div>
      <Greeting name="Nathan" age={27} occupation="Software Developer" />
      <Greeting name="Jane" age={24} occupation="Frontend Developer" />
    </div>
  );
}

function Greeting(props) {
  return (
    <p>
      Hello! I'm {props.name}, a {props.age} years old {props.occupation}.
      Pleased to meet you!
    </p>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

잘됐네요! 그러나 소품은 읽기 전용이므로 리액션 응용 프로그램의 수명 동안 수동으로 변경해서는 안 되기 때문에 리액트 앱의 소품만 사용해도 사용자 상호 작용에 대응하고 그에 따라 렌더링할 수 있는 동적 앱이 되지 않습니다. 그러기 위해서는 state를 사용해야 한다.

상태 및 소품이 함께 반응 애플리케이션의 데이터 "모델"을 형성합니다. 소품은 읽기 전용이지만 상태는 사용자 작업에 따라 변경될 수 있는 데이터에 사용됩니다. 동적 애플리케이션을 만들기 위해 어떻게 협력하는지 살펴보겠습니다.

먼저 `App` 구성 요소에 부울 값을 저장하는 `textSwitch`라는 새 상태를 추가하고 이를 `Greeting` 구성 요소에 전달하겠습니다. 인사말 구성 요소는 이 상태 값을 보고 다음 항목을 지정합니다.

이 코드 예제는 `state` 및 `props`를 사용하는 사용자 작업을 기준으로 응용 프로그램의 보기를 조건부로 렌더링하는 방법을 보여줍니다. 반응에서 상태는 소품으로 한 구성 요소에서 다른 구성 요소로 전달됩니다. 소품 이름과 값은 일반적인 `prop` 개체 속성으로 구성 요소로 전달되기 때문에 데이터의 출처와 상관 없습니다.

### propType 및 defaultProp

대응 응용프로그램을 개발할 때 버그와 오류를 방지하기 위해 구조화 및 정의해야 하는 경우가 있습니다. 이와 같은 방법으로 `함수`는 필수 인수가 필요할 수 있으며, Retact 구성 요소는 제대로 렌더링되어야 하는 경우 프로포트를 정의해야 할 수 있다.

실수할 수 있으며 필요한 소품을 필요한 구성 요소에 전달하는 것을 잊어버릴 수 있습니다.

```js
import React from "react";
import ReactDOM from "react-dom";

function App() {
  return <Greeting name="Nathan" />;
}

function Greeting(props) {
  return (
    <p>
      Hello! I'm {props.name}, a {props.age} years old {props.occupation}.
      Pleased to meet you!
    </p>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

`프롭스` 할 때.나이와 pops.occation은 인사말 구성 요소에서 정의되지 않은 것으로, React는 단순히 값을 부르는 표현을 무시하고 나머지 텍스트를 전달할 뿐이다. 어떤 오류도 유발하지 않지만, 이런 종류의 문제를 해결하지 않고 내버려둘 수는 없습니다.

여기서 `propTypes`가 도움이 된다. PropTypes는 구성 요소에 있는 소품을 검증하는 데 사용할 수 있는 특수 구성 요소 속성입니다. 별도의 npm 패키지이므로 사용하기 전에 먼저 설치해야 합니다.

```undefined
npm install --save prop-types
```

이제 인사 구성 요소에 필요한 소품을 만들어 보겠습니다.

```cpp
import React from "react";
import ReactDOM from "react-dom";
import PropTypes from "prop-types";

function App() {
  return <Greeting name="Nathan" />;
}

function Greeting(props) {
  return (
    <p>
      Hello! I'm {props.name}, a {props.age} years old {props.occupation}.
      Pleased to meet you!
    </p>
  );
}

Greeting.propTypes = {
  name: PropTypes.string.isRequired, // must be a string and defined
  age: PropTypes.number.isRequired, // must be a number and defined
  occupation: PropTypes.string.isRequired  // must be a string and defined
};

ReactDOM.render(<App />, document.getElementById("root"));
```

`propTypes` 속성이 선언된 상황에서 `Greeting` 구성 요소는 해당 소품이 propTypes 검증을 통과하지 못할 경우 콘솔에 경고를 던질 것이다.

또한 `defaultProps`라는 다른 특수 속성을 사용하여 호출 시 소품이 구성 요소로 전달되지 않는 경우 소품에 대한 기본값을 정의할 수 있습니다.

이제 소품 없이 인사말을 부를 때 디폴트 프롭스(defaultProps)의 디폴트 값이 사용된다.

기본 소품에 대한 자세한 내용은 여기를 참조하십시오.

### 하위 구성 요소에서 상위 구성 요소로 데이터 전달

상위 컴포넌트는 코드 블록에서 다른 컴포넌트를 호출하는 모든 컴포넌트인 반면 하위 컴포넌트는 상위 컴포넌트에 의해 호출되는 컴포넌트이다. 상위 구성요소는 소품을 사용하여 하위 구성요소로 데이터를 전달합니다.

"어떻게 하면 하위 구성 요소에서 상위 구성 요소로 데이터를 전달할 수 있습니까?"라고 생각할 수 있습니다.

정답은 그것이 가능하지 않다는 것이다. 적어도 직접적으로. 하지만 리액트에는 `기능`을 소품으로 전달할 수도 있다. 그것이 그 문제와 어떻게 관련이 있는가? 먼저 `state`와 함께 코드 예제로 돌아가 보겠습니다.

```js
import React, { useState } from "react";
import ReactDOM from "react-dom";

function App() {
  const [textSwitch, setTextSwitch] = useState(true);
  return (
    <div>
      <button onClick={() => setTextSwitch(!textSwitch)} type="button">
        Toggle Name
      </button>
      <Greeting text={textSwitch} />
    </div>
  );
}
function Greeting(props) {
  console.log(props.text);
  if (props.text) {
    return (
      <p>
        Hello! I'm Nathan and I'm a Software Developer. Pleased to meet you!
      </p>
    );
  }
  return (
    <p>Hello! I'm Jane and I'm a Frontend Developer. Pleased to meet you!</p>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

리액트 애플리케이션에는 최대 3개의 구성 요소 계층이 있으며, 최상위 구성 요소는 다른 하위 구성 요소를 호출합니다. 우리는 이 점을 설명하기 위해 위의 예시를 약간 조정해야 합니다.

`앱`에서 `버튼` 요소를 `앱`에서 벗어나 `버튼`의 구성 요소로 옮겨보자. 간단하게 말하자면, 그것을 `변화 인사`라고 부르자. 그런 다음 앱 구성 요소 대신 인사말 구성 요소에서 이 구성 요소를 호출합니다.

```js
import React, { useState } from "react";
import ReactDOM from "react-dom";

function App() {
  const [textSwitch, setTextSwitch] = useState(true);
  return (
    <div>
      <Greeting
        text={textSwitch}
      />
    </div>
  );
}

function Greeting(props) {
  let element;
  if (props.text) {
    element = (
      <p>
        Hello! I'm Nathan and I'm a Software Developer. Pleased to meet you!
      </p>
    );
  } else {
    element = (
      <p>Hello! I'm Jane and I'm a Frontend Developer. Pleased to meet you!</p>
    );
  }
  return (
    <div>
      {element}
      <ChangeGreeting />
    </div>
  );
}

function ChangeGreeting(props) {
  return (
    <button type="button">
      Toggle Name
    </button>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

이제 상태 설정 버튼은 상태가 있는 곳(App 구성 요소)에서 두 단계 아래인 `ChangeGreeting` 구성 요소에 있습니다. 어떻게 하면 국가를 바꿀 수 있을까요? 정답은 기능이 필요한 구성 요소에 도달할 때까지 기능을 아래로 보내는 것입니다.

위의 예에서 `App` 구성 요소는 상태를 `Greeting` 구성 요소로 변경하는 기능을 가진 `handleClick` prop를 전송하고 있다. 인사부품은 필요없었지만 자녀부품은 변경인사가 필요하기 때문에 소품 전달을 한다.

ChangeGreeting 구성 요소에서는 버튼을 클릭하면 핸들 클릭 기능이 호출되어 앱이 실행된다.

앱 상태가 업데이트되면 리액션뷰가 다시 렌더링되고, 새로운 상태 값은 소품을 통해 인사말로 전송됩니다.

따라서 예: 대응은 하위 구성 요소의 데이터를 상위 구성 요소로 전송할 수 없지만 상위 구성 요소는 하위 구성 요소로 기능을 전송할 수 있습니다. 이를 통해 상태를 업데이트하는 함수를 하위 구성 요소로 보낼 수 있으며, 해당 기능이 호출되면 상위 구성 요소가 상태를 업데이트합니다.

데이터를 보낼 수는 없지만 기능을 사용하여 변경 신호를 보낼 수는 있습니다.

### 프로펠러 드릴링 및 처리 방법

데이터 전달의 마지막 예는 실제로 소품 및 상태, 즉 프로펠링을 처리할 때 발생할 수 있는 또 다른 일반적인 문제를 나타냅니다.

프로펠러 드릴링은 지정된 하위 구성 요소에 도달할 때까지 구성 요소 계층 아래로 소품을 전달하는 것을 의미하지만, 다른 상위 구성 요소에는 실제로 소품이 필요하지 않습니다.

위의 예에서는 괜찮아 보일 수 있지만, 우리에게는 세 가지 요소만 있다는 것을 명심하세요. 구성 요소가 많고 모두 소품 및 상태를 사용하여 서로 상호작용하는 경우, 소품 시추는 유지관리의 골칫거리가 될 수 있습니다.

이 문제를 방지하려면 구성 요소 수를 줄이고 특정 구성 요소를 재사용해야 하는 경우에만 새 구성 요소를 만드는 것이 좋습니다.

예를 들어 인사말 이외의 다른 구성 요소가 실제로 동일한 코드를 호출하기 전까지는 별도의 인사말 구성 요소가 전혀 필요하지 않다. 이 작업은 다음 두 가지 구성 요소에서만 수행할 수 있습니다.

```js
import React, { useState } from "react";
import ReactDOM from "react-dom";

function App() {
  const [textSwitch, setTextSwitch] = useState(true);
  return (
    <div>
      <Greeting
        text={textSwitch}
        handleClick={() => setTextSwitch(!textSwitch)}
      />
    </div>
  );
}

function Greeting(props) {
  let element;
  if (props.text) {
    element = (
      <p>
        Hello! I'm Nathan and I'm a Software Developer. Pleased to meet you!
      </p>
    );
  } else {
    element = (
      <p>Hello! I'm Jane and I'm a Frontend Developer. Pleased to meet you!</p>
    );
  }
  return (
    <div>
      {element}
      <button onClick={props.handleClick} type="button">
        Toggle Name
      </button>
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

여기 있습니다. 이런 식으로 소품을 물려줄 때 필요한 천공은 필요 없습니다.

### 반응의 소품 전달: 결론

리액트 학습의 모든 것이 그렇듯이 소품은 배우기는 쉽지만 숙달하기는 어렵다. 이제 여러분은 소품이 반응 구성 요소가 서로 "대화"되도록 만드는 데 사용되는 불변(읽기 전용) 데이터라는 것을 알게 되었습니다. 그것들은 어떤 함수에 전달되는 주장과 매우 유사하며, 이것은 개발자 자신이 지정한 어떤 것이 될 수 있다.

상태 및 소품을 사용하면 재사용 가능하고 유지관리가 가능하며 데이터 중심적인 솔리드 코드베이스를 사용하여 다이내믹 React 애플리케이션을 생성할 수 있습니다.