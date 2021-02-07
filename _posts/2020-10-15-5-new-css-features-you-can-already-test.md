---
layout: post
title: "지금 바로 테스트할 수 있는 5가지 새로운 CSS 기능"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/5-new-css-features-you-can-already-test.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/5-new-css-features-you-can-already-test.png?fit=730%2C487&ssl=1)

브라우저가 점차적으로 그것들을 구현하기 시작하기 전에, CSS 기능은 W3 컨소시엄의 사양에서 처음 정의된다. 언급할 가치가 있는 수많은 새로운 CSS 기능이 있지만, 본 가이드에서는 적어도 하나의 웹 브라우저의 안정적인 버전에서 이미 테스트할 수 있는 5가지 기능에 초점을 맞추겠습니다.

- CSS 하위 그리드
- 플렉스 박스 간극
- 컨텐츠 가시성 속성
- 컨테이너 내크기
- 사이비 클래스: is 및 where

이러한 기능에 대한 브라우저 지원은 항상 변경되므로 항상 Can I Use, MDN CSS Reference(지원 정보는 각 페이지 하단에 있음), Chrome Platform Status(크롬 플랫폼 상태) 등의 사이트에서 현재 지원 수준을 확인합니다.

## 1. CSS 하위 그리드

CSS 그리드는 개발자가 자바스크립트를 사용하거나 지저분한 CSS 해킹에 의존하지 않고도 복잡한 레이아웃을 만들 수 있는 유연한 레이아웃 모듈이다.

그리드 레이아웃을 HTML 요소에 적용하려면 다음 규칙을 추가합니다.

```css
 .grid-container {
    display: grid;
}
```

필요한 정확한 레이아웃을 설정하는 데 사용할 수 있는 몇 가지 그리드 관련 속성이 있습니다.

예를 들어 위의 예에서 `.grid-container`의 하위 요소는 그리드 항목이 되며, `grid-template-columns` 및 `grid-template-row` 속성으로 정의하는 규칙에 따라 배치됩니다.

```css
.grid-container {
    display: grid;
    grid-template-columns: 1fr 2fr 1fr;
    grid-template-rows: 50px 70vh 50px;
}
```

위의 코드는 다음과 같은 CSS 그리드 레이아웃을 정의합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/css-grid-layout.jpeg?resize=720%2C359&ssl=1)

그러나 `.grid-container`의 손자 요소 중 일부(또는 전부)를 그리드 레이아웃에 포함시키려면 어떻게 해야 하는가? 여기서 CSS 하위 그리드가 작동하게 됩니다.

그리드 항목에 다음 규칙을 추가하여 그리드 항목이 상위 그리드 트랙(그리드 선 및 영역 이름 포함, 자체도 정의할 수 있지만)을 채택할 수 있도록 할 수 있습니다.

```css
.grid-item {
    /* these rules specify the subgrid's position within the layout */
    grid-column: 2 / 4;      /* two columns vertically */
    grid-row: 1 / 3;         /* two rows horizontally */

    /* these rules belong to the subgrid itself */
    display: grid;
    grid-template-columns: subgrid;
    grid-template-rows: subgrid;
}
```

그리드 열 및 `그리드 행` 속성은 그리드 열 또는 행 내에서 그리드 항목의 위치를 정의합니다. .grid-item의 하위 요소는 하위 그리드를 형성합니다. 그리드 항목은 둘 이상의 그리드 셀에 걸쳐 있을 수 있습니다. 예를 들어, 여기서는 네 개의 셀로 퍼집니다(위의 예에서 `그리드 열`과 `그리드 행`의 값은 임의적입니다).

보시다시피 subgrid는 독립 실행형 CSS 속성이 아니라 grid-template-columns 및 grid-template-rows 속성에 추가할 수 있는 값입니다. 그리드 레이아웃에 포함된 .grid-item의 하위 항목을 다음과 같이 만든다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/css-subgrid-layout.jpeg?resize=720%2C363&ssl=1)

보시는 바와 같이, 서브 그리드는 그리드 레이아웃의 일부가 되었고, 우리가 원하는 위치에 배치되었습니다. (두 번째와 네 번째 수직 그리드 라인과 첫 번째와 세 번째 수평 그리드 라인 사이).

나머지 그리드 항목은 정상적인 그리드 흐름을 유지하며, 레이아웃 하단에 네 번째 행도 나타납니다. 그러나 격자-템플릿-행 속성으로 3개 행만 정의했기 때문에 4번째 행은 사전 설정된 값이 없어 내용물의 자연 높이에 불과하다. 마지막 세 개의 그리드 항목에서 텍스트를 제거하면 자연 높이가 0이기 때문에 나타나지 않을 것입니다.

