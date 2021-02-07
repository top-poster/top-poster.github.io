---
layout: post
title: "Three.js에서 조명 및 WebGL 렌더러를 사용하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/threejs-rendering-lighting-1.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/threejs-rendering-lighting-1.png?fit=730%2C487&ssl=1)

Three.js는 브라우저에서 3D 그래픽을 렌더링하는 데 사용되는 JS 그래픽 라이브러리이다. 주요 그래픽 조직은 영화, 애니메이션, 광고 및 게임을 위한 3D 장면을 만들고 렌더링하는 데 Three.js를 사용합니다.

Three.js는 장면 렌더링에 브라우저의 WebGL 엔진을 사용한다. API는 데스크톱 그래픽 API인 OpenGL(Gl은 그래픽 라이브러리를 의미한다)을 기반으로 한다. 이 게시물에서는 Three.js의 조명과 WebGL 렌더러에 대해 알아보겠습니다.

## 라이트

먼저, 사용할 수 있는 조명의 유형과 각 조명의 효과를 알 수 있도록 Three.js에서 조명을 사용하는 방법에 대해 알아보겠습니다.

Three.js는 몇 가지 다른 종류의 빛을 제공한다. Three.js의 모든 표시등은 `Three`의 인스턴스이다."빛" "그들은 우리가 장면에서 물체를 밝힐 수 있도록 조명을 제공합니다.

Three.js의 객체에 있는 모든 재료 질감이 조명의 영향을 받는 것은 아니다. 메쉬 램버트 재료와 메쉬퐁 재료의 재료 질감을 가진 물체만 조명에 의해 영향을 받을 수 있다.

다음은 Three.js에서 사용할 수 있는 조명의 유형입니다.

### 주변

주변 조명은 장면에서 물체를 비추는 데 사용됩니다. 그것은 빛을 방향을 가리키거나 지시하지 않기 때문에 그림자를 드리울 수 없다.

```css
THREE.AmbientLight(color) 
```

이 물체들은 주변 빛에 있는 색깔로 빛난다. 주변 빛은 장면의 모든 조명 물체에 동일하게 영향을 미치며, 기본적으로 물체의 물질 색에 색을 더하는 빛입니다.

```undefined
var cubeGeometry = new THREE.CubeGeometry(20, 20, 20);
var basicMaterial = new THREE.MeshLambertMaterial({
    color: 0x0095DD
});

var cubeMesh = new THREE.Mesh(cubeGeometry, basicMaterial);

var light = new THREE.AmbientLight(0x404040);
scene.add(light);

scene.add(cubeMesh)
```

예를 들어, 입방체 물체는 부드러운 흰색 빛으로 켜집니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/threejs_light_renderer_2-e1604950577765.png?resize=730%2C372&ssl=1)

```undefined
var light = new THREE.AmbientLight(0xf6e86d);
```

이 코드는 큐브 개체를 녹색으로 밝게 표시합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/threejs_light_renderer_1-e1604950529735.png?resize=730%2C372&ssl=1)

### 방향성

이름에서 알 수 있듯이, 방향 빛은 특정 지점에서 나와 대상에게 직접 방출됩니다.

이 빛은 그림자를 드리울 수 있고, 광선은 모두 평행하며 마치 태양처럼 무한히 멀리 떨어져 있는 것처럼 행동한다.

```undefined
THREE.DirectionalLight(color, intensity = 1)
```

이런 종류의 빛에서, 모든 빛은 평행하고 마치 근원이 매우 멀리 떨어져 있는 것처럼 주어진 방향에서 오는 것이다.

방향등의 경우 빛의 방향은 `light.position`에서 `light.target.position`으로 가는 방향이며, 두 방향 모두 변경할 수 있는 벡터이며, 대상은 기본적으로 세계 원점으로 설정됩니다.

이것은 우리가 태양에서 지구로 내려오는 빛의 종류입니다. 우리는 광원이 멀리 있고, 광선이 평행하다는 것을 안다.

또 다른 예는 방의 먼 구석에 위치한 넓은 범위의 가까운 홀에 있는 토치나 램프이다. 이것은 홀의 광선을 지시하고 홀의 물체를 비추고 그림자를 드리웁니다.

빛깔은 빛의 색이고, 강도는 빛이 얼마나 강렬하고 밝을까 하는 점이다. 기본값은 1로, 가장 높은 명암을 나타냅니다.

```undefined
var light = new THREE.DirectionalLight(0x404040, 0.5);
scene.add(light);
```

### 반구

반구형 빛은 장면 위에서 지상으로 바로 온다. 옅은 색은 위에서 땅으로 내려오면서 점차 희미해지고, 그림자를 드리우지 않는다.

```undefined
THREE.HemisphereLight(skyColor, groundColor, intensity = 1)
```

태양으로부터 반사되는 빛을 시뮬레이션한다는 것을 주목하세요. 두 개의 반대 방향 빛처럼 말이죠.

하늘색은 하늘의 시작점부터의 색이다. 그라운드 컬러(ground color)는 그라운드의 빛의 색이다. 강도는 색의 밝기를 조절한다.

```undefined
var light = new THREE.HemisphereLight(0x404040, 0xFFFFFF, 0.5);
scene.add(light);
```

