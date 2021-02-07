---
layout: post
title: "반응에서 상태 사용: 전체 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2019/03/react-usestate.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/03/react-usestate.png?fit=730%2C487&ssl=1)

`useState`는 함수 구성 요소에 상태 변수를 둘 수 있는 후크입니다. 초기 상태를 이 함수에 전달하면 현재 상태 값(초기 상태일 필요는 없음)과 이 값을 업데이트하기 위한 다른 함수가 있는 변수가 반환됩니다.

이 튜토리얼은 "this.state"/"this에 해당하는 "useState" Hook in React에 대한 완벽한 가이드 역할을 한다.기능 구성 요소에 대해 `Sate`를 설정합니다. 다음 사항에 대해 자세히 설명합니다.

- 반응의 클래스 및 기능 구성 요소
React.use State hook은 무엇을 합니까?
반응 상태 선언
반응 후크: 업데이트 상태
useState 후크를 사용하여 상태 변수로 개체 사용
후크를 사용하여 응답에서 중첩된 개체의 상태를 업데이트하는 방법
다중 상태 변수 또는 하나의 상태 개체
useState 사용 규칙
사용자 리듀서 후크
- 반응의 클래스 및 기능 구성 요소
- React.use State hook은 무엇을 합니까?
- 반응 상태 선언
- 반응 후크: 업데이트 상태
- useState 후크를 사용하여 상태 변수로 개체 사용
- 후크를 사용하여 응답에서 중첩된 개체의 상태를 업데이트하는 방법
- 다중 상태 변수 또는 하나의 상태 개체
- useState 사용 규칙
- 사용자 리듀서 후크

React Hooks를 시작하고 시각적인 가이드를 찾는 경우 아래 비디오 튜토리얼을 확인하십시오.

## 반응의 클래스 및 기능 구성 요소

반응에는 클래스 구성 요소와 기능 구성 요소의 두 가지 유형이 있습니다.

클래스 구성 요소는 반응에서 확장된 ES6 클래스입니다.구성 요소 및 상태 및 라이프사이클 방법을 사용할 수 있습니다.

```js
class Message extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      message: ‘’    
    };
  }

  componentDidMount() {
    /* ... */
  }

  render() {
    return <div>{this.state.message}</div>;
  }
}
```

함수 구성 요소는 인수를 구성 요소의 속성으로 받아들이고 유효한 JSX를 반환하는 함수입니다.

```js
function Message(props) {
  return <div>{props.message}</div>
}
// Or as an arrow function
const Message = (props) =>  <div>{props.message}</div>
```

보시다시피 상태 또는 수명 주기 방법이 없습니다. 그러나, 리액션 16.8의 경우, 후크를 사용할 수 있습니다.

반응 후크는 기능 구성 요소에 상태 변수를 추가하고 클래스의 수명 주기 방법을 계측하는 함수입니다. 그들은 `사용`으로 시작하는 경향이 있다.

## 리액트.usage State 후크는 무엇을 하는가?

앞에서 설명한 것처럼 `useState`를 사용하면 함수 구성요소에 상태를 추가할 수 있습니다. 함수 구성 요소 내에서 `React.useState`를 호출하면 해당 구성 요소와 관련된 단일 상태가 생성됩니다.

클래스의 상태는 항상 객체이지만 Hooks를 사용하면 상태는 모든 유형이 될 수 있습니다. 각 상태 조각에는 개체, 배열, 부울 또는 상상할 수 있는 다른 유형이 포함될 수 있는 단일 값이 있습니다.

그렇다면 언제 use State Hook을 사용해야 하는가? 특히 로컬 구성 요소 상태에 유용하지만 대규모 프로젝트에는 추가 상태 관리 솔루션이 필요할 수 있습니다.

## 반응 상태 선언

useState는 react에서 명명된 내보내기입니다. 사용하기 위해 다음을 쓸 수 있습니다.

```css
React.useState
```

또는 가져오려면 `사용 상태`를 쓰십시오.

```coffeescript
import React, { useState } from 'react';
```

그러나 클래스에서 선언할 수 있는 상태 개체와는 달리 다음과 같이 둘 이상의 상태 변수를 선언할 수 있습니다.

```js
import React from 'react';

class Message extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      message: '',
      list: [],    
    };
  }
  /* ... */
}
```

`useState` 후크를 사용하면 다음과 같이 한 번에 하나의 상태 변수(모든 유형)만 선언할 수 있습니다.