다음 CodePen 데모를 사용하여 위의 예를 테스트할 수 있습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="265" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/amonus/embed/dyMgeLP?height=265&amp;theme-id=light&amp;default-tab=css%2Cresult&amp;user=amonus&amp;slug-hash=dyMgeLP&amp;pen-title=Subgrid%20test&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="Subgrid test" loading="lazy" id="cp_embed_dyMgeLP"></iframe></div>

또한 `그리드 템플릿 열` 또는 `그리드 템플릿 행`만 채택하고 다른 값에 대해 새 값을 사용하여 1차원 하위 그리드를 생성할 수도 있습니다.

예를 들어, 다음 경우 하위 그리드는 기본 그리드의 열만 채택하지만 행에 대한 새 규칙을 생성합니다.

```css
.grid-item {
        display: grid;
        grid-template-columns: subgrid;
        grid-template-rows: 200px 400px 200px;
}
```

### 브라우저 지원

CSS 서브 그리드 사양은 2020년 8월 현재 W3C 후보 추천서이다. 현재는 파이어폭스 71+에서만 지원되고 있지만 크롬, 오페라, 브레이브, 새로운 마이크로소프트 엣지 등 주요 브라우저의 기반으로 사용되는 오픈 소스 웹 브라우저인 크롬에도 도입되고 있다.

폴백 메서드가 필요한 경우 중첩 그리드(`상속` 값으로 정의됨)가 CSS 하위 그리드와 동일하지 않습니다. 중첩된 그리드를 사용하여 그리드 트랙을 재계산하는 하위 그리드를 에뮬레이트할 수 있지만, 이 경우 그리드 항목의 일부 또는 모든 하위와 트랙을 공유하는 그리드 대신 두 개의 독립 그리드가 있을 수 있습니다.

## 2. 플렉스 박스 간격

Flexbox 레이아웃에서 Flex 행 또는 Flex 열 사이의 간격을 추가하는 것은 오랫동안 어려운 문제였습니다. 일반적으로 유연한 항목에 여백을 추가하면 해결되지만 여백의 문제는 각 Flex 행이나 열의 시작과 끝에도 추가된다는 것입니다. 첫 번째/마지막 요소에 음의 여백을 더하면 이러한 여백을 제거할 수 있지만 가장 우아한 솔루션은 아닙니다.

다행히 플렉스박스 격차에 대한 브라우저 지원이 점점 개선되고 있습니다. 갭(gap), row-gap(row-gap), 칼럼-gap(column-gap) 속성은 브라우저 지원 수준도 다르다. 다음 레이아웃 모듈에서 간격 속성을 사용할 수 있습니다.

- Flexbox, `display: flex;` 선언으로 정의
- CSS 그리드, `디스플레이: 그리드;` 선언으로 정의
- 열 개수 및/또는 열 너비 속성으로 정의된 다중 열(멀티콜) 레이아웃

CSS Box Alignment Module(레벨 3) 사양에서 갭 속성의 공유 구문을 찾을 수 있습니다.

길이 값(px, em, 렘, cm, mm, vmin, vmax 등) 또는 백분율(%)과 함께 갭, 행 갭 및 컬럼 갭 속성을 사용할 수 있습니다.

Flexbox 컨텍스트에서는 Flex 항목이 아닌 Flex 컨테이너에 추가해야 합니다.

```css
.flex-container {
  row-gap: 10px;
  column-gap: 15px;
}
```

갭(gap)은 갭(row-gap)과 칼럼(column-gap)의 줄임말이다. 두 개의 값으로 사용할 경우 첫 번째 값은 행 갭에 속하고 두 번째 값은 열 갭에 속합니다.

```css
.flex-container {
  gap: 10px 15px;
}
```

한 값만 가지고 사용하면 행갭과 열갭이 같은 값을 갖는다.

```css
.flex-container {
  gap: 10px;
}
```

### 브라우저 지원

다양한 컨텍스트에서 지원을 표시하는 관련 Can I Use 테이블에서 갭 속성에 대한 브라우저 지원을 확인할 수 있습니다. CSS 그리드 레이아웃에서 가장 널리 지원되는 것을 알 수 있습니다. CSS 그리드 레이아웃이 처음 정의된 위치이기 때문입니다.

