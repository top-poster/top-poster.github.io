---
layout: post
title: "CSS를 사용하여 삼각형 만들기"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/triangles.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/triangles.png?fit=750%2C1000&ssl=1)

이 게시물에서는 CSS에서 삼각형을 만드는 방법에 대해 알아보겠습니다. 먼저 CSS에 대해 몇 가지 살펴보겠습니다.

## CSS

CSS는 디자인 도구가 아닙니다. 그러나 문서 또는 문서 모음의 스타일을 매우 간단하게 만드는 선언적 구문을 제공합니다.

CSS는 우리의 페이지를 매우 멋지고 스타일리쉬하게 만들어 줍니다. 문서는 요소를 포함하고 있으며 각 요소는 OS에서 기본 스타일링을 갖습니다. CSS는 우리가 원하는 대로 요소를 스타일링할 수 있는 힘을 줍니다.

```xml
<button>Click</button>
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_default_button.png?resize=730%2C606&ssl=1)

OS 기본 스타일링의 심플한 버튼입니다. CSS를 통해 우리만의 스타일을 추가할 수 있습니다.

```css
button {
    margin: 0;
    padding: 6px 12px;
    background-color: orangered;
    color: white;
    border: 1px solid #b93605;
    border-radius: 2px;
}
```

우리의 스타일링이 버튼 요소의 기본 스타일링을 능가할 것이다. 우리는 우리의 스타일을 설정하기 위해 CSS 속성을 사용합니다.

여백 0은 버튼의 여백을 제거합니다.

패딩은 상하 6픽셀, 좌우 12픽셀 등 버튼의 내용 영역을 패딩한다.

background-color 속성은 버튼의 배경색을 보다 주황색으로 설정합니다.

텍스트 색상은 `color` 속성 규칙에 의해 흰색으로 설정됩니다.

버튼의 테두리는 `border: 1px solid #b93605;`border rule을 설정하여 더 정의됩니다.

버튼은 2픽셀 단위의 테두리 반경을 사용하여 곡선 모양을 제공합니다.

버튼이 보다 세련되고 매력적으로 보일 것입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_styled_button.png?resize=730%2C469&ssl=1)

보시는 것처럼 요소 선택기를 사용하여 버튼을 스타일링했습니다. CSS에서 요소를 선택하고 스타일링하기 위해 사용할 수 있는 다른 선택기가 있습니다.

- `계급` 선택자
- id 선택기
- 의사 선택자

CSS는 인라인 CSS, 내부 `스타일` 태그, 링크 태그를 이용한 HTTP 링크, 스타일시트를 통해 웹 문서에 추가될 수 있다.

### 인라인 CSS

CSS 스타일링은 요소의 `style` 속성에 삽입됩니다.

```xml
<html>
    <body>
        <button style="margin: 0;padding: 6px 12px;background-color: orangered;">Click</button>
    </body>
</html>
```

이것은 인라인 CSS입니다.

### '스타일'

HTML 페이지의 `style` 태그 안에 CSS를 삽입합니다.

```xml
<html>
    <style>
        button {
            margin: 0;
            padding: 6px 12px;
            background-color: orangered;
        }
    </style>

    <body>
        <button>Click</button>
    </body>
</html>
```

### '링크' 태그/스타일시트

CSS 규칙 집합은 확장자가 .css인 스타일시트 파일에 있으며 파일 경로는 `링크` 태그의 `href` 속성에 삽입됩니다.

```css
/* main.css */

button {
    margin: 0;
    padding: 6px 12px;
    background-color: orangered;
}
```

```js
<html>
    <link href="./main.css" rel="stylesheet">
    <body>
        <button>Click</button>
    </body>
</html>
```

CSS는 게임을 만들고, 놀라운 예술작품을 만들고, 미켈란젤로의 인기 있는 예술작품을 재창조하는 데에도 사용되어 왔다.

이러한 작업은 CSS 트릭으로 수행됩니다. 여기서는 CSS를 사용하여 삼각형을 만드는 요령을 알아보겠습니다. 다양한 유형의 삼각형을 만들고 실제 사용 시 적용할 수 있는 위치도 확인할 것입니다.

### 속임수

테두리, 너비 및 높이를 사용하여 삼각형을 만드는 것이 요령입니다. 어떻게 되는지 보여드릴게요.

먼저 간단한 div로 시작하겠습니다.

