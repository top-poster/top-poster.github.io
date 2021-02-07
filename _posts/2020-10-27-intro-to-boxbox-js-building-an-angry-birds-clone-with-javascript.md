---
layout: post
title: "Boxbox.js 소개: JavaScript를 사용하여 '앵그리 버드' 복제 구축"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/intro-to-boxbox-js-building-an-angry-birds-clone-with-javascript-1.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/intro-to-boxbox-js-building-an-angry-birds-clone-with-javascript-1.png?fit=730%2C487&ssl=1)

게임을 시작하는 방법, 개발 프로세스를 간소화하는 데 사용할 수 있는 다양한 게임 엔진 등 기본 사항을 잘 모르면 자바스크립트로 게임을 만든다는 생각은 처음에는 부담스러워 보일 수 있다.

이 가이드에서는 간단한 "앵그리 버드" 클론을 구축하여 Boxbox.js 및 Box2D를 사용하는 방법을 시연합니다. JavaScript Boxbox.js 엔티티 및 블록을 생성하는 방법과 실제 개체를 표시하는 방법에 초점을 맞춥니다.

## Boxbox.js 및 Box2D 시작

코딩에 대해 조사하기 전에, 게임을 개발하는 데 사용할 라이브러리를 빠르게 살펴보도록 하겠습니다.

Boxbox.js는 게임을 만들기 위한 자바스크립트 프레임워크이다. 후드 아래 박스2d 엔진을 사용, 초보자들도 얼핏 보기에는 매우 힘들다. Boxbox.js는 Box2D 라이브러리로 간단하고 재미있는 2D 게임을 만드는 과정을 단순화하도록 도와준다.

먼저 라이브러리를 다운로드하여 HTML 파일에 추가합니다.

`index.html` 파일을 생성하고 다음 코드를 추가하십시오.

```xml
<html>
  <head>
    <script src="https://github.com/hecht-software/box2dweb/blob/master/Box2d.min.js"></script>
    <script src="https://github.com/incompl/boxbox/blob/master/boxbox.min.js"></script>
  </head>
  <body>
    <main>
      <canvas id="angrybird" width=640 height=380></canvas>
    </main>
    // Added our custom Game
    <script src="AngryBird.js"></script>
  </body>
</html>
```

## 캔버스 설정

`AngryBird.js`라는 파일을 만들어 다음 초기화 코드에 붙여 넣습니다.

```js
const canvas = document.getElementById("angrybird");
const ourWorld = boxbox.createWorld(canvas);
```

우리의 세계 개체를 사용하여 "createEntity" 메서드를 호출하여 "우리 세계"에 다양한 유형의 엔티티 또는 개체를 만들 수 있습니다.

## 새와 지상의 실체 생성

아래의 경우, 우리는 `새`라고 불리는 동그라미 실체를 만들고 있습니다. 지금쯤 짐작하셨겠지만, 우리는 우리의 새를 원으로 표현하려고 합니다.

우리는 우리의 새에게 아래와 같은 성질을 부여하고 어떤 키다운 이벤트에서도 새가 점프할 수 있도록 `onKeyDown` 기능을 추가했다.

```css
ourWorld.createEntity({
  name: "bird",
  shape: "circle",
  radius: 1,
  imageStretchToFit: true,
  density: 4,
  x: 2,
  onKeyDown: function (e) {
    this.applyImpulse(200, 60);
  },
});
```

다음으로, 우리는 우리 층을 대표하는 또 다른 실체 `그라운드`를 만들 것이다. 물리세계에서, 만약 어떤 것이 물체를 막고 있지 않거나 정적으로 설정되어 있지 않다면, 중력은 물체를 표면 아래로 이동시킬 것이다.

실체를 잃지 않기 위해 다음과 같은 속성을 가진 `지상` 실체를 만들었다. 아래쪽으로 움직이지 않도록 정적인 상태인지도 확인해야 합니다.

```css
ourWorld.createEntity({
  name: "ground",
  shape: "square",
  type: "static",
  color: "rgb(0,0,0)",
  width: 20,
  height: 0.5,
  y: 12.5,
});
```

## 장애물을 쌓는 것

다음 코드는 블록을 만들어 내는데, 이것은 우리 새가 부딪히는 장애물을 나타낼 것이다. 같은 유형의 블록을 3개 생성하게 되므로 이를 나타낼 `진정` 개체를 만드는 것이 좋습니다.

```js
const block = {
  name: "block",
  shape: "square",
  color: "red",
  width: 0.5,
  height: 4,
  onImpact: function (entity, force) {
    if (entity.name() === "bird") {
      this.color("black");
    }
  },
};
```

위에서 생성된 블록으로 다음과 같은 고유한 속성을 가진 첫 번째 장애물을 만들었습니다.

```css
ourWorld.createEntity(block, {
  x: 15
}
```

위에서 생성된 블록으로 다음과 같은 고유한 속성을 가진 두 번째 장애물을 만들었습니다.

```css
ourWorld.createEntity(block, {
  x: 17
}
```

위에서 생성된 블록으로 다음과 같은 고유한 속성을 가진 세 번째 장애물을 만들었습니다.

```css
ourWorld.createEntity(block, {
  x: 16,
  y: 1,
  width: 4,
  height: .5
}
```

## 게임 종료 중

위의 코드를 합치면 아래 코드가 생성됩니다.

```js
const canvas = document.getElementById("angrybird");
const ourWorld = boxbox.createWorld(canvas);

ourWorld.createEntity({
  name: "bird",
  shape: "circle",
  radius: 1,
  imageStretchToFit: true,
  density: 4,
  x: 2,
  onKeyDown: function (e) {
    this.applyImpulse(200, 60);
  },
});

ourWorld.createEntity({
  name: "ground",
  shape: "square",
  type: "static",
  color: "rgb(0,0,0)",
  width: 20,
  height: 0.5,
  y: 12.5,
});

const block = {
  name: "block",
  shape: "square",
  color: "red",
  width: 0.5,
  height: 4,
  onImpact: function (entity, force) {
    if (entity.name() === "bird") {
      this.color("black");
    }
  },
};

ourWorld.createEntity(block, {
  x: 15,
});

ourWorld.createEntity(block, {
  x: 17,
});

ourWorld.createEntity(block, {
  x: 16,
  y: 1,
});
```

만약 당신이 모든 것을 제대로 했다면, 당신의 "앵그리 버드" 복제 게임은 이렇게 보일 것이다.

![image](https://paper-attachments.dropbox.com/s_3BE39264ED5775C22302AED4BAFA70996F77A68735AE7947CB2EA11B29F2154D_1601225598159_angrybird.gif)

다음 단계에서는 예를 들어 원을 새처럼 보이게 하거나 상상할 수 있는 모든 것을 포함시키는 것을 고려할 수 있습니다.

## 결론.

이 기사에서는 JavaScript를 사용하여 간단한 2D 게임을 만드는 방법과 BoxBox.js를 사용하여 "Angry Birds" 클론을 생성하는 방법을 시연했다.

이 새로운 지식으로 무엇을 쌓을 것인가? 얼마든지 댓글로 공유해주세요.