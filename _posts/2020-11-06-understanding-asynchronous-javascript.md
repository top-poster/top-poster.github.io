---
layout: post
title: "비동기식 JavaScript 이해(그리고 효과적으로 사용)"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/understandingasynchronousjavascript.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/understandingasynchronousjavascript.png?fit=730%2C487&ssl=1)

자바스크립트는 오늘날 세계에서 가장 인기 있는 언어 중 하나로 성장했다. 그것은 한 번에 한 가지 일만 할 수 있다는 것을 의미하는 단일 스레드 언어이다. 이전에는 약속과 비동기/대기 기능을 사용하여 비동기식 JavaScript가 JavaScript에 추가되기 전까지는 이러한 제한이 있었습니다.

이 기사에서는 비동기식 JavaScript를 보다 효과적으로 사용하는 방법에 대해 알아보겠습니다.

## 도입

JavaScript는 단일 스레드 언어로서, 한 번에 하나의 논리만 수행할 수 있으며, 이로 인해 JavaScript의 주 스레드를 차단하는 복잡한 긴 함수를 수행할 수 없습니다. 이를 해결하기 위해, 나중에 실행할 인수로 다른 함수로 전달되는 함수인 콜백(callback)이 비동기 함수를 수행하는 데 사용되었다. 비동기 JavaScript를 사용하면 JavaScript의 메인 스레드를 차단하지 않고도 큰 기능을 수행할 수 있습니다.

이를 더 잘 이해하기 위해 동기식 및 비동기식 JavaScript가 무엇을 의미하는지 살펴보겠습니다.

## 동기식 JavaScript

이름이 암시하는 동기식 JavaScript, 시퀀스 또는 순서에서의 평균입니다. 여기서, 모든 기능 또는 프로그램은 시퀀스로 수행되며, 각 기능은 다음 기능이 실행되기 전에 첫 번째 기능이 실행되기를 기다리고 있으며, 동기 코드는 위에서 아래로 이동한다.

동기식 JavaScript를 더 잘 이해하기 위해 아래 코드를 살펴보겠습니다.

```js
let a = 5;
let b = 10;
console.log(a);
console.log(b);
```

결과는 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/synchronousjs.png?resize=843%2C310&ssl=1)

여기서 자바스크립트 엔진은 방정식의 첫 번째 것을 실행하는데, 이 경우는 5이다. 그리고 아래로 내려가서 두 번째 줄의 코드를 실행시켜 콘솔로 10을 인쇄한다. 만약 우리가 다른 코드 라인을 추가하면, 자바스크립트 엔진은 우리가 추가하는 위치에 따라 그것을 실행하는데, 이것은 동기 자바스크립트, 즉 코드를 실행하는 순차적인 방법을 수반한다.

## 비동기 JavaScript

이제 동기식 JavaScript의 작동 방식에 대해 알아보겠습니다. 비동기식 JavaScript에 대해 살펴보겠습니다. 이를 설명하기 위해 아래 코드를 살펴보겠습니다.

```js
console.log("Hello.");
setTimeout(function() {
  console.log("Goodbye!");
}, 3000);
console.log("Hello again!");
```

다른 예와 달리 자바스크립트 엔진은 위의 코드를 동시에 실행하지 않습니다. 아래 결과를 살펴보겠습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/asynchronousjavascript.png?resize=859%2C294&ssl=1)

코드에 헬로(Hello)를 기록하고 3초 후 굿바이(Goodbye)를 콘솔에 기록하며 코드 마지막 부분에 헬로 어게인(Hello again)을 콘솔에 기록한다. 여기서 자바스크립트 엔진은 첫 번째 기능을 거쳐서 실행하며 헬로를 콘솔로 출력하고 다음 기능으로 이동하면 setTimeout 기능을 보고 3초 동안 기다리지 않고 마지막 기능으로 가서 실행해 다시 Hello를 출력한 다음 3초 동안 실행한다. 제2의 기능

따라서 비동기 자바스크립트를 사용하면 자바스크립트는 함수를 실행할 때 응답을 기다리지 않고 다른 기능을 계속 실행한다. 비동기 JavaScript를 실행하는 방법을 살펴보겠습니다.

## 비동기 JavaScript 작성 방법

