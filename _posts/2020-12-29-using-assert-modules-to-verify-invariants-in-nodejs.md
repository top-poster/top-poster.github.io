---
layout: post
title: "Assert 모듈을 사용하여 Node.js의 불변량 확인"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/nodejs-assert-modules-invariants.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/nodejs-assert-modules-invariants.png?fit=730%2C487&ssl=1)

Node.js의 주장 모듈은 Node.js 애플리케이션의 기능에 대한 표현식을 테스트하는 데 사용된다. 테스트 결과가 false를 반환하는 경우, admit은 오류를 발생시키고 프로그램을 중지합니다. 유닛 Js, Mocha, Chai와 같은 유닛 테스트 툴과 함께 Assert 모듈을 사용할 수 있습니다.

개발자는 응용 프로그램의 불변성을 테스트하기 위해 주장을 사용합니다. 그러나 불변성은 무엇이며 이를 검증하기 위한 다른 방법은 무엇인가?

## 불변이란 무엇인가?

불변량은 프로그램의 어느 시점에서 참을 반환해야 하는 표현식 또는 조건입니다. 아래 표현을 자세히 살펴보겠습니다.

```js
let a = 5;
let b = 3;
let correct = (a > b);
console.log (correct);
```

터미널 또는 Bash에서 실행할 때 "true"가 반환됩니다. 만약 어떤 이유로 위의 어플리케이션이 거짓을 반환하고 거기에 따라 다른 표현이 있다면 어떻게 될까요?

>보다 큰 것을 <>로 바꾸고 이 코드를 실행하면 "거짓"이 된다. 위의 표지판의 변화가 실수이고 프로그램이 이 실수로 인해 예상치 못한 결과를 반환한다고 생각해 보자. 개발 팀은 어떻게 이러한 실수를 추적하여 재발이 발생하지 않도록 할 것인가?

위의 식은 불변의 예입니다. 코드가 예상대로 "true"를 반환한다는 것을 보장하기 위해, Node.js 개발 팀은 주장 모듈을 사용하여 식을 테스트한다.

## 주장 방법의 유형

여러 가지 주장 방법이 있습니다. 즉, 사용하는 주장 방법은 Node.js 응용 프로그램에서 불변량을 테스트하는 것으로 요약됩니다. 이 Node.js 모듈을 사용하려면 아래 명령을 실행하여 설치하십시오.

```coffeescript
npm i assert
```

응용 프로그램에서 그것들을 사용할 수 있는 다양한 주장 방법과 예를 살펴보자.

### '주장(값[, 메시지])' 방법

이 방법은 불변량 값이 참인지 확인하는 데 사용됩니다. assert.ok(값[, 메시지])의 별칭으로 같은 방법으로 사용할 수 있습니다.

```js
//import assert module
var assert = require('assert');

//declare variables
var a = 5;
var b = 3;

//This code below would throw an assert error and end program
assert(a < b);

/**This is an expample to show how to use assert.ok(). Comment assert() out, run this code to get the same response.
assert.ok(a < b);
**/
```

위의 프로그램은 응용 프로그램에서 `assert() 메서드를 사용하는 방법의 예입니다. 자세히 보면 이 식이 불변량을 설명하는 데 사용된 첫 번째 식과 같다는 것을 알 수 있습니다.

참고: 불변 식이 true를 반환하는 경우 어떤 주장 방법에서도 예외를 발생시키지 않습니다. 불변량이 거짓 또는 0으로 반환되는 경우에만 오류를 발생시킵니다.

### Assert.deepStrictEqual(실제, 예상 [, 메시지]) 방법

이 메서드는 엄격한 모드에서 주장을 사용하므로 `assert.deepEqual` 메서드 대신 동등성을 확인할 때 이 메서드를 사용합니다. 이는 `assert.deep equal`이 추상적인 평등 비교를 사용하기 때문에 놀라운 결과를 얻을 수 있기 때문이다.

`Assert.deepStrictEqual()` 방법을 사용하는 방법의 예는 아래 식에 나와 있습니다.

```java
//This is how to import the assert module in strict mode
const assert = require('assert').strict;

