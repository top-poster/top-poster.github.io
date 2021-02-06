---
layout: post
title: "Graphy SVG(gySVG): 소개서"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/graphery.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/graphery.png?fit=730%2C487&ssl=1)

Graphy SVG(Graphy SVG)는 자바스크립트에서 SVG 그래픽의 구성과 조작을 단순화하는 데 사용되는 작지만 강력한 라이브러리이다. 이 라이브러리는 Vanilla JavaScript와 React, Vue, Svelte, Stencil 또는 Angular와 같은 프레임워크에서 사용할 수 있다. 이 기사에서는 SVG의 기본 사항 및 Graphy를 사용하여 간단하고 복잡한 모양을 만드는 방법에 대해 설명합니다.

## SVG란 무엇인가?

SVG는 확장 가능한 벡터 그래픽스를 의미하며 XML 형식의 웹에 대한 벡터 기반 그래픽을 정의하는 데 사용된다.

다음은 SVG(녹색 테두리가 있는 노란색 원)의 매우 간단한 예입니다.

```js
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="green" stroke-width="4" fill="yellow" />
</svg>
```

## 왜 새로운 SVG 라이브러리인가?

주요 목표는 코드 복잡성을 줄이는 것이다. 수년간 많은 SVG 라이브러리가 존재했지만, 여전히 더 작고 빠르며 SVG 형식에 더 가까운 새로운 라이브러리를 구축해야 할 필요성이 있었다. 결과는 gySVG이다. 이전 SVG 라이브러리에서 최소화된 라이브러리 크기는 약 3KB 미만이었다. 이 값은 gzip을 사용하는 경우 1.5KB로 감소하였다. Graphy SVG도 속도가 빨라 SVG.js의 경우 40ms에 비해 20ms가 소요된다.

gySVG는 프로젝트 성능이나 크기에 불이익을 주지 않고 SVG로 쉽게 작업할 수 있는 간단한 라이브러리이다.

## Vanilla JavaScript 코드를 gySVG와 비교

우리들 중 많은 사람들이 알고 있듯이, 간단한 SVG를 만드는 것은 Vanilla JavaScript에서 많은 줄의 코드를 필요로 하며, 이는 이해하고 유지하기가 어려워질 수 있다. gySVG를 사용하면 SVG DOM 속성 및 메서드에 맞는 일련의 매우 가벼운 방법을 통해 코드 생성과 조작을 단순화할 수 있다.

### Vanilla JavaScript를 사용하여 간단한 SVG 생성

```js
const div = document.querySelector('#drawing');
const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
svg.setAttribute('width', '100%');
svg.setAttribute('height', '100%');
div.appendChild(svg);
const rect = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
rect.setAttribute('x', '10');
rect.setAttribute('y', '10');
rect.setAttribute('width', '90');
rect.setAttribute('height', '90');
rect.setAttribute('fill', '#F06');
svg.appendChild(rect);
```

### gySVG를 사용하여 동일한 SVG 생성

```undefined
const svg = gySVG().width('100%').height('100%');
const rect = svg.add('rect').x(10).y(10).width(90).height(90).fill('#f06');
svg.attachTo('#drawing');
```

그 결과 HTML DOM의 일부로 제한 없이 사용할 수 있는 완전히 유효한 SVG입니다. 보시다시피, 이것은 훨씬 더 단순한 블록입니다.

### 브라우저 지원

gySVG의 매직은 ES6의 가장 강력한 기능 중 하나인 프록시의 사용에 있다. 자바스크립트의 프록시는 성능을 유지하면서 각 SVG 요소에 대해 동적으로 래퍼를 만들 수 있게 해준다.

프록시는 다음에서 지원됩니다.

- Microsoft Edge 12 이상
- Firefox 18 이상
- 크롬 49 이상
- Safari 10 데스크톱 및 모바일 등
- 오페라 36 이상

프록시는 Internet Explorer 11에서 지원되지 않으므로 폴리필 또는 트랜스필러를 사용하여 통합할 수 없습니다.

## gySVG 시작

gySVG를 시작하는 방법에는 CDN에서 로드하거나 npm으로 로컬로 설치하는 방법 두 가지가 있습니다.

### CDN에서 로드

CDN에서 로드할 때 모듈 가져오기를 사용하거나 스크립트 버전을 로드합니다. gySVG 라이브러리를 사용하는 가장 쉬운 방법은 CDN 서비스에서 ES 모듈로 가져오는 것입니다.

```coffeescript
import gySVG from 'https://cdn.graphery.online/svg/0.1.4/module/index.js';
```

