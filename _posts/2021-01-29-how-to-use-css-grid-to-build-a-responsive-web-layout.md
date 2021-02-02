---
layout: post
title: "CSS Grid를 사용하여 반응 형 웹 레이아웃을 만드는 방법
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-21-at-11.30.27-AM.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-21-at-11.30.27-AM.png?fit=730%2C487&ssl=1)

CSS 그리드 레이아웃은 웹 개발자가 웹 애플리케이션을위한 일관되고 매끄러운 레이아웃을 만들기 위해 요소를 열과 행으로 나눌 수 있도록 설계된 2 차원 그리드 시스템입니다.
 

CSS 그리드의 논리는 개발자가 `div`와 같은 요소를 가져 와서 그리드를 표시하면 해당 요소를 열과 행 (통칭 트랙이라고 함)으로 분할하여 항목을 가져 와서 배치 할 수 있다는 것입니다.
 그리드.
 CSS 그리드를 사용하면 위치 지정 속성 (`top`,`right`,`left`,`bottom`)을 사용하는 추가 작업없이이 모든 작업을 수행 할 수 있습니다.
 

CSS 프레임 워크를 사용하는 경우와 CSS Grid를 사용하는 경우가 있지만 웹 개발의 대부분과 마찬가지로 사용 사례에 따라 다릅니다.
 

이 기사에서는 CSS 그리드를 사용하여 간단한 반응 형 웹 애플리케이션을 구축하기 위해 행, 열 및 영역을 사용하는 기본 디자인에 중점을 둘 것입니다.
 

## CSS 그리드 : 기본 용어
 

잠시 시간을내어 CSS Grid의 핵심 기본 사항을 이해하는 것으로 시작하겠습니다.
 

- 그리드 컨테이너 – 그리드를 보관하는 상자.
 이것은 CSS 그리드의 주요 빌딩 블록입니다.
 
- 그리드 셀 – 그리드에서 하나씩
 
- 그리드 영역 – 정사각형 또는 직사각형 (L 자형이 아님)의 형태를 취하는 단일 셀 또는 여러 셀
 
- 그리드 트랙 –`grid-template-columns` 및`grid-template-rows` 속성을 사용하여 정의 된 행 및 열 모음
 
- 그리드 간격 – 그리드에 공간을 만드는 데 도움이됩니다.
 그리드 간격 안이나 안에 내용을 가질 수 없습니다.
 
- 명시 적 그리드 –`grid-template-columns` 및`grid-template-rows`를 사용하여 정의
 
- 암시 적 그리드 – 브라우저에 의해 정의 됨
 

## CSS 그리드 시작하기
 

### 그리드 컨테이너 및 요소 표시
 

그리드 컨테이너는 요소에 그리드를 표시하기위한 시작점입니다.
 그리드를 사용하려면 먼저 아래 구문을 사용하여 컨테이너에 그리드를 표시해야합니다.
 

```undefined
display: grid;
```

CSS Grid에서 그리드 컨테이너와 그 항목 (요소) 간의 관계는 각각 부모와 자식의 관계입니다.
 모든 그리드에는 항목을 포함하는 컨테이너가 있으며 그리드에 배치 된 모든 요소는 그리드 항목입니다.
 

컨테이너를 그리드 (`display : grid;`)로 만들면 모든 직계 자식이 자동으로 그리드 항목이됩니다.
 

```js
<div class="container"> <div class ="item item1">1</div> <div class = "item item2">2</div> </div> .container{ display: grid; }
```

그러나 항목을 그리드로 표시하는 것은 그 자체로 많은 일을하지 않습니다.
 아래 데모 앱 코드 스 니펫에서 볼 수 있듯이`grid-template-columns` 및`grid-template-rows`와 같은 다른 개념이 작동하는 곳입니다.
 

### `fr` 단위 및`repeat ()`표기법 사용
 

그리드를 정의 할 때 항상`px`와 같은 고정 측정 단위를 사용할 수 있지만 CSS 그리드 레이아웃에는`fr` 단위라는 새로운 측정 단위도 도입되었습니다.
 

fr 단위는 그리드 컨테이너에서 사용 가능한 공간의 일부를 나타내며 유연한 그리드 트랙을 만드는 데 사용할 수 있습니다.
 

예를 들면 다음과 같습니다.
 

```css
.container {
display: grid;
grid-template-columns: 2fr 1fr 1fr;
}
```

위의 코드 조각은 사용 가능한 공간을 네 부분으로 분할합니다. 두 부분은 첫 번째 트랙에 할당되고 나머지 두 부분은 남은 사용 가능한 공간에 따라 각각 하나의 트랙에 할당됩니다.
 이렇게하면 그리드를 구축 할 때 일관성을 유지할 수 있습니다.
 

`repeat ()`표기법은 일관성을 보장하는 데 도움이되며 다음과 같이 트랙 목록의 전체 또는 일부를 반복하는 데 사용할 수 있습니다.
 

```css
.container {
display: grid;
grid-template-columns: repeat(3, 1fr);
}
```

## 데모 : CSS Grid로 웹앱 빌드
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/freeborncharles/embed/ZEpVLzR?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=freeborncharles&amp;slug-hash=ZEpVLzR&amp;pen-title=Responsive%20Grid%20Demo&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="Responsive Grid Demo" loading="lazy" id="cp_embed_ZEpVLzR"></iframe></div>

