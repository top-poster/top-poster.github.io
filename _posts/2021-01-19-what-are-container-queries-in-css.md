---
layout: post
title: "CSS의 컨테이너 쿼리는 무엇입니까?
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/css-feathers.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/css-feathers.png?fit=730%2C487&ssl=1)

컨테이너 쿼리는 CSS에서 가장 많이 언급되고 요청 된 기능 중 하나입니다.
 개발자 커뮤니티에서 진부한 표현이되었습니다.
 그러나 실제로 컨테이너 쿼리는 무엇입니까?
 

컨테이너 쿼리는 너비 및 높이와 같은 속성을 기반으로 컨테이너 콘텐츠의 스타일을 지정하는 데 도움이되는 쿼리입니다.
 이는 뷰포트의 변경 사항을 기반으로 웹 페이지 / 웹 사이트의 스타일을 지정하는 데 도움이되는 미디어 쿼리와 다른 접근 방식을 취합니다.
 

컨테이너 쿼리는 현재 2020 년 현재 CSS에 포함되어 있지 않지만 그와 함께 제공되는 몇 가지 이점을 얻으려면 공식적인 추가가 실제로 필요합니까?
 

음, 정확히는 아닙니다.
 코드에서 컨테이너 쿼리와 같은 동작을 달성하기 위해 변경할 수있는 조정이 있습니다.
 아래에서 몇 가지를 살펴 보겠습니다.
 

## Flexbox를 사용한 컨테이너 쿼리
 

Flexbox는 훨씬 더 반응이 빠른 웹 사이트를 만들 수있는 단방향 레이아웃 모델입니다.
 

이 기사에서는 컨테이너 쿼리를 모방하기 위해 Heldon Pickering이 만든 flexbox 기반의 기술을 사용할 것입니다.
 우리의 목표는 flexbox로 세 개의 열을 만들고 특정 컨테이너 너비의 행으로 모두 이동하는 것입니다.
 

원하는 디렉토리에‘container_query’라는 폴더를 만든 다음 코드 편집기에서 계속 엽니 다.
 다음으로 `container_query`폴더에 두 개의 파일을 만들고 각각 `index.html`및 `style.css`라는 이름을 지정합니다.
 

계속해서 `index.html`파일에 아래 코드를 붙여 넣으십시오.
 

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>container queries</title>
</head> 
<body>
    <div class="container">
        <div class="child child1">
           Lorem ipsum dolor sit amet consectetur adipisicing elit. Ex, enim!
        </div>
        <div class="child child2">
            Lorem ipsum Lorem ipsum dolor sit amet consectetur, adipisicing elit. Laudantium aliquid esse fugiat!
        </div>
        <div class="child child3">
             consectetur adipisicing elit. Tempora totam, eos rerum ipsum consequuntur suscipit?
        </div>
    </div>
</body>
</html>
```

위의 HTML 코드에서 다른 두 개의 `div`를 콘텐츠로 포함하는 `div`컨테이너를 만들었습니다.
 

다음으로 기본 CSS 스타일을 추가하여 모양을 지정하겠습니다.
 

`style.css`파일에 아래 코드를 추가합니다.
 

```css
.child{
    background: rgb(231, 227, 227);
    padding:1em;
    font-size: 18px;
}
```

이제 Heldon의 방법을 적용 해 보겠습니다.
 모든 CSS 코드를 추가 한 다음 차례로 설명하겠습니다.
 CSS 파일을 아래와 같이 업데이트하겠습니다.
 

```undefined
.container{
    display:flex;
    flex-wrap:wrap;
    gap:1em;

}
.child{
    flex-basis: calc( calc(500px - 100%) * 999);;
    flex-grow: 1;
    background: rgb(231, 227, 227);
    padding:1em;
    font-size: 18px;
}
```

클래스가‘.container’인 컨테이너에‘flex’와‘wrap’의‘flex-wrap’을 표시했습니다.
 이것은 우리의 자식 요소가 필요할 때 구부러지고 깨지도록 도와주기 때문에 필요합니다.
 

기본 컨테이너의 모든 하위에는 1의 `flex-grow`가 지정됩니다. 이렇게하면 요소가 원래 너비 이상으로 커지고 나머지 공간을 채울 수 있습니다.
 

`flex-basis`속성은 모든 하위 요소의 이상적인 너비를 설정하는 데 도움이됩니다.
 flexbox는 이것을 엄격히 따르지 않으며 필요한 경우 조정합니다.
 

큰 음수 `플렉스 기준`은 하위 요소가 결국 초기 열 위치에서 행 위치를 차지하게됩니다.
 

위의‘flex-basis’속성에서 수행 한 것은‘calc ()’를 사용하여‘flex-basis’속성을 설정하여 500px 아래 어딘가에 음수 값을 제공한다는 것입니다.
 이렇게하면 모든 자식 요소가 열 위치에서 행 위치를 차지하게됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/flex-basis-property-in-action.gif?resize=730%2C530&ssl=1)

위와 동일한 접근 방식을 취하고 첫 번째 하위 요소 내에 두 개의 `p`태그를 생성 할 수 있습니다.이 태그는 상위 너비가 감소함에 따라 열에서 행으로 이동합니다.
 

아래 코드를 사용하여‘index.html’파일에서 첫 번째 하위 요소의 콘텐츠를 업데이트 해 보겠습니다.
 

```undefined
<p>
    Lorem ipsum, dolor sit amet consectetur adipisicing elit. Ducimus, perferendis!
