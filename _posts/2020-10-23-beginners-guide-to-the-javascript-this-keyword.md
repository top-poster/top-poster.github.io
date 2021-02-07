---
layout: post
title: "이 키워드 초급 자바스크립트 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/beginners-guide-to-the-javascript-this-keyword.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/beginners-guide-to-the-javascript-this-keyword.png?fit=730%2C487&ssl=1)

프로그래밍 언어의 기본 개념을 이해하는 것은 많은 길을 갈 수 있다. 자바스크립트에서 이 키워드는 그런 초석 중 하나다.

자바스크립트의 this 키워드는 깨지기 어려운 난관이 될 수 있다. 어떤 초보자는 단어와 씨름하고, 다른 초보자는 그 역동적인 성격을 가지고 있다.

이 튜토리얼에서는 JavaScript의 `this` 키워드를 해독하고, 그렇게 함으로써 상당히 복잡한 디버깅 문제를 연습할 수 있도록 도와줍니다.

## 이 키워드는 무엇인가?

몇 줄의 코드 내에서 매우 많은 일들이 벌어지고 있는 가운데, 해당 코드를 실행하기 위해 대답해야 하는 두 가지 기본적인 질문이 있습니다.

- 코드가 실행되는 위치(예: 함수 또는 전역)는 어디입니까?
- 정확히 어떤 코드(예: 배열 방식 또는 객체 방식)인가?

이 키워드는 실행 컨텍스트의 일부이기 때문에 코드가 어디에 있는지 확인하는 데 도움이 됩니다. 실행 컨텍스트는 코드 조각의 인접성을 정의합니다. 불행히도, 이름이 제대로 지정되지 않아 새로운 JavaScript 개발자들이 혼란을 겪게 됩니다.

기본적으로 모든 코드는 함수 본체와 같은 로컬 실행 컨텍스트와 반대로 전역 실행 컨텍스트에서 실행됩니다. 글로벌 실행 컨텍스트를 여러분의 안뜰로, 로컬 실행 컨텍스트를 여러분의 집이라고 생각하십시오.

범위는 실행 컨텍스트 내에서 코드의 가시성/접근성을 정의하는 범위입니다. 예를 들어 변수 cats가 함수 catmaker()의 범위를 벗어났다고 하는 것은 catmaker()가 catmaker()의 스코프 체인에 없기 때문에 함수 catmaker()가 변수 cats에 액세스할 수 없음을 의미한다. 스코프 체인은 특정 실행 컨텍스트에서 액세스할 수 있는 모든 변수를 정의합니다.

받아들여야 할 많은 정보들이지만 이것을 이해하기 위해서는 이것을 이해해야 한다. 만약 여러분이 여전히 따라가기 위해 애쓰고 있다면, 걱정하지 마세요. 여러분은 혼자가 아닙니다. 다시 읽어보거나 리소스 섹션으로 이동하여 유용한 가이드를 찾아보십시오.

## 이것을 어디서 찾을 것인가?

이 키워드를 접하게 될 만한 곳을 살펴보자.

### 글로벌 실행 컨텍스트

전역 실행 컨텍스트는 기본 실행 컨텍스트이며, 이 컨텍스트 내에 로컬 실행 컨텍스트가 있습니다. 코드를 작성한다면 각 컨텍스트는 다음과 같이 정의될 것입니다.

```js
let myName = "John Doe";
// global execution context

function sayName() {
   // local execution context
   console.log(myName);
}
```

전역 실행 환경에서 this 값은 브라우저의 window object와 동일합니다. 창 객체가 브라우저의 탭을 나타내는 것으로 생각하십시오(창 객체에 대한 모든 종류의 고급 세부 정보가 포함되어 있기 때문입니다). 전역 실행 컨텍스트에서 "this"가 "window" 개체와 동일한지 확인하려면 다음 코드를 실행하면 됩니다.

```coffeescript
console.log(this === window); // prints true
```

### 일반 함수

