---
layout: post
title: "더 나은 React 컴포넌트를 작성하는 방법
 "
author: 'Code Tower'
thumbnail: https://www.freecodecamp.org/news/content/images/size/w600/2021/01/modern_way-1.jpeg
tags: undefined
---


ES6의 JavaScript에 많은 기능이 추가되었습니다.
 그리고 이러한 변경 사항은 개발자가 짧고 이해하고 유지하기 쉬운 코드를 작성하는 데 도움이됩니다.
 

create-react-app을 사용하여 React App을 만들 때 이미 이러한 변경 사항을 지원하고 있습니다.
 이는 Babel.js를 사용하여 ES6 + 코드를 모든 브라우저가 이해하는 ES5 코드로 변환하기 때문입니다.
 

이 기사에서는 React 코드를 더 짧고 간단하며 이해하기 쉽게 작성할 수있는 다양한 방법을 살펴 봅니다.
 그럼 시작하겠습니다.
 

아래 코드 샌드 박스 데모를 살펴보십시오.
 

여기에는 사용자의 입력을받는 두 개의 입력 텍스트 상자와 입력으로 제공된 숫자의 더하기와 빼기를 계산하는 두 개의 버튼이 있습니다.
 

## 이벤트 핸들러를 수동으로 바인딩하지 마십시오.
 

React에서 알고 있듯이`onClick` 또는`onChange` 또는 다음과 같은 다른 이벤트 핸들러를 첨부 할 때 :
 

```undefined
<input
  ...
  onChange={this.onFirstInputChange}
/>

```

그러면 핸들러 함수 (onFirstInputChange)가`this`의 바인딩을 유지하지 않습니다.
 

이것은 React의 문제가 아니지만 JavaScript 이벤트 핸들러가 작동하는 방식입니다.
 

따라서 다음과 같이`this`를 올바르게 바인딩하려면`.bind` 메소드를 사용해야합니다.
 

```undefined
constructor(props) {
  // some code
  this.onFirstInputChange = this.onFirstInputChange.bind(this);
  this.onSecondInputChange = this.onSecondInputChange.bind(this);
  this.handleAdd = this.handleAdd.bind(this);
  this.handleSubtract = this.handleSubtract.bind(this);
}

```

위의 코드 줄은 핸들러 함수 내에서 클래스의`this` 바인딩을 올바르게 유지합니다.
 

그러나 모든 새로운 이벤트 핸들러에 새로운 바인딩 코드를 추가하는 것은 지루합니다.
 다행히도 클래스 속성 구문을 사용하여 수정할 수 있습니다.
 

클래스 속성을 사용하면 클래스 내부에서 직접 속성을 정의 할 수 있습니다.
 

Create-react-app은 내부적으로 Babel 버전> = 7에 대해`@ babel / babel-plugin-transform-class-properties` 플러그인을 사용하고 Babel 버전 <7에 대해`babel / plugin-proposal-class-properties` 플러그인을 사용합니다.
 수동으로 구성 할 필요가 없습니다.
 

이를 사용하려면 이벤트 핸들러 함수를 화살표 함수 구문으로 변환해야합니다.
 

```undefined
onFirstInputChange(event) {
  const value = event.target.value;
  this.setState({
    number1: value
  });
}

```

위의 코드는 다음과 같이 작성할 수 있습니다.
 

```undefined
onFirstInputChange = (event) => {
  const value = event.target.value;
  this.setState({
    number1: value
  });
}

```

비슷한 방식으로 다른 세 가지 함수를 변환 할 수 있습니다.
 

```undefined
onSecondInputChange = (event) => {
 // your code
}

handleAdd = (event) => {
 // your code
}

handleSubtract = (event) => {
 // your code
}

```

또한 생성자에서 이벤트 핸들러를 바인딩 할 필요가 없습니다.
 그래서 우리는 그 코드를 제거 할 수 있습니다.
 이제 생성자는 다음과 같습니다.
 

```undefined
constructor(props) {
  super(props);

  this.state = {
    number1: "",
    number2: "",
    result: "",
    errorMsg: ""
  };
}

```

우리는 그것을 더욱 단순화 할 수 있습니다.
 클래스 속성 구문을 사용하면 클래스 내에서 직접 변수를 선언 할 수 있으므로 아래와 같이 생성자를 완전히 제거하고 상태를 클래스의 일부로 선언 할 수 있습니다.
 

```undefined
export default class App extends React.Component {
  state = {
    number1: "",
    number2: "",
    result: "",
    errorMsg: ""
  };

  render() { }
}

```

다음은 코드 샌드 박스 데모입니다. https://codesandbox.io/s/trusting-dust-ukvx2
 

