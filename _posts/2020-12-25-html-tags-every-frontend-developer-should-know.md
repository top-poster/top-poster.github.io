---
layout: post
title: "모든 프런트 엔드 개발자가 알아야 할 HTML 태그"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/html-tags-frontend-developer-should-know.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/html-tags-frontend-developer-should-know.png?fit=730%2C487&ssl=1)

JavaScript 프레임워크와 라이브러리에 대한 집중도가 높아짐에 따라 많은 개발자들이 HTML에 대한 우선 순위를 낮게 설정하고 있습니다. 그 결과, 접근성(화면 판독기), 웹 크롤러, 봇, SEO 등의 측면에서 사이트의 기능을 향상시킬 수 있는 HTML 기능을 최대한 활용하지 못하고 있습니다. 또한 의미론적 HTML을 작성하면 사이트 컨텐츠에 적합한 컨텍스트가 추가되어 사용자 환경이 크게 향상됩니다.

이 가이드에서는 간과할 수 있는 가장 유용한 HTML 태그에 대해 설명합니다. 각 태그의 기능과 HTML을 사용하여 개발 프로세스를 간소화하고 앱 또는 웹 사이트의 사용자 환경을 개선하는 방법을 보여 드리겠습니다.

다음 사항에 대해 자세히 설명합니다.

- <베이스>
- 이미지 맵
- <영역>
- abbrand and and
- <사전>과 <코드>
- <그림자>와 <그림자 자르기
- 【➡】 [➡➡]와 【➡】
- ➡ 및 ➡견적 ➡

시작해 봅시다!

## <<밑면>>

`<base> 태그를 사용하면 문서의 모든 상대 URL에 대한 접두사를 작동하는 기본 URL이 있는 시나리오를 만들 수 있습니다. 태그에는 기본 URL을 포함하는 `href` 또는 `target` 특성이 있거나 둘 다 있어야 합니다.

```xml
<!DOCTYPE html>
<html>
<head>
  <base href="https://www.google.com/" target="_blank">
</head>
<body>

<h1>The base element(Google As a case study)</h1>

<p> <a href="gmail">Gmail</a> - Used to send emails; which are messages distributed by electronic means from one computer user to one or more recipients via a network.</p>

<p><a href="hangouts">Hangouts</a> - It's used for Messaging, Voice and Video Calls</p>
</body>
</html>
```

모든 요청에 대해 URL 접두사를 반복할 필요가 없으므로 코드를 추상화하여 반복하지 않도록 할 수 있습니다.

문서에는 하나의 ➡기본 요소만 있을 수 있으며, ➡머리 요소 안에 있어야 합니다.

## 이미지 맵

이미지 맵은 클릭할 수 있는 특정 영역이 있는 이미지이며, `맵` 태그로 정의됩니다. 이러한 영역은 `영역` 태그를 사용하여 설정됩니다. 기본적으로, 이것은 다른 페이지로 이어질 수 있는 이미지의 다른 부분에 링크를 포함시킬 수 있게 해줍니다. 이것은 그림 내에서 사물을 설명하는 데 매우 좋습니다.

예를 들어 보겠습니다.

첫 번째 단계는 평소와 마찬가지로 img 태그를 사용하여 이미지를 삽입하는 것이지만 이번에는 use map 속성을 사용합니다.

```xml
<img src="study.jpg" alt="Workplace" usemap="#workmap">
```

다음으로, `<map> 태그를 별도로 생성하고 태그의 `ussemap` 속성과 값이 동일한 `name` 속성을 사용합니다. 이 태그는 <이미지> 태그와 지도 태그를 연결합니다.

```cpp
  <map name="workmap">

  </map>
```

재미있는 부분은 클릭할 수 있는 영역을 직접 만드는 것입니다. 각 영역을 어떻게 그릴 것인지 정의해야 합니다. 일반적으로 도형과 좌표를 사용하여 도형을 추적합니다.

## <영역>

```js
<map name="workmap">
  <area shape="rect" coords="255,119,634,373" alt="book" href="book.html">
</map>
```

