---
layout: post
title: "Chrome DevTools를 사용하여 CSS 그리드 디버그"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/Chrome-DevTools-CSS-grid-debug.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/Chrome-DevTools-CSS-grid-debug.png?fit=730%2C487&ssl=1)

CSS 그리드는 해마다 인기가 높아지고 있는 강력한 레이아웃 사양이다. 올해 국가 CSS 조사에서는 응답자의 73.3%가 그리드를 사용해 2019년보다 18.6% 증가한 것으로 나타났다.

그리드의 성공 사례에서 중요한 부분은 이제 대부분의 주요 브라우저가 DevTools에 그리드 지원을 추가했다는 것이다. 육안 검사는 인터페이스를 구축할 때 보다 효율적인 피드백 루프를 제공합니다.

그러면 레이아웃 문제를 디버깅할 때 발생할 수 있는 몇 가지 문제를 해결할 수 있습니다. 레이아웃을 빌드할 때 수행되는 작업을 확인하기 위해 빨간색 테두리(또는 유사한 해킹)를 더 이상 추가하지 않아도 됩니다.

이 기사에서는 크롬에 초점을 맞추겠습니다. 페이지의 그리드를 검색하는 방법과 페이지 레이아웃 및 디버그 레이아웃 문제를 검사하는 방법에 대한 팁을 알려드리겠습니다.

## Chrome DevTools를 사용하여 페이지에서 그리드 검색

첫 번째, 첫 번째. HTML 요소를 그리드로 만드는 이유는 무엇입니까?

페이지의 HTML 요소에 `display:grid` 또는 `display:inline-grid`가 적용된 경우, 이는 그리드입니다.

DevTools 설정에서 Grid 디버깅을 사용하도록 설정해야 할 수 있습니다. Grid 디버깅은 여전히 실험적인 설정으로 간주되기 때문입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/enable-grid.png?resize=732%2C461&ssl=1)

페이지에 다음과 같은 방법으로 그리드 요소가 있는지 빠르게 검색할 수 있습니다.

### 1. 점검에 의한 발견

요소 창에는 그리드 요소 옆에 회색 `그리드` 배지가 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/elements-tab-find-grids.png?resize=708%2C306&ssl=1)

이 간단한 예에서 "래퍼" 클래스가 있는 "div"는 그리드입니다.

이 배지를 클릭하면 요소에 대한 오버레이가 다음과 같이 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/toggle-grid-overlay.png?resize=721%2C316&ssl=1)

배지의 색상이 변경되어 현재 활성 상태임을 나타냅니다.

오버레이에는 다음이 표시됩니다.

- 번호가 매겨진 격자선
- 그리드 항목 사이의 간격은 점선 패턴을 가진 거터로 표시됩니다. 우리의 경우는 10px로 쉽게 알 수 있습니다. 물론 틈이 없을 수도 있고, 시궁창도 없을 것이다. 대신 그리드 항목이 서로 플러시되는 것을 볼 수 있습니다(아래 참조).

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/grid-no-gaps.png?resize=383%2C247&ssl=1)

### 2. 개요별 검색

좀 더 복잡한 웹 페이지에는 그리드가 많을 수 있으며, 이전의 예처럼 `몸통`의 자녀가 아닐 수도 있다. 페이지의 레이아웃을 이해하려면 검사로 찾는 것이 더 많은 작업입니다.

Layout(레이아웃) 창에는 그리드 섹션이 있으며,

그리드를 사용하여 인스타그램의 프로필 페이지를 다시 만드는 다른 예를 들어보겠습니다. 8개의 그리드가 있습니다. 하위 제목인 "그리드 오버레이" 아래에 나열된 항목을 볼 수 있습니다(아래의 빨간색 상자 참조).

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/layout-pane.png?resize=663%2C656&ssl=1)

그리드는 발생 순서대로 나열됩니다. 레이블은 요소 이름, ID 선택기(있는 경우) 및 클래스 선택기(있는 경우)로 구성됩니다.

인접 확인란을 선택하여 오버레이를 표시할 수 있습니다.

![image](https://i0.wp.com/roboleary.net/assets/img/logrocket/chrome-devtools/img/select-overlay.gif?resize=600%2C672&ssl=1)

보시는 바와 같이 레이아웃 창에 몇 가지 다른 설정이 있으며, 이 설정은 다음에 다룹니다.

## Chrome에서 오버레이 사용자 정의

레이아웃 창에서 오버레이를 사용자 정의하는 몇 가지 옵션이 있습니다. 먼저 그리드 오버레이 섹션을 살펴보겠습니다.

### 그리드 오버레이 섹션

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/grid-overlay-options.png?resize=324%2C261&ssl=1)

Chrome은 오버레이를 쉽게 구분할 수 있도록 기본적으로 여러 색상을 선택합니다. 직접 선택하려는 경우 인접 색상 선택기가 있습니다. 그리드 오버레이 색상과 배경 사이에 충돌이 있는 경우 특히 유용합니다.