여기에 완전한 CodePen을 포함 시켰지만, 아래에서는 CSS Grid를 사용하여 웹 앱을 빌드하는 방법을 보여주기 위해 내부 주석과 함께 코드 스 니펫을 분석하겠습니다.
 

### HTML
 

먼저 간단한 HTML 템플릿을 만들고 일부 구조를 설정하는 데 도움이되는 더미 텍스트로 채울 것입니다.
 

```undefined
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset=utf-8>
  <title>Demo Responsive CSS Grid Site</title>
  <link rel="stylesheet" href="/main.css">
</head>

<!-- Main body of the site where we have the container --> 

<!-- this div contain all elements that will be visible on the screen -->

<div class="container">     
  <nav>
    <ul>
      <li><a href="#">Home</a></li>
      <li><a href="#">About</a></li>
      <li><a href="#">Contact</a></li>
    </ul>  
  </nav>
  <main><h2>A Demo Site showcasing CSS Grid</h2>
    <p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Itaque pariatur possimus alias quod ratione incidunt dicta assumenda repudiandae optio eveniet, quisquam fuga! Nam eaque fuga similique quia, esse non libero?</p>
    <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Illo libero doloremque, eum quis laudantium hic iste ab sed ipsum veniam, quam dolor rem cupiditate corrupti aliquam repudiandae officia soluta impedit!</p>
  </main>
  <div class="sidebar img"><h2>Find Me on Social Media</h2>
    <p><a href="https://twitter.com/charliecodes">Twitter</a></p>
    <p><a href="https://www.linkedin.com/in/charleseteure/">LinkedIn</a></p>
  </div>
  <div class="about"><h2>About </h2>
    <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Deserunt voluptatem reprehenderit non. Magni sit alias quia, vel quidem autem quos optio quam at porro aliquid necessitatibus aut et eos nulla.</p>
  </div>
  <div class="contact"><h2>Contact</h2>
    <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Animi consequuntur magnam ipsum, commodi quam, non dolorem numquam veritatis, qui nam voluptas asperiores neque magni. Placeat, natus reprehenderit. Fugit, voluptatum commodi.</p>
  </div>
  <footer>Built with <3 by <a href="https://twitter.com/charliecodes">Charles Freeborn</a></footer>
</div>
</html>
```

### CSS
 

여기서는 스타일 시트 (CSS)를 HTML 템플릿에 연결하고 CSS 그리드를 사용하여 레이아웃을 구현합니다.
 명확성을 돕기 위해 코드 조각과 함께 몇 가지 주석을 추가했습니다.
 

```css
body{
  background: #F1F0EE;
  margin: 0;
}

.container {
   /* first, create a grid container */

  display: grid;        
  
  /* define the amount and size of each column. Here we define 4 columns with equal fractions  */

  grid-template-columns: repeat(4, 1fr); 

  /* create horizontal tracks as rows and here we create 4 rows with different sizes  */
  grid-template-rows: 0.2fr 1.5fr 1.2fr 0.8fr; 

  
/* define where each element will be in the grid. We achieve this with grid-template-areas */

  grid-template-areas:
    "nav nav nav nav"
    "main main main main"
    "about about about sidebar"
    "contact contact contact sidebar"
    "footer footer footer footer";

/* we can set gaps - gutters between the rows and columns*/

  gap: 0.5rem;
  height: 100vh;
  font-weight: 800;
  font-size: 12px;
  color: #004d40;
  text-align: center;
}
 /* Styling the Nav starts here */

nav{
  background-color: #3770F6;
  grid-area: nav;
}

nav ul{
  list-style: none;
  font-size: 16px;
  font-weight: bolder;
  float: left;
}

li{
  display: inline-block;
}

li a{
  color: #ffffff;
}
  /* Styling the Nav ends here */

a:hover{
  color: #FF7F50;
}

main {
  grid-area: main;
}

main h2 {
  font-weight: bolder;
}

main p{
text-align: left;
}

.sidebar {
  background: #D3D4D7;
  grid-area: sidebar;
}

.about {
  background: #D7D6D3;
  grid-area: about;
}

.contact {
  background: #BDBCBB;
  grid-area: contact;
}


footer {
  background-color: #3770F6;
  grid-area: footer;
  color: #ffffff;
}

footer a {
  color: #ffffff;
}

a {
  text-align: center;
  display: block;
  font-family: inherit;
  text-decoration: none;
  font-weight: bold;
  margin: 1rem;
}

/* For */

@media only screen and (max-width: 550px) {
  .container {
    grid-template-columns: 1fr;
    grid-template-rows: 0.4fr 0.4fr 2.2fr 1.2fr 1.2fr 1.2fr 1fr;
    grid-template-areas:
      "nav"
      "sidebar"
      "main"
      "about"
      "contact"
      "footer";
  }
}
```

## CSS 그리드 : 핵심 요점
 

CSS에서 유사한 목표를 달성하는 방법에는 여러 가지가 있습니다.
 CSS Grid를 사용하는 것은 사용자 친화적 인 인터페이스로 일관되고 원활한 웹 애플리케이션을 디자인하기 위해 요소를 행과 열에 배치하는 한 가지 방법 일뿐입니다.
 

CSS Grid에 대한 자세한 내용은 W3 CSS Grid Layout Module 및 MDN CSS Grid 웹 문서를 권장합니다.
 