JavaScript에서 비동기 코드를 작성하는 방법에는 약속과 비동기/대기라는 두 가지가 있습니다.

## 약속들

약속은 어떤 기준이 진실일 경우에만 통과된다. JavaScript 약속을 사용하면 비동기 요청이 완료될 때까지 코드 실행을 연기할 수 있으며, 이렇게 하면 스레드를 차단하지 않고 다른 기능이 계속 실행될 수 있습니다.

약속은 비동기 자바스크립트를 작성하는 새로운 방식으로, 일반적으로 세 가지 주요 상태를 가지는 객체이며, 여기에는 다음이 포함된다.

- 보류 중 - 약속 통과 또는 실패 전 프로그램의 초기 상태
- 해결됨 - 성공적인 약속
- 거부됨 — 실패한 약속

이 점을 더 잘 이해하기 위해 아래에 약속을 만들어 보겠습니다.

```js
const hungry = true;
const eat = new Promise(function(resolve, reject) {
  if (hungry) {
      const fastfood = {
        activity: 'Cook noodles',
        location: 'Market Square'
      };
  resolve(fastfood)
  } else {
    reject(new Error('Not hungry'))
    }
});
```

위의 코드에서 배고픔이 사실이라면, `쿡면`이라고 적힌 액티비티로 데이터를 반환하는 약속을 풀고, 그렇지 않으면 `배고프지 않다`는 오류 객체를 반환한다.

## 약속 사용

이것을 더 밀어붙여서 위에서 초기화한 약속을 사용해 아래 약속에 .then()과 .catch()를 연결할 수 있습니다.

```js
const willEat = function() {
  eat
    .then(function(hungry) {
      console.log('Going to eat noodles!')
      console.log(hungry)
    })
    .catch(function(error) {
        console.log(error.message)
    })
}

willEat();
```

위의 코드에서 우리는 `먹는다`는 약속과 함께 `윌에트()라는 새로운 기능을 만들었고, 다음에는 `.then()`을 사용하여 약속의 결심을 담은 기능을 추가했다. 그런 다음 약속에서 오류 메시지를 반환하기 위해 .catch() 방법을 추가했습니다.

배고픈 값은 사실이기 때문에 윌 이트() 함수를 호출하면 다음과 같은 결과를 얻을 수 있습니다.

```css
Going to eat noodles!
{
  activity: 'Cook noodles',
  location: 'Market square'
}
```

만약 우리가 배고픔의 가치를 거짓으로 바꾼다면 우리의 약속은 `배고프지 않을` 실패한 약속의 상태를 보여줄 것이다. 우리는 이전의 예에서 매개 변수를 가져오는 새로운 약속을 만들어냄으로써 약속을 더욱 추진할 수 있습니다.

```js
const foodTour = function(fastfood) {
  return new Promise(function(resolve, reject) {
    const response = `I'm going on a food tour at
        ${fastfood.location`;

    resolve(response)
  });
}
```

위의 코드에서 우리는 `음식`이라는 새로운 약속을 만들었다.이전 예에서 패스트푸드 값을 가져온 투어에서는 이전 예에서 패스트푸드 위치에 대한 템플릿 문자열로 응답을 해결합니다.

## 비동기/대기

(ES2017+) 릴리스에 Async/wait가 추가되었으며, 자바스크립트에서 약속을 쉽게 작성할 수 있도록 하는 구문 설탕이다. 비동기/대기 기능을 사용하면 비동기식으로 작동하는 동기식 JavaScript 코드를 작성할 수 있습니다.

비동기 함수는 약속을 반환합니다. 함수가 값을 반환하면 약속은 값으로 해결되지만, 비동기 함수가 오류를 발생시키면 약속은 해당 값으로 거부됩니다. 아래 간단한 비동기 기능을 생성해 보겠습니다.

```js
async function favoriteDrink() {
    return 'Monster energy drink'
}
```

여기서는 몬스터 에너지 드링크(Monster Energy Drink)를 반환하는 favorite drink() 기능을 선언합니다. 비동기 함수에서 약속이 거부되면 다음과 유사한 거부된 메서드를 표시합니다.

```js
async function() {
  throw 3;
}
```

Wait는 비동기식으로 함수에 반환되는 모든 약속이 동기화되도록 합니다. 비동기/대기 상태에서는 콜백을 사용할 수 없습니다. try와 true는 또한 비동기 함수의 거부 값을 얻기 위해 사용된다. 이전 예제를 사용하여 `try…catch` 메서드에 포함된 비동기/대기 함수를 생성해 보겠습니다.

```js
async function willEat() {
  try {
    let fastfood = await eat;
    let response = await foodTour(fastfood);
  console.log(response);
  } catch(error) {
      console.log(error.message);
    }
}

