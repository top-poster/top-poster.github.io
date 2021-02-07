---
layout: post
title: "현재 사용할 수 있는 6가지 최첨단 JavaScript 기능"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/javascript-logo.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/javascript-logo.png?fit=730%2C487&ssl=1)

자바스크립트 프로그래머가 되기에 매우 흥미로운 시기입니다. 웹 기술은 더 빠른 속도로 발전하고 있으며, 브라우저 공급업체들은 더 이상 새롭고 혁신적인 기능을 즉시 구현하는 것을 꺼리지 않습니다. 이러한 개발의 변화는 프로그래머들이 자신의 역할에서 경쟁력을 유지하기 위해 자신의 기술을 지속적으로 업데이트해야 한다는 것을 의미합니다.

이 기사에서는 최신 브라우저에 의해 최근에 구현된 6가지 ES2020 및 ES2021 기능에 대해 살펴보고, 그것들이 어떻게 자바스크립트 개발자들이 오류 발생률을 낮추고 보다 효율적인 코드를 작성하도록 지원하는지 알아보겠습니다.

## 빅 인트

자바스크립트에서 큰 정수를 처리할 때, `숫자` 유형은 253보다 큰 정수 값을 안전하게 나타낼 수 없기 때문에 종종 타사 라이브러리를 사용해야 한다.

다음 예를 고려하십시오.

```cpp
console.log(9999999999999999);    // => 10000000000000000
```

이 코드에서 999999999999999999는 100000000000000으로 반올림되는데, 이는 숫자형이 지원하는 가장 큰 정수보다 크기 때문이다. 주의하지 않을 경우, 이러한 반올림은 프로그램의 보안을 손상시킬 수 있습니다.

다른 예는 다음과 같습니다.

```cpp
// notice the last digit
9800000000000007 === 9800000000000008;    // => true
```

다행히 ECMA스크립트는 최근 숫자보다 큰 정수를 쉽게 표현할 수 있는 빅인트(BigInt) 데이터 유형을 도입했다.

정수에 n을 더하면 빅인트를 만들 수 있다.

비교:

```cpp
console.log(9800000000000007n);    // => 9800000000000007n
console.log(9800000000000007);     // => 9800000000000008
```

생성자를 사용할 수도 있습니다.

```undefined
BigInt('9800000000000007');    // => 9800000000000007n
```

이제 해결 방법을 사용할 필요 없이 표준 JavaScript의 큰 정수에 대해 산술 연산을 수행할 수 있습니다.

```cpp
9999999999999999 * 3;      // => 30000000000000000

// with BigInt, integer overflow won’t be an issue
9999999999999999n * 3n;    // => 29999999999999997n
```

Number와 BigInt는 서로 다른 두 가지 데이터 유형이며 이를 엄격한 평등 운영자와 비교할 수 없다는 점을 이해해야 합니다.

```undefined
5n === 5;     // => false
 
typeof 5n;    // => bigint
typeof 5;     // => number
```

그러나 다음과 비교하기 전에 피연산자를 동일한 유형으로 암시적으로 변환하므로 동등 연산자를 계속 사용할 수 있습니다.

```undefined
5n == 5; // => true
```

BigInt에 대한 산술 연산은 Number와 동일하게 수행할 수 있습니다.

```cpp
50n + 30n;    // => 80n
50n - 30n;    // => 20n
50n * 20n;    // => 1000n
50n / 5n;     // => 10n
56n % 10n;    // => 6n
50n ** 4n;    // => 6250000n
```

증가, 감소 및 단항 부정 연산자도 예상대로 작동합니다. 단ary 플러스(+) 연산자는 예외이며 빅인트에 적용하면 Type Error(유형 오류)

```undefined
let x = 50n;
++x;    // => 51n
--x;    // => 50n
 
-50n;    // => -50n
+50n;    // => TypeError: Cannot convert a BigInt value to a number
```

## 무효화 병합 연산자

ES2020은 자바스크립트 언어에 또 다른 단락 연산자(nullish combersion)를 추가한다.`) 연산자. 이 연산자는 왼쪽 피연산자가 거짓이 아닌 null(`null` 또는 `정의되지 않은`)인지 확인하는 점에서 기존 단락 연산자와 다르다.

다른 말로 하자면, `??왼쪽 피연산자가 `ㄹ` 또는 `ㄹ`인 경우에만 오른쪽 피연산자를 반환합니다.

```js
null ?? 2; // => 2
undefined ?? 2; // => 2

0 ?? 2; // => 0
false ?? true; // => false
```

