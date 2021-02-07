---
layout: post
title: "JavaScript 미디어 쿼리를 사용하는 방법 및 이유"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/daniel-romero-WyHoupIHXMg-unsplash.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/daniel-romero-WyHoupIHXMg-unsplash.png?fit=730%2C408&ssl=1)

CSS3에서 처음 도입된 미디어 쿼리는 반응성 웹 설계의 핵심 구성 요소를 형성한다. 응용 프로그램은 각 장치 유형(예: 휴대폰, 태블릿, 랩톱, 데스크톱 컴퓨터)의 제약 조건에 맞게 조정되어야 하며, 미디어 쿼리는 응용 프로그램이 보고 있는 장치의 크기에 따라 뷰포트 치수를 쉽게 설정할 수 있는 방법을 제공한다.

미디어 쿼리를 사용하면 화면 크기에 따라 뷰포트 치수를 변경할 수 있을 뿐만 아니라 색 구성표, 글꼴 스타일, 모션 설정 및 애니메이션, 테두리 및 간격, 생각할 수 있는 거의 모든 기타 CSS 속성을 포함하여 다양한 장치에 대해 다양한 스타일 속성을 설정할 수 있습니다.

일부 프런트 엔드 개발자들이 언뜻 놓치는 사실은 미디어 쿼리가 자바스크립트에서 지원된다는 것이다. 자바스크립트 미디어 쿼리는 CSS 미디어 쿼리만큼 대중적이지는 않지만 유연성과 많은 장점을 제공하여 특정 사용 사례에 대해 더 나은 선택을 할 수 있다.

## JavaScript 미디어 쿼리의 이점

이 시점에서 여러분은 CSS3 라우트가 가능한데 도대체 왜 개발자가 JS 미디어 쿼리를 선택했을까?라고 생각할 수 있습니다.

JavaScript 미디어 쿼리에 의해 제공되는 두 가지 주요 이점이 있습니다.

- 유연성: 특정 이벤트 시작 시 또는 특정 조건이 충족될 때만 트리거되도록 JavaScript 코드에 미디어 쿼리를 프로그래밍 방식으로 통합할 수 있습니다. CSS3 전용 접근 방식을 사용하면 미디어 쿼리에 의해 설명된 변경 사항이 모든 화면 크기 조정 이벤트에 적용됩니다.
- 편의성: JavaScript 미디어 쿼리는 CSS 작업 시 익숙한 구문을 사용합니다.

이를 고려하십시오. 다양한 화면 크기에 대한 속성을 동적으로 변경하려면 어떻게 해야 합니까? 여러분은 여전히 머리를 긁적이며, 이와 같은 일이 잘 될 것이라고 주장할 수 있습니다.

```js
// A function we want executed based on changes in screen size 
function foo() {
   if (window.innerWidth < 1024) { 
               /* whatever you want to do here */ 
     }
}
// Set up a listener 
window.addEventListener('changesize', foo);
```

위의 코드 블록에서 `window.innerWidth`에 대해 "if" 문이 1024(즉, 데스크톱 디스플레이의 표준 화면 크기)보다 작습니다. 이 방법은 응용 프로그램이 데스크톱 컴퓨터보다 작은 장치에서 실행될 때마다 실행되도록 되어 있습니다.

안타깝게도 이 방법은 사용자가 휴대폰이나 태블릿에서 앱을 열 때뿐만 아니라 크기 조정 시마다 트리거되기 때문에 비용이 많이 든다. 맞습니다. 이 방법은 사용자가 데스크톱 컴퓨터의 화면 크기를 수동으로 조정할 때마다 실행됩니다. 이러한 작업의 수가 너무 많으면 결국 애플리케이션이 지연될 수 있습니다.

감사하게도, 우리는 역동적인 상황과 반응 설계를 처리할 완벽한 API를 가지고 있습니다: 매치 미디어 API에 인사드립니다.

## JavaScript 미디어 쿼리 사용 방법

위의 예에서처럼 크기 조정 이벤트에 수신기를 첨부하는 대신 미디어 API를 매치할 수 있습니다.

Windows 인터페이스의 매치미디어() 방법은 기본적으로 미디어 쿼리에 수신기를 연결하지만 창이나 화면 크기의 모든 변경 사항에 응답하지 않아 성능이 크게 향상됩니다. 이 방법을 활용할 경우 다른 조건, 유효성 검사 및 코드 최적화에 대해 걱정할 필요 없이 화면 크기를 조정하기 위해 실행할 로직만 개발할 책임이 있습니다.

