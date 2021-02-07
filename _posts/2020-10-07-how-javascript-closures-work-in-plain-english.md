---
layout: post
title: "JavaScript 폐쇄의 작동 방식, 쉬운 영어"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/how-javascript-closures-work-in-plain-english.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/how-javascript-closures-work-in-plain-english.png?fit=730%2C487&ssl=1)

JavaScript는 간단한 랜딩 페이지에서 프로덕션 등급의 전체 스택 애플리케이션에 이르는 모든 것을 구축하는 데 사용할 수 있는 널리 채택된 언어입니다. 자바스크립트와 프로그래밍이 일반적으로 진화함에 따라 개발자들은 객체 지향 프로그래밍(OOP) 패러다임이 대부분의 사용 사례에 바람직하지 않다는 것을 깨닫게 되었다. 기능적 프로그래밍은 OOP와 관련된 많은 문제의 해결책으로 떠올랐다.

폐쇄는 기능 프로그래밍의 세계에서 널리 논의되는 주제이지만, 종종 느슨하게 기술 전문용어로 정의됩니다. 우리는 여기서 자바스크립트 폐쇄가 어떻게 일반인의 관점에서 작동하는지를 설명하기 위해 최선을 다할 것이다.

이 튜토리얼을 완료하면 다음을 이해할 수 있습니다.

- 폐쇄 식별 방법
- 실행 컨텍스트 및 호출 스택과 관련하여 폐쇄란 무엇이며, 폐쇄의 동작 방식
- 폐쇄에 대한 일반적인 사용 사례

## JavaScript 폐쇄 이해

먼저 폐쇄가 어떻게 보이는지 보여드리겠습니다.

```js
function makeCounter() {
  let count = 0;
  return function increment() {
    count += 1;
    return count;
  };
};

const countIncrementor = makeCounter();
countIncrementor(); // returns 1
countIncrementor(); // returns 2
```

스스로 코드를 실행해 보십시오. 엄밀히 말해 makeCounter라는 함수는 증분이라는 또 다른 함수를 반환한다. 이 증가 함수는 makeCount 함수가 실행된 후에도 count 변수에 접근할 수 있다. 여기서 폐쇄의 일부는 "카운트" 변수이다. "카운트" 변수가 정의될 때 "증분" 함수가 사용가능하며, 심지어 "makeCounter"가 끝난 후에도 말이다. 또 다른 부분은 `증분` 기능이다.

여러분이 집을 둘러싸고 있는 집과 정원을 가지고 있다고 상상해 보세요. 정원의 문을 열고 닫으면 다시 열 수 없습니다. 정원에 접근할 수 없게 됩니다. 배가 고픈데, 다행히 정원에는 오렌지 나무와 사과 나무가 있어요. 당신은 작은 가방을 들고 오렌지와 사과를 따고 집 안으로 돌아간다. 다시 나갈 수 없다는 걸 명심해

이제, 일단 여러분이 집 안에 들어가면, 여러분은 가방에서 오렌지나 사과를 꺼내서 여러분이 다시 배가 고플 때마다 그것을 먹을 수 있습니다. 이 예에서 작은 가방은 클로즈업입니다. 클로저는 여러분이 정원에 있을 때, 심지어 여러분이 집 안에 있을 때 다시 밖으로 나갈 수 없을 때에도 이용할 수 있었던 모든 변수와 기능들을 포함합니다.

코드로 표시되는 방법을 살펴보겠습니다.

```js
function makeFruitGarden() {
  let fruits = ['apple', 'orange'];
  return function() {
    return fruits.pop();
  };
};

const consumeFruit = makeFruitGarden();
consumeFruit(); // returns orange
consumeFruit(); // returns apple
```

makeFruitGarden이 실행되면 반환되는 함수에 과일 변수를 사용할 수 있기 때문에 과일 변수와 내부 함수는 폐쇄가 된다. `소비과일`이 실행될 때마다 `팝()`이 사용되기 때문에 `과일` 배열의 마지막 요소인 `과일`이 반환된다. 두 가지 과일을 모두 다 먹고 나면 먹을 것이 남아 있지 않을 것이다.

## 어휘 범위 이해

폐쇄를 진정으로 이해하기 위해서는 "범위"라는 용어에 익숙해져야 한다. 어휘적 범위는 현재 환경에 대한 멋진 용어입니다.

다음 예제에서 myName이라는 변수의 범위를 글로벌 범위라고 합니다.

```js
// global scope
const myName = "John Doe"

function displayName() {
   // local/function scope
   console.log(myName);
};

displayName()
```

var가 블록 범위가 아닌 방법과 const/`let`이 어떻게 되는지 읽을 때 이 개념이 참조되는 것을 본 적이 있을 것이다. JavaScript에서 함수는 항상 자체 범위를 생성합니다. 코드 예제와 같이 이를 로컬 또는 함수 범위라고 합니다.

주목해 왔다면 마이네임과 디스플레이네임이 폐쇄의 한 부분이라고 생각할 수 있다. 당신이 옳을 거예요! 하지만 이 함수와 변수는 글로벌 범위에 존재하기 때문에 폐쇄라고 부르는 것은 별로 가치가 없습니다.