또 다른 쉬운 방법은 CDN에서 스크립트 버전을 `<script> 태그로 로드하는 것입니다.

```xml
<script src="https://cdn.graphery.online/svg/0.1.4/script/index.js"></script>;
```

### 'npm'을(를) 사용하여 로컬 설치

``npm`으로 Graphery SVG 라이브러리를 로컬로 설치할 수 있습니다.

```coffeescript
npm install @graphery/svg
```

로컬로 설치한 경우 `node_modules/@grappery/svg`를 참조하고 다음 코드를 가져와야 합니다.

```coffeescript
import SVG from './node_modules/@graphery/svg/index.js';
```

또는 다음과 같이 스크립트를 로드합니다.

```xml
<script src="./node_modules/@graphery/svg/script/index.js"></script>
```

웹 팩이나 다른 로더를 사용하는 경우 이러한 호출에서 `node_modules` 폴더를 생략할 수 있습니다.

## SVG 요소 이해

함수 `gySVG()`는 SVG 요소를 생성하고 해당 요소 위에 gySVG 래퍼와 함께 개체를 반환합니다. 다른 요소를 중첩하거나 좌표계를 정의하거나 다른 구성 매개 변수를 설정하는 데 사용할 수 있습니다.

```cpp
const svg = gySVG();
```

위의 내용은 SVG HTML 태그(<svg>)를 가지는 것과 유사합니다.

표시 요소의 경우, `.viewBox() 메서드를 사용하여 그래픽의 내부 캔버스를 정의할 수 있으며, `.width() 및 `.height()` 메서드를 사용하여 뷰포트를 정의할 수 있습니다. 우리는 다음 섹션에서 이것에 대해 더 자세히 토론할 것입니다.

### viewBox

.viewBox() 방법은 SVG의 내부 위치와 치수를 정의합니다. min-x, min-y, width, 높이 등 네 가지 매개변수를 사용한다. 이러한 숫자는 SVG 요소와 연결된 내부 경계 내에서 매핑되는 사각형을 지정합니다. 내포된 원소의 모든 측도는 이러한 치수를 기준으로 삼습니다. 매개 변수 없이 이 메서드를 호출하면 현재 뷰 상자 값이 반환됩니다.

예:

```cpp
const svg = gySVG().viewBox(0, 0, 100, 100);
```

### 폭 및 높이

이러한 방법은 뷰포트, 즉 해당 뷰포트가 포함될 HTML 페이지의 이미지 크기를 정의하는 데 사용됩니다. 수평 길이는 `.width()`로 정의되고 수직 길이는 `.height()로 정의됩니다.

예:

```cpp
const svg = gySVG().width(100).height(100);
```

### DOM 요소에 연결

마지막으로, HTML 페이지의 DOM에 SVG를 포함시키기 위해 요소를 찾기 위한 선택기로 매개 변수를 제공하는 `.attachTo() 방법을 사용한다. 결과적으로, 우리의 SVG는 페이지에 삽입된다.

예:

```undefined
const svg = gySVG().viewBox(0, 0, 100, 100).width(50).height(50);
svg.attachTo('#content')
```

gySVG를 사용하여 원과 직사각형과 같은 기본 모양을 만드는 방법을 계속 보기 전에 먼저 내포 요소를 추가하는 방법을 검토하겠습니다.

### 중첩 요소 추가

