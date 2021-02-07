---
layout: post
title: "게임 개발자를 위한 Three.js 소개"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/intro-to-three-js-for-game-developers.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/intro-to-three-js-for-game-developers.png?fit=730%2C487&ssl=1)

게임산업은 음악과 영화산업을 합친 것보다 수입이 더 많다. 게임 생산이 증가하고 있고 엑스박스나 플레이스테이션 같은 콘솔이 미친 듯이 팔리고 있는 것은 당연하다.

게임 개발 분야가 수년간 진화하면서 게임은 브라우저 영역으로 변모하기 시작했다. 오늘날, 우리가 PC와 콘솔에서 하는 많은 게임들은 또한 브라우저에서 실행되도록 만들어진다. 이는 개발자들이 웹을 위한 3D 게임을 훨씬 더 효율적으로 만들 수 있도록 도와주는 수많은 게임 엔진 덕분입니다.

이 튜토리얼에서는 놀라운 3D 애니메이션을 만들기 위해 가장 인기 있는 JavaScript 라이브러리 중 하나인 Three.js를 시작하는 방법에 대해 설명합니다.

## Three.js란 무엇입니까?

Three.js는 3차원 모델과 게임을 만드는 강력한 라이브러리이다. 몇 줄의 JavaScript를 사용하면 간단한 3D 패턴부터 사실적인 실시간 장면에 이르기까지 모든 것을 만들 수 있습니다. 간단하고 복잡한 3D 지오메트리를 제작하고, 실제와 같은 장면을 통해 객체를 애니메이션하고 이동하는 등의 작업을 수행할 수 있습니다.

3.js를 사용하면 개체에 질감과 재료를 적용할 수 있습니다. 또한 다양한 광원을 제공하여 장면, 고급 후 처리 효과, 사용자 지정 셰이더 등을 조명합니다. 게임에서 사용할 3D 모델링 소프트웨어에서 개체를 로드할 수 있습니다.

이러한 이유로, Three.js는 자바스크립트 게임을 빌드하기 위한 나의 이동 라이브러리이다.

## 시작 중

먼저 Three.js 파일을 다운로드합니다.

그런 다음 `3js-prj` 폴더를 만듭니다. 폴더 내에 src와 libs의 두 폴더를 더 만듭니다.

```undefined
threejs-prj
    - /src
    - /libs
```

루트 폴더에 `index.html` 파일을 생성하고 `src` 폴더에 `main.js` 파일을 생성하십시오. 그런 다음 3.min.js를 libs 폴더에 복사합니다.

```undefined
threejs-prj
    - /src
        - main.js
    - /libs
        - three.min.js
    - index.html
```

main.js에는 게임 코드가 포함되어 있습니다. `3.min.js`는 축소된 Three.js 프레임워크이며, `index.html`은 Three.js가 객체를 렌더링하는 메인 페이지입니다.

index.html을 열고 다음 코드에 붙여넣습니다.

```xml
<!-- index.html -->

<!DOCTYPE html>
<html>
    <head>
        <title>Three.js demo</title>
        <meta charset="utf-8">
        <style>
            body {
                background-color: #ffffff;
                margin: 0;
                overflow: hidden;
            }
        </style>
    </head>
    <body>
        <script src="./libs/three.min.js"></script>
        <script src="./src/main.js"></script>
    </body>
</html>
```

이것은 기본적인 코드 설정일 뿐입니다. 여백을 없애고 넘쳐나는 내용을 숨겼습니다.

다음 섹션에서는 Three.js에서 기본 개체 및 장면을 만드는 방법에 대해 설명합니다.

## 기본사항

시작하기 전에 한 걸음 물러서서 3D 게임의 기본 장면이 어떤 모습인지 살펴보도록 하겠습니다. 따라서 장면, 지오메트리, 재료, 카메라 및 렌더러를 포함한 몇 가지 일반적인 용어를 숙지해야 합니다.

### 장면

