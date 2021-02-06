---
layout: post
title: "Immer 및 Inmovable.js: 그들은 어떻게 비교하나요?"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/immer-immutable.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/immer-immutable.png?fit=730%2C487&ssl=1)

불변성은 프로그래밍에서 새로운 개념이 아니다. 순수 함수형 프로그래밍과 같은 프로그래밍 패러다임의 기초가 된다. 전체 아이디어는 데이터가 생성된 후 데이터에 대한 직접적인 변경을 피하는 것입니다.

다음은 이 기사에서 논의할 내용에 대한 요약입니다.

- JavaScript의 불변성
- Immer 및 Inmovable.js 라이브러리에 대한 소개
- Immer와 Inmovable.js의 비교

## JavaScript의 불변성

문자열과 숫자와 같은 자바스크립트 원리는 불변의 것으로 알려져 있다. 예를 들어 문자열은 어떤 메서드나 작업으로도 변경할 수 없기 때문입니다. 새 문자열만 만들 수 있습니다.

아래 변수를 살펴보겠습니다.

```undefined
var name = "mark cuban"
name = "John Steve"
```

이름 변수가 다른 문자열에 재할당되었기 때문에 데이터를 변경할 수 있다고 주장할 수 있지만, 이 경우는 그렇지 않습니다.

재배정은 불변성과 다릅니다. 변수가 재할당됐음에도 `Eze Sunday` 문자열이 존재한다는 사실을 바꾸지 않았기 때문이다. 3에 13을 더해도 원래 변수 13이 바뀌지 않거나, 18세가 되더라도 이전에 17살이었던 사실이 바뀌지 않는 것과 같은 이유입니다.

변수가 재할당되더라도 불변 데이터는 그대로 유지됩니다.

우리는 위의 예로부터 원시성은 불변의 것이라는 것을 확인했지만, 그것이 이야기의 끝은 아닙니다. 자바스크립트에는 돌연변이가 가능한 데이터 구조가 있다. 그 중 하나가 배열입니다.

이를 입증하기 위해 아래 나온 것처럼 변수를 선언하고 값을 빈 배열로 설정해 보겠습니다.

```undefined
let arrOne = []
```

".push() 기능을 사용하여 위의 배열의 내용을 쉽게 업데이트할 수 있습니다.

```css
arrOne.push(2)
```

이렇게 하면 데이터 2가 어레이 끝에 추가되고 이전에 있던 원래 어레이가 변경됩니다.

### Immer 및 Inmovable.js 라이브러리를 소개합니다.

JavaScript는 데이터가 단독으로 불변하도록 작성되지는 않았지만 데이터 세트의 변경 사항을 쉽게 추적하거나 기록하기 위해 불변 어레이나 맵이 필요한 경우가 있습니다.

이는 특히 상태 및 소품을 다룰 때 리액트 프레임워크에서 분명하게 드러납니다. 바로 여기서 불변성 도서관이 시작됩니다. 이러한 라이브러리를 사용하면 응용 프로그램을 최적화하여 응용 프로그램의 변경 사항을 쉽게 추적할 수 있습니다. 이 기사에서는 두 개의 주요 불변성 라이브러리를 살펴볼 것입니다. 즉, Immer 및 Inmovable.js입니다.

## 임머

Immer는 응용 프로그램에서 사용할 수 있는 많은 불변성 라이브러리 중 하나입니다. 공식 웹사이트에 따르면, Imer는 쓰기 시 복사 메커니즘을 기반으로 한다. 전체 구상은 현 상태에 대한 대리 역할을 하는 임시 `초안 상태`에 대한 변경 사항을 적용하는 데 초점이 맞춰져 있다. Imer는 데이터와의 상호 작용을 용이하게 하는 동시에 불변성과 함께 제공되는 모든 이점을 유지할 수 있도록 지원합니다.

### 설치

프로그램에서 Immer를 즉시 사용하려면 다음 명령을 사용하십시오.

```js
<script src="https://cdn.jsdelivr.net/npm/immer"></script>

//  It can also be installed in your application using NPM;
npm install immer
Or with yarn;
yarn add immer
```

### 사용법

Imer에서는 대부분의 불변성 작업이 기본 함수의 도움을 받아 수행됩니다.

```coffeescript
produce(currentState, producer: (draftState) => void): nextState
```

이 기능은 현재 상태(current state)와 초안 상태(draft state)를 취하며, 초안 상태의 변경사항을 반영하도록 다음 상태를 업데이트합니다.

예

아래 코드를 고려하십시오.

```js
import produce from "immer"
const baseState = [
    {
        todo: "Learn typescript",
        done: true
    },
    {
        todo: "Try immer",
        done: false
    }
]
```

다음과 같이 Imer 기본 기능을 사용하여 새 데이터를 상태에 추가할 수 있습니다.

```php
const nextState = produce(baseState, draftState => {
    draftState.push({todo: "Tweet about it"})
    draftState[1].done = true
})
```

이 경우 기초국가는 손대지 않고 다음국가는 초안국가의 변화를 반영해 업데이트된다. 당신은 이곳의 공식 웹사이트에서 임머에 대해 더 자세히 알 수 있습니다.

## 불변.js