// This fails because 1 !== '1 for strict equality comparison.
assert.deepStrictEqual({ a: 1 }, { a: '1' });

/**Where { a: 1 } = actual value and { a: '1' } = expected value.
AssertionError: Expected inputs to be strictly deep-equal:
**/
```

### 'Assert.fail([message])' 메서드

주장이 거짓을 반환할 때 사용자 정의 메시지를 불변 검정에 추가해야 한다고 상상해 보십시오. 이것은 간단한 유형의 디버깅 방법입니다. 예를 들어, 개발자는 다음 사항을 코드에 포함시켜 오류가 무엇인지 알 수 있습니다.

```coffeescript
console.log('When you get here, log this so i know my error') 
```

아래 식과 함께 사용자 정의 오류 메시지를 삽입할 수 있습니다.

```java
//Import assert module
const assert = require('assert').strict;

/** AssertionError [ERR_ASSERTION]: Failed. This just prints failed as error message. This is not a good practice**/
assert.fail();

// AssertionError [ERR_ASSERTION]: This is why I failed
assert.fail('This is why I failed');

// TypeError: This is why I failed
assert.fail(new TypeError('This is why I failed'));
```

### 'Assert.ifError(값)' 메서드

이 그림을 보세요. 여러분은 아마 오류가 어느 선에서 오는지 알 것입니다. 하지만 왜 프로그램이 오류를 발생시키는지 이해만 할 수는 없습니다.

`Assert.ifError` 방법을 사용하면 오류 콜백 인수를 테스트하여 오류가 발생하는 정확한 위치를 파악할 수 있습니다. 이 방법을 사용하는 간단한 예는 아래 식에 나와 있습니다.

```js
//This method is used in strict mode
const assert = require('assert').strict;

//Define error callback arguement
let err;
(function error1() {
  err = new Error('test error');
})();

//create an ifError function
(function ifError1() {
//return this error if program resolves into an error
  assert.ifError(err);
})();
/**AssertionError [ERR_ASSERTION]: ifError got unwanted exception: test error
at ifError1
at error1 **/
```

### 'Assert.doesNotMatch(string, regexp[, message])' 메서드

이 메서드는 실제 입력 문자열이 예상 식과 일치하지 않는지 확인하는 데 사용됩니다. Node.js 버전 v13.6.0, v12.16.0에 추가된 `assert.doesNotMatch() 메서드는 실험적이며 완전히 제거될 수 있습니다. 이 방법은 현재 엄격한 모드에서 주장을 사용합니다.

아래의 간단한 식은 `assert.doesNotMatch() 방법을 사용하여 불변량을 확인하는 방법을 보여줍니다.

```python
//import assert using strict mode
const assert = require('assert').strict;

assert.doesNotMatch('I will fail', /fail/);
/** AssertionError [ERR_ASSERTION]: The input was expected to not match because fail is contained in the actual and expected string ...**/

assert.doesNotMatch(123, /pass/);
/**AssertionError [ERR_ASSERTION]: The "string" argument must be of type string.**/

assert.doesNotMatch('I will pass', /different/);
/** This passes the test with no error since actual and expeted string have no word alike **/
```

### Assert.notReject(asyncFn\[, 오류\][, 메시지]) 메서드

이 메서드는 비동기 함수에서 약속이 거부되지 않는지 확인하는 데 사용됩니다. 해당 표현이 약속을 반환하지 않는 경우, `Assert.doesnotReject()`는 거부된 약속을 반환합니다.

이 방법을 사용하는 것은 거절을 다시 거부하기 위해 잡는 것이 유익하지 않기 때문에 그다지 유용하지 않다. 가장 좋은 방법은 오류 메시지를 거부해서는 안 되는 특정 코드 경로 옆에 설명을 추가하고 가능한 한 표현적으로 유지하는 것입니다.

이 방법을 사용하는 방법에 대한 예는 아래 식에 나와 있습니다.

```js
(async () => {
  await assert.doesNotReject(
    async () => {
      throw new TypeError('Wrong value');
    },
    SyntaxError
  );
})();