</p>
<p>
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Nostrum, soluta.
</p>
```

`style.css`파일에 아래 추가 코드를 추가합니다.
 

```css
.child1{
    display:flex;
    flex-wrap:wrap;
}
```

```css
.child1 p{
    flex-basis: calc( calc(400px - 100%) * 999);
    flex-grow: 1;
}
```

첫 번째 예에서와 같이 `flex-basis`속성을 사용하여 이상적인 너비를 설정 한 다음 값이 음수가 될 때 `p`태그가있는 요소가 행 위치를 차지하도록 허용합니다.
 

다음은 결과 코드입니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/flex-basis-property-with-p-tags.gif?resize=730%2C530&ssl=1)

### CSS 그리드를 사용한 컨테이너 쿼리
 

flexbox와 마찬가지로 CSS 그리드와 그 속성을 사용하여 조건부 컨테이너 스타일을 얻을 수 있습니다.
 

위의 예에서 사용한 것과 동일한 HTML 코드를 사용할 것이므로 계속해서 새 색인 파일 위의 예에서 HTML 코드를 복사합니다.
 

이제 아래 코드를 새 CSS 파일에 복사 해 보겠습니다.
 이에 따라 모든 추가 사항에 대해 설명하겠습니다.
 

```css
.container{
    display:grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    grid-gap:1em;

}
.child{

    background: rgb(231, 227, 227);
    padding:1em;
    font-size: 18px;
}
```

먼저 디스플레이를 그리드로 설정 한 다음 자식 컨테이너에 1em의 `그리드 간격`을 부여합니다.
 `grid-template-columns`속성은이 경우 모든 마법을 수행합니다.
 우리는‘repeat ()’를 사용하여 최소 너비가‘300px’이고 최대 너비가‘1fr’이고 제한이‘자동 맞춤’인 격자를 만듭니다.
 이렇게하면 그리드가 전체 뷰포트를 채울 수 있습니다.
 

결과는 상위 컨테이너가‘300px’미만일 때 행으로 전환되는 3 열 레이아웃입니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/three-column-layout.gif?resize=730%2C530&ssl=1)

### 감시 상자
 

Watched box는 컨테이너가없는 쿼리 문제를 해결하기위한 유일한 목적으로 만들어졌습니다.
 이를 사용하기 위해 설치 후 다음과 같이 코드로 가져올 수 있습니다.
 

```coffeescript
import WatchedBox from './path/to/watched-box.min.js';
```

그런 다음 HTML 코드를 감싸서 사용할 수 있습니다.
 

```xml
<watched-box widthBreaks="70ch, 900px" heightBreaks="50vh, 60em">
<!-- HTML and text stuff here -->
  </watched-box>
```

너비와 높이 중단 점을 각각 `widthBreaks`와 `heightBreaks`로 설정했습니다.
 

마지막으로 CSS 파일에 아래 코드를 추가 할 수 있습니다.
 

```css
watched-box {
    display: block;
  }
```

여기에서 공식 Github 저장소를 방문하여 Watched Box에 대해 자세히 알아보세요.
 

## 결론
 

컨테이너 쿼리가 아직 CSS에 공식적으로 추가 된 것은 아니지만 위의 방법 중 하나를 사용하지 않는 한계를 항상 극복 할 수 있습니다.
 부트 스트랩은 위의 일부와 유사한 솔루션을 제공하지만 제한적으로 여기에서 부트 스트랩에 대해 자세히 읽을 수 있습니다.
 