이 섹션의 두 번째 옵션은 웹 페이지의 요소에 대한 오버레이를 강조 표시하고 요소 창에서 요소를 선택합니다(아래 참조). 이렇게 하면 오버레이가 사용자 정의되지 않고 사용자의 방향을 지정하고 관련 스타일을 탐색하는 데 도움이 됩니다.

![image](https://i1.wp.com/roboleary.net/assets/img/logrocket/chrome-devtools/img/select.gif?ssl=1)

### 오버레이 표시 설정

이 디스플레이 설정이 무엇인지 분명히 알 수 있을 것입니다. 하지만 자세한 사항에 대해 각 설정을 살펴보도록 하겠습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/overlay-display-settings.png?resize=308%2C199&ssl=1)

#### 1. 트랙 크기 설정 표시

트랙 크기 표시 설정은 그리드 항목이 특정 크기로 끝나는 이유를 이해하는 데 유용합니다.

용어를 정확하게 이해하려면 트랙을 정의해 봅시다.

W3C에 의한 CSS 그리드 레이아웃 모듈 레벨 1에 따르면, 그리드 트랙은 그리드 열 또는 그리드 행의 일반적인 용어이다. 즉, 두 개의 인접한 그리드 선 사이의 공간이다. 각 그리드 트랙에는 크기 조정 기능이 할당되어 있으며, 이 기능은 열 또는 행이 얼마나 넓거나 높게 성장할 수 있는지, 그리고 그 경계 그리드 선이 얼마나 떨어져 있는지 제어합니다.

이 컨텍스트에서는 여러 행 또는 열에 걸쳐 있는 그리드 항목의 너비와 높이를 알려 주는 것입니다.

트랙 크기 레이블의 형식은 `[authorized size] - [computed size]입니다. `authorated size`는 스타일시트에 선언된 크기입니다(정의된 것이 없는 경우 생략됨). 계산된 크기는 화면에 표시되는 실제 크기입니다.

이것을 설명하기 위해 우리의 인스타그램 예시로 돌아가자.

스토리 섹션의 폭은 115px로 고정되어 있음을 알 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/track-size-story.png?resize=759%2C152&ssl=1)

```css
.story {
    display: grid;
    grid-template-columns: repeat(auto-fill, 115px);
}
```

크기가 고정되어 있기 때문에 열의 작성자 크기와 실제 크기가 동일합니다.

해당 트랙에 대해 선언된 것이 없으므로 행 레이블은 실제 크기만 표시합니다.

게시물 섹션의 경우 열 너비는 범위입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/track-size-gallery.png?resize=730%2C279&ssl=1)

```css
.tabbed-pane__posts {
   display: grid;
   grid-template-columns: repeat(3, minmax(50px, 1fr));
   grid-gap: 25px;
}
```

작성된 크기는 minmax(50px, 1fr)이고 실제 크기는 이 뷰포트 크기의 경우 236px입니다. 실제 크기는 뷰포트 너비에 따라 달라집니다.

행 레이블에 대해 정의된 것이 없으므로 행 레이블은 실제 크기만 표시합니다.

#### 2. 영역 이름 설정 표시

그리드에서 영역 이름을 지정한 경우 이 설정은 각 항목의 왼쪽 상단 모서리에 삽입 레이블로 표시됩니다.

이 예에서는 머리글, 사이드바 및 콘텐츠의 세 가지 영역에 이름을 지정합니다. 우리는 `그리드 템플릿 영역`을 사용하여 트랙에 영역을 할당한다.

첫 번째 그리드 항목을 비워 두었으므로 라벨이 없습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/area-names.png?resize=429%2C205&ssl=1)

```undefined
<div class="wrapper">
  <div class="box header">Header</div>
  <div class="box sidebar">Sidebar</div>
  <div class="box content">Content</div>
</div>
```

```css
.sidebar {
 grid-area: sidebar;
}

.content {
 grid-area: content;
}

.header {
 grid-area: header;
}

