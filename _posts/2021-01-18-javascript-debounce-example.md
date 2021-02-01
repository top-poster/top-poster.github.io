---
layout: post
title: "JavaScript Debounce 예제 – JS (ES6)에서 함수를 지연시키는 방법
 "
author: 'Code Tower'
thumbnail: https://www.freecodecamp.org/news/content/images/size/w600/2021/01/teaser.jpg
tags: undefined
---


자바 스크립트에서 디 바운스 함수는 코드가 사용자 입력 당 한 번만 트리거되도록합니다.
 검색 상자 제안, 텍스트 필드 자동 저장 및 이중 버튼 클릭 제거는 모두 디 바운스의 사용 사례입니다.
 

이 튜토리얼에서는 JavaScript에서 디 바운스 함수를 만드는 방법을 배웁니다.
 

## 디 바운스는 무엇입니까?
 

디 바운스라는 용어는 전자 제품에서 비롯됩니다.
 버튼을 누를 때, 예를 들어 TV 리모컨에서 신호가 리모컨의 마이크로 칩으로 너무 빨리 이동하여 버튼을 놓기 전에 튕기고 마이크로 칩이 "클릭"을 여러 번 등록합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/debounce-button.png)

이를 완화하기 위해 버튼에서 신호가 수신되면 마이크로 칩은 버튼의 신호 처리를 몇 마이크로 초 동안 중지하지만 물리적으로 다시 누르는 것은 불가능합니다.
 

## 자바 스크립트에서 디 바운스
 

JavaScript에서 사용 사례는 비슷합니다.
 함수를 트리거하고 싶지만 사용 사례 당 한 번만 실행합니다.
 

방문자가 검색어를 입력 한 후에 만 검색어에 대한 제안을 표시하려고한다고 가정 해 보겠습니다.
 

또는 양식에 대한 변경 사항을 저장하고 싶지만 사용자가 변경 사항에 대해 적극적으로 작업하지 않는 경우에만 "저장"할 때마다 데이터베이스 이동 비용이 발생합니다.
 

그리고 제가 가장 좋아하는 것은 어떤 사람들은 Windows 95에 정말 익숙해 져서 이제 모든 것을 더블 클릭합니다 😁.
 

다음은 디 바운스 함수 (CodePen 여기)의 간단한 구현입니다.
 

```undefined
function debounce(func, timeout = 300){
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => { func.apply(this, args); }, timeout);
  };
}
function saveInput(){
  console.log('Saving data');
}
const processChange = debounce(() => saveInput());

```

입력에 사용할 수 있습니다.
 

```html
<input type="text" onkeyup="processChange()" />

```

또는 버튼 :
 

```html
<button onclick="processChange()">Click me</button>

```

또는 창 이벤트 :
 

```undefined
window.addEventListener("scroll", processChange);

```

그리고 간단한 JS 함수와 같은 다른 요소에 대해서도 마찬가지입니다.
 

그래서 여기서 무슨 일이 일어나고 있습니까?
 `debounce`는 두 가지 작업을 처리하는 특수 함수입니다.
 

- 타이머 변수에 범위 할당
 
- 특정 시간에 트리거되도록 함수 예약
 

텍스트 입력을 사용하는 첫 번째 사용 사례에서 이것이 어떻게 작동하는지 설명하겠습니다.
 

방문자가 첫 글자를 쓰고 키를 놓으면`debounce`가 먼저`clearTimeout (timer)`로 타이머를 재설정합니다.
 이 시점에서는 아직 예약 된 항목이 없으므로 단계가 필요하지 않습니다.
 그런 다음 제공된 함수`saveInput ()`이 300ms 내에 호출되도록 예약합니다.
 

그러나 방문자가 계속해서 글을 쓰고 있으므로 각 키 릴리스가 `디 바운스`를 다시 트리거한다고 가정 해 보겠습니다.
 모든 호출은 타이머를 재설정해야합니다. 즉,`saveInput ()`을 사용하여 이전 계획을 취소하고 향후 300ms의 새 시간으로 다시 예약해야합니다.
 방문자가 300ms 미만의 키를 계속 누르는 한 계속됩니다.
 

마지막 일정은 지워지지 않으므로 마침내`saveInput ()`이 호출됩니다.
 

## 반대의 경우-후속 이벤트를 무시하는 방법
 

자동 저장을 실행하거나 제안을 표시하는 데 유용합니다.
 하지만 단일 버튼을 여러 번 클릭하는 사용 사례는 어떻습니까?
 마지막 클릭을 기다리지 않고 첫 번째 클릭을 등록하고 나머지는 무시합니다 (여기에서 CodePen).
 

```undefined
function debounce_leading(func, timeout = 300){
  let timer;
  return (...args) => {
    if (!timer) {
      func.apply(this, args);
    }
    clearTimeout(timer);
    timer = setTimeout(() => {
      timer = undefined;
    }, timeout);
  };
}

```

여기에서 첫 번째 버튼 클릭으로 인한 첫 번째`debounce_leading` 호출에서`saveInput ()`함수를 트리거합니다.
 300ms 동안 타이머 파괴를 예약합니다.
 해당 시간 범위 내에서 모든 후속 버튼 클릭은 이미 타이머가 정의되어 있으며 향후 300ms로만 파괴를 푸시합니다.
 

## 라이브러리의 디 바운스 구현
 

이 기사에서는 자바 스크립트에서 디 바운스 기능을 구현하고 웹 사이트 요소에 의해 트리거 된 이벤트를 디 바운스하는 데 사용하는 방법을 보여주었습니다.
 

그러나 원하지 않는 경우 프로젝트에서 자체적으로 디 바운스 구현을 사용할 필요는 없습니다.
 널리 사용되는 JS 라이브러리에는 이미 구현이 포함되어 있습니다.
 다음은 몇 가지 예입니다.
 