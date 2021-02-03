---
layout: post
title: "Curtains.js에서 클래스 작업
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/02/Screen-Shot-2021-02-02-at-9.27.01-AM.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/Screen-Shot-2021-02-02-at-9.27.01-AM.png?fit=730%2C488&ssl=1)

curtains.js는 이미지, 비디오 및 캔버스를 3D WebGL 그래픽으로 변환하는 사용하기 쉬운 WebGL 라이브러리입니다.
 WebGL (Web Graphics Library)은 사용자가 상호 작용할 수있는 3D 및 2D 그래픽을 렌더링하는 데 사용되는 JavaScript API입니다.
 curtains.js는 WebGL JavaScript 라이브러리로 구축되었습니다.
 

버전 7.0 릴리스 이전에 커튼을 사용했다면 이전 버전과 새 버전 사이에 많은 변경 사항을 발견했을 것입니다.
 버전 7.0에서 커튼은 더 나은 가독성과 유지 관리를 위해 다시 작성되었으며 새 라이브러리는 작업하기 쉬운 여러 클래스 모듈로 분류되었습니다.
 

이 기사에서는 이러한 클래스 모듈에 대해 자세히 설명하지만 먼저 커튼 기본 사항을 검토하겠습니다.
 

## 기본으로 돌아 가기 : 커튼은 무엇을합니까?
 

이 질문을 더 잘 이해하려면 먼저 커튼이 해결하려는 문제를 살펴볼 필요가 있습니다.
 공식 웹 사이트에 따르면 curtains.js는 이미지, 비디오 및 캔버스가 포함 된 HTML 요소를 3D WebGL로 변환하여 셰이더를 사용하여 이러한 미디어를 더욱 애니메이션화 할 수 있습니다.
 이것은 Curtains.js 3D 그래픽을 웹 페이지의 DOM에 상대적으로 쉽게 배치 할 수 있도록합니다. 이는 다른 3D 라이브러리에서 흔히 볼 수없는 사치입니다.
 

또한 curtains.js는 WebGL API로 빌드되기 때문에 객체 크기 및 위치와 같은 작업을 직접 수행해야하는 스트레스를 덜어줍니다.
 대신 커튼은 이러한 개발을 자체적으로 처리하여 개발을 쉽고 간단하게 만듭니다.
 

마지막으로 커튼에는 명확한 SEO 이점이 있으므로 다른 3D 라이브러리보다 검색 엔진에서 높은 순위를 차지할 가능성이 높은 디자인에 대한 깔끔한 HTML 코드를 작성할 수 있습니다.
 

## 버전 7.0의 커튼 클래스
 

버전 7.0 릴리스 이전에 커튼을 사용했다면 이전 버전과 새 버전 사이에 많은 변경 사항을 발견했을 것입니다.
 버전 7.0에서 커튼은 더 나은 가독성과 유지 관리를 위해 다시 작성되었으며 새 라이브러리는 작업하기 쉬운 여러 클래스 모듈로 분류되었습니다.
 아래에서 이러한 클래스 모듈에 대해 자세히 설명합니다.
 

### 핵심 수업
 

이러한 핵심 클래스는 curtains.js에서 기본 WebGL 장면을 만들 때 필수적입니다.
 커튼에 WebGL 그래픽을 만드는 데 필요한 백본 역할을합니다.
 핵심 수업에는 커튼, 평면 및 질감이 포함됩니다.
 

#### '커튼'클래스
 

Curtains.js 프로젝트를 시작하려면 제공된 평면 장면을 처리 할`Curtains` 객체를 만들어야합니다.
 이 개체는 캔버스를 래핑 할 HTML 요소의 ID를받습니다.
 

다음은 샘플`Curtains` 개체 인스턴스입니다.
 

```coffeescript
// the id of the given html element in this case is canvas
const curtainsInstance = new Curtains({
    container: document.getElementById("canvas")
});
```

설정할 다른 주목할만한 매개 변수는 다음과 같습니다.
 

- 프로덕션 :이 매개 변수는 부울 값을 수신하며 라이브러리를 프로덕션 모드에서 원하는지 여부를 지정하는 데 사용됩니다.
 