위의 Code Sandbox 데모를 확인하면 기능이 이전과 같이 작동하는 것을 확인할 수 있습니다.
 

그러나 클래스 속성을 사용하면 코드가 훨씬 간단하고 이해하기 쉽습니다.
 

요즘에는 이렇게 작성된 React 코드가 있습니다.
 

## 단일 이벤트 처리기 메서드 사용
 

위의 코드를 살펴보면 각 입력 필드에 대해 별도의 이벤트 핸들러 함수 인`onFirstInputChange` 및`onSecondInputChange`가 있음을 알 수 있습니다.
 

입력 필드의 수가 증가하면 이벤트 핸들러 함수의 수도 증가하여 좋지 않습니다.
 

예를 들어 등록 페이지를 만드는 경우 많은 입력 필드가 있습니다.
 따라서 각 입력 필드에 대해 별도의 핸들러 함수를 생성하는 것은 불가능합니다.
 

그것을 바꾸자.
 

모든 입력 필드를 처리 할 단일 이벤트 핸들러를 생성하려면 해당 상태 변수 이름과 정확히 일치하는 각 입력 필드에 고유 한 이름을 지정해야합니다.
 

이미이 설정이 있습니다.
 입력 필드에 부여한 `number1`과 `number2`이름도 상태에 정의됩니다.
 따라서 두 입력 필드의 핸들러 메서드를 다음과 같이 `onInputChange`로 변경해 보겠습니다.
 

```undefined
<input
  type="text"
  name="number1"
  placeholder="Enter a number"
  onChange={this.onInputChange}
/>

<input
  type="text"
  name="number2"
  placeholder="Enter a number"
  onChange={this.onInputChange}
/>

```

다음과 같이 새로운`onInputChange` 이벤트 핸들러를 추가합니다.
 

```undefined
onInputChange = (event) => {
  const name = event.target.name;
  const value = event.target.value;
  this.setState({
    [name]: value
  });
};

```

여기서 상태를 설정하는 동안 동적 값으로 동적 상태 이름을 설정합니다.
 따라서`number1` 입력 필드 값을 변경할 때`event.target.name`은`number1`이되고`event.target.value`는 사용자가 입력 한 값이됩니다.
 

그리고`number2` 입력 필드 값을 변경할 때`event.target.name`은`number2`가되고`event.taget.value`는 사용자가 입력 한 값이됩니다.
 

따라서 여기서는 ES6 동적 키 구문을 사용하여 해당 상태 값을 업데이트합니다.
 

이제`onFirstInputChange` 및`onSecondInputChange` 이벤트 핸들러 메서드를 삭제할 수 있습니다.
 더 이상 필요하지 않습니다.
 

다음은 코드 샌드 박스 데모입니다. https://codesandbox.io/s/withered-feather-8gsyc
 

## 단일 계산 방법 사용
 

이제`handleAdd` 및`handleSubtract` 메서드를 리팩터링 해 보겠습니다.
 

우리는 코드 중복을 생성하는 거의 동일한 코드를 가진 두 가지 별도의 방법을 사용하고 있습니다.
 단일 메서드를 만들고 더하기 또는 빼기 연산을 식별하는 함수에 매개 변수를 전달하여이 문제를 해결할 수 있습니다.
 

```undefined
// change the below code:
<button type="button" className="btn" onClick={this.handleAdd}>
  Add
</button>

<button type="button" className="btn" onClick={this.handleSubtract}>
  Subtract
</button>

// to this code:
<button type="button" className="btn" onClick={() => this.handleOperation('add')}>
  Add
</button>

<button type="button" className="btn" onClick={() => this.handleOperation('subtract')}>
  Subtract
</button>

```

여기에서는 작업 이름을 전달하여 새로운`handleOperation` 메서드를 수동으로 호출하는`onClick` 핸들러에 대한 새 인라인 메서드를 추가했습니다.
 

이제 다음과 같이 새로운`handleOperation` 메서드를 추가합니다.
 

```undefined
handleOperation = (operation) => {
  const number1 = parseInt(this.state.number1, 10);
  const number2 = parseInt(this.state.number2, 10);

  let result;
  if (operation === "add") {
    result = number1 + number2;
  } else if (operation === "subtract") {
    result = number1 - number2;
  }

  if (isNaN(result)) {
    this.setState({
      errorMsg: "Please enter valid numbers."
    });
  } else {
    this.setState({
      errorMsg: "",
      result: result
    });
  }
};

```

이전에 추가 된`handleAdd` 및`handleSubtract` 메서드를 제거합니다.
 

다음은 코드 샌드 박스 데모입니다. https://codesandbox.io/s/hardcore-brattain-zv09d
 

## ES6 구조화 구문 사용
 