함수에는 자체 실행 컨텍스트와 범위가 있지만 전역 실행 컨텍스트에서 함수를 정의하면 다시 이 값이 창 개체와 동일합니다.

```js
function someFunc() {
  return this;
}

someFunc() === window; // returns true
```

이것은 바람직할 수도 있고 바람직하지 않을 수도 있다. 이 문제를 방지하려면 JavaScript에서 엄격한 모드라는 것을 사용하도록 설정할 수 있습니다. 이것은 문자 그대로 자바스크립트가 더 많은 오류를 발생시키고, 궁극적으로 더 예측 가능한 코드를 발생시킨다. strict 모드를 활성화하면 `this`가 정의되지 않은 상태로 나타납니다.

```js
function someFunc() {
  "use strict"
  console.log(this);
}

someFunc(); // returns undefined
```

함수에 대한 `이`를 다른 함수로 변경하려는 경우도 있을 수 있으며, 그 함수의 맥락을 변경하려는 경우도 있다. 이렇게 하려면 `call()the`ap()the`ap(적용) 및 `bind()`bind() 함수를 사용합니다. 후자를 시작으로 `bind()` 함수는 사용자가 제공하는 `this` 값과 함수를 바인딩하고 새 함수를 반환합니다.

```js
const obj = { 
   message: "hello world"
}

function printMessage() {
   console.log(this.message);
};

const boundPrintMessage = printMessage.bind(obj);
printMessage(); // prints undefined
boundPrintMessage(); // prints "hello world"
```

bind() 기능은 재사용 가능한 코드를 생성하고 몇 가지 까다로운 상황을 해결하는데 도움을 줄 수 있는 매우 강력한 도구입니다.

이 값에 바인딩된 새 함수를 반환하지 않으려면 호출() 또는 적용()을 사용해야 합니다. 호출과 적용은 모두 호출을 제외하고 사용자가 제공하는 이 값인 함수를 호출할 수 있으며 호출을 사용하면 매개 변수를 함수에 전달할 수 있으며 적용하면 해당 매개 변수를 배열로 전달할 수 있습니다.

```js
const user = {
 name: 'John Doe'
}

function printUser(likes) {
  console.log(`My name is ${this.name}, and I like ${likes}`)
};

printUser.call(user, 'apples')
printUser.apply(user, ['apples'])
```

### 화살표 함수

ES6 지방 화살표 함수로도 알려져 있는 화살표 함수는 몇 가지 중요한 예외와 함께 일반 함수와 거의 동일하다. 하나의 경우 일반 함수와는 달리 이 값은 윈도우 객체로 기본값이 되지 않는다. 객체에 화살표 기능을 선언하여 이를 입증할 수 있습니다.

```js
const obj = {
  message: "hello world",
  arrowFunc: () => console.log(this.message),
  plainFunc: function() {
   console.log(this.message);
  }
}

obj.arrowFunc() // prints undefined
obj.plainFunc() // prints hello world
```

이 경우 화살표 함수는 `이것`이라는 자체 값이 없으므로 화살표 함수를 개체 방법으로 사용하지 않는 것이 좋습니다. bind()는 함수의 this(이) 값을 변경할 수 있으므로 bind()를 사용하여 이 동작을 피할 수 있다고 생각할 수 있지만, 이는 작동하지 않습니다.

```js
const obj = {
  message: "hello world",
  arrowFunc: () => console.log(this.message),
  plainFunc: function() {
   console.log(this.message);
  }
}