장면이 좌표계에서 시작됩니다. 그것은 큐브, 피라미드, 자동차, 집 등과 같은 물체들을 포함할 수 있습니다. 기본적으로 여러분이 상상할 수 있는 것은 무엇이든, 마치 영화의 한 장면처럼 말입니다.

먼저 `장면` 변수를 선언합니다.

```xml
<script> 
    var scene
</script>
```

장면 클래스를 사용하여 장면을 만듭니다.

```undefined
scene = new THREE.Scene();
```

씬(scene) 변수에는 씬(scene) 객체가 있습니다. 장면에 객체를 추가하는 데 `추가()` 방법을 사용할 수 있습니다.

### 기하학

기하학은 우리가 만드는 모양을 일컫는다 - 정육면체, 정사각형, 피라미드 등. 3.js는 기본적인 원시 모양을 제공하며, 표면과 꼭짓점을 수정하여 사용자 자신의 복잡한 모양을 만들 수 있습니다.

보를 생성하려면 THREE 변수의 Box Geometry 클래스를 사용합니다.

```undefined
var boxGeometry = new THREE.BoxGeometry(10, 10, 10);
```

이렇게 하면 길이가 10 단위, 너비가 10 단위, 두께가 10 단위인 입방체가 만들어집니다.

```undefined
isoGeometry = new THREE.IcosahedronGeometry(200, 1);
```

이것은 이코사면체 모양을 만듭니다.

### 망사 및 재료

빛과 재료는 색상, 질감 등을 적용하여 물체를 스타일링합니다. 재료는 모양에 질감과 색을 입히는 데 사용됩니다.

색상과 질감을 위한 재료를 만들려면 `TREW`를 사용하십시오.Mesh Basic Material` 클래스입니다. 이것은 우리의 관습적인 색조와 질감을 통과시킬 것이다.

```undefined
var basicMaterial = new THREE.MeshBasicMaterial({
    color: 0x0095DD
});
```

여기서 우리는 `0095` 색상의 재료 객체를 만들었습니다.DD.

```coffeescript
material = new THREE.MeshBasicMaterial({ 
    color: 0x000000, 
    wireframe: true, 
    wireframeLinewidth: 2
});
```

우리는 더 많은 속성을 전달하여 기본 재료를 만들었습니다. 이번에는 와이어프레임의 너비가 두 단위인 와이어프레임 형태가 객체가 되기를 원합니다.

우리는 그냥 기본 재료를 사용했어요. Three.js에는 Phong, Lambert 등과 같이 더 많은 미리 정의된 재료가 사용된다.

그물망은 물체에 재료를 바르는 데 사용된다. `쓰리.Mesh` 클래스는 이 작업을 처리합니다.

basic material을 box Geometry에 적용하려면:

```undefined
var cubeMesh = new THREE.Mesh(boxGeometry, basicMaterial);
```

큐브메쉬는 10x10x10박스로 색감 0095로 입체적으로 칠해진다.DD.

### 카메라

카메라는 장면에서 물체를 보는 눈이다. 씬(scene)에는 하나 이상의 카메라가 하나 이상 있어야 합니다.

Three.js의 카메라는 특정 관점에서 장면에서 볼 수 있는 것을 제어합니다. 카메라를 움직여서 주위를 둘러볼 수 있습니다. 현실 세계와 마찬가지로 다양한 각도에서 환경을 볼 수 있습니다.

Three.js는 많은 종류의 카메라를 가지고 있지만, 기본적인 것은 `Three`이다.투시 카메라"입니다. A TREW.Perspective Camera 인스턴스(instance)는 당신의 눈처럼 공간의 한 지점에서 세계를 표시합니다. `3`도 있다.비행기에서 밖을 내다보는 듯한 Orthographic Camera 클래스.

```coffeescript
camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 1, 1000);