`onInputChange` 메소드 안에 다음과 같은 코드가 있습니다.
 

```undefined
const name = event.target.name;
const value = event.target.value;

```

ES6 비 구조화 구문을 사용하여 다음과 같이 단순화 할 수 있습니다.
 

```undefined
const { name, value } = event.target;

```

여기서는`event.target` 객체에서`name` 및`value` 속성을 추출하고 해당 값을 저장할 로컬`name` 및`value` 변수를 생성합니다.
 

이제`handleOperation` 메서드 내에서`this.state.number1`과`this.state.number2`를 사용할 때마다 상태를 참조하는 대신 미리 해당 변수를 분리 할 수 있습니다.
 

```undefined
// change the below code:

const number1 = parseInt(this.state.number1, 10);
const number2 = parseInt(this.state.number2, 10);

// to this code:

let { number1, number2 } = this.state;
number1 = parseInt(number1, 10);
number2 = parseInt(number2, 10);

```

다음은 코드 샌드 박스 데모입니다. https://codesandbox.io/s/exciting-austin-ldncl
 

## 향상된 개체 리터럴 구문 사용
 

`handleOperation`함수 내에서 `setState`함수 호출을 확인하면 다음과 같습니다.
 

```undefined
this.setState({
  errorMsg: "",
  result: result
});

```

향상된 객체 리터럴 구문을 사용하여이 코드를 단순화 할 수 있습니다.
 

속성 이름이`result : result`와 같은 변수 이름과 정확히 일치하면 콜론 뒤의 부분을 언급하지 않아도됩니다.
 따라서 위의`setState` 함수 호출은 다음과 같이 단순화 할 수 있습니다.
 

```undefined
this.setState({
  errorMsg: "",
  result
});

```

다음은 코드 샌드 박스 데모입니다. https://codesandbox.io/s/affectionate-johnson-j50ks
 

## 클래스 구성 요소를 React Hooks로 변환
 

React 버전 16.8.0부터 React는 React Hooks를 사용하여 기능 구성 요소 내에서 상태 및 수명주기 메서드를 사용하는 방법을 추가했습니다.
 

React Hooks를 사용하면 훨씬 짧고 유지 관리 및 이해하기 쉬운 코드를 작성할 수 있습니다.
 따라서 위 코드를 React Hooks 구문을 사용하도록 변환 해 보겠습니다.
 

React Hooks를 처음 사용하는 경우 React Hooks 소개 기사를 확인하십시오.
 

먼저 App 구성 요소를 기능 구성 요소로 선언하겠습니다.
 

```undefined
const App = () => {

};

export default App;

```

상태를 선언하려면`useState` 후크를 사용해야하므로 파일 상단에서 가져옵니다.
 그런 다음 3 개의 `useState`호출을 생성합니다. 하나는 숫자를 객체로 함께 저장하기위한 것입니다.
 단일 핸들러 함수와 결과 및 오류 메시지를 저장하는 두 개의 다른 `useState`호출을 사용하여 함께 업데이트 할 수 있습니다.
 

```undefined
import React, { useState } from "react";

const App = () => {
  const [state, setState] = useState({
    number1: "",
    number2: ""
  });
  const [result, setResult] = useState("");
  const [errorMsg, setErrorMsg] = useState("");
};

export default App;

```

`onInputChange` 핸들러 메소드를 다음과 같이 변경하십시오.
 

```undefined
const onInputChange = () => {
  const { name, value } = event.target;

  setState((prevState) => {
    return {
      ...prevState,
      [name]: value
    };
  });
};

```

여기서는 상태를 설정하기 위해 업데이터 구문을 사용합니다. React Hooks로 작업 할 때 객체를 업데이트 할 때 상태가 자동으로 병합되지 않기 때문입니다.
 

그래서 우리는 먼저 상태의 모든 속성을 펼친 다음 새로운 상태 값을 추가합니다.
 

`handleOperation` 메소드를 다음과 같이 변경하십시오.
 

```undefined
const handleOperation = (operation) => {
  let { number1, number2 } = state;
  number1 = parseInt(number1, 10);
  number2 = parseInt(number2, 10);

  let result;
  if (operation === "add") {
    result = number1 + number2;
  } else if (operation === "subtract") {
    result = number1 - number2;
  }

  if (isNaN(result)) {
    setErrorMsg("Please enter valid numbers.");
  } else {
    setErrorMsg("");
    setResult(result);
  }
};

```

이제 클래스 컴포넌트의 render 메소드에서 반환 된 동일한 JSX를 반환하지만 JSX에서`this` 및`this.state`의 모든 참조를 제거합니다.
 