이미지에서 클릭 가능한 영역은 `영역` 요소를 사용하여 정의됩니다. 지도 요소 내부에 추가됩니다.

특성은 다음과 같습니다.

- 쉐이프는 해당 영역에 직사각형 모양을 그릴 때 사용됩니다. 직사각형, 원, 다각형 또는 기본값(전체 이미지)과 같은 다른 모양을 가질 수 있습니다.
- 영역 요소가 어떤 이유로든 렌더링할 수 없는 경우 `alt`는 렌더링할 대체 텍스트를 지정합니다.
- `href`는 클릭 가능한 영역을 다른 페이지로 연결하는 URL을 보유합니다.
- α는 좌표(픽셀)를 사용하여 정확하게 도형을 자릅니다. 다양한 소프트웨어를 사용하여 사진의 정확한 좌표를 얻을 수 있습니다. 간단한 예로 Microsoft Paint를 사용할 수 있습니다. 모양에 따라 좌표가 다르게 작성됩니다. 직사각형의 경우 왼쪽, 위쪽, 오른쪽, 아래쪽입니다.

여기 `상단, 왼쪽` 좌표가 있습니다.

다음 스크린샷은 `오른쪽, 아래` 좌표를 보여줍니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/cursor-bottom-right.png?resize=730%2C411&ssl=1)

마지막으로 다음과 같은 이점을 얻을 수 있습니다.

```xml
<img src="study.jpg" alt="Workplace" usemap="#workmap">

<map name="workmap">
  <area shape="rect" coords="255,119,634,373" alt="book" href="book.html">
</map>
```

다른 도형을 사용할 수 있지만 각 도형마다 좌표가 다르게 작성됩니다.

`원`의 경우 원의 중심 좌표를 얻은 다음 반지름을 추가합니다.

```js
<map name="workmap">
  <area shape="circle" coords="504,192,504" alt="clock" href="clock.html">
</map>
```

`poly`를 생성하는 것은 자유형 도면에 가깝습니다. 이미지의 다른 점만 연결하면 다음과 같이 연결됩니다.

```js
<map name="workmap">
  <area shape="poly" coords="154,506,168,477,252,429,187,388,235,332,321,310,394,322,465,347,504,402,510,469512,532,454,581,423,585,319,593,255,589,240,536" alt="clock" href="clock.html">
</map>
```

HTML로 도형을 만드는 간단한 치트시트는 다음과 같습니다.

## <abbr>와 <dfn>

`dfn` 태그는 상위 요소 내에서 정의될 용어를 지정합니다. 이것은 "정의 요소"를 나타냅니다. `dfn` 태그의 이 상위에는 해당 용어가 `dfn` 안에 있는 동안 용어에 대한 정의/설명이 포함되어 있습니다. 다음을 추가할 수도 있습니다.

```js
<p><dfn title="HyperText Markup Language">HTML</dfn> 
  Is the standard markup language for creating web pages.
</p>
```

abbr은 abbr과 함께 사용할 수도 있습니다.

```xml
<!DOCTYPE html>
<html>
<body>

<p><dfn><abbr title="HyperText Markup Language">HTML</abbr></dfn> 
  It's the standard markup language for creating web pages.</p>
</body>
</html>
```

이렇게 의미론적 HTML을 쓰는 것은 화면 리더와 브라우저가 사용자에게 적합한 컨텍스트에서 페이지에 있는 내용을 해석할 수 있기 때문에 접근성에 매우 좋습니다.

`<abbr>`를 독립적으로 사용할 수 있습니다.

```xml
 <abbr title="Cascading Stylesheet">CSS</abbr>
```

## <사전>과 <코드>

미리 포맷된 텍스트 또는 `사전` 태그는 쓰여진 대로 텍스트(일반적으로 코드)를 표시하는 데 사용됩니다. 모든 공백, 탭 및 블록에서 포맷된 그대로 표시됩니다.

```xml
 <pre>
    <code>
      p {
          color: black;
          font-family: Helvetica, sans-serif;
          font-size: 1rem;
      }
    </code>
  </pre>
```

## <그림>과 <그림자>