assert.doesNotReject(Promise.reject(new TypeError('Wrong value')))
  .then(() => {
    // ...
  });
```

불변한 약속이 거부되었는지 확인하고 싶다면 어떻게 해야 합니까?

이 시나리오에서 사용할 주장 방법은 "Assert.rejects() 방법"입니다. 이 메서드는 비동기 함수로 반환된 약속을 기다려 약속이 거부되었는지 확인합니다.

약속이 거부되지 않으면 오류가 발생합니다. 아래 표현은 우리가 이 방법을 사용할 수 있는 간단한 방법을 보여줍니다.

```js
(async () => {
  await assert.rejects(
    async () => {
      throw new TypeError('Wrong value');
    },
    {
      name: 'TypeError',
      message: 'Wrong value'
    }
  );
})();

assert.rejects(
  Promise.reject(new Error('Wrong value')),
  Error
).then(() => {
  // ...
});
```

## 엄격한 레거시 모드에서 주장

엄격한 모드에서 주장에는 불변성을 검증하기 위해 엄격한 평등 비교 방법을 사용하는 것이 포함된다. 엄격한 모드를 주장할 때, 엄격하지 않은 방법은 상대적으로 엄격한 방법으로 동작한다.

예를 들어, `assert.deepEqual()은 `assert.deepStrictEqual()처럼 동작하는 반면 `assert.notEqual()은 `assert.notEqual`()처럼 동작합니다.

엄격한 모드에서 주장을 사용하려면 다음 방법 중 하나를 사용하여 주장 모듈을 애플리케이션으로 가져오십시오.

```java
//import the assert module using strict mode
const assert = require('assert').strict;

//import the assert module using strict mode
const assert = require('assert/strict');
```

레거시 모드에서 불변량의 비교는 추상적 평등 비교의 사용을 포함한다. 항상 엄격한 모드를 사용하여 불변량의 실제 값과 예상 값을 비교하는 것이 좋습니다.

레거시 모드를 사용하여 애플리케이션으로 어설션 모듈을 가져오려면 아래 식을 사용하십시오.

```java
const assert = require('assert');
```

다음은 레거시 모드의 다양한 주장 비교 방법입니다.

### Assert.deep equal(실제, 예상 [, 메시지]) 방법

주장 깊은 평등 방법은 추상적인 평등 비교를 사용한다. 실제 결과와 예상 결과의 동일성을 확인하는 데 사용됩니다. 이 방법을 사용하면 하위 속성도 테스트됩니다. `assert.deepEqual() 방법을 사용하는 간단한 예는 아래 식에 나와 있습니다.

```java
const assert = require('assert');

const val1 = {
  x: {
    y: 1
  }
};
const val2 = {
  x: {
    y: 2
  }
};
const val3 = {
  x: {
    y: 1
  }
};

assert.deepEqual(val1, val3);
// Returns OK since val1 is equal to val3

// Values of b are different (Child properties)
assert.deepEqual(val1, val2);
// AssertionError: { a: { b: 1 } } deepEqual { a: { b: 2 } }
```

오히려 평등을 위해, 깊은 불평등을 시험하고 싶다면? 이 경우에 사용할 주장 방법은 DeepEqual(실제, 예상 [, 메시지])이 아닌 adst.not assist입니다. 이 방법은 심층 부등식을 검정하는 데 사용되며 실제 값과 예상 값이 같을 경우 예외를 발생시킵니다.

이 방법의 사용 예는 다음과 같습니다.

```java
const assert = require('assert');

const val1 = {
  x: {
    y: 1
  }
};
const val2 = {
  x: {
    y: 2
  }
};
const val3 = {
  x: {
    y: 1
  }
};

assert.notDeepEqual(val1, val3);
// AssertionError: { x: { y: 1 } } notDeepEqual { x: { y: 2 } }