const boundArrowFunc = obj.arrowFunc.bind(obj);
boundArrowFunc(); // prints undefined
```

사용자가 정의하는 범위에서 함수를 실행할 수 있도록 호출을 "apply"와 "apply"로 설정했지만 화살표 함수에서 "this"의 값은 정의된 범위에 따라 다릅니다.

### 반

ES6 클래스는 항상 엄격한 모드로 작동하므로 클래스에 대한 this 값이 window 개체와 동일하지 않습니다. 하지만 아시다시피 ES6 클래스는 일종의 구문 슈가이기 때문에 전통적인 기능 스타일로 ES6 클래스를 쓴다면 이 값이 윈도우 객체가 될 것입니다.

수업에서 이것의 가치는 어떻게 부르느냐에 달려 있다. 따라서 `이` 값을 클래스의 인스턴스로 설정할 수 있습니다.

```js
class Person {
  constructor() {
   this.name = "John Doe"
   this.sayName = this.sayName.bind(this); // Try running the code without this line
 } 
   sayName() {
    console.log(this.name);
  }
}

const somePerson = new Person();
somePerson.sayName();
const sayName = somePerson.sayName;
sayName();
```

반응을 사용하는 데 익숙한 경우 클래스 구성 요소를 쓸 때 하드 바인딩이라고 하는 이 패턴에 익숙할 수 있습니다. 이벤트 핸들러의 "this" 값을 클래스의 값에 바인딩하지 않으면 "this" 값이 정의되지 않는 경향이 있습니다.

## 이 문제를 해결할 수 있는 방법을 결정하는 방법

가장 흔한 경우를 몇 가지 살펴봤지만, 물론, 여러분이 직면할 수 있는 다른 상황도 있습니다. 아래 팁을 참조하여 주어진 시나리오에서 `이`가 해결할 수 있는 방법을 결정하십시오.

- 코드가 본문/블록 안에 없으면 `this`는 브라우저의 `window` 개체와 동일합니다.
- 코드를 객체 방법으로 호출하고 점 표기법으로 표현하면 왼쪽을 보고 왼쪽에 있는 것이 "이"입니다.
- 코드가 함수 내부에 있으면 this는 strict 모드가 활성화되지 않았을 때의 window 개체와 같습니다. strict 모드를 활성화하면 `this`가 정의되지 않습니다.
- 함수가 객체 메소드인 경우, 일반 함수인 경우, `이`는 함수가 정의된 객체로 결정되는 반면 화살표 함수는 폐쇄 실행 컨텍스트를 참조합니다.
- 함수에 대해 `this`의 값을 정의하려면 `call()`apply()또는 `bind()`를 사용해야 합니다.

## 결론

복잡한 프레임워크와 라이브러리와 논쟁을 시작할 때 JavaScript의 기본 사항을 이해하는 것이 큰 도움이 될 것입니다. 이상하지 않은 오류 없는 코드를 디버깅하고 쓰는 방법을 배우려면 이 키워드 같은 주제에 대한 확실한 이해가 필요하다.

당장 이해하지 못하더라도 걱정하지 마십시오. 이 복합체가 가라앉는 데 시간이 걸릴 수 있는 주제입니다. 명확한 이해를 얻으려면 코드를 작성하고, 이 게시물에서 설명한 상황에 대한 느낌을 얻어야 하며, 시행착오로 인해 발생하는 문제를 해결해야 합니다. 그것은 여러분의 이해를 굳히고 다음 단계로 가는 데 도움이 될 것입니다.

## 자원.

- "JavaScript의 `실행 컨텍스트`는 정확히 무엇입니까?" — 실행 컨텍스트, 범위 지정 및 기타 많은 관련 용어를 다양한 방법으로 설명하는 스택 오버플로 응답 호스트. 컨텍스트 및 범위에 대해 자세히 알아보려면 체크 아웃하십시오.
- MDN의 this 키워드에 대한 문서 - 다양한 예와 주의 사항을 살펴보는 this 키워드에 대한 기술적 고려. 이 문서는 참조 가이드로 책갈피로 사용할 가치가 있습니다.
- "JavaScript에서 변수, 범위 및 호이스팅 이해" — 디지털 오션에서 Tania Rascia의 모든 변수를 심층적으로 살펴봅니다. JavaScript에 대한 사전 요구 사항 지식을 충족하지 못하는 경우 이 게시물을 확인해 볼 가치가 있습니다.