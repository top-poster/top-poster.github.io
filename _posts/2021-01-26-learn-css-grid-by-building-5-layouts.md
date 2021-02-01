---
layout: post
title: "17 분 안에 5 개의 레이아웃을 구축하여 CSS 그리드 배우기
 "
author: 'Code Tower'
thumbnail: https://www.freecodecamp.org/news/content/images/size/w600/2021/01/GridThumb.jpg
tags: undefined
---


CSS Grid는 웹 사이트의 레이아웃을 만드는 데 사용할 수있는 도구입니다.
 다른 요소의 위치, 레이어 또는 크기에 대해 생각해야하는 경우 특히 유용합니다.
 

CSS Grid는 복잡하고 배울 것이 많습니다.
 그러나 좋은 소식은 모든 것을 한꺼번에 알 필요가 없다는 것입니다.
 

이 튜토리얼에서는 CSS Grid를 사용하여 5 개의 다른 레이아웃 (아래 5 개의 개별 작업으로 설명 됨)을 빌드합니다.
 튜토리얼이 끝나면 다음 프로젝트에서 CSS Grid를 사용할 수 있습니다.
 

함께 코딩하려면 리소스를 다운로드해야합니다.
 

- 작업-디자인
 
- CSS 그리드 스타터
 

이 기사를 보완하려면 다음 비디오를 시청할 수 있습니다.
 

다음은 우리가 구축 할 처음 두 가지 레이아웃입니다.
 