```js
import React, { useState } from 'react';

const Message= () => {
   const messageState = useState( '' );
   const listState = useState( [] );
}
```

`useState`는 상태 변수의 초기 값을 인수로 사용합니다.

이전 예제와 같이 직접 전달하거나 함수를 사용하여 변수를 느리게 초기화할 수 있습니다(초기 상태가 값비싼 계산의 결과일 때 유용).

```js
const Message= () => {
   const messageState = useState( () => expensiveComputation() );
   /* ... */
}
```

초기 값은 초기 렌더에서만 할당됩니다(함수인 경우 초기 렌더에서만 실행됩니다).

이후 렌더링(구성 요소 또는 상위 구성 요소의 상태 변경으로 인해)에서는 `useState` Hook의 인수가 무시되고 현재 값이 검색됩니다.

예를 들어 구성 요소가 수신하는 새 속성에 따라 상태를 업데이트하려는 경우 다음과 같은 점을 염두에 두어야 합니다.

```js
const Message= (props) => {
   const messageState = useState( props.message );
   /* ... */
}
```

`useState`를 단독으로 사용하는 것은 해당 인수가 처음에만 사용되기 때문에 작동하지 않습니다. 속성이 변경될 때마다 `useState`를 사용합니다(여기서 올바른 방법을 찾아 보십시오).

그러나 useState는 앞의 예에서 알 수 있듯이 변수만 반환하지 않는다.

첫 번째 요소는 상태 변수이고 두 번째 요소는 변수의 값을 업데이트하는 함수인 배열을 반환합니다.

```js
const Message= () => {
   const messageState = useState( '' );
   const message = messageState[0]; // Contains ''
   const setMessage = messageState[1]; // It’s a function
}
```

일반적으로 어레이 구조를 사용하여 위에 표시된 코드를 단순화할 수 있습니다.

```js
const Message= () => {
   const [message, setMessage]= useState( '' );
}
```

이렇게 하면 다른 변수와 마찬가지로 기능 성분의 상태 변수를 사용할 수 있습니다.

```js
const Message = () => {
  const [message, setMessage] = useState( '' );

  return (
    <p>
      <strong>{message}</strong>
    </p>
  );
};
```

그러나 왜 useState는 어레이를 반환하는가?

개체에 비해 어레이가 더 유연하고 사용하기 쉽기 때문입니다.

메소드가 고정된 속성 집합을 가진 개체를 반환한 경우 사용자 지정 이름을 쉽게 할당할 수 없습니다.

다음과 같은 작업을 수행해야 합니다(개체 속성이 `상태` 및 `상태 설정`인 경우).

```php
// Without using object destructuring
const messageState = useState( '' );
const message = messageState.state;
const setMessage = messageState

// Using object destructuring
const { state: message, setState: setMessage } = useState( '' );
const { state: list, setState: setList } = useState( [] );
```

## 반응 후크: 업데이트 상태

useState가 반환하는 두 번째 요소는 상태 변수를 업데이트하기 위해 새로운 값을 사용하는 함수입니다.

다음은 텍스트 상자를 사용하여 모든 변경 시 상태 변수를 업데이트하는 예입니다.

```js
const Message = () => {
  const [message, setMessage] = useState( '' );

  return (
    <div>
      <input
         type="text"
         value={message}
         placeholder="Enter a message"
         onChange={e => setMessage(e.target.value)}
       />
      <p>
        <strong>{message}</strong>
      </p>
    </div>
  );
};
```

여기서 한번 해보세요.

그러나 이 업데이트 기능은 값을 즉시 업데이트하지 않습니다.

오히려 업데이트 작업을 지연시킵니다. 그런 다음 구성 요소를 다시 렌더링한 후 useState라는 인수는 무시되고 이 함수는 가장 최근의 값을 반환합니다.
이전 값을 사용하여 상태를 업데이트하는 경우 이전 값을 수신하고 새 값을 반환하는 함수를 전달해야 합니다.

```js
const Message = () => {
  const [message, setMessage] = useState( '' );

  return (
    <div>
      <input
        type="text"
        value={message}
        placeholder="Enter some letters"
        onChange={e => {
          const val = e.target.value;
          setMessage(prev => prev + val)
        } }
      />
      <p>
        <strong>{message}</strong>
      </p>
    </div>
  );
};
```

여기서 한번 해보세요.

## 'useState' 후크가 있는 상태 변수로 개체 사용

개체를 사용할 때 업데이트에 주의해야 할 두 가지가 있습니다.