Inminfulous.js는 불변 라이브러리를 찾을 때 고려해야 할 또 다른 옵션이다. Implicant.js는 Immer와 같은 목적으로 사용되지만 다른 접근 방식을 취한다. 맵과 리스트와 같은 데이터 구조를 위한 API를 제공합니다.

### 설치

Invalutive.js는 npm을 사용하여 설치할 수 있습니다.

```coffeescript
npm install immutable
```

예

우리는 설치된 패키지에서 map을 요구하여 다음과 같이 활용하여 Implicable.js로 매핑 작업을 수행할 수 있다.

```undefined
const { Map } = require('immutable');

const map1 = Map({ a: 1, b: 2, c: 3 });

const map2 = map1.set('b', 50);

map1.get('b') + " vs. " + map2.get('b'); // 2 vs. 50
```

위의 예에서, 우리의 객체 `{a:1, b:2,c:3 }`은 `지도()` 함수로 싸여 있다. "우리는 데이터를 Invariable 상태로 유지하면서 get 및 set 작업을 계속 수행했다.

객체 외에도 아래와 같이 `List` 기능을 사용하여 불변 어레이를 생성할 수 있습니다.

```php
List(['apple','orange','grape'])
```

위의 내용은 `List` 함수를 사용하는 Implicable.js의 어레이 구현입니다.

fromJS 함수(fromJS function)는 객체와 배열을 직접 불변 데이터로 변환하여 맵({})과 리스트([]) 함수로 싸야 하는 필요성을 우회하도록 도와준다.

```bash
fromJS(['apple','orange','grape'])
```

위에서는 배열을 직접 불변 데이터로 변환합니다.

### Immer v.Immetable.js: 어떤 것을 선택해야 합니까?

여기 중요한 질문이 있습니다. 이 두 도서관 중 어떤 것을 선택해야 하는가? 먼저 이러한 라이브러리에 대한 장점과 단점을 개별적으로 나열해 보겠습니다. 임머 씨부터 시작하죠

## 임머와 함께 제공되는 이점

Implicable.js와 같은 다른 라이브러리를 사용하는 대신 Imper를 사용하면 많은 이점이 있습니다. 그 중 일부는 다음과 같습니다.

- 보일러 플레이트 감소: 임머를 사용하면 별도의 상용구 없이 보다 정확한 코드를 작성할 수 있으므로 코드베이스가 전반적으로 감소합니다.
- 불변성은 일반적인 JavaScript 데이터 유형 및 구조와 함께 작동합니다. Immer는 새로운 API를 배울 필요 없이 일반 JavaScript, 객체, 지도 및 어레이를 사용할 수 있습니다.
- Imer는 문자열 기반 경로 선택기가 없는 강력한 유형입니다.
- 패치 지원

### 도서관으로서의 임머에 대한 단점

임머를 사용하는 것을 어렵게 만드는 몇 가지가 있다. 우선 Imer를 사용하려면 프록시 개체를 지원하는 환경이 필요합니다. 또한 Immer는 클래스 인스턴스와 같은 복잡한 개체 유형에 대한 지원을 제공하지 않습니다.

### Immetable.js와 함께 제공되는 이점

Immer와 마찬가지로 Implicable.js에도 장점이 있습니다. 그 중 일부는 다음과 같습니다.

- Immer는 다른 일반적인 JavaScript 데이터 구조 외에도 JavaScript의 기본이 아닌 데이터 구조를 제공합니다. 그 중 일부는 주문된 지도와 기록을 포함한다.
- 불변,js를 사용하면 감산기에서 변경된 정확한 데이터를 알 수 있으므로 개발 작업이 쉬워집니다.
- Invalutive.js는 다른 불변 라이브러리와 비교할 때 기록 속도로 데이터를 씁니다.

### Importable.js로 다운사이드 처리

Implicable.js는 데이터 쓰기가 빠르지만 읽기 작업을 수행할 때 속도가 훨씬 느립니다.

또한 Implicable.js는 기본 작업을 수행하기 위해 새롭고 때로는 복잡한 구문과 API를 학습하도록 강제합니다. 예를 들어, 어레이에 추가 데이터를 추가하려면 자바스크립트에서는 전통적인 방식이 아닌 `.set()` 메서드를 사용해야 합니다.

Inmoval.js는 또한 응용 프로그램의 모든 곳에서 고유한 구성 유형을 사용하도록 강제합니다. 즉, 생성한 모든 불변 컬렉션에 대해 기존 컬렉션보다 적절한 Inmovable.js 컬렉션을 사용해야 합니다. 특히 이러한 컬렉션을 사용하지 않는 다른 코드베이스로 코드를 마이그레이션할 때 많은 스트레스를 받을 수 있습니다.

반면에 임머는 훨씬 더 융통성 있게 거의 같은 것을 제공한다. 이것이 많은 개발자들이 그것을 고수하는 이유이다. 이 페이지에서 이미 익숙한 클래식 객체로 컬렉션을 만들 수 있습니다.

## 결론

응용 프로그램에서 더 빠른 쓰기 속도가 필요한 경우 Implicable.js로 이동할 수 있습니다. 반면 기존 JavaScript 데이터 구조와 개체 유형을 고수하면서 코드를 적게 쓰려면 Imer가 적합합니다.

전반적으로 Immer 및 Inmovable.js는 모두 응용 프로그램에서 사용해야 하는 훌륭한 라이브러리입니다.