- 컨테이너 :이 매개 변수는 캔버스를 래핑 할 HTML 요소를 지정합니다.
 이 매개 변수는 문자열 또는 HTML 요소를받습니다.
 이 속성은 선택 사항이며 지정하지 않으면 Curtains.js는 WebGL 컨텍스트가 설정 될 때까지`setContainer ()`가 호출 될 때까지 기다려야합니다.
 
- watchScroll :이 매개 변수는 curtains.js가 스크롤 이벤트를 수신해야하는지 여부를 결정합니다.
 값으로 부울을받습니다.
 

Curtains 개체 인스턴스는 또한 사용자가 프로젝트를 더 잘 제어 할 수 있도록 다른 매개 변수 외에 다른 방법을 사용합니다.
 몇 가지 방법은 다음과 같습니다.
 

- `clear ()`: WebGL 컨텍스트의 색상과 깊이를 모두 지 웁니다.
 
- `disableDrawing ()`: 이미 그려진 장면이 다시 그려지는 것을 방지합니다.
 그러면 장면이 일시 중지 상태가되고 유니폼이 비활성화됩니다.
 
- `enableDrawing ()`: 이미 일시 중지 된 장면을 다시 활성화하는 데 필요합니다.
 특정 이벤트 후에 장면을 시작할 계획 인 경우 유용합니다.
 
- `dispose ()`:`requestAnimationFrame` 루프를 취소 한 후 플레인과 WebGL 컨텍스트를 삭제합니다.
 

마지막으로 Curtains 객체를 사용하여 curtains.js 라이브러리에서 제공하는`onError ()`및`onContextLost ()`와 같은 함수로 이벤트를 처리 할 수 있습니다.
 

#### 비행기 클래스
 

Curtains.js에서 제공하는`plane` 클래스는 제공된 HTML 요소를 기반으로 WebGL 평면을 생성합니다.
 Plane은 WebGL을 설정하는 데 사용되는 클래스 인 `baseplane`에서 확장됩니다.
 

평면을 생성하려면 아래와 같이 HTML 요소와 이미 생성 된 `커튼`클래스를 전달해야합니다.
 

```js
// the id of the given html element in this case is “canvas”
const curtainInstance = new Curtains({
    container: document.getElementById(“canvas”)
});

const planeElement = document.getElementById(“plane-element”);

const plane = new Plane(curtainInstance, planeElement)
```

`curtains` 클래스와 마찬가지로`plane` 클래스는 다음과 같이 다른 옵션을 지정할 수있는 세 번째 매개 변수를받습니다.
 

```undefined
// the id of the given html element in this case is “canvas”
const curtainInstance = new Curtains({
    container: document.getElementById(“canvas”)
});

const planeElement = document.getElementById(“plane-element”)

const params = {
    vertexShaderID: "plane-element-vs", 
    fragmentShaderID: "plane-element-fs", /
    uniforms: {
         time: {
             name: "uTime", 
             type: "1f", 
             value: 0, 
         },
    },
};

const plane = new Plane(curtainInstance, planeElement , params)
```

`vertixShaderID` 및`fragmentShaderID`는 셰이더의 정점과 조각을 설정하는 데 사용되는 반면`uniforms`는 생성 된 평면과 상호 작용할 수 있도록합니다.
 

#### 텍스처 클래스
 

이름에서 알 수 있듯이 `texture`클래스는 텍스처를 만드는 데 사용됩니다.
 

다음과 같이 생성자를 호출하여 텍스처를 쉽게 만들 수 있습니다.
 

```js
// the id of the given html element in this case is “canvas”
const curtainsInstance = new Curtains({
    container: document.getElementById("canvas")
});

const texture = new Texture( curtainInstance , {
    sampler: ‘texture’
})
```

또는 비행기에서 createTexture () 메서드를 호출 할 수 있습니다.
 

```js
// the id of the given html element in this case is “canvas”
const curtainInstance = new Curtains({
    container: document.getElementById(“canvas”)
});

const planeElement = document.getElementById(“plane-element”)

const plane = new Plane(curtainInstance, planeElement)

const myTexture = plane.createTexture({
    sampler: “uTexture”
})
```

핵심 클래스를 다루었으므로 이제 웹 페이지를 디자인 할 때 사용할 수있는 커튼 v7.0의 몇 가지 클래스에 대해 논의 할 것입니다.
 