그 빛은 부드러운 흰색의 하늘로부터 시작해서 지상에서 정상적인 빛으로 희미해진다. 큐브의 윗부분이 부드러운 흰색으로 조명되고 옆부분이 정상적인 빛과 부드러운 흰색 빛으로 되어 있는 것을 보십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/threejs_light_renderer_3-e1604950836717.png?resize=730%2C372&ssl=1)

다른 예는 다음과 같습니다.

```undefined
var light = new THREE.HemisphereLight(0xf6e86d, 0x404040, 0.5);
scene.add(light);
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/threejs_light_renderer_4.png?resize=730%2C372&ssl=1)

상단의 밝은 색상은 0xf6e86d의 녹색으로 시작되는데, 이것이 정육면체 상단의 색이 녹색인 이유이다. 그러면 0x4040으로 변한다.

### 점

점광은 단일 위치에서 방출되며 모든 방향으로 빛납니다. 이것에 의해 빛난 물체들은 그것의 광선 안에 있는 물체들이다.

```undefined
THREE.PointLight(color, intensity = 1, distance = 0)
```

방의 전구와 마찬가지로, 빛은 한 지점에서 발산되어 모든 방향으로 퍼지지만 반지름 내의 조명된 물체만 방출됩니다. 점광은 그림자를 던질 수 있다.

생성자의 색 호는 방출될 빛의 색이다. 강도는 빛의 세기로, 기본값은 1이며, 거리는 방출된 빛이 이동할 수 있는 거리 또는 빛의 최대 범위인 0으로 제한은 없다.

### 스폿

이로부터 오는 빛의 유형은 특정 지점에서 특정 방향으로 가는 빛의 원뿔을 형성한다. 원추형의 빛은 소스에서 멀어질수록 크기가 커진다. 빛의 원뿔 안에 있는 물체들은 빛을 받아 물체에 그림자를 드리울 수 있습니다.

```js
THREE.SpotLight(color, intensity, distance = 0, angle = Math.PI/2)
```

색상은 빛의 색이다. 강도는 빛의 힘이다. 거리는 빛이 그 근원으로부터 얼마나 멀리 이동하느냐이다. 각도(angle)는 방향에서 빛의 최대 분산 각도입니다.

## WebGL렌더

Three.js는 이제 브라우저에서 WebGLRender만 사용하여 3D 개체를 렌더링하고 브라우저의 WebGL API를 사용하여 개체와 장면을 렌더링합니다. 이렇게 하려면 WebGL을 지원하는 브라우저가 장면을 표시해야 합니다. 일반적으로 대부분의 최신 데스크톱 브라우저는 WebGL을 잘 지원하지만 일부 모바일 기기는 아직 WebGL을 지원하지 않는다.

WebGL은 클라이언트 그래픽 처리 장치를 사용하여 작업하기 때문에 빠릅니다. 클라이언트의 CPU가 아무것도 하지 않고 렌더링의 일부가 아닙니다. WebGL은 그래픽 렌더링을 위해 구축된 전용 GPU가 작동하는 동안 CPU가 절약되기 때문에 성능이 우수하다.

쓰리.WebGL 렌더러, 먼저 다음 인스턴스를 만듭니다.

```undefined
var renderer = new THREE.WebGLRenderer({
    antialias: true
});
```

오브젝트 매개변수의 `antialias` 옵션은 Three.js에서 매우 강력한 기능을 시작하는 생성자에게 전달되었다. Three.js의 안티앨리어싱은 장면에서 렌더링된 객체의 들쭉날쭉한 가장자리를 매끄럽게 만들어 장면의 객체가 실제처럼 보이게 한다.

다음으로, 렌더링 창의 크기를 `setSize() 방법`을 호출하여 설정할 수 있습니다.

```css
renderer.setSize(window.innerWidth, window.innerHeight);
```

첫 번째 매개 변수는 렌더링 창의 너비입니다. 클라이언트 브라우저의 너비를 취하도록 설정할 것입니다. 두 번째 매개변수는 렌더링 창의 높이이며, 클라이언트 브라우저의 높이를 갖도록 설정했습니다.

다음으로, 우리는 우리의 장면을 렌더링하기 위해 렌더러에 `렌더` 방식을 부른다.

```css
renderer.render(scene, camera);
```

씬(scene)에는 객체와 조명이 포함됩니다. 여기서 첫 번째 파라미터는 렌더링하려는 씬(scene)이고 두 번째 파라미터는 씬(scene)에서 객체를 보는 데 사용할 카메라입니다.

마지막으로, 렌더러가 페이지 문서에 추가됩니다.

```css
document.body.appendChild(renderer.domElement);
```

렌더러의 `dom Element` 소자에는 문서 본문에 추가하는 DOM 요소가 들어 있습니다. 이렇게 하면 브라우저에 장면이 표시됩니다.

## 결론.

우리는 스리.js에서 사용할 수 있는 모든 유형의 조명을 수평 조명에서 점 조명까지 포함시켰고, 어떻게 작동하는지 보여주는 예를 들었다. 우리는 또한 Three.js에서 WebGL 렌더러에 대해 배웠고, 그것이 얼마나 성능적이고 유용한지를 보여주었다.

Three.js는 매우 강력하고 빠르기 때문에 다음 번에 브라우저용 그래픽을 구축하려면 Three.js를 사용하는 것을 고려해야 합니다.