JavaScript에는 다양한 유형의 스코프가 있지만 클로저에 대해서는 다음과 같은 세 가지 스코프가 있습니다.

- 글로벌 범위는 모든 사람이 사는 기본 범위입니다. 그것을 당신의 거리라고 생각하세요.
- 외부 함수 범위는 함수를 반환하는 함수입니다. 그것은 그 나름의 범위가 있다. 그것을 당신의 정원이라고 생각하세요.
- 내부/로컬 함수 범위는 폐쇄가 되는 반환된 함수입니다. 당신의 집이라고 생각하세요.

이제 몇 가지 사용 사례에 대해 살펴보겠습니다.

## 폐쇄에 대한 일반적인 사용 사례

### 카레링

기능 카레링은 기능 프로그래밍에서 또 다른 강력한 개념이다. JavaScript에서 currated 기능을 구현하려면 closure를 사용합니다.

함수를 커링하는 것은 함수를 변환하는 것으로 설명될 수 있으며, `추가(1, 2, 3)`에서 `추가(2)(3)까지`와 같이 실행됩니다.

```js
function add(a) { 
  return function(b) {
    return function(c) {
      return a + b + c;
    };
  };
};

add(1)(2)(3) // returns 6
```

add 함수는 단일 인수를 사용한 다음 중첩된 두 함수를 차례로 반환합니다. 카레의 목적은 여러 가지 주장을 받아들여 결국 하나의 가치로 귀결되는 것이다.

### 고차 함수

고차 함수의 목표는 함수를 인수로 사용하여 결과를 반환하는 것입니다. 맵(map)과 reduce(reduce)와 같은 배열 방식은 고차 함수의 예이다.

```js
const arrayOfNumbers = [1, 2, 3];
const displayNumber = (num) => {
  console.log(num);
}
arrayOfNumbers.forEach(displayNumber)
```

`어레이. 프로토타입`입니다여기서 각 고차 함수는 `displayNumber`를 인수로 받아들인 다음 `arrayOfNumbers`의 각 요소에 대해 실행한다. Vue 또는 React와 같은 UI 프레임워크를 사용한 경우 고차 구성 요소에 익숙할 수 있습니다. 고차 구성 요소는 고차 기능과 기본적으로 동일합니다.

그렇다면 고차 기능과 카레의 차이점은 무엇일까요? 고차 함수는 인수가 값을 반환할 때 함수를 사용하는 반면, 큐레이드 함수는 결과로 함수를 반환하고, 결과적으로 값으로 이어진다.

### DOM 요소 관리자

이것은 DOM 요소의 속성을 가져오고 설정하는 데 자주 사용되는 일반적인 설계 패턴입니다. 다음 예제에서는 요소를 스타일링할 요소 관리자를 만듭니다.

```js
function makeStyleManager(selector) {
    const element = document.querySelector(selector);
    const currentStyles = {...window.getComputedStyle(element)};
 
    return {
        getStyle: function(CSSproperty) {
            return currentStyles[CSSproperty];
        },
        setStyle: function(CSSproperty, newStyle) {
            element.style[CSSproperty] = newStyle;
        },
    };
};

const bodyStyleManager = makeStyleManager('body');
bodyStyleManager.getStyle('background-color'); // returns rgb(0,0,0)
bodyStyleManager.setStyle('background-color', 'red'); // sets bg color to red
```

makeStyleManager는 요소 변수와 현재 스타일 변수와 함께 폐쇄의 일부인 두 가지 기능에 액세스할 수 있는 객체를 반환합니다. makeStyleManager의 실행이 끝난 후에도 getStyle과 setStyle 함수는 변수에 액세스할 수 있다.

## 결론

자바스크립트 폐쇄는 전문적인 경험을 가진 개발자들에게조차 이해하기 어려울 수 있다. 폐쇄를 이해하면 궁극적으로 더 나은 개발자가 될 것입니다.

이제 코드베이스에서 사용 중 이상하거나 말이 되지 않는 폐쇄를 식별할 수 있어야 합니다. 폐쇄는 기능 프로그래밍에서 중요한 개념이며, 이 가이드가 당신이 그것을 마스터하기 위한 여정에 한 걸음 더 나아가는 데 도움이 되었기를 바랍니다.

### 추가 읽기

- "JavaScript 클로저를 전혀 이해하지 못했습니다." - 클로저가 작동하는 방식을 이해하는 심층적인 기술 가이드입니다. 실행 컨텍스트, 통화 스택, 어휘 환경 등과 관련하여 폐쇄를 이해하고자 하는 사람에게 적합합니다.
- JavaScript 닫힘에 대한 MDN 참조 — 메모리를 빠르게 브러싱해야 할 경우 닫힘(일부 일반적인 함정 포함)에 대한 빠른 참조 가이드
- "JavaScript 인터뷰 마스터: Closure란 무엇인가?" – Closure는 Eric Elliott에 의해 설명됨. 기능적 프로그래밍 개념을 이해하는 개발자에게 매우 적합합니다.