SVG 내에 요소를 추가하려면 생성할 요소의 이름으로 매개 변수를 전달하는 `.add() 메서드를 사용합니다. 이 메서드는 명명된 요소의 모든 특성(이 예에서는 `원`)을 설정하는 데 사용할 수 있는 개체를 반환합니다.

예:

```undefined
const svg    = gySVG().viewBox(0, 0, 100, 100).width(75).height(75);
const circle = svg.add('circle').cx(50).cy(50).r(50).fill('#f06');
```

## gySVG를 사용하여 기본 모양 만들기

다음은 gySVG로 기본 모양을 만드는 방법에 대해 알아보겠습니다. 원하는 경우 SVG 파일의 모든 요소와 속성을 애니메이션으로 만들 수 있습니다.

### 직사각형

사각형을 만들기 위해 `.add(`rect`)` 방법을 사용합니다. .width()와 .height()는 직사각형의 크기를 설정하고, .x()와 .y()는 적절한 위치로 모양을 이동시킨다. 이러한 모든 값은 viewBox에 정의된 치수와 관련이 있습니다.

예:

```undefined
const svg  = gySVG().viewBox(0, 0, 100, 100).width(75).height(75);
const rect = svg.add('rect').x(10).y(10).width(90).height(90).fill('#00D800');
```

.fill은 도형의 내부 색상을 설정하고, .stroke는 테두리 색상을, .stroke는 이 테두리 두께를 `.stroke_width`로 설정합니다. 지정하지 않은 상태로 두면 모든 색상이 검은색으로 기본 설정됩니다.

.rx() 및 .ry() 내에서 값을 지정하여 둥근 모서리를 만들 수도 있습니다.

### 원

원을 만들기 위해 중심점()과 .cy()로 정의된 원과 외부 반지름()으로 정의된 원()을 만드는 .add(`circle`) 방법을 사용할 것이다. 이러한 모든 값은 viewBox에 정의된 차원에 따라 결정됩니다.

예:

```undefined
const svg    = gySVG().viewBox(0, 0, 100, 100).width(75).height(75);
const circle = svg.add('circle').cx(50).cy(50).r(50).fill('#0000D8');
```

.fill() 메서드는 원의 내부 색상을 설정하는 반면, .stroke() 메서드는 원의 테두리 색상과 .stroke_width()를 사용하여 테두리 두께를 설정할 수 있습니다.

예:

```undefined
const svg    = gySVG().viewBox(0, 0, 100, 100).width(75).height(75);
const circle = svg.add('circle').cx(50).cy(50).r(45)
                  .fill('#D80000').stroke('#00D800').stroke_width(10);
```

여기서 타원, 다각형, 선 등과 같은 다른 모양을 만드는 방법에 대해 알아 보십시오.

### 경로 요소

.add(`path`) 메서드는 SVG 경로를 생성하는 데 사용됩니다. 이 요소는 도형의 윤곽선을 나타냅니다. 모든 기본 셰이프는 경로 요소를 사용하여 만들 수 있지만 경로 요소는 일반적으로 더 복잡한 셰이프를 위해 예약됩니다.

경로 정보는 `.d`와 특수 방법 그룹 또는 `.d(path_data)로 직접 설명된다. 가장 쉬운 방법은 `.d`와 같은 `.d` 방법을 사용하는 것이다.M() 또는 `.d`입니다.L()를 선택한 다음 거기서 단계별로 경로 데이터를 작성합니다.

대문자 메소드는 좌표를 절대 용어로 표현하고 소문자 메소드는 가장 최근에 선언된 좌표의 상대적 용어로 표현한다. 다음은 `.d`에 사용 가능한 방법의 간단한 목록입니다.

예:

gySVG()는 아래 코드에서 경로 요소를 가진 SVG 하트를 만드는 데 사용됩니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="475" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/olawanlejoel/embed/RwGKJYe?height=475&amp;theme-id=dark&amp;default-tab=js%2Cresult&amp;user=olawanlejoel&amp;slug-hash=RwGKJYe&amp;pen-title=Heart%20%7C%20SVG&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="Heart | SVG" loading="lazy" id="cp_embed_RwGKJYe"></iframe></div>

## gySVG를 사용하여 그라데이션 및 패턴 생성

앞의 예에서, 우리는 SVG에 색을 추가하기 위해 `.fill()과 `.stroke()`를 사용하는 방법을 보았다. 이 섹션에서는 그라데이션과 패턴을 사용하여 모양을 채우는 방법에 대해 설명합니다.
그라데이션 및 패턴의 메서드 `.url()로 ".fill() 및 ".stroke()" 값을 사용할 수 있습니다. 이 메서드는 의사 함수 `url()를 사용하여 요소의 고유 ID 참조를 반환합니다.

### 그라데이션

우리가 고려할 그레이디언트에는 선형과 방사형의 두 가지 유형이 있습니다.

#### 선형 구배