### 프레임 버퍼 개체
 

평면에 애프터 이펙트를 추가하는 데 사용되는 클래스입니다.
 여기에는`renderTarget` 및`ShaderPass` 클래스가 포함됩니다.
 

아래 코드에서`ShaderPass` 인스턴스를 만드는 방법을 볼 수 있습니다.
 여기에서 프레임 버퍼 개체에 대해 자세히 알아볼 수 있습니다.
 

```js
const curtainsInstance = new Curtains({
    container: document.getElementById("canvas")
});

const shaderPass = new ShaderPass(curtains)
```

### 로더
 

로더는 HTML 미디어 요소를 기반으로 WebGL 텍스처를 구현하는 데 사용되는 클래스입니다.
 이에 대한 예는`TextureLoader`입니다.
 이 클래스는 이미지, 비디오 및 캔버스를 소스로 사용하여 텍스처를 만듭니다.
 `TextureLoader`를 사용하여 웹 사이트의 텍스처를 GPU에 미리로드하면서 여전히 로더를 표시 할 수 있습니다.
 

아래 예제와 같이 생성자를 사용하여 새 텍스처 로더를 만들 수 있습니다.
 

```php
// the id of the given html element in this case is “canvas”
const curtainsInstance = new Curtains({
    container: document.getElementById("canvas")
});

// create a new texture loader
const loader = new TextureLoader(curtainsInstance);

// load an image with the loader
const image = new Image();
image.crossOrigin = "anonymous";
image.src = "path/to/my-image.jpg";

loader.loadImage(image, {
    // options are set here
    sampler: "uTexture"
}, (texture) => {
    // successfully created a texture
}, (image, error) => {
    // error handler
});
```

각 미디어의 텍스처는 개별 방법을 사용하여 만들어집니다.
 `TextureLoader` 클래스는`loadCanvas` 및`loadVideo`를 비롯한 다양한 메서드를 사용합니다.
 

`loadCanvas` 함수는 전달 된 캔버스를 기반으로 텍스처를 생성합니다.
 주어진 캔버스 요소, 텍스처 옵션 및 콜백 함수의 세 가지 매개 변수를받습니다.
 여기에서 예를 찾을 수 있습니다.
 제공된`loadCanvases ()`메서드를 사용하여 캔버스 배열로 작업 할 수도 있습니다.
 

`loadCanvas`와 마찬가지로`loadVideo` 함수는 전달 된 비디오를 기반으로 텍스처를 생성하지만 오류 처리를 위해 네 번째 매개 변수도 사용합니다.
 `loadVideos ()`메서드를 사용하여 두 개 이상의 동영상으로 작업 할 수 있습니다.
 

`loadImage ()`및`loadSource ()`와 같은 다른 메서드는 위에 나열된 두 메서드와 유사하게 작동합니다.
 

### 수학
 

커튼의 수학 수업은 three.js 수학 유틸리티 수업을 기반으로합니다.
 차원 벡터 조작에 사용됩니다.
 예를 들어, `Vec2`, `Vec3`, `Mat4`는 각각 2D, 3D, 4D 벡터 계산 및 조작에 사용됩니다.
 

다음은 생성자를 사용한 예입니다.
 

```cpp
const nullVector = new Vec2();
const vector = new Vec2(1, 1)
```

`Vec2` 및`Vec3` 인스턴스는 이벤트 변경시 작업을 수행 할 수있는 setter 및 getter를 사용합니다.
 

이것은`onChange ()`메소드를 사용하여 수행 할 수 있습니다.
 

```cpp
// create a new Vec2 vector add an event listener
const vector = new Vec2().onChange(() => {
const normalizedVector = vector.normalize();
});

// the actions below triggers to different events
vector.x = 4;
vector.y = 6;
```

## 결론
 

curtains.js는 이미지, 비디오 및 캔버스를 포함한 미디어에 2D 및 3D 효과를 추가하는 데 필요한 라이브러리로 명성을 얻었습니다.
 그리고 버전 7.0 이상의 최근 개발 (보다 간소화 된 빌드를위한 클래스 모듈 포함)으로 인해 개발이 훨씬 쉬워졌습니다.
 