```xml
<style>
    div {
        background: orangered;
        width: 400px;
        height: 150px;
    }
</style>
<body>
    <div></div>
</body>
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_tri_1.png?resize=730%2C182&ssl=1)

높이와 너비는 각각 150px와 300px이다.

다음과 같이 div에 대한 경계를 설정하는 경우:

```undefined
div {
    ...
    border: 10px solid black;
}
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_tri_2.png?resize=730%2C183&ssl=1)

이렇게 하면 div 10px의 테두리가 검은색으로 설정됩니다.

테두리를 두껍게 만들면:

```css
div {
    border: 88px solid black;
}
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_tri_3.png?resize=730%2C183&ssl=1)

div의 경계 영역과 내용 영역이 대각선을 이루며, 경계가 커지면 더욱 뚜렷해집니다. 이것을 더 눈에 띄게 하기 위해, 테두리가 독특한 색을 가지도록 합시다.

```undefined
div {
    ...
    border-top-color: blue;
    border-right-color: yellow;
    border-bottom-color: green;
    border-left-color: palevioletred;
}
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_tri_4.png?resize=730%2C183&ssl=1)

보셨죠? 국경 지역 가장자리가 만나는 것 같아요. 그들이 만나면 어떻게 되죠?

그들은 삼각형을 이룰 것이다!

이를 충족시키기 위해 폭과 높이를 0px로 설정합니다.

```css
div {
    background: orangered;
    color: white;
    width: 300px;
    height: 150px;

    border: 88px solid black;
    border-top-color: blue;
    border-right-color: yellow;
    border-bottom-color: green;
    border-left-color: palevioletred;
}
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_tri_5.png?resize=730%2C183&ssl=1)

이제 테두리 삼각형이 있습니다. 요소의 각 측면에 대한 테두리는 삼각형을 형성합니다.

위쪽을 가리키는 삼각형인 일반 삼각형을 만들려면 위쪽, 오른쪽 및 왼쪽 테두리를 투명하게 만들어야 합니다. 배경색도 제거해야 합니다.

```css
div {
    color: white;
    width: 0px;
    height: 0px;
    border: 88px solid black;
    border-top-color: transparent;
    border-right-color: transparent;
    border-bottom-color: green;
    border-left-color: transparent;
}
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_tri_up.png?resize=730%2C179&ssl=1)

삼각형이 왼쪽을 가리키도록 할 수 있습니다.

```css
div {
    color: white;
    width: 0px;
    height: 0px;
    border: 88px solid black;
    border-top-color: transparent;
    border-right-color: yellow;
    border-bottom-color: transparent;
    border-left-color: transparent;
}
```

왼쪽 삼각형을 표시하기 위해 노란색으로 설정된 오른쪽 테두리를 제외한 모든 테두리가 투명합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_tri_left.png?resize=730%2C181&ssl=1)

우리는 오른쪽 아래 방향 포인팅 삼각형에 동일하게 적용한다.

```css
div {
    color: white;
    width: 0px;
    height: 0px;
    border: 88px solid black;
    border-top-color: transparent;
    border-right-color: transparent;
    border-bottom-color: transparent;
    border-left-color: palevioletred;
}
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_tri_right.png?resize=730%2C180&ssl=1)

아래를 가리키는 삼각형이 있습니다.

```css
div {
    color: white;
    width: 0px;
    height: 0px;
    border: 88px solid black;
    border-top-color: blue;
    border-right-color: transparent;
    border-bottom-color: transparent;
    border-left-color: transparent;
}
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_tri_down.png?resize=730%2C177&ssl=1)

## 사용량: 채팅 위젯

채팅 위젯을 만들 때 유용합니다.

채팅 위젯은 실시간 통신에 사용되는 웹 사이트의 독립 실행형 소프트웨어입니다. 고객/사용자가 웹 사이트의 관리자와 통신할 수 있는 방법을 제공합니다. 통신은 일반적으로 입력된 텍스트를 통해 이루어집니다.

대화 위젯에서는 두 통신원에게 어떤 메시지가 누구에게 전달되는지, 누가 메시지를 입력했는지, 또한 몇 시에 전송되었는지 알려주는 방법이 있습니다. 이러한 식별은 화살표(왼쪽 또는 오른쪽 화살표)를 사용하여 수행됩니다.