- 불변성의 중요성
- 그리고 useState가 반환한 setter가 setState()와 같은 객체를 클래스 구성요소에서 병합하지 않는다는 사실

첫 번째 지점에서는 현재 상태와 동일한 값을 사용하여 상태를 업데이트하는 경우(Ract는 Object.is을 사용하여 비교함), React가 다시 트리거하지 않습니다.

개체를 사용할 때 다음과 같은 오류를 범하기 쉽습니다.

```js
const Message = () => {
  const [messageObj, setMessage] = useState({ message: '' });

  return (
    <div>
      <input
        type="text"
        value={messageObj.message}
        placeholder="Enter a message"
        onChange={e => {
          messageObj.message = e.target.value;
          setMessage(messageObj); // Doesn't work
        }
      />
      <p>
        <strong>{messageObj.message}</strong>
      </p>
  </div>
  );
};
```

여기서 한번 해보세요.

위의 예에서는 새 개체를 만드는 대신 기존 상태 개체를 변형합니다. 반응하는 것도 같은 목적입니다.

제대로 작동하려면 새 개체를 만들어야 합니다.

```undefined
onChange={e => {
  const newMessageObj = { message: e.target.value };
  setMessage(newMessageObj); // Now it works
}
```

이것은 당신이 기억해야 할 두 번째 중요한 것으로 우리를 이끈다.

상태 변수를 업데이트할 때 `이것`과 달리클래스 구성 요소에서 setState는 `useState`가 반환하는 함수가 업데이트 개체를 자동으로 병합하지 않고 대신합니다.

앞의 예에 따라 메시지 개체(`id`)에 다른 속성을 추가할 경우:

```js
const Message = () => {
  const [messageObj, setMessage] = useState({ message: '', id: 1 });

  return (
    <div>
      <input
        type="text"
        value={messageObj.message}
        placeholder="Enter a message"
        onChange={e => {
          const newMessageObj = { message: e.target.value };
          setMessage(newMessageObj); 
        }
      />
      <p>
        <strong>{messageObj.id} : {messageObj.message}</strong>
      </p>
  </div>
  );
};
```

또한 위의 예에서와 같이 `메시지` 속성만 업데이트합니다. React는 원래 상태 개체를 대체합니다.

```css
{ message: '', id: 1 }
```

`message` 속성만 포함하는 `onChange` 이벤트에 사용된 개체:

```coffeescript
{ message: 'message entered' } // id property is lost
```

여기서 해보세요, 여러분은 어떻게 `id`가 분실되는지를 볼 수 있을 거예요.

바꿀 객체와 객체 스프레드 구문을 포함하는 함수 인수를 사용하여 `setState()`의 동작을 복제할 수 있습니다.

```js
onChange={e => {
  const val = e.target.value;
  setMessage(prevState => {
    return { ...prevState, message: val }
  });
}
```

...prevState 부분은 객체의 모든 속성을 가져오고 메시지:val 부분은 메시지 속성을 덮어씁니다.

그러면 Object.assign을 사용하는 것과 동일한 결과가 나타납니다(새 개체를 생성하는 것만 기억).

```js
onChange={e => {
  const val = e.target.value;
  setMessage(prevState => {
    return Object.assign({}, prevState, { message: val });
  });
}
```

여기서 한번 해보세요.

그러나 스프레드 구문은 이 작업을 단순화하고 어레이에서도 작동합니다.

기본적으로 배열에 적용되면 스프레드시트 구문이 브래킷을 제거하므로 원래 배열의 값을 사용하여 다른 구문을 만들 수 있습니다.

```cpp
[
  ...['a', 'b', 'c'],
  'd'
]
// Is equivalent to
[
  'a', 'b', 'c',
  'd'
]
```

다음은 어레이와 함께 `useState`를 사용하는 방법을 보여 주는 예입니다.

```php
const MessageList = () => {
  const [message, setMessage] = useState("");
  const [messageList, setMessageList] = useState([]);

  return (
    <div>
      <input
        type="text"
        value={message}
        placeholder="Enter a message"
        onChange={e => {
          setMessage(e.target.value);
        }
      />
      <input
        type="button"
        value="Add"
        onClick={e => {
          setMessageList([
            ...messageList,
            {
              // Use the current size as ID (needed to iterate the list later)
              id: messageList.length + 1,
              message: message
            }
          ]);
          setMessage(""); // Clear the text box
        }
      />
      <ul>
        {messageList.map(m => (
          <li key={m.id}>{m.message}</li>
        ))}
      </ul>
    </div>
  );
};
```