camera.position.z = 500;
```

여기서 논점을 정리해 봅시다.

- 첫 번째 인수는 시야(도)입니다. 그것은 카메라 렌즈의 폭을 조절한다.
- 두 번째는 가로 세로 비율, 즉 캔버스의 너비와 높이에 대한 비율입니다.
- 세 번째 아그는 거의 평면상에 가까운 좌절감이다. 이것은 물체가 카메라에 얼마나 가까이 있고 여전히 보이는지 제어합니다.
- 마지막 arg는 원플레인의 좌절감이다. 이 기능은 카메라에서 객체가 얼마나 멀리 떨어져 있고 여전히 렌더링될 수 있는지 제어합니다.

위의 예에서, 우리는 카메라 공간 좌표계 중 하나를 사용하여 카메라를 z축에서 500단위 앞으로 이동시켰다.

우리는 또한 `카메라.위치`를 사용할 수 있다.y와 camer.position.x`는 카메라를 위/아래로, 왼쪽/오른쪽으로 각각 이동합니다.

### 렌더러

렌더러는 씬(scene)과 카메라를 브라우저에 칠합니다. Three.js는 WebGL 기반 렌더러, 캔버스, SVG, CSS, DOM을 포함한 여러 렌더링 엔진을 제공한다.

WebGL 렌더러 `TREW`를 사용합니다.WebGL렌더`가 사용됩니다.

```js
var renderer = new THREE.WebGLRenderer({
    antialias:true
});
renderer.setSize(window.innerWidth, window.innerHeight);

document.body.appendChild(renderer.domElement);
```

WebGL 렌더러를 만들고 있습니다. WebGL 렌더러 객체에 전달된 객체의 안티알리아스 속성은 `true`로 설정되어 있어 WebGL이 객체를 원활하게 렌더링할 수 있습니다. setSize(설정 크기) 방법은 브라우저의 렌더링 창을 브라우저의 전체 너비와 높이로 설정합니다. 마지막으로, 렌더러의 `domElement` 속성의 DOM이 DOM에 추가됩니다. 이렇게 하면 브라우저에 장면이 표시됩니다.

### 라이트

조명은 장면에서 지정된 영역을 비추는 데 사용됩니다. 특정한 방향으로 횃불을 가리키는 것처럼 생각해보세요.

스리.js에는 포인트, 앰비언트, 방향, 반구, 스팟 등 광원이 많다. 모두 `Light` 객체의 인스턴스입니다.

Isaac Sukin의 Game Development With Three.js에 설명된 대로 각 광원에 대해 더 깊이 들어가 보겠습니다.

#### '주변'

`주변`은 장면의 모든 조명 대상에게 똑같이 영향을 미친다.

```css
THREE.AmbientLight(color) 
```

#### '방향'

이 유형의 경우, 모든 빛은 평행하며 선원이 매우 멀리 떨어져 있는 것처럼 주어진 방향에서 나온다.

```undefined
THREE.DirectionalLight(color, intensity = 1)
```

#### 반구

반구는 태양으로부터 반사되는 빛을 모방한다. 두 개의 반대 방향 표시등이라고 생각해보세요.

```undefined
THREE.HemisphereLight(skyColor, groundColor, intensity = 1)
```

#### 점

이 광원은 전구와 같은 우주의 특정 지점에서 나온다. 반경 내의 물체만 켜집니다.

```undefined
THREE.PointLight(color, intensity = 1, radius = 0)
```

#### '스팟'

이는 공간의 특정 지점에서 특정 방향으로 방출됩니다. 원뿔 모양의 물체가 회전 방향을 향하도록 하여 반지름의 거리 내에서 기하급수적으로 떨어지도록 조명한다.

```js
THREE.SpotLight(color, intensity, radius = 0, coneAngle = Math.PI / 3, falloff = 10)
```

### 애니메이션

애니메이션은 장면 속의 물체들을 생동감 있게 한다. 공간 좌표계의 어느 방향으로든 객체를 이동할 수 있습니다.

지오메트리 및 카메라 클래스에는 개체를 스케일링, 이동 및 회전하는 데 사용할 수 있는 방법과 속성이 있습니다.

개체의 크기를 조정하려면 "스케일" 속성을 사용하십시오.

```undefined
boxGeometry.scale = 2
boxGeometry.scale = 1
```

이렇게 하면 `박스 지오메트리` 객체가 커지고 축소됩니다.

다음으로 position 속성을 사용해서 x, y, z축을 따라 box Geometry 객체를 이동합니다.

```undefined
boxGeometry.position.x = 4
```

그러면 `box Geometry` 객체가 좌우로 이동합니다.

```undefined
boxGeometry.position.y = 2
```

그러면 `box Geometry` 개체가 위아래로 이동합니다.

```undefined
boxGeometry.position.z = 1
```

그러면 `boxGeometry` 객체가 앞뒤로 이동합니다.

개체를 회전하려면 `회전` 속성을 사용하십시오.

```undefined
boxGeometry.rotation.x += 0.01
```

그러면 박스 지오메트리 객체가 x 방향으로 회전합니다.

```undefined
boxGeometry.rotation.y += 0.01
boxGeometry.rotation.z += 0.01
```

그러면 박스 지오메트리 객체가 y와 z 방향으로 회전합니다.

## 모든 것을 하나로 묶는 것

main.js 파일을 열고 다음을 붙여넣습니다.

```js
// ./src/main.js
var scene = new THREE.Scene();