이 두 태그는 일반적으로 함께 나타납니다. `그림자` 요소는 `그림자`의 캡션 역할을 한다.

```xml
 <fig>
  <img src="https://images.unsplash.com/photo-1600618538034-fc86e9a679aa?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entropy&cs=srgb&ixid=eyJhcHBfaWQiOjE0NTg5fQ">
  <figcaption>basketball<figcaption/>
<fig>
```

이러한 태그는 아래 나온 것처럼 코드 블록, 비디오 및 오디오 클립에서도 사용할 수 있습니다.

코드 블록:

```xml
 <figure>
  <pre>
    <code>
      p {
          color: black;
          font-family: Helvetica, sans-serif;
          font-size: 1rem;
      }
    </code>
  </pre>
   <figcaption>The code block</figcaption>
</figure>
```

비디오:

```xml
 <figure>
 <video src="ex-b.mov"></video>
 <figcaption>Exhibit B. The <cite>Rough Copy</cite> trailer.</figcaption>
</figure>
```

오디오:

```xml
 <figure>
    <audio controls>
  <source src="audio.ogg" type="audio/ogg">
  <source src="audio.mp3" type="audio/mpeg">
</audio>
  <figcaption>An audio file</figcaption>
</figure>
```

## <<<<>>와 <<<<<<>>>.

【➡】【➡】【➡】키로 전환 가능한 코너를 만든다. `요약` 태그는 `자세히` 태그 안으로 들어가고, 클릭하면 숨겨진 내용이 자동으로 표시됩니다.

가장 좋은 점은 CSS로 요소를 스타일링할 수 있고 JavaScript 없이도 완벽하게 작동합니다.

```xml
 <details>
    <summary>
        <span>I am an introvert</span>
    </summary>

    <div>An introvert is a person with qualities of a personality type known as introversion, which means that they feel more comfortable focusing on their inner thoughts and ideas, rather than what's happening externally. They enjoy spending time with just one or two people, rather than large groups or crowds</div>
    <div>        
</details>
```

## '<block quote>와 'block quote'

<block quote>는 기본적으로 다른 출처에서 인용된 섹션이다. 소스를 나타내기 위해 < 속성을 추가합니다.

```coffeescript
<blockquote cite="https://en.wikipedia.org/wiki/History_of_Nigeria">
The history of Nigeria can be traced to settlers trading across the middle East and Africa as early as 1100 BC. Numerous ancient African civilizations settled in the region that is known today as Nigeria, such as the Kingdom of Nri, the Benin Empire, and the Oyo Empire. Islam reached Nigeria through the Borno Empire between (1068 AD) and Hausa States around (1385 AD) during the 11th century,[1][2][3][4] while Christianity came to Nigeria in the 15th century through Augustinian and Capuchin monks from Portugal. The Songhai Empire also occupied part of the region.[5]
</blockquote>
```

`cite` 특성이 사용된 경우, 소스로 연결되는 유효한 URL이어야 합니다. 해당 인용 링크를 얻으려면 특성의 값을 요소의 노드 문서에 대해 구문 분석해야 합니다. 예를 들어, 서버측 스크립트가 일반적으로 클라이언트측이 아닌 사이트의 인용문 사용에 대한 통계를 수집하는 등의 개인 용도로 사용될 수 있습니다.

## <<<<<>>

cite 요소는 책, 논문, 에세이, 시, 노래 등과 같은 저작물 또는 지적 재산의 제목을 나타냅니다. 이 작업은 상세하게 인용된 작업(예: 인용문) 또는 단순히 지나가는 참조일 수 있습니다.

```xml
<p>The best movie ever made is <cite>The Godfather</cite> by
Francis Ford Coppola . My favorite song is <cite>Monsters You Made</cite> by the Burna boy.</p>
```

## 결론

개발자들은 이러한 덜 인기 있는 태그에 더 많은 주의를 기울이고 사이트의 기능을 향상시키기 위해 더 많은 의미 코드를 작성해야 한다.

이러한 HTML 태그에 대한 자세한 내용은 w3 학교와 HTML 표준 공식 웹사이트에서 확인할 수 있습니다.