여기서 한번 해보세요.

분산 구문은 예상대로 작동하지 않으므로 다차원 배열에 적용할 때 주의해야 합니다.

이것은 우리가 사물을 상태로서 다룰 때 고려해야 할 또 다른 것으로 이끈다.

## 후크를 사용하여 응답에서 중첩된 개체의 상태를 업데이트하는 방법

JavaScript에서 다차원 배열은 배열 내의 배열입니다.

```undefined
[
  ['value1','value2'],
  ['value3','value4']
]
```

이러한 변수를 사용하여 모든 상태 변수를 한 곳에 그룹화할 수 있습니다. 그러나 이를 위해 중첩된 개체를 사용하는 것이 좋습니다.

```bash
{
  'row1' : {
    'key1' : 'value1',
    'key2' : 'value2'
  },
  'row2' : {
    'key3' : 'value3',
    'key4' : 'value4'
  }
}
```

그러나 다차원 배열과 중첩된 객체를 사용할 때 문제는 객체.assignment와 스프레드 구문이 딥 카피가 아닌 얕은 카피를 만든다는 점이다.

스프레드 구문 설명서에서 다음을 참조하십시오.

> 스프레드 구문은 배열을 복사하는 동안 한 수준 깊이로 이동합니다. 따라서 다음 예와 같이 다차원 배열을 복사하는 것은 적절하지 않을 수 있습니다. ('Object.assign()' 및 스프레드 구문'에서도 마찬가지입니다.)

```undefined
let a = [[1], [2], [3]];
let b = [...a];

b.shift().shift(); //  1
//  Array 'a' is affected as well: [[], [2], [3]]
```

이 StackOverflow 쿼리는 위의 예에 대해 좋은 설명을 제공하지만, 중요한 점은 중첩된 개체를 사용할 때 스프레드 구문을 사용하여 상태 개체를 업데이트할 수 없다는 것입니다.

예를 들어 다음 상태 개체를 고려하십시오.

```undefined
const [messageObj, setMessage] = useState({
  author: '',
  message: {
    id: 1,
    text: ''
  }
});
```

다음 코드 조각은 `text` 필드를 업데이트하는 몇 가지 잘못된 방법을 보여 줍니다.

```js
// Wrong
setMessage(prevState => ({
  ...prevState,
  text: 'My message'
}));

// Wrong
setMessage(prevState => ({
  ...prevState.message,
  text: 'My message'
}));

// Wrong
setMessage(prevState => ({
  ...prevState,
  message: {
    text: 'My message'
  }
}));
```

텍스트 필드를 올바르게 업데이트하려면 원래 개체의 전체 필드/내포된 개체 집합을 새 개체에 복사해야 합니다.

```js
// Correct
setMessage(prevState => ({
  ...prevState,           // copy all other field/objects
  message: {              // recreate the object that contains the field to update
    ...prevState.message, // copy all the fields of the object
    text: 'My message'    // overwrite the value of the field to update
  }
}));
```

동일한 방법으로 상태 개체의 `작성자` 필드를 업데이트하는 방법은 다음과 같습니다.

```js
// Correct
setMessage(prevState => ({
  author: 'Joe',          // overwrite the value of the field to update
  ...prevState.message    // copy all other field/objects
}));
```

메시지 개체가 변경되지 않는다고 가정합니다. 변경되면 다음과 같이 개체를 업데이트해야 합니다.

```js
// Correct
setMessage(prevState => ({
  author: 'Joe',          // update the value of the field
  message: {              // recreate the object that contains the field to update
    ...prevState.message, // copy all the fields of the object
    text: 'My message'    // overwrite the value of the field to update
  }
}));
```

## 다중 상태 변수 또는 하나의 상태 개체

여러 필드 또는 값을 응용 프로그램의 상태로 사용할 경우 여러 상태 변수를 사용하여 상태를 구성할 수 있습니다.

```undefined
const [id, setId] = useState(-1);
const [message, setMessage] = useState('');
const [author, setAuthor] = useState('');
```

또는 객체 상태 변수:

```undefined
const [messageObj, setMessage] = useState({ 
  id: 1, 
  message: '', 
  author: '' 
});
```

그러나 구조가 복잡한 상태 개체(내포된 개체)를 사용할 때는 주의해야 합니다. 다음 예를 고려하십시오.

```undefined
const [messageObj, setMessage] = useState({
  input: {
    author: {
      id: -1,
      author: {
        fName:'',
        lName: ''
      }
    },
    message: {
      id: -1,
      text: '',
      date: now()
    }
  }
});
```

개체 깊숙이 중첩된 특정 필드를 업데이트해야 하는 경우 해당 필드를 포함하는 개체의 키 값 쌍과 함께 다른 모든 개체를 복사해야 합니다.

```js
setMessage(prevState => ({
  input: {
    ...prevState.input,
    message: {
      ...prevState.input.message, 
      text: 'My message'
    }
  }
}));
```

경우에 따라 Relect는 변경되지 않은 필드에 종속된 응용프로그램의 일부를 다시 렌더링할 수 있기 때문에 중첩된 개체를 복제하는 데 많은 비용이 들 수 있습니다.

이러한 이유로, 가장 먼저 고려해야 할 것은 상태 개체를 평평하게 만드는 것입니다. 특히, 반응 설명서는 함께 변경되는 경향이 있는 값을 기준으로 상태를 여러 상태 변수로 분할할 것을 권장합니다.

이 작업을 수행할 수 없는 경우 imfulous.js 또는 immer와 같은 불변의 개체로 작업하는 데 도움이 되는 라이브러리를 사용하는 것이 좋습니다.

## 구성 요소와의 상태 및 사용자 상호 작용 추적

프로덕션 React 앱에서 모든 작업이 예상대로 작동하는지 확인하는 것이 중요합니다. 구성 요소와 관련된 문제를 모니터링하고 추적하고 사용자가 특정 구성 요소와 어떻게 상호 작용하는지를 확인하는 데 관심이 있는 경우 LogRocket을 사용해 보십시오. https://logrocket.com/signup/

![image](https://i2.wp.com/files.readme.io/27c94e7-Image_2017-06-05_at_9.46.04_PM.png?ssl=1)

LogRocket은 웹 애플리케이션용 DVR과 유사하며, 사이트에서 발생하는 모든 것을 문자 그대로 기록합니다. LogRocket React 플러그인을 사용하면 사용자가 앱에서 특정 구성 요소를 클릭하는 사용자 세션을 검색할 수 있습니다. LogRocket을 사용하면 사용자가 구성 요소와 상호 작용하는 방식을 이해하고 렌더링하지 않는 구성 요소와 관련된 오류를 해결할 수 있습니다.

또한 LogRocket은 Redux 스토어의 모든 작업 및 상태를 기록합니다. LogRocket은 헤더 + 본문을 사용하여 요청/응답 내용을 기록하도록 앱에 계측합니다. 또한 페이지에 HTML과 CSS를 기록하여 가장 복잡한 단일 페이지 앱의 픽셀 단위 동영상을 재생성한다.

React 앱 디버그 방법 현대화 – 무료로 모니터링 시작

## useState 사용 규칙

use State는 모든 후크가 따르는 규칙과 동일한 규칙을 준수합니다.

- 상단 레벨에서만 후크 호출
- 반응 함수의 후크만 호출

두 번째 규칙은 따르기 쉽다. 클래스 구성 요소에서 `useState`를 사용하지 마십시오.

```js
class App extends React.Component {
  render() {
    const [message, setMessage] = useState( '' );

    return (
      <p>
        <strong>{message}</strong>
      </p>
    );
  }
}
```

또는 일반 JavaScript 기능(기능 구성 요소 내부에서 호출되지 않음):

```js
function getState() {
  const messageState = useState( '' );
  return messageState;
}
const [message, setMessage] = getState();
const Message = () => {
 /* ... */
}
```

오류가 발생합니다.

첫 번째 규칙은 Retact가 특정 상태 변수에 대한 올바른 값을 얻기 위해 `useState` 함수를 호출하는 순서에 의존하기 때문에 기능 구성 요소 내부에서도 루프, 조건 또는 중첩 함수에서 `useState`를 호출해서는 안 된다는 것을 의미한다.

그런 점에서 가장 일반적인 실수는 `사용 상태` 호출을 조건문으로 포장하는 것입니다(항상 실행되지는 않을 것입니다).

```php
if (condition) { // Sometimes it will be executed, making the order of the useState calls change
  const [message, setMessage] = useState( '' );
  setMessage( aMessage );  
}
const [list, setList] = useState( [] );
setList( [1, 2, 3] );
```

기능 구성 요소는 useState 또는 다른 Hooks에 대한 많은 호출을 가질 수 있습니다. 각 후크는 목록에 저장되며 현재 실행된 후크를 추적하는 변수가 있습니다.

useState가 실행되면 현재 Hook의 상태가 읽기(또는 첫 번째 렌더 중에 초기화됨)된 다음 변수가 다음 Hook을 가리키도록 변경됩니다.

그렇기 때문에 후크 호출을 항상 동일한 순서로 유지하는 것이 중요합니다. 그렇지 않으면 다른 상태 변수에 속하는 값이 반환될 수 있습니다.

일반적인 용어로, 이 작업이 단계적으로 작동하는 예는 다음과 같습니다.

- 후크 목록과 현재 후크를 추적하는 변수를 초기화합니다.
- 구성 요소에 처음으로 응답 전화
- React는 `useState`에 대한 호출을 찾고, 초기 상태에서 새 Hook 객체를 만들고, 이 객체를 가리키도록 현재 Hook 변수를 변경하고, 객체를 Hooks 목록에 추가한 다음, 초기 상태와 업데이트 기능이 있는 배열을 반환합니다.
- React는 `useState`에 대한 다른 호출을 찾아 이전 단계의 작업을 반복하여 새 Hook 개체를 저장하고 현재 Hook 변수를 변경합니다.
- 구성 요소 상태 변경
- React는 상태 업데이트 작업(`useState`가 반환하는 함수에 의해 수행됨)을 처리할 대기열에 전송합니다.
- 반응으로 구성 요소를 다시 렌더링해야 합니다.
- 반응이 현재 후크 변수를 재설정하고 구성 요소를 호출합니다.
- React는 `useState`에 대한 호출을 찾지만, 이번에는 이미 후크 목록의 첫 번째 위치에 후크가 있기 때문에 현재 후크 변수를 변경하고 현재 상태 및 업데이트 기능이 있는 배열을 반환합니다.
- React는 `useState`에 대한 다른 호출을 찾아내고 두 번째 위치에 후크가 존재하므로 현재 후크 변수를 다시 한 번 변경하고 현재 상태 및 업데이트 기능이 있는 배열을 반환합니다.

코드를 읽으려면 `RactFiber`훅스는 후크 밑에서 어떻게 작동하는지 배울 수 있는 수업이다.

## 사용 리듀서 후크

고급 사용 사례의 경우 `사용 상태` 대신 `사용 상태` 후크를 사용할 수 있습니다. 이 기능은 여러 하위 값을 사용하는 복잡한 상태 논리가 있거나 상태가 이전 하위 값에 종속되어 있을 때 특히 유용합니다.

`Reducer` 후크 사용 방법은 다음과 같습니다.

```cpp
const [state, dispatch] = useReducer(reducer, initialArgument, init)
useReducer returns an array that holds the current state value and a dispatch method. This should be familiar territory if you have experience using Redux.
```

useState를 사용하면 상태 업데이터 함수를 호출하여 상태를 업데이트하고, useReduator를 사용하면 디스패치 함수를 호출하여 동작(즉, `type` 이상의 속성을 가진 개체)을 전달합니다.

```css
dispatch({type: 'increase'})
```

일반적으로 작업 개체에는 `payload`(예: `{action: `증가`, 페이로드: 10})도 있을 수 있습니다.

이 패턴을 따르는 액션 개체를 전달할 필요는 없지만, Redux가 대중화한 매우 일반적인 패턴입니다.

## 결론,

`useState`는 함수 구성 요소에서 상태 변수를 가질 수 있는 후크(함수)입니다. 초기 상태를 이 함수에 전달하면 현재 상태 값(초기 상태일 필요는 없음)과 이 값을 업데이트하기 위한 다른 함수가 있는 변수가 반환됩니다.

다음은 기억해야 할 몇 가지 중요한 사항입니다.

- 업데이트 기능이 값을 즉시 업데이트하지 않습니다.
- 이전 값을 사용하여 상태를 업데이트하는 경우 이전 값을 수신하고 업데이트된 값을 반환하는 함수를 전달해야 합니다(예: `setMessage(이전 Val = > 이전 Val + currentVal)`).
- 현재 상태와 동일한 값을 사용하는 경우(Ract는 Object.is을 사용하여 상태를 업데이트하면 Returns가 다시 트리거되지 않습니다.
- 이것과는 다르게.클래스 구성 요소의 setState, `useState`는 상태가 업데이트될 때 개체를 병합하지 않습니다. 그것이 그들을 대신한다.
- useState는 모든 훅스가 하는 것과 동일한 규칙을 따른다. 특히 이러한 기능이 호출되는 순서에 유의하십시오(이러한 규칙을 적용하는 데 도움이 되는 ESLint 플러그인이 있음).