반면 논리 OR(||) 연산자는 왼쪽이 0, -0, 0n, 거짓, (빈 문자열), Null, 정의되지 않음 또는 NaN이면 오른쪽 피연산자를 반환합니다. 비교:

```undefined
null || 2;       // => 2
undefined || 2;  // => 2

0 || 2;           // => 2
false || true;    // => true
```

속성 또는 변수의 기본값을 설정할 때 특히 유용합니다. 예를 들어:

```js
function Config(darkMode)  {
    this.darkMode = darkMode ?? true;
    // …
}
 
new Config(null);     // => {darkMode: true}
new Config();         // => {darkMode: true}
new Config(false);    // => {darkMode: false}
```

지정된 값이 null이거나 값이 지정되지 않은 경우 `구성` 생성자는 `다크 모드` 속성에 대한 기본값을 제공합니다.

`???`는 DOM API를 사용할 때도 유용합니다.

```coffeescript
// querySelector() returns null when the element doesn’t exist in the document
const elem = document.querySelector('elem') ?? document.createElement('elem');
```

사용 시 유의하십시오.다른 단락 연산자가 식에 있을 경우 괄호로 평가 순서를 나타내야 하며 그렇지 않으면 코드가 오류를 발생시킵니다.

괄호는 코드의 가독성에도 도움이 됩니다.

```undefined
false || (true ?? true);   // no error
false || true ?? true;     // => SyntaxError
```

## 약속. 아무거나()

ES2015는 약속의 대상을 약속.모두()와 약속.레이스()의 두 가지 방법으로 소개했다. ES2021은 Promise.any()를 추가하여 JavaScript 비동기 기능을 더욱 향상시킨다. 이 새로운 방법은 주어진 허용 가능한 약속 중 하나가 이행할 때 이행하는 약속을 반환하거나, 모든 약속이 거부될 경우 거부한다.

작동 방식은 다음과 같습니다.

```coffeescript
const promise = Promise.any([
    Promise.reject('Error'),
    fetch('https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_92x30dp.png', {mode: 'no-cors'}).then(() => 'google.com'),
    fetch('https://en.wikipedia.org/static/images/project-logos/enwiki.png', {mode: 'no-cors'}).then(() => 'wikipedia.org'),
    fetch('https://s.w.org/images/home/swag_col-1.jpg?1', {mode: 'no-cors'}).then(() => 'w.org')
]);
 
promise.then((fastest) => {
    // the first promise that fulfills
    console.log(fastest);
}).catch((error) => {
    console.log(error);
});
```

이 코드는 세 가지 가져오기 요청을 실행합니다. 약속 중 하나가 이행되는 즉시, 그 약속의 가치를 충족시키는 약속을 반환한다. 약속.any()는 거절 처리 방식에서 Promise.race()와 다릅니다. `약속.any()`가 반환한 약속은 모든 약속이 거부될 경우에만 거부된다.

```js
const promise = Promise.any([
    Promise.reject('Exception1'),
    Promise.reject('Exception2'),
    Promise.reject('Exception3')
]);
 
promise.then((response) => {
    // ...
}).catch((e) => {
    console.log(e.errors);
});
 
// logs:
// => ["Exception1", "Exception2", "Exception3"]
```

모든 약속의 거부 값이 `catch()` 메서드에 배열로 전달되는 방식을 주목하십시오. 또는 `sync`와 `wait`를 사용하여 결과를 처리할 수 있습니다.

```js
(async () => {
    try {
        result = await Promise.any([
            Promise.reject('Exception1'),
            Promise.reject('Exception2'),
            Promise.resolve('Success!')
        ]);
 
        console.log(result);
    } catch(e) {
        console.log(e);
    }
})();
 
// logs:
// => Success!
```

## 약속해.모두 결제됨()

최근 약속 객체에 추가된 또 다른 유용한 방법은 `약속`이다.모두 결제됨() 기존의 `약속.all() 방법`을 보완하는 이 방법은 거부되거나 이행되든 모든 약속의 결과를 반환하도록 설계되었다.

예는 다음과 같습니다.

```js
const p1 = Promise.resolve('Success');
const p2 = Promise.reject('Exception');
const p3 = Promise.resolve(123);
 
Promise.allSettled([p1, p2, p3]).then((response) => {
    response.forEach(result => console.log(result.value || result.reason))
});

// logs:
// => Success
// => Error!
// => 123
```

모든 약속의 결과가 어떻게 배열로 `그때()`로 전달되는지 주목하십시오. 그 다음()에서는 배열 항목 위에 "각각()" 메서드 루프가 있습니다. || 연산자의 왼쪽 피연산자가 정의되지 않은 경우 콘솔에 기록됩니다. 그렇지 않으면 약속이 거부되고 오른쪽 피연산자가 기록됩니다.