Flexbox 레이아웃에서 갭 속성은 현재 Edge 84+, Firefox 63+, Chrome 84+ 및 Opera 70+에서 지원됩니다. Internet Explorer(명백한 경우) 및 Safari는 이를 지원하지 않습니다.

갭(gap) 속성의 문제는 사파리 및 구형 브라우저의 경우 3개의 레이아웃 모듈(Flexbox, CSS grid, multicol)과 가장 널리 지원되는 컨텍스트(CSS grid)에 대한 @supports(@supports)의 브라우저 지원이 다르기 때문에 @supports(@supports) 기능 쿼리로 테스트할 수 없다는 점이다. Safari, IE 또는 이전 브라우저를 지원해야 하는 경우에는 JavaScript를 사용하여 추가 검사를 수행하지 않는 것이 좋습니다.

## 3. 컨텐츠-가시성(content-visibility) 속성

`콘텐츠 가시성` 속성을 통해 화면 외부 요소의 렌더링 프로세스(및 가시성)를 관리할 수 있습니다. 이 구성 요소는 CSS Containment Module의 일부로 페이지의 렌더링 성능을 크게 향상시키는 데 도움이 될 수 있습니다.

다음 세 가지 값을 사용할 수 있습니다.

- `visible`(기본값) — 요소의 렌더링이 정상적으로 수행됩니다.
- `숨김` — 요소 렌더링을 건너뛸 때(꺼짐 또는 화면 중)
- `자동` — 요소가 화면 밖에 있으면 렌더링이 생략되고, 요소가 화면에 나타나면 렌더링이 자동으로 구현됩니다.

렌더링 프로세스를 변경할 요소에 `콘텐츠 가시성` 속성을 간단히 추가할 수 있습니다.

```css
article {
  content-visibility: auto;
} 
```

컨텐츠 가시성이 자동화되면(`자동` 값을 취함) 렌더링 프로세스의 다양한 측면(레이아웃, 스타일, 페인트 및 크기 제약)이 자동으로 켜지고 꺼집니다. 이 훌륭한 가이드에서 자세한 내용을 읽을 수 있습니다. 저자들은 시험 사이트(여행 블로그)에서 내용-가시성:자동;이라는 규칙을 시험 블로그의 여러 섹션에 추가해 초기 페이지 로드에서 렌더링 시간을 7배 이상 단축(232ms에서 30ms로)할 수 있었다.

### 브라우저 지원

콘텐츠 가시성 속성은 현재 Chrome 85+, Edge 85+, Opera 71+에서 지원됩니다. 파이어폭스 팀도 이 기능을 추가하는 방안을 논의하고 있지만 아직 초기 단계입니다.

## 4. 컨테이너 내크기 재산

container-intrinic-size 속성은 크기 격납이 활성화된 요소의 명시적 폭과 높이를 정의하며, 이는 요소의 크기가 자식 크기에 영향을 받지 않음을 의미한다. 명시적 너비 및/또는 높이를 설정하는 것은 특정 상황에서 이러한 요소가 0으로 붕괴되는 것을 방지하는 것을 목표로 한다.

예를 들어, "content-visibility"가 "auto"로 설정된 경우 "container-intrinic-size"에 대한 값을 정의하는 것이 좋습니다(위의 섹션 참조). 화면 밖의 요소의 렌더링 프로세스를 건너뛰면 빈 요소로 렌더링되므로 기본적으로 너비와 높이가 `0`으로 설정됩니다. 그러나, 그들이 보기로 스크롤하기 전에, 그들의 컨텐츠는 자동적으로 화면에 렌더링되고, 이것은 스크롤바의 크기가 갑자기 바뀌는 것과 같은 UX 문제로 이어질 수 있다.

container-intrinic-size의 기본값은 none이지만 길이 값(px, 렘, em, cm, mm 등)도 사용할 수 있다. 하나 또는 두 개의 값과 함께 사용할 수 있습니다. 한 값의 고유 폭과 높이는 동일하고 두 값은 첫 번째 값이 폭을 제공하고 두 번째 값이 높이 값을 제공합니다. 예를 들어:

```css
article {
  content-visibility: auto;
  contain-intrinsic-size: 700px 1000px;
} 
```

### 브라우저 지원

콘텐트 인트로닉 사이즈는 현재 크롬 83+, 에지 83+, 오페라 69+가 지원하고 있다. Firefox는 그것을 지원하지 않는다.

## 5.:is()와:where() 사이비 클래스