var boxGeometry = new THREE.BoxGeometry(10, 10, 10);

var basicMaterial = new THREE.MeshBasicMaterial({
    color: 0x0095DD
});

var cubeMesh = new THREE.Mesh(boxGeometry, basicMaterial);
cubeMesh.rotation.set(0.4, 0.2, 0);

var camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 1, 1000);
camera.position.z = 50;

scene.add(camera)
scene.add(cubeMesh)

var renderer = new THREE.WebGLRenderer({
    antialias:true
});
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.render(scene, camera);

document.body.appendChild(renderer.domElement);
```

다음 사항을 확인해야 합니다.

```undefined
scene.add(camera)
scene.add(cubeMesh)
```

카메라와 큐브메쉬는 add() 방식으로 장면에 추가된다. 이렇게 하지 않으면 브라우저에서 파일이 실행될 때 큐브가 렌더링되지 않습니다.

```css
cubeMesh.rotation.set(0.4, 0.2, 0);
```

위는 큐브 메쉬 0.4, 0.2, 0을 각각 x, y, z축을 따라 회전시킨다. 이렇게 하면 큐브를 3D 형태로 볼 수 있습니다. 입방체 x뿐만 아니라 y축도 표시됩니다.

브라우저에서 `index.html`을 로드합니다. 브라우저에 밝은 파란색 큐브가 렌더링된 것을 볼 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/threejscube.png?resize=720%2C405&ssl=1)

## 와이어프레임 큐브

와이어프레임 큐브를 만들려면 다음을 추가하십시오.

```undefined
var wireframeBasicMaterial = new THREE.MeshBasicMaterial({
    color: 0x0095DD,
    wireframe: true,
    wireframeLinewidth: 2
});
```

그런 다음 다음을 편집하십시오.

`var cubeMesh = 새로운 TREE.메쉬(상자 지오메트리, 기본 재료);`

…다음과 같다.

```undefined
// var cubeMesh = new THREE.Mesh(boxGeometry, basicMaterial);
var cubeMesh = new THREE.Mesh(boxGeometry, wireframeBasicMaterial);
```

index.html을 다시 로드하면 와이어프레임 큐브가 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/threejscubewireframe.png?resize=720%2C405&ssl=1)

## 결론

이것은 Three.js로 할 수 있는 일의 시작에 불과합니다. 사실, 너무 강력해서 블렌더와 비교해보겠습니다. 3.j는 블렌더가 할 수 있는 거의 모든 것을 할 수 있다.

추가, 수정 또는 제거해야 할 사항이나 이와 관련하여 궁금한 점이 있으시면 언제든지 의견을 주거나 이메일을 보내거나 DM을 보내주십시오.

고마워요!!!

## 참고문헌

- Jos Dirksen의 Three.js Essentials
- 아이작 수킨의 쓰리.js로 게임 개발