이에 비해 약속.모두()는 약속 중 하나가 거부되자마자 즉각 거부한다.

## 선택적 체인 연산자

선택적 체인 연산자(?)입니다.`)를 사용하면 체인의 각 속성을 확인하지 않고도 중첩 속성에 액세스할 수 있습니다.

다음 예를 고려하십시오.

```cpp
const obj = {};
const nickname = obj?.user?.profile?.nickname;
 
console.log(nickname);    // => undefined
```

이 코드는 중첩된 속성의 값을 상수에 할당하려고 합니다. 그러나 obj에는 그런 재산이 없다. 또한 사용자 및 프로필은 존재하지 않습니다. 그러나 선택적 체인 연산자 덕분에 코드는 오류를 발생시키는 대신 정의되지 않은 것을 반환한다.

일반 체인 연산자를 사용하면 다음과 같은 오류가 발생합니다.

```cpp
const obj = {};
const nickname = obj.user.profile.nickname;
 
console.log(nickname);    // => TypeError
```

선택적 체인 연산자도 개체의 메서드를 호출할 때 사용할 수 있습니다.

```undefined
const obj = {};
const value = obj.myMethod?.();
 
console.log(value);    // => undefined
```

여기서 myMethod는 obj에 존재하지 않지만 선택적 체인 연산자를 사용하여 호출되므로 반환 값은 정의되지 않음입니다. 다시 말하지만, 일반 체인 오퍼레이터가 있으면 오류가 발생합니다.

하지만 동적으로 자산에 액세스하려면 어떻게 해야 합니까? `?[]` 토큰을 사용하면 괄호 표기법을 사용하여 변수를 참조할 수 있습니다.

작동 방식은 다음과 같습니다.

```js
const obj = {
    user: {
      id: 123
    }
};
 
const prop = 'nickname';
const nickname = obj?.user?.profile?.[prop];
 
console.log(nickname);    // => undefined
```

## 글로벌 디스

자바스크립트는 웹 브라우저에서 복잡한 기능을 실행할 목적으로 만들어졌지만, 이제는 서버, 스마트폰, 심지어 로봇과 같은 완전히 다른 환경에서 실행될 수 있다. 각 환경에는 고유한 개체 모델이 있으므로 글로벌 개체에 액세스하려면 다른 구문을 사용해야 합니다.

브라우저 환경에서는 window, frame 또는 self를 사용할 수 있습니다. Web Workers에서는 self를 사용할 수 있습니다. 노드에서는 `글로벌`을 사용할 수 있습니다. 이러한 불일치로 인해 웹 개발자들은 휴대용 자바스크립트 프로그램을 쓰기가 더 어려워진다.

`global this`는 글로벌 개체에 액세스할 수 있는 모든 환경에서 단일 범용 속성을 제공합니다.

```cpp
// browser environment
console.log(globalThis);    // => Window {...}
 
// web worker environment
console.log(globalThis);    // => DedicatedWorkerGlobalScope {...}
 
// node environment
console.log(globalThis);    // => Object [global] {...}
```

이전에는 개발자들이 정확한 속성을 참조할 수 있도록 추가 수표를 작성해야 했습니다. 글로벌로이것은 더 이상 필요하지 않으며 코드는 윈도우와 비 윈도우 컨텍스트에서 모두 작동합니다. 이전 브라우저에서 이전 버전과의 호환성을 위해 여전히 폴리필을 사용해야 할 수 있습니다.

## 결론

JavaScript는 빠르게 발전하고 있으며 흥미로운 새로운 기능이 자주 추가되고 있습니다. 이 기사에서는 BigInt, 무효화 결합 연산자 Promise.any() Promise 등 6가지 새로운 JavaScript 기능을 살펴봤다.모든 Settled()는 선택적 체인 연산자이고 `global`입니다.이거.

빅인트는 큰 정수 값을 나타낼 수 있다. null 병합 연산자는 새 단락 연산자를 JavaScript로 가져옵니다. 약속.아무리.약속.약속.allSettled()는 비동기 작업에 대한 추가 제어를 허용합니다. 선택적 체인 연산자는 중첩된 속성에 대한 액세스를 단순화합니다. 그리고 글로벌이는 글로벌 개체에 액세스할 수 있는 모든 환경에서 하나의 범용 속성을 제공합니다.

최신 기능을 업데이트하려면 완료된 제안 목록을 확인하십시오. 댓글로 궁금한 거 있으면 저도 트위터 하고 있어요.