.wrapper {
 display: grid;
 grid-gap: 10px;
 grid-template-columns: 120px  120px  120px;
 grid-template-areas:
   "....... header  header"
   "sidebar content content";
}
```

#### 3. 격자선 설정 연장

독립 요소를 정렬하려는 경우 확장 그리드 선 설정이 유용합니다. 그리드의 경계를 벗어나는 점선을 생성합니다.

우리의 인스타그램 예에서, 우리가 헤더와 스토리 섹션을 어떤 식으로든 정렬하고 싶다면, 확장된 격자선이 지침으로 작용할 수 있다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/extend-grid-lines.png?resize=730%2C434&ssl=1)

## Chrome DevTools를 사용한 디버깅 팁

요소에 ID 또는 클래스를 추가하거나 영역 이름을 사용하여 그리드를 쉽게 식별할 수 있습니다.

우리의 인스타그램 예에서, 우리는 처음에 그리드이지만 ID나 클래스가 없는 세 개의 `ul` 요소를 가지고 있었다. (빨간색 상자 참조). 그래서 우리는 그것들을 구별할 수 없습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/ul-grids.png?resize=318%2C296&ssl=1)

ul에 클래스를 추가하면 더 쉽게 식별할 수 있습니다!

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/ul-grids-classes.png?resize=346%2C299&ssl=1)

진부하게 들릴지 모르지만 예방이 치료보다 낫다. 레이아웃을 먼저 계획하면 일부 문제를 방지할 수 있습니다. 그리드의 힘은 질서를 잡기가 더 쉽다는 것입니다. 그래서 여러분이 그것을 하기 전에 여러분이 무엇을 하고 있는지 결정하는 것은 어떨까요?

설계가 상세할 필요는 없으며, 아래 설계로도 충분합니다.

중요한 부분은 레이아웃의 요소 크기, 정렬 및 간격을 미리 고려하는 것입니다. 다양한 장치 크기에 맞게 설계하고 사용 가능한 공간 내에서 콘텐츠를 표시하는 방법을 결정해야 합니다. 여러 장치 크기에 대해 항목을 줄바꿈하거나 그리드 레이아웃을 변경해야 할 수 있습니다.

나는 레이아웃 디버그 방법을 규정적으로 항목화 하고 싶지 않다. 너무 많은 순열이 있다. 대신, 저는 우리의 인스타그램 예에서 몇 가지 디자인 결정에 대해 토론할 것입니다. 이렇게 하면 문제를 검토하고 해결할 수 있는 방법에 대한 몇 가지 아이디어를 얻을 수 있습니다.

## 버그의 가장 일반적인 원인

트랙과 콘텐츠의 크기가 문제의 가장 일반적인 원인일 수 있습니다.

트랙의 고정 크기를 설정하는 경우 물론 원하는 내용이 아닌 한 내용이 오버플로되지 않도록 해야 합니다. 오버플로를 처리하려면 콘텐츠의 고정 크기를 설정하고, `오버플로` 속성 또는 이미지 및 비디오의 `개체 적합` 속성을 사용할 수 있습니다.

예를 들어, 인스타그램 예제의 스토리 섹션에는 모바일 장치의 고정 열 크기가 115px로 설정되어 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/track-size-story-1.png?resize=759%2C152&ssl=1)

우리는 이미지의 크기를 56px로 고정합니다. 이미지 아래에 있는 제목(`h2`)의 경우 오버플로우되는 내용을 숨기고 텍스트 줄바꿈을 금지합니다(그래서 항상 텍스트의 한 줄임). 텍스트 오버플로:ellipsis;`를 추가하면 텍스트 끝에 타원이 추가되어 텍스트가 잘렸음을 나타냅니다.

이제 우리는 모든 "이야기"의 크기가 균일하다는 것을 보장할 수 있다. 동적 컨텐츠를 다룰 때는 다음과 같은 상황이 발생할 것으로 예상해야 합니다.

```css
.story img {
    border-radius: 50%;
    width: 56px;
    height: 56px;
}

.story h2 {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
```

만약 우리가 헤딩의 오버플로우를 관리하지 않는다면, 다음과 같을 것이다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/story-no-overflow.png?resize=237%2C107&ssl=1)

크기 조정의 다른 부분은 사용하는 단위입니다. 항목의 크기가 동적인 경우에만 fr 또는 minmax()를 선택해야 합니다. 크기를 고정하려면 다른 값을 사용하십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/header.png?resize=490%2C56&ssl=1)

헤더 섹션을 예로 들어 보겠습니다. 첫 번째 열(트랙)의 폭은 1개의 분수 단위(`1fr`)입니다. 이는 사용 가능한 공간이 더 많을 때 증가할 것입니다. 로고 이미지의 크기가 절대적이고 아이템의 시작 부분에 위치하기 때문에 로고 이미지의 크기 조정이나 이동이 발생하지 않습니다.

최소 너비 또는 최소 높이로 선언된 콘텐츠가 있는 경우 분수 단위로 예기치 않은 결과를 얻을 수 있습니다.

`fr` 단위는 사용 가능한 공간을 채우지만 그리드 컨테이너의 최소 크기나 행 또는 열의 내용보다 작지 않습니다.

```css
header {
    display: grid;
    grid-template-columns: 1fr auto auto;
    align-items: center;
    width: 100%;
}

.header__logo {
    justify-self: start;
}
```

다른 두 그리드 항목(로그인 및 등록 버튼)은 항상 동일한 크기를 유지합니다.

문제를 해결할 때 보기 포트의 크기를 조정하여 그리드 항목의 크기가 어떻게 변하는지 확인하는 것이 유용할 수 있습니다. 이 예제에서는 크기를 변경하는 첫 번째 항목임을 분명히 확인할 수 있습니다.

![image](https://i1.wp.com/roboleary.net/assets/img/logrocket/chrome-devtools/img/resizing-header.gif?resize=442%2C57&ssl=1)

## 결론

Chrome DevTools에 그리드 지원이 추가되어 이제 레이아웃을 쉽게 시각화하고 디버깅할 수 있습니다. 레이아웃을 구축하는 동안 더 나은 피드백을 얻는 것은 생산성을 크게 향상시키는 것입니다. 나는 이 기사가 CSS Grid를 사용할 때 Chrome DevTools를 사용함으로써 얻을 수 있는 이점을 보여줄 수 있기를 바랍니다.