![image](https://dev-to-uploads.s3.amazonaws.com/i/xciaiefay1v6in3hegmw.png)

## 1 : CSS 그리드로 팬케이크 스택을 구축하는 방법
 

첫 번째 작업에서는 팬케이크 스택 레이아웃을 만들어야합니다.
 이 레이아웃을 만들기 위해`grid-template-rows : auto 1fr auto`를 사용하여 3 개의 행을 만들 수 있습니다.
 값이 `1fr`인 두 번째 행은 가능한 한 많이 확장되는 반면 다른 두 행은 내용을 래핑하여 충분한 공간 만 가지고 있습니다.
 

따라서이 레이아웃을 달성하려면 컨테이너에 다음 매개 변수를 제공하기 만하면됩니다.
 

```css
.task-1.container {
  display: grid;
  height: 100vh;

  grid-template-rows: auto 1fr auto;
}

```

예를 들어 내 튜토리얼 중 하나에서이 레이아웃을 모든 곳에서 볼 수 있습니다.
 

![image](https://dev-to-uploads.s3.amazonaws.com/i/srqeinbsoirmayvi2uvf.png)

함께 시청하고 코딩하려면 여기에 YouTube 링크가 있습니다.
 

## 2 : CSS 그리드를 사용하여 간단한 12 기둥 그리드 레이아웃을 구축하는 방법
 

기본 12 개 기둥 그리드 레이아웃은 영원히 사용되었습니다.
 CSS Grid를 사용하면 훨씬 더 쉽게 사용할 수 있습니다.
 이 간단한 작업에서는`item-1`에 4 개의 열을,`items-2`에 6 개의 열을 제공해야합니다.
 

먼저 12 개의 열을 만들어야합니다.
 `grid-template-columns : repeat (12, 1fr);`으로 할 수 있습니다.
 

```css
.task-2.container {
  display: grid;
  height: 100vh;

  grid-template-columns: repeat(12, 1fr);
  column-gap: 12px;

  align-items: center;
}

```

여기에서 모든 열 사이에`12px` 간격이 있습니다.
 Flex와 마찬가지로`align-items` 및`justify-content`도 사용할 수 있습니다.
 

다음으로해야 할 일은 항목이 차지해야하는 열을 지정하는 것입니다.
 

항목 1의 경우 2 열에서 시작하여 6 번에서 끝나기를 원합니다.
 

```css
.task-2 .item-1 {
  grid-column-start: 2;
  grid-column-end: 6;
}

```

항목에는 열 번호 6이 포함되지 않고 열 2, 3, 4, 5 만 포함됩니다.
 

다음과 같이 작성하여 동일한 영향을 미칠 수도 있습니다.
 

```css
.task-2 .item-1 {
  grid-column-start: 2;
  grid-column-end: span 4;
}

```

또는
 

```css
.task-2 .item-1 {
  grid-column: 2 / span 4;
}

```

동일한 논리로 항목 2에 대해 다음을 갖게됩니다.
 

```css
.task-2 .item-2 {
  grid-column: 6 / span 6;
}

```

12 개의 열 레이아웃이 어디에나있는 것을 볼 수 있습니다. 여기에이 기술을 사용하는 튜토리얼이 있습니다.
 

![image](https://dev-to-uploads.s3.amazonaws.com/i/bj6vyu3smpr4705k8ynq.png)

함께 시청하고 코딩하려면 여기에 YouTube 링크가 있습니다.
 

## 3 :`grid-template-areas`를 사용하거나 사용하지 않고 반응 형 레이아웃을 구축하는 방법
 

여기서 두 가지 옵션을 보여 드리겠습니다.
 첫 번째 옵션에서는 두 번째 작업에서 배운 12 개의 기둥 그리드를 사용합니다.
 

두 번째 옵션으로`grid-template-areas`라는 속성을 사용합니다.
 

![image](https://dev-to-uploads.s3.amazonaws.com/i/z65zhc0qjbotart85kny.png)

### 첫 번째 옵션 : 12 기둥 그리드를 사용하는 방법
 

이것은 매우 간단합니다.
 첫 번째 과제에서 배운 것을 사용하여 메인 섹션을 확장 할 수 있습니다.
 또한 데스크톱에서와 같이 그리드에 `간격 : 24px`를 지정할 수도 있습니다.
 행뿐만 아니라 열이 있습니다.
 

```css
.task-3-1.container {
  display: grid;
  height: 100vh;

  grid-template-rows: auto auto 1fr auto auto auto;
  gap: 24px;
}

```

화면이 `720px`보다 넓은 태블릿에서는 열 12 개와 행 4 개를 원합니다.
 세 번째 행은 가능한 한 많이 확장됩니다.
 

```css
@media (min-width: 720px) {
  .task-3-1.container {
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: auto auto 1fr auto;
  }
}

```

이제 12 개의 열이 있으므로 각 항목이 차지해야하는 열 수를 알려야합니다.
 

```css
@media (min-width: 720px) {
 
  // The header section takes 12 columns
  .task-3-1 .header {
    grid-column: 1 / span 12;
  }

  // The navigation section also takes 12 columns
  .task-3-1 .navigation {
    grid-column: 1 / span 12;
  }

  // The main section takes 10 columns start from column 3
  .task-3-1 .main {
    grid-column: 3 / span 10;
  }

  // The sidebar takes 2 columns start from column 1
  .task-3-1 .sidebar {
    grid-column: 1 / span 2;
    grid-row: 3;
  }

  // The ads section takes 2 columns start from column 1
  .task-3-1 .ads {
    grid-column: 1 / span 2;
  }

  // The footer section takes 10 columns start from column 3
  .task-3-1 .footer {
    grid-column: 3 / span 10;
  }
}

```

여기서는 사이드 바가 DOM의`main` 섹션 뒤에 있기 때문에`.task-3-1 .sidebar``grid-row : 3;`을 제공해야합니다.
 

데스크톱보기의 경우`1020px`보다 큰 화면으로 작업합니다.
 이미 12 개의 열이 있으므로 이제 사용할 열 수만 지정하면됩니다.
 

```css
@media (min-width: 1020px) {

  // The navigation takes 8 columns starting from column 3
  .task-3-1 .navigation {
    grid-column: 3 / span 8;
  }

  // The main section takes 8 columns starting from column 3
  .task-3-1 .main {
    grid-column: 3 / span 8;
  }

  // The sidebar starts from row 2 and ends at row 4
  .task-3-1 .sidebar {
    grid-row: 2 / 4
  }

  // The ads section takes 2 columns starting from column 11
  // it also takes 2 rows starting from row 2 and ending at row 4
  .task-3-1 .ads {
    grid-column: 11 / span 2;
    grid-row: 2 / 4;
  }

  // The footer section takes 12 columns start from column 1
  .task-3-1 .footer {
    grid-column: 1 / span 12;
  }
}

```

### 실제 사례
 

실제로 Dev.to의 홈페이지에서 유사한 레이아웃을 찾을 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/Screenshot-2021-01-27-at-0.36.34.png)

### 두 번째 옵션 :`grid-template-areas` 사용 방법
 

`grid-template-areas`를 사용하기 전에`grid-area`를 사용하여 항목의 영역을 정의해야합니다.
 

```css
.task-3-2 .header {
  grid-area: header;
}

.task-3-2 .navigation {
  grid-area: nav;
}

.task-3-2 .ads {
  grid-area: ads;
}

.task-3-2 .sidebar {
  grid-area: sidebar;
}

.task-3-2 .main {
  grid-area: main;
}

.task-3-2 .footer {
  grid-area: footer;
}

```

항목 영역을 정의한 후`grid-template-areas`를 사용하여 컨테이너에 위치를 지정하기 만하면됩니다.
 

```css
.task-3-2.container {
  display: grid;
  height: 100vh;

  gap: 24px;

  // Creating 6 rows and 3rd row expands as much as it can  
  grid-template-rows: auto auto 1fr auto auto auto;

  // Defining the template
  grid-template-areas:
    "header"
    "nav"
    "main"
    "sidebar"
    "ads"
    "footer";
}

```

따라서 모바일에서는 1 개의 열과 6 개의 행을 만듭니다.
 그리고 주 행인 행 번호 3은 최대한 확장해야합니다.
 

또한 나중에 항목의 순서 / 위치를 변경하려는 경우에도 쉽게 할 수 있습니다.
 예를 들어 헤더 이전에 탐색을 원하면 다음을 수행 할 수 있습니다.
 

```css
...
 grid-template-areas:
    "nav"
    "header"
    "main"
    "sidebar"
    "ads"
    "footer";
...

```

```css
@media (min-width: 720px) {
  .task-3-2.container {
    // Creating 4 rows and the 3rd row expands as much as it can
    grid-template-rows: auto auto 1fr auto;
      
    // Defining the template (3 columns)
    grid-template-areas:
      "header header header"
      "nav nav nav "
      "sidebar main main"
      "ads footer footer";
  }
}

```

위의 코드를 사용하여 화면이 720px보다 넓은 경우 3 개의 열과 4 개의 행을 생성하려고합니다.
 헤더와 내비게이션은 모두 3 개의 열을 차지합니다.
 

세 번째와 네 번째 행에서 사이드 바와 광고는 1 개의 열을 사용하고 기본 및 바닥 글은 2 개의 열을 사용합니다.
 

```css
@media (min-width: 1020px) {
  .task-3-2.container {
    // Creating 4 rows and the 3rd row expands as much as it can
    grid-template-rows: auto auto 1fr auto;
      
    // Defining the template (4 columns)
    grid-template-areas:
      "header header header header"
      "sidebar nav nav ads"
      "sidebar main main ads"
      "footer footer footer footer";
  }
}


```

여기에서 태블릿보기와 유사한 논리를 찾을 수 있습니다.
 데스크톱의 경우 `grid-template-areas`값에 따라 4 개의 열과 4 개의 행과 배치를 생성합니다.
 

### 무엇을 선택해야합니까?
 

12 기둥 그리드 사용 :
 

➕ 쉽고 빠른 시작
➕ 열 중심 레이아웃 유지 관리가 쉽습니다.
➖ 복잡한 레이아웃으로 항목을 정렬하기 어려움
 

주로 기둥 배열에 초점을 맞춘 덜 복잡한 레이아웃의 경우 12 기둥 그리드를 사용해야합니다.
 

`grid-template-areas` 사용 :
 

➕ 복잡한 레이아웃에 유연함
➕ 시각화하기 쉬움
➖ 구현하는 데 더 많은 시간이 걸립니다.
 

많은 요소의 위치 나 크기에 신경을 써야하는 더 복잡한 레이아웃의 경우`grid-template-areas`를 사용해야합니다.
 

두 옵션 모두 장단점이 있지만 자신에게 더 쉽고 특정 시나리오에서 의미가있는 것을 선택해야합니다.
 

## 4 : CSS 그리드에서 미디어 쿼리없이 반응 형 레이아웃을 구축하는 방법
 

![image](https://dev-to-uploads.s3.amazonaws.com/i/61w4dsetc7xtq4uspz8v.png)

이렇게하는 것은 놀랍도록 간단합니다.
 다음과 같이 한 줄의 코드로 만들 수 있습니다.`grid-template-columns : repeat (auto-fill, minmax (150px, 1fr));`
 

```css
.task-4.container {
  display: grid;
  gap: 24px;

  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
}

```

방금 유연한 열 레이아웃을 만들고 열이 150px보다 작아서는 안되며 공간을 균등하게 공유해야한다고 지정했습니다.
 

## 5 : CSS 그리드로 12 x 12 Chess 그리드를 만드는 방법
 

마지막 작업에서는 열 수를 정의 할 수있을뿐만 아니라 CSS 그리드를 사용하여 행 수를 정의 할 수도 있습니다.
 

```css
.task-5.container {
  display: grid;
  height: 100vh;

  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: repeat(12, 1fr);
}

```

이제 원하는 곳에 항목을 배치 할 수 있습니다.
 따라서이 레이아웃을 생성하려면 :
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/Screenshot-2021-01-27-at-0.49.40.png)

우리는 할 수있어:
 verified_user

```css
...
// First item starts from column 1 and expand 3 columns
// and from row 1 and expand 3 columns
.task-5 .item-1 {
    grid-row: 1 / span 3;
    grid-column: 1 / span 3;
}

// Second item starts from column 4 and expand 3 columns
// and from row 4 and expand 3 columns
.task-5 .item-2 {
    grid-row: 4 / span 3;
    grid-column: 4 / span 3;
}

// First item starts from column 7 and expand 3 columns
// and from row 7 and expand 3 columns
.task-5 .item-3 {
    grid-row: 7 / span 3;
    grid-column: 7 / span 3;
}

// First item starts from column 10 and expand 3 columns
// and from row 10 and expand 3 columns
.task-5 .item-4 {
    grid-row: 10 / span 3;
    grid-column: 10 / span 3;
}

```

## 결론
 

이 기사를 읽어 주셔서 감사합니다.
 이 주제는 Learn.DevChallenges.io에서 업데이트 할 비디오 시리즈에 속합니다.
 업데이트라고 말하려면 소셜 미디어에서 나를 따르거나 YouTube 채널을 구독하십시오.
 그렇지 않으면 즐거운 코딩을하고 다음 비디오와 기사에서 만나요 👋.
 

## __________ 🐣 내 정보 __________
 

저는 풀 스택 개발자, UX / UI 디자이너 및 콘텐츠 제작자입니다.
 이 짧은 비디오에서 저에 대해 더 많이 알 수 있습니다.
 

- 저는 DevChallenges의 창립자입니다.
 
- 내 유튜브 채널 구독
 
- 내 트위터 팔로우
 
- Discord 가입
 