선택기 레벨 4 규격에 의해 정의된 `:is() 및 `:where()` 의사 클래스는 더 긴 CSS 선택기 목록의 반복을 줄일 수 있게 한다. 반복 선택기 내에서 고유한 항목을 표시하여 여러 선택기 대신 하나의 선택기만 추가하면 됩니다.

이 두 의사 클래스는 거의 같은 일을 합니다. 유일한 차이점은 특수성 수준입니다. :is는 괄호 안에 있는 가장 구체적인 요소의 특수성을 취하며, ":여기서"의 특수성은 항상 " 특정 CSS 규칙을 재정의하는 것이 얼마나 쉬운지를 결정하기 때문에 구체성이 중요하다.

예를 들어 다음과 같은 선택 도구 목록이 있는 경우:

```coffeescript
.my-class p em,
.my-class li em,
.my-class section em {
    // CSS rules
}
```

해당 규칙을 후속 선언으로 재정의하기 어렵도록 특수성을 높게 유지하려면 `:is()`를 사용하여 목록을 단축할 수 있습니다.

```cpp
.my-class :is(p, li, section) em {
  // CSS rules
}
```

특수성 `0`을 유지하려면 `:where()`를 사용하여 해당 규칙을 쉽게 재정의할 수 있습니다.

```cpp
.my-class :where(p, li, section) em {
  // CSS rules
}
```

위의 예에서 `.my-class` 선택기는 `:where` 규칙을 재정의하지만 `:is`를 재정의하지는 않습니다. 아래 코드펜 데모에서 두 유사 클래스의 효과를 비교할 수 있습니다. 현재는 파이어폭스(파이어폭스:is)에서만 작동하지만, `:nywhere`는 작동하지 않습니다. 크롬://플래그/URL에서 `실험 웹 플랫폼 기능` 플래그를 활성화하면 크롬에서 기능을 테스트할 수 있습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="265" width="100%" name="cp_embed_2" scrolling="no" src="https://codepen.io/amonus/embed/dyMEbqQ?height=265&amp;theme-id=light&amp;default-tab=html%2Cresult&amp;user=amonus&amp;slug-hash=dyMEbqQ&amp;pen-title=Testing%20%3Ais%20and%20%3Awhere%20pseudo-classes&amp;name=cp_embed_2" style="width: 100%; overflow:hidden; display:block;" title="Testing :is and :where pseudo-classes" loading="lazy" id="cp_embed_dyMEbqQ"></iframe></div>

### 브라우저 지원

:is 의사 클래스는 현재 Firefox 78+ 및 Safari 14+에서 지원됩니다. 크롬 기반 브라우저(Chrome 15+, Edge 79+, Opera 15+)는 `:-webkit-any()` 접두어를 사용하여 접두어 구문을 지원한다. Chrome 68+, Opera 55+, Edge 79+에서 확인하도록 `실험용 웹 플랫폼 기능` 플래그를 설정하여 기능을 활성화할 수도 있습니다.

사이비 계급은 널리 지지받지 못한다. 현재는 파이어폭스 78+만 지원하지만, `실험용 웹 플랫폼 기능` 플래그를 사용하면 크롬 72+에서 활성화할 수 있다.

## 마지막 생각

이 문서에서 설명한 새로운 CSS 기능을 주의 깊게 사용해야 합니다. 이상적으로는 피드백 방법을 제공하거나, 접두사를 사용하거나, 또는 더 널리 구현될 때까지 기다려야 합니다.

그러나 실험을 원한다면 이미 컨텐츠 가시성 및 컨테이너 내부 크기 속성을 사용할 수 있습니다. 이미 지원하는 브라우저에서 성능 최적화를 수행할 수 있으며(@supports 규칙으로 브라우저 지원을 테스트할 수 있음) 아직 지원하지 않는 브라우저에는 영향을 미치지 않습니다.

이 5가지 새로운 CSS 기능 외에도 아직 어떤 브라우저에도 구현되지 않은 가로 세로 비율(W3, MDN, Can I Use), 선도 트리밍(leading-trim) 속성, 그리고 어떤 브라우저에도 구현되지 않은 :marker 의사 요소(W3, MDN, I) 등 여러 가지 흥미로운 개발이 진행되고 있다.아리

전반적으로, 새로운 CSS 기능의 표준화 및 구현 과정을 지속적으로 지켜볼 가치가 있습니다.브라우저에는 궁극적으로 프런트엔드 개발을 더 쉽고 빠르게 할 수 있는 많은 유용한 기능들이 있다.