대화 상자에 방향 화살표를 표시하여 대화 상대를 표시합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_triangle_chat.png?resize=500%2C118&ssl=1)

코드 참조:

```xml
<style>
    .chat {
        width: 150px;
        padding: 3px 10px;
        border-radius: 3px;
        position: relative;
        margin-bottom: 2px;
    }

    .chat-left {
        color: white;
        background: orangered;
    }

    .chat-right {
        color: black;
        background: grey;
        text-align: right;
    }

    .chat-left::before, .chat-right::after {
        content: "";
        color: white;
        width: 0px;
        height: 0px;
        border: 4px solid transparent;

        position: absolute;
        top: 5px;
    }

    .chat-left::before {
        border-top-color: transparent;
        border-right-color: orangered;
        border-bottom-color: transparent;
        border-left-color: transparent;
        left: -7px;
    }

    .chat-right::after {
        border-top-color: transparent;
        border-right-color: transparent;
        border-bottom-color: transparent;
        border-left-color: gray;
        right: -7px;
    }

</style>

<div class="chat chat-left">
    Hello, I'm Chidume Nnamdi
</div>
<div class="chat chat-left">
    How are you doing?
</div>
<div class="chat chat-right">
    How are you doing?
</div>
```

우리는 삼각형을 그리기 위해 `::전`과 `::후` 유사 선택기를 사용했다. 보시다시피 폭과 높이는 0px입니다. .chat-left::before의 왼쪽 화살표의 경우 오른쪽 테두리를 제외한 모든 테두리를 투명하게 설정합니다. .chat-right::after`의 오른쪽 화살표의 경우 왼쪽 테두리를 제외한 모든 테두리를 투명하게 설정합니다.

## 사용량: 드롭다운

또한 드롭다운에 화살표가 사용되어 드롭다운이 있는 요소를 나타냅니다. 또는 드롭다운이 떨어진 위치입니다.

HTML 참조:

```xml
<body>
    <span>
    <button class="dropdown">
        <a class="dropdown-toggle" data-toggle="dropdown" aria-expanded="false">
            Dropdown <span class="caret"></span>
        </a>
        <ul class="dropdown-menu">
            <li><a>Action</a></li>
            <li><a>Another action</a></li>
            <li><a>Something else here</a></li>
            <li class="divider"></li>
            <li><a>Separated link</a></li>
        </ul>
    </button>
    </span>
</body>
```

span.caret 요소는 아래쪽을 가리키는 삼각형이 될 것이다.

스타일링하자:

```css
.caret {
    display: inline-block;
    width: 0;
    height: 0;
    margin-left: 2px;
    vertical-align: middle;
    border-top: 4px solid;
    border-right: 4px solid transparent;
    border-left: 4px solid transparent;            
}
```

폭과 높이를 0으로 만들어 테두리만 보여주고 합칠 수 있었습니다. 그런 다음 아래쪽을 가리키는 삼각형을 만들기 위해 아래쪽, 오른쪽, 왼쪽 테두리를 투명하고 위쪽 테두리는 검은색으로 단색입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_tri_button.png?resize=730%2C512&ssl=1)

드롭다운이 나타나는 위치를 나타내는 삼각형이 있습니다.

나머지 CSS 코드는 다음과 같습니다.

```css
button {
    margin: 0;
    padding: 6px 12px;
    border: 0;
}

.dropdown {
    position: relative;
    text-decoration: none;
}

.dropdown-menu {
    position: absolute;
    z-index: 1000;
    background: white;
    color: black;
    list-style: none;
    margin: 0;
    padding: 5px 0;
    border-color: rgba(255, 255, 255, 0.9);
    box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.33);
    top: 100%;
    left: 0;        
    min-width: 100px;
}

.dropdown li a {
    padding: 5px 20px;
    font-size: 20px;
    display: block;
    color: black;        
    text-decoration: none;
    white-space: nowrap;
    text-align: left;
}

.dropdown li a:active {
    background: #ddd;
}

.dropdown li a:hover {
    background: #ddd;
}
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css_tri_dropdown.png?resize=594%2C398&ssl=1)

## 결론

그게 요령이야. 테두리, 너비 및 높이가 조작되어 CSS에 삼각형을 만듭니다!

추가, 수정 또는 제거해야 할 사항이나 이와 관련하여 궁금한 점이 있으시면 언제든지 의견을 주거나 이메일을 보내거나 DM을 보내주십시오.