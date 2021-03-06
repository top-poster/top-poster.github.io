---
layout: post
title: "새로운 CSS Houdini Painting API 탐색"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Chart showing if Houdini is ready for different browsers](https://miro.medium.com/max/2614/1*SURbk6sxLK7AkAdN2k35og.png)

참고: 이 기사의 후반부에서 논의된 데모의 소스 코드는 GitLab에서 찾을 수 있습니다.

# 도입부

리액트 컴포넌트의 캔버스 애니메이션에 대한 최근 기사에서 언급했듯이, 저는 HTML 캔버스를 좋아합니다. 그래서 저는 새로운 CSS Houdini APIs에 대해 알게 된 것을 더 이상 기대하지 않았습니다. Stephen Fulghum의 css-tricks.com에서 짧은 기사를 읽으면서 말이죠.

제가 흥분하는 주된 이유는 PaintRenderingContext2D(PaintRendering Context2D(일반 Canvas API를 사용할 때 그리던 2D 컨텍스트의 거의 정확한 사본)를 사용하여 PaintRendering API를 통해 사용자 지정 CSS 이미지를 만들 수 있기 때문입니다.

Painting API를 사용하여 우리는 CSS에 이미지를 프로그래밍 방식으로 그리고 이러한 이미지를 사용할 수 있다. 그림을 그릴 때 DOM 및 적용된 스타일시트에서 가져온 정보가 포함된 파라미터를 수신할 수 있습니다.

MDN Web Docs에서는 다음과 같이 설명합니다.

특히 모든 주요 브라우저가 이러한 기능을 구현하고 있다는 징후를 볼 수 있기 때문에 매우 흥미롭습니다.

우리는 이 기사에서 CSS Painting API (및 Worklet)에 대해 알아보겠습니다.

중요한 참고: CSS Houdini는 일반적으로 여전히 실험적인 기술입니다. 그러나 앞서 언급했듯이, 대부분의 브라우저들은 그것을 구현하고 있거나 또는 그것을 실행하는 것을 강력히 고려하고 있다. Google Chrome은 초기 어댑터이며 65 버전부터 Painting API에 대한 지원을 제공합니다. 그래서 오늘 사용할 브라우저입니다.

# CSS 페인팅 API

이 API를 사용함으로써 우리는 CSS에서 이미지를 프로그래밍 방식으로 그리고 사용할 수 있다. 이를 통해 다음과 같은 작업을 수행할 수 있습니다.

![Lorem ipsum text](https://miro.medium.com/max/1752/1*dEG8em6db6rTjvzCAUPJaQ.png)

인상적이게도, 이것들은 세 개의 DIV 요소와 세 개의 DIV 요소만 있다.

index.html 파일은 다음을 위한 증거입니다.

세 개의 창 배경에 대한 "스타일링"(도면…)은 프로그래밍 방식으로 그림 API를 사용하여 수행됩니다. 세 개의 창은 모두 동일한 기능에 의해 그려집니다.

우리 스타일시트의 CSS 클래스 `pane`을 살펴봅시다.

우리는 2번 줄에 창이라고 불리는 것을 그립니다. 그것은 설명이 필요하고, 우리는 곧 그것을 살펴볼 것이다.

도장 기능은 한 번만 실행되지 않습니다. 브라우저의 렌더 엔진이 지시할 때마다 실행되며 이미지를 다시 도색합니다. 예를 들어 사용자가 브라우저 창의 크기를 조정하거나 DIV 요소의 다른 CSS 속성이 변경되면 DIV 요소가 다른 차원을 얻을 수 있습니다.

우리의 기능은 렌더링 엔진에 "걸려" 있기 때문에, 이것은 매우 성능적이고 비용이 적게 드는 작업입니다.

그 외에도 모든 창의 글꼴 크기가 다르다는 점을 언급할 필요가 있습니다(이 점은 나중에 다시 참조할 것입니다). 두 가지 사용자 지정 CSS 변수가 사용되고 있습니다.

CSS 변수는 새로운 것이 아닙니다. 그들은 2014/2016년부터 존재해왔다. Firefox/Google Chrome). 그러나 CSS 속성 및 값 API라고 하는 새로운 CSS Houdini API들 중 하나는 브라우저가 이들에 대해 더 많이 알 수 있도록 하기 위해 우리가 이러한 사용자 지정 변수를 등록할 수 있게 해 준다.

스타일시트에서 다음과 같이 등록할 수 있습니다.

우리는 또한 자바스크립트에서도 이것을 할 수 있었다. 결과는 같을 것이다.

그럼 왜 이런 짓을 하는 거죠? 이제 브라우저와 렌더링 엔진이 이러한 속성에 대한 세부 정보를 알게 되었습니다. `-panel-color`에 색상 값이 포함되어 있으며 기본값은 "#646464"입니다. 또한 `-도트 간격`에는 길이 값이 포함되어 있으며 기본값은 "5px"입니다.

우리의 두 가지 새로운 변수는 이제 등록된 사용자 지정 변수라고 불립니다.

이 선으로 돌아가기:

```js
배경 이미지: 그림판(그림판);
```

Painting API를 사용하면 이미지를 그릴 수 있습니다. `그림판` 함수는 하나의 파라미터를 수신합니다. 이 매개 변수는 `worklet.js`라는 파일에 Paint로 등록된 JavaScript 클래스입니다.

그러나! 이 코드는 일반적인 JavaScript 실행 환경에서 실행할 수 없습니다. 이른바 파일 이름(따라서 파일 이름) 내에서 실행해야 합니다.

다음과 같이 index.html 내부에 로드된 일반 JavaScript 파일(`main.js`)에 다음 줄을 추가하여 Worklet을 실행할 수 있습니다.

```js
// main.js
CSS.paintWorklet.addModule("Worklet.js");
```

해당 호출이 오류를 반환하는 경우 브라우저는 Painting API를 지원하지 않습니다.

이제 Paint 클래스 "Pane"이 "pane"이라는 이름으로 등록되었으며 앞에서 살펴본 것처럼 CSS에서 사용할 수 있습니다.

```js
배경 이미지: 그림판(그림판);
```

## 그림판 클래스 창의 세부 정보

worklet.js의 Paint 클래스에 대한 세부 정보를 살펴보겠습니다.

정적 함수 `inputProperties`는 우리가 언제 이미지를 그릴지 관심 있는 CSS 속성 목록을 반환해야 한다. 이것은 임의적이며 원하는 CSS 속성을 추가할 수 있습니다.

컨텍스트 옵션에서 반환되는 값은 캔버스에 투명도를 사용할 수 있음을 나타냅니다.

마지막으로, 10번 라인에서는 페인트 기능이 있습니다. 그 기능에서, 우리는 실제 도면을 할 것입니다. 이 경우 다음과 같은 세 가지 매개 변수를 수신합니다.

마지막 매개 변수에 대한 자세한 내용은 CSS Typed Object Model API에 대해 읽거나 StylePropertyMapReadOnly 인터페이스에 대해 읽을 수 있습니다.

팁: 다음을 지원하는 브라우저에서 computedStyleMap을 호출하면 DOM의 모든 HTML 요소에 대한 전체 StylePropertyMapReadOnly(모든 계산 CSS 스타일 포함)를 얻을 수 있습니다.

```js
constyleMap =
document.getElementById('myElement').computedStyleMap(;
```

이러한 인스턴스에서 값을 검색하는 방법을 명확히 하기 위해 다음과 같은 인라인 코멘트가 포함된 조각을 만들었습니다.

# 실제 이미지 그리기

이 작은 데모의 논리로 돌아가죠. 남은 것은 우리 몸의 페인트 기능 내용뿐입니다.

먼저 채워진 직사각형을 배경 이미지로 그리는 것으로 시작할 수 있습니다(일반 HTML 캔버스에 그림을 그릴 때의 명령을 인식해야 함).

…이렇게 보일 수 있습니다.

![Drawn background image in Houdini](https://miro.medium.com/max/1740/1*D6L-Nruae3CardTqLCax3w.png)

사용자 지정 CSS 속성 `-panel-color`를 사용하여 채우기 스타일을 동적으로 설정하는 방법에 주목하십시오.

브라우저의 dev-tools를 사용하면 색상 값도 업데이트할 수 있으며 배경 이미지가 즉시 다시 칠해집니다!

![Google Chrome dev-tools, color picker.](https://miro.medium.com/max/696/1*w7dtxUoxloEa4nXd5P_AHA.png)

좀 더 화려한 로직으로 페인트 기능을 업데이트하면(리포지토리의 worklet.js 파일에서 세부 정보를 볼 수 있음) 배경 이미지는 다음과 같습니다.

![Lorem ipsum text](https://miro.medium.com/max/1752/1*dEG8em6db6rTjvzCAUPJaQ.png)

# CSS 전환

오른쪽 아래 모서리에 있는 점 시퀀스가 표시됩니다. 이 점 사이의 간격은 사용자 지정 CSS 속성 `-dot-spacing`의 값에 의해 결정되며 기본값은 "5px"입니다.

이 숙박시설에 전환을 추가해 재미를 좀 더해보자. 또한 ".pane" 요소 위를 맴돌 때 점 간격의 값을 증가시킵니다.

…요소를 맴돌면 다음과 같은 매끄러운 애니메이션이 탄생합니다.

![Animated image showing hovering](https://miro.medium.com/max/694/1*Z_Uw007tXTg7u5zQ6U3gcw.gif)

이 그림에서는 사용자 지정 페인트 기능이 얼마나 성능이 우수하며 초당 60회 이상 실행할 수 있는 성능을 보여 줍니다.

사용자 정의 CSS 변수에 전환을 추가할 수 있는 이유는 앞에서 속성을 등록했기 때문입니다(요지서의 1-5줄 참조).

팁: 사용자 지정 CSS 속성 중 하나를 위해 무한 CSS 애니메이션을 추가할 수도 있습니다. 그러면 사용자 지정 애니메이션 배경을 가지게 됩니다!

# 결론

나는 Houdini Painting API가 흥미로울 뿐만 아니라 이해하기 쉽고 강력하다는 것을 알게 되었습니다. 제 머릿속에는 이 새로운 기능을 사용하기 위한 아이디어들이 가득합니다. 하지만 저는 인내심을 가지고 모든 주요 브라우저들이 이 기능을 지원할 때까지 기다려야 합니다. 이 기능을 생산에 사용할 수 있습니다.

시간 내주셔서 감사합니다!