/** Values of b are different (Child properties)**/
assert.notDeepEqual(val1, val2);
// Ok because val1 and val2 are not equal
```

참고: 이 방법은 불안정하기 때문에 감가상각되므로 `자산.딥스트릭트 등가`와 `자산.딥스트릭트 등가`와 같은 엄격한 모드에서 주장을 사용하는 것이 좋습니다.

### Assert.equal(실제, 예상 [, 메시지]) 방법

이 방법은 추상적 평등 비교를 사용하여 실제 결과와 예상 결과 간의 얕은 비교를 테스트하는 데 사용됩니다. 얕은 시험을 위한 것이기 때문에 아동의 가치를 시험하는 데 거의 사용되지 않으며, 이 방법 대신 "Assert.deep Strict Equal()" 방법을 사용하는 것이 좋습니다. `Assert.equal()` 사용 방법의 예는 아래 식에 나와 있습니다.

```java
//Import the assert module
const assert = require('assert');

assert.equal(1, 1);
// This throws no exception since 1 == 1

assert.equal(1, '1');
/** This throws no exception since with abstract equality comparison 1 == '1' **/

assert.equal(NaN, NaN);
// This throws no exception

assert.equal(1, 2);
// This throws an error, AssertionError: 1 == 2

assert.equal({ a: { x: 1 } }, { a: { x: 1 } });
/**This throws an error AssertionError: { a: { x: 1 } } == { a: { x: 1 } } because it dosen't check child value**/
```

등가()의 경우 등가()의 경우 등가()의 경우 등가()의 경우 등가()의 경우처럼 얕은 불평등을 확인할 때 사용합니다.

### 'Assert.not equal(실제, 예상됨[, 메시지])' 메서드

accept.equal 방법이 얕은 평등 비교에 사용되는 것과 마찬가지로, 이 방법은 얕은 불평등 비교에 사용된다.

이 방법은 얕은 검정을 위한 것이기 때문에 자식 값을 검정하는 데 사용하면 안 됩니다.

또한 이것은 레거시 모드 불변 방법이고 레거시 모드 불변 방법은 추상 비교를 위한 것이다. 엄격한 비교와 하위 값을 테스트하려면 `Assert.not Strict Equal` 방법을 사용하십시오.

```java
//Import the assert module
const assert = require('assert');

assert.notEqual(1, 1);
// This throws an exception since 1 == 1

assert.notEqual(1, '1');
/** This throws an exception since with abstract equality comparison 1 == '1' **/

assert.notEqual(1, 2);
// This throws no error, since 1 !== 2
```

### Approst.notDeepEqual(실제, 예상 [, 메시지]) 방법

주장 not deep equality 방법은 실제 결과와 예상 결과 사이의 얕은 불평등을 확인하는 데 사용된다. 자식 속성 간의 비교를 테스트하려는 경우 이 방법이 적합합니다.

`assert.notDeepEqual() 방법을 사용하는 간단한 예는 아래 식에 나와 있습니다.

```java
//Import module
const assert = require('assert');

const val1 = {
  x: {
    y: 1
  }
};
const val2 = {
  x: {
    y: 2
  }
};
const val3 = {
  x: {
    y: 1
  }
};

assert.notDeepEqual(val1, val3);
// Returns an error, since val1 is equal to val3

// Values of b are different (Child properties)
assert.deepEqual(val1, val2);
// Returns no error since the child properties are not equal
```

## 결론

이 기사에서는 노드를 사용해야 하는 방법과 이유를 살펴봤습니다.Js는 우리의 응용 프로그램에서 모듈을 주장한다.

기존 상대방이 추상적 평등 비교를 사용하고 있으며 현재 감가상각 상태이기 때문에 생산용 코드를 작성하는 동안 주로 엄격한 모드에서 주장을 사용하는 것이 좋다.

이 문서에서는 불변 식이 필요한 횟수만큼 호출되었는지 확인하는 `콜 트래커 개체`를 생성하는 방법을 설명합니다.

이 기사를 읽어주셔서 정말 감사합니다.