선형 그라데이션은 각 색상 포인트(정지)와 그 사이의 전환에 따라 일직선을 따라 색상이 균일하게 변한다. 기본적으로 그라데이션은 왼쪽에서 오른쪽으로 이동하며, 다른 각도(예: `45`)가 필요한 경우 `.gradientTransform(회전(45)을 사용할 수 있습니다.

정지 요소는 백분율을 정의하기 위해 .offset()로 정의되고, 해당 포인트의 색상 참조를 정의하기 위해 .stop_color()와 .stop_opacity()로 정의됩니다.

아래 코드는 선형 그라데이션 채우기를 사용하여 원 모양 SVG를 만드는 데 사용됩니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="444" width="100%" name="cp_embed_2" scrolling="no" src="https://codepen.io/olawanlejoel/embed/LYRxaxg?height=444&amp;theme-id=dark&amp;default-tab=js%2Cresult&amp;user=olawanlejoel&amp;slug-hash=LYRxaxg&amp;pen-title=Linear%20Grad.%20%7C%20Graphery%20SVG&amp;name=cp_embed_2" style="width: 100%; overflow:hidden; display:block;" title="Linear Grad. | Graphery SVG" loading="lazy" id="cp_embed_LYRxaxg"></iframe></div>

#### 반지름 구배

방사 그라데이션은 선형 그라데이션과 매우 유사하지만 이 경우 그라데이션 좌표가 원형으로 되어 중앙에서 가장자리로 이동한다.

아래 코드는 반지름 그라데이션 채우기를 사용하여 원 모양 SVG를 만드는 데 사용됩니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="412" width="100%" name="cp_embed_3" scrolling="no" src="https://codepen.io/olawanlejoel/embed/mdrRowN?height=412&amp;theme-id=dark&amp;default-tab=js%2Cresult&amp;user=olawanlejoel&amp;slug-hash=mdrRowN&amp;pen-title=Radial%20Grad.%20%7C%20Graphery%20SVG&amp;name=cp_embed_3" style="width: 100%; overflow:hidden; display:block;" title="Radial Grad. | Graphery SVG" loading="lazy" id="cp_embed_mdrRowN"></iframe></div>

### 패턴

패턴(pattern)은 SVG로 간격마다 다시 그릴 수 있는 그래픽 객체를 정의합니다. 고유한 .viewBox() 바운드 참조가 있으며 크기는 ".width() 및 ".height()로 정의됩니다.

너비 및 높이의 동작은 기본적으로 `objectBoundingBox`인 `.patternUnits()` 구성에 따라 달라집니다. 계속하려면 `userSpaceOnUse`로 구성해야 합니다. 이 구성에서 높이와 너비는 `패턴`을 사용하는 요소의 좌표계를 참조한다.

아래 코드는 패턴 채우기를 사용하여 원 모양 SVG를 만드는 데 사용됩니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="439" width="100%" name="cp_embed_4" scrolling="no" src="https://codepen.io/olawanlejoel/embed/PoGWLOq?height=439&amp;theme-id=dark&amp;default-tab=js%2Cresult&amp;user=olawanlejoel&amp;slug-hash=PoGWLOq&amp;pen-title=Pattern%20%7C%20Graphery%20SVG&amp;name=cp_embed_4" style="width: 100%; overflow:hidden; display:block;" title="Pattern | Graphery SVG" loading="lazy" id="cp_embed_PoGWLOq"></iframe></div>

## 동적 도형의 추가 예제

gySVG는 기본적이고 복잡한 모양을 만드는 데 탁월한 기능을 제공할 뿐만 아니라 다음과 같은 보다 역동적인 요소를 구축하는 데도 사용할 수 있습니다.

#### 작업시계

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="358" width="100%" name="cp_embed_5" scrolling="no" src="https://codepen.io/graphery/embed/QWNqgvW?height=358&amp;theme-id=dark&amp;default-tab=result&amp;user=graphery&amp;slug-hash=QWNqgvW&amp;pen-title=gySVG%3A%20clock&amp;name=cp_embed_5" style="width: 100%; overflow:hidden; display:block;" title="gySVG: clock" loading="lazy" id="cp_embed_QWNqgvW"></iframe></div>

#### 자동차 경주

이 예는 매우 간단한 자동차 경주 게임입니다. 시작 버튼을 클릭하여 작동 방식을 확인하십시오!

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="412" width="100%" name="cp_embed_6" scrolling="no" src="https://codepen.io/graphery/embed/QWExdZP?height=412&amp;theme-id=dark&amp;default-tab=result&amp;user=graphery&amp;slug-hash=QWExdZP&amp;pen-title=car%20racing&amp;name=cp_embed_6" style="width: 100%; overflow:hidden; display:block;" title="car racing" loading="lazy" id="cp_embed_QWExdZP"></iframe></div>

여기에서 더 많은 gySVG 예제와 설명서를 살펴보십시오.

## 결론

이 기사에서는 gySVG를 사용하여 기본 모양과 복잡한 모양을 모두 만드는 방법을 비롯하여 JavaScript에서 Graphy SVG를 시작하는 방법에 대해 알아보았습니다. 자세한 내용은 추가 그래프 설명서 또는 빠른 참조 가이드를 참조하십시오.