이 API를 사용하기 위해 window.matchMedia()를 호출하고 응답할 화면 크기를 지정하는 미디어 쿼리 문자열을 전달합니다.

```js
// This media query targets viewports that have a minimum width of 320px
const mQuery = window.matchMedia('(min-width: 320px)')
```

matchMedia() 메서드는 위의 예에서 mQuery라는 이름을 가진 새 MediaQueryList 개체를 반환합니다. 이 개체는 특정 문서에 적용된 미디어 쿼리에 대한 정보와 이벤트 기반 및 즉시 일치에 대한 지원 방법을 저장합니다. 이를 통해 크기 조정 이벤트가 시작될 때 사용자 지정 논리를 트리거할 수 있습니다.

필요한 크기 조정 로직을 실행하려면 미디어 쿼리가 일치하면 "true"를 반환하고 일치하지 않으면 "false"를 반환하는 부울 속성인 window.matches를 확인해야 한다. 이 예에서 이 속성은 지정된 조건과 실제로 일치하는지 여부를 알려줍니다(즉, 화면의 최소 너비가 320px임).

```js
// Check whether the media query has been matched 
if (mQuery.matches) { 
    // Print a message to the console 
    console.log('Media query matched!') 
}
```

쉽죠? 딱 한 가지 방법이 있습니다. window.matches는 이 검사를 한 번만 수행할 수 있습니다. 반응성이 뛰어난 웹 설계를 위해, 우리는 지속적으로 변경사항이 있는지 확인하려고 합니다. 다행히 window.matches와 페어링할 수 있는 또 다른 도구가 있습니다. 바로 addListener() 방법입니다.

## Add Listener() 메서드

matchMedia API는 addListener() 메서드와 해당 removeListener() 메서드를 제공합니다. addListener()를 호출할 때 미디어 쿼리 일치 상태의 변경을 감지할 때마다 실행되는 콜백 함수를 전달합니다. 이 콜백 기능은 크기 조정 이벤트에서 트리거하려는 기능입니다.

```js
// This media query targets viewports that have a minimum width of 320px
const mQuery = window.matchMedia('(min-width: 320px)')

function handleMobilePhoneResize(e) {   
   // Check if the media query is true
   if (e.matches) {     
        // Then log the following message to the console     
        console.log('Media Query Matched!')   
   } 
} 

// Set up event listener 
mQuery.addListener(handleMobilePhoneResize)
```

이 기술을 사용하면 미디어 쿼리 변경에 대응하고 필요에 따라 동적으로 추가 방법을 호출할 수 있다. 동적으로 호출되는 메소드는 글꼴 스타일, 테두리 및 간격, 애니메이션 등과 같은 다양한 문서 속성을 변경할 수 있습니다.

예를 들어, 동적 글꼴 스타일을 통합하려는 경우 다음과 같은 방법으로 이러한 작업을 수행할 수 있습니다.

```js
function changeFontStyleMobile(e) {
   // Check whether the media query has been matched
   if (e.matches) {
      // Change font size
      document.getElementById("p2").style.color="blue";
      var span = document.document.getElementById("span");
      span.style.fontSize = "25px";
      span.style.fontcolor = "white"; span.style.fontSize = "36px";
   }
}

// Set up event listener
mQuery.addListener(changeFontStyleMobile)
```

## 결론

이제 JavaScript의 미디어 쿼리와 이러한 쿼리가 어떻게 효율적이고 적응적인 설계를 가능하게 하는지 기본적으로 이해해야 합니다. matchMedia API를 사용하면 JavaScript에서 미디어 쿼리를 만들 수 있으며, addListener()를 사용하면 콜백 기능을 호출하여 응답성이 높은 크로스 플랫폼 환경을 구축할 수 있습니다.

문서 상태 변경에 대한 주기적인 폴링은 비효율적이고 리소스를 많이 사용하는 접근 방식으로, 결국 응용프로그램이 지연될 수 있습니다. 매치미디어()를 사용하면 특정 문서를 관찰하고, 미디어 쿼리 조건의 변경을 탐지하며, 미디어 쿼리 상태에 따라 문서 속성을 프로그래밍 방식으로 변경할 수 있습니다.