다음은 코드 샌드 박스 데모입니다. https://codesandbox.io/s/musing-breeze-ec7px?file=/src/App.js
 

## 암시 적으로 개체 반환
 

이제 최신 ES6 기능을 사용하고 코드 중복을 방지하도록 코드를 최적화했습니다.
 우리가 할 수있는 또 하나의 일은`setState` 함수 호출을 단순화하는 것입니다.
 

`onInputChange`핸들러 내에서 현재 `setState`함수 호출을 확인하면 다음과 같습니다.
 

```undefined
setState((prevState) => {
  return {
    ...prevState,
    [name]: value
  };
});

```

화살표 함수에서 다음과 같은 코드가있는 경우 :
 

```undefined
const add = (a, b) => {
 return a + b;
}

```

그런 다음 아래와 같이 단순화 할 수 있습니다.
 

```undefined
const add = (a, b) => a + b;

```

이것은 화살표 함수 본문에 단일 문이 있으면 중괄호와 return 키워드를 건너 뛸 수 있기 때문에 작동합니다.
 이를 암시 적 반환이라고합니다.
 

따라서 다음과 같이 화살표 함수에서 객체를 반환하는 경우 :
 

```undefined
const getUser = () => {
 return {
  name: 'David,
  age: 35
 }
}

```

그러면 다음과 같이 단순화 할 수 없습니다.
 

```undefined
const getUser = () => {
  name: 'David,
  age: 35
}

```

여는 중괄호는 함수의 시작을 나타 내기 때문에 위 코드는 유효하지 않습니다.
 작동하게하려면 다음과 같이 둥근 괄호로 개체를 감쌀 수 있습니다.
 

```undefined
const getUser = () => ({
  name: 'David,
  age: 35
})

```

위 코드는 아래 코드와 동일합니다.
 

```undefined
const getUser = () => {
 return {
  name: 'David,
  age: 35
 }
}

```

따라서 동일한 기술을 사용하여`setState` 함수 호출을 단순화 할 수 있습니다.
 

```undefined
setState((prevState) => {
  return {
    ...prevState,
    [name]: value
  };
});

// the above code can be simplified as:

setState((prevState) => ({
  ...prevState,
  [name]: value
}));

```

다음은 코드 샌드 박스 데모입니다. https://codesandbox.io/s/sharp-dream-l90gf?file=/src/App.js
 

코드를 둥근 괄호로 묶는이 기술은 React에서 많이 사용됩니다.
 

- 기능 구성 요소를 정의하려면
 

```undefined
const User = () => (
   <div>
    <h1>Welcome, User</h1>
    <p>You're logged in successfully.</p>
   </div>
);

```

- react-redux의 mapStateToProps 함수 내부 :
 

```undefined
const mapStateToProps = (state, props) => ({ 
   users: state.users,
   details: state.details
});

```

- Redux 액션 생성기 기능 :
 

```undefined
const addUser = (user) => ({
  type: 'ADD_USER',
  user
});

```

그리고 다른 많은 장소.
 

## 더 나은 React 구성 요소를 작성하는 데 도움이되는 추가 팁
 

다음과 같은 구성 요소가있는 경우 :
 

```undefined
const User = (props) => (
   <div>
    <h1>Welcome, User</h1>
    <p>You're logged in successfully.</p>
   </div>
);

```

나중에 코드를 아래 코드로 변환하는 대신 테스트 또는 디버깅을 위해 콘솔에 소품을 기록하려고합니다.
 

```undefined
const User = (props) => {
 console.log(props);
 return (
   <div>
    <h1>Welcome, User</h1>
    <p>You're logged in successfully.</p>
   </div>
 );
}

```

다음과 같이 논리 OR 연산자 (`||`)를 사용할 수 있습니다.
 

```undefined
const User = (props) => console.log(props) || (
   <div>
    <h1>Welcome, User</h1>
    <p>You're logged in successfully.</p>
   </div>
);

```

어떻게 작동합니까?
 

`console.log` 함수는 전달 된 값만 인쇄하지만 아무것도 반환하지 않습니다. 따라서 정의되지 않은`||`(...)로 평가됩니다.
 

그리고`||`연산자는 첫 번째 진실 값을 반환하기 때문에`||`이후의 코드도 실행됩니다.
 

### 읽어 주셔서 감사합니다!
 verified_user

나의 Mastering Modern JavaScript 책에서 ES6 + 기능에 대한 모든 것을 자세히 배울 수 있습니다.
 

또한 무료 React Router 소개 과정을 확인할 수 있습니다.
 

내 주간 뉴스 레터를 구독하여 1000 명 이상의 다른 구독자와 함께 놀라운 팁, 요령, 기사 및 할인 거래를받은 편지함에서 직접 받아보세요.
 