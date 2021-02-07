---
layout: post
title: "Chromium의 브라우저 호환성이 스크롤을 위한 의미"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/google-chromium-browser-compatibility.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/google-chromium-browser-compatibility.png?fit=730%2C487&ssl=1)

## 도입

2019년 MDN은 전 세계 수천 명의 개발자를 대상으로 웹에 대해 현재 무엇이 불만스러운지, 그리고 무엇이 불만스러운지 파악했습니다.

이 조사에서 웹 개발자들이 가장 불만족스러웠던 점은 브라우저 호환성이었다.

브라우저 호환성은 다른 브라우저, 특히 IE11(Internet Explorer 11)과 호환되는 웹 애플리케이션을 구축하려는 웹 개발자 및 디자이너에게 항상 문제가 되어 왔습니다.

이 기사에서는 Google Chrome이 브라우저 호환성에 초점을 맞추어 이 문제를 어떻게 해결하고자 하는지, 스크롤에 대한 의미는 무엇인지에 대해 설명합니다.

## 브라우저 호환성이란 무엇입니까?

브라우저 호환성(Browser compatibility)은 특정 웹 응용 프로그램이 서로 다른 브라우저에서 완전히 작동하는 것처럼 보이는 기능을 말한다.

여러 브라우저와 호환되는 웹 응용 프로그램을 개발하는 경우 HTML, CSS 및 JavaScript를 코드화하거나 사용자가 웹 사이트에 액세스하는 플랫폼에 따라 다른 버전의 웹 사이트를 만들어야 합니다. 이는 개발자들의 생태계에서 지속적인 이슈였다.

MDN 조사에 따르면, 크롬 팀은 구글 크롬에서 이러한 호환성 문제들 중 일부를 해결하려고 노력해왔다. 다음은 이들이 호환성을 개선하기 위해 노력하는 몇 가지 방법입니다.

### 플렉스박스

Flexbox는 웹 애플리케이션 구조를 구성하는 강력한 툴입니다. 브라우저 호환성을 위한 최고의 도구 중 하나입니다.

플렉스박스 못지않게 크롬84에도 호환성 문제를 해결하는 데 도움이 될 새로운 개선점이 몇 가지 소개될 예정이다. Chrome 팀은 Chromium Flexbox 구현을 현대적인 레이아웃으로 재설계하는 것을 고려하고 있습니다.NG 엔진. Flexbox를 시작하려면 여기에서 초급 가이드를 확인할 수 있습니다.

### CSS 그리드

CSS 그리드는 브라우저 호환성을 위한 또 다른 훌륭한 도구이며, 구글 크롬 팀에 따르면, CSS 그리드는 크롬 브라우저에서 지원된다. 크롬이 작성 당시 여전히 서브그리드(Subgrid)를 지원하지 않지만, 현재 개발 중이며 새로운 레이아웃의 일부로 추가될 수 있다는 점도 주목할 만하다.NG 엔진. CSS 그리드에 대한 자세한 내용은 이 유용한 문서를 참조하십시오.

### 스크롤링

설문 응답에서는 다음과 같은 다양한 유형의 스크롤 관련 문제가 제기되었습니다.

- 모바일 기기에서 스크롤할 때 뷰포트 크기에 따라 URL 표시줄을 축소/숨기는 효과
- 네이티브 스크롤 제어에 어려움이 있으므로 개발자들은 대신 자바스크립트를 사용하게 된다. 여기에는 오버스크롤 동작 및 스크롤 스냅이 포함됩니다.
- 스크롤링 관련 API 지원 또는 `scroll IntoView`와 같은 동작의 차이

다행히도 CSS 스크롤 스냅을 사용하여 이러한 문제를 해결할 수 있습니다. 어떻게 하는지 설명해보자.

CSS 스크롤 스냅을 사용하면 사용자가 스크롤을 마친 후 특정 요소의 뷰포트를 잠글 수 있습니다. 이렇게 상호작용을 잘 할 수 있습니다.

CSS 스크롤 스냅은 2016년에 도입되어 지난 몇 년간 크게 개선되었으며 대부분의 브라우저와 최신 버전을 지원한다.

시작하려면 HTML 파일을 만드십시오.

```xml
<div class="container">
  <section class="child"></section>
  <section class="child"></section>
  <section class="child"></section>
  <p>...</p>
</div>
```

이제 다음 CSS 속성을 추가하십시오.

```css
.container {
  scroll-snap-type: y mandatory;
}

.child {
  scroll-snap-align: start;
}
```

여기서 y 값 단순 스크롤 컨테이너는 수평 축의 위치에만 스냅되며, medictionary는 스크롤 컨테이너의 비주얼 뷰포트가 현재 스크롤되지 않은 경우 스냅 지점에 고정됨을 의미합니다.

여기서 CSS 스크롤 스냅 및 다양한 속성 값에 대한 자세한 내용을 볼 수 있습니다.

Body Scroll Lock이라는 NPM 패키지를 사용하여 스크롤 잠금을 제대로 수행할 수 있는 방법을 살펴보겠습니다.

패키지를 프로젝트로 가져옵니다.

```undefined
yarn add body-scroll-lock
// or
npm install body-scroll-lock
```

그런 다음 `index.js` 파일을 생성하고 다음 코드에 붙여넣으십시오.

```js
const bodyScrollLock = require('body-scroll-lock');
const disableBodyScroll = bodyScrollLock.disableBodyScroll;
const enableBodyScroll = bodyScrollLock.enableBodyScroll;
```

그런 다음 모든 요소를 쿼리하여 선택합니다.

```js
const targetElement = document.querySelector('.child');

// Disable Body Scrolling for the element
disableBodyScroll(targetElement);

// Renable the Scrollin with the library
enableBodyScroll(targetElement);
```

그게 네가 해야 할 전부야!

## 결론

이 기사에서는 CSS Scroll Snap 및 Body Scroll Lock NPM 패키지를 사용하여 브라우저 호환 스크롤을 처리하는 방법에 대해 배웠습니다. 계속 코딩하십시오!