willEat();
```

여기서는 이전의 예를 try…catch(트리…캐치) 방식으로 포장된 비동기/대기(sync/wait)로 변환하여 응답 내용을 이전 예시로 기록했는데, 이 예에서는 "시장 광장에서 푸드 투어를 갑니다"라는 문자열을 반환합니다.

## JavaScript에서 비동기 요청 만들기

최근 자바스크립트에서는 URL에 대한 API 요청에 fetch() API가 사용되고 있다. 이 전에는 XMLHttpRequest를 사용하여 요청이 수행되었습니다. petch API 및 비동기/대기 기능을 사용하는 `ES2017+it`를 사용하면 URL 끝점에 비동기 요청을 할 수 있습니다. 먼저 함수를 비동기 함수로 정의하고 `json`에서 응답을 기다렸다가 데이터를 반환해야 합니다. 이를 더 잘 설명하기 위해 아래 코드를 살펴보겠습니다.

```js
async function getJobAsync()
{
  let response = await fetch(`https://cors-anywhere.herokuapp.com/https://jobs.github.com/positions.json`);
  let data = await response.json()
  return data;
}
getJobAsync('jobPositionHere')
  .then(data => console.log(data));
```

위의 코드에서 외부 URL로 가져오기 요청을 하는 비동기 함수 getJobAsync()를 작성하고, 그 다음 json 형식의 응답을 기다렸다가 요청이 해결되면 데이터를 반환했다. 비동기 JavaScript를 사용하여 비동기 요청을 만드는 방법입니다. 아래 이미지에서 기능의 결과를 살펴보겠습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/jobsasyncrequest.png?resize=730%2C323&ssl=1)

다음으로, 우리는 비동기 API 호출의 응답을 반환하는 방법에 대해 알아보겠습니다.

## 비동기 호출에서 응답 반환

JavaScript의 비동기 호출, 콜백 및 약속에서 응답을 반환하는 방법은 여러 가지가 있습니다. 비동기식 통화를 하고 있으며 함수에서 호출 결과를 가져오길 원한다고 가정해 보겠습니다. 이 호출은 비동기/대기 기능을 사용하여 수행할 수 있습니다. 자세한 설명은 아래 코드에서 확인할 수 있습니다.

```js
const getResult = async (request) => {
        let response = await new Promise((resolve, reject) => {
                request((err, res, body) => {
                        if (err) return reject(err);
                        try{
                                resolve(JSON.parse(body));
                        } catch(error) {
                                reject(error);
                        }
                });
        });

        try{
                console.log(response);
        }
        catch(err){
                console.error(err);
        }
}

getResult();
console.log('This is how to return async JavaScript');
```

위의 코드 블록에서 우리는 요청으로부터의 응답을 약속으로 포장한 다음, 해결되거나 거부되는 동안 그것을 기다리고 또한 약속의 응답을 기다립니다. JavaScript 작업에서는 당사의 기능에 있을 수 있는 오류를 처리하기 위해 `try…catch` 방식으로 코드를 포장하는 것이 좋습니다. 마지막으로 프로그램 종료 시 함수를 호출하고 콘솔에 `이것이 비동기 JavaScript를 반환하는 방법입니다`라는 메시지를 기록합니다. 이 방법은 JavaScript, 콜백 또는 비동기/대기에서의 비동기 호출에 응답하는 방법입니다.

## 결론

이 기사에서는 비동기 자바스크립트가 무엇이며 약속과 비동기/대기 기능을 사용하여 비동기 자바스크립트를 작성하는 방법에 대해 배웠다. 또한 가져오기 API 및 비동기/대기 기능을 사용하여 요청을 보내는 방법과 비동기 호출에 대한 응답을 반환하는 방법도 확인했습니다. 비동기 JavaScript에 대한 자세한 내용은 여기에서 확인할 수 있습니다.