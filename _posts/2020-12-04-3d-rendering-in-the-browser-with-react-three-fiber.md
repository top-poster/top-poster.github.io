---
layout: post
title: "반응 섬유 3D를 사용한 브라우저의 3D 렌더링"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/reactthreefiber.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/reactthreefiber.png?fit=730%2C487&ssl=1)

## 반응 섬유는 무엇입니까?

공식 문서 페이지에 따라:

> 리액트-세-파이버는 웹 및 리액트-네이티브의 3.js에 대한 리액트렌더입니다.

그렇다면 임대업자는 정확히 무엇일까? 렌더러는 대응 트리가 기본 플랫폼 호출로 전환되는 방식을 관리합니다. 즉, 재사용 가능한 반응 구성요소(React 패러다임에 머무름)의 형태로 3D 아티팩트를 구축할 수 있으며 반응-세파이버는 해당 구성요소를 해당 3.j 원시 요소로 변환하는 작업을 수행할 수 있다.

## 3.js에 대한 간단한 소개

이제 리액션 3-파이버가 3.js 위에 있는 추상화라는 것을 알았으니, 클라이언트 측 3D 렌더링과 관련하여 전환 솔루션을 만드는 3.js가 무엇인지, 브라우저에서 무엇을 하는지 알아보는 데 시간을 할애해 보겠습니다.

Three.js는 WebGL(Web Graphics Library)의 맨 위에 있는 래퍼로, 처음부터 직접 쓰기가 상당히 어려울 수 있는 코드의 대부분을 난독화한다. 3.js 설명서에서 직접 인용하려면:

> WebGL은 점, 선 및 삼각형만 그리는 매우 낮은 수준의 시스템입니다. WebGL로 유용한 일을 하기 위해서는 일반적으로 많은 코드가 필요하며 여기서 3.js가 나온다. 그것은 장면, 조명, 그림자, 재료, 텍스처, 3D 수학 등과 같은 것들을 처리합니다.

이러한 이해를 바탕으로 이제 리액트-세-파이버의 핵심 개념에 대해 알아보겠습니다.

## 핵심 개념

### 장면

장면은 우리가 서로 다른 사물의 도움을 받아 설정해 놓은 환경이다. 이것은 영화의 한 장면의 개념과 비슷하며 아래 이미지와 같이 3차원에 걸쳐 있습니다. 그곳은 모든 행동이 일어날 장소입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/scene.png?resize=1913%2C1455&ssl=1)

### 카메라

이제 3차원 장면도 있고 2차원인 브라우저 창에 표시할 필요가 있기 때문에 장면의 어떤 보기가 표시될지 결정해야 합니다. 여기서 카메라가 작동하게 됩니다. 다양한 렌즈로 다양한 종류의 카메라 중에서 선택할 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/camera.png?resize=730%2C574&ssl=1)

### 빛

씬(scene)과 카메라가 설치되어 있으면 씬(scene)이 기본적으로 켜지지 않으므로 수동으로 광원을 배치해야 합니다. 주변(모든 방향) 또는 지점(특정 방향에서 오는 방향) 또는 스폿(실제 스포트라이트와 유사한)과 같은 다양한 유형의 광원 중에서 선택할 수 있다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/light.png?resize=730%2C286&ssl=1)

### 망사

그물체는 우리가 우리의 장면에 넣을 물체입니다. 그것은 우리 영화의 배우들과 유사점들을 구성한다. 메쉬를 좌표계의 특정 지점에 배치할 수 있으며 상호작용을 위해 이벤트 핸들러를 부착할 수 있다. 망사 자체는 일종의 자리 표시자일 뿐입니다. 우리가 기하학적인 것과 물질을 첨가해서 그것이 보이게 하고 특정한 성질을 얻을 수 있도록 하는 것이죠.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/mesh-1.png?resize=730%2C637&ssl=1)

### 기하학

기하학은 망사에 다른 모양을 지정하는 방법입니다. 기하학은 우리가 구체, 정육면체, 피라미드 등을 만들 수 있게 해준다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/geometry.png?resize=730%2C407&ssl=1)

### 재료

재료는 망사에 재료 특성을 제공하는 구성요소입니다. 표면 레벨 효과(예: 광택, 광택, 색상 등)는 재료를 통해 할당됩니다. 아래는 거칠고 시멘트 같은 구조와 금속 구조를 보여주는 예이다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/material2.png?resize=1114%2C580&ssl=1)

## 시작 중

이러한 이해를 바탕으로 리액트-세파이버 공식 GitHub 페이지의 기본 예를 살펴보고 실제 리액트 코드와 관련하여 다양한 구성 요소와 해당 사용법을 이해하도록 해보자. 코드 샌드박스를 직접 사용하고 싶으시면 여기에 링크해 주세요.

반응 및 반응 섬유 실행에 필요한 최소값을 가져오면 다음과 같은 이점이 있습니다.

```coffeescript
import React, { useRef, useState } from 'react';
import { Canvas } from 'react-three-fiber';
```

우리는 useRef와 use State Hooks를 사용할 것이다. 또한 가져온 Canvas 요소는 마지막 섹션에서 발견한 장면과 동일합니다.

앞으로, 우리는 기능적인 구성 요소를 생성하고 장면(즉, 이 반응 구성 요소가 연결되는 DOM 노드에 부착될 캔버스)을 반환합니다.

```js
export default function App() {
  return (
    <Canvas>
    </Canvas>
  )
}
```

다음 작업은 조명을 설치하는 것입니다. 하나의 주변 조명을 설치하여 전체 장면을 조명합니다. 일부 그림자 효과를 얻기 위한 스포트라이트와 보조 빛으로 포인트 라이트를 제공합니다. 카메라가 명시적으로 언급되지 않았기 때문에 기본 위치를 유지합니다. 또한.js 아티팩트는 모두 리액트 쓰리-파이버에서 네이티브 HTML 요소로 사용할 수 있으므로 별도로 가져올 필요가 없습니다.

```xml
export default function App() {
  return (
    <Canvas>
      <ambientLight intensity={0.5} />
      <spotLight position={[10, 10, 10]} angle={0.15} penumbra={1} />
      <pointLight position={[-10, -10, -10]} />
    </Canvas>
  )
}
```

spotLight와 pointLight가 조명의 위치를 제어하는 위치 속성을 어떻게 취하는지 관찰한다. 전달할 세 가지 특성은 각각 x, y 및 z 위치 좌표입니다.

다음으로, 우리의 주요 물체(분홍색 큐브)를 만들어 장면에 배치해 봅시다. 큐브는 박스 형태이므로 박스 버퍼 지오메트리를 사용할 것입니다. 그러나 먼저 다음 동작이 예상되는 재사용 가능한 상자 구성 요소를 생성할 수 있습니다.

```php
function Box(props) {
  const mesh = useRef()
  const [state, setState] = useState({isHovered: false, isActive: false})

  return (
    <mesh
      {...props}
      ref={mesh}
      scale={state.isHovered ? [1.5, 1.5, 1.5] : [1, 1, 1]}
      onClick={(e) => setState({...state, isActive: !state.isActive, })}
      onPointerOver={(e) => setState({...state, isHovered: true})}
      onPointerOut={(e) => setState({...state, isHovered: false})}>
      <boxBufferGeometry args={[1, 1, 1]} />
      <meshStandardMaterial color={state.isActive ? '#820263' : '#D90368'} />
    </mesh>
  )
}
```

우리는 상태 후크를 사용하여 `상자`가 유지/클릭되는지 여부를 추적하고 있다. 또한, 홀딩(스케일 속성 사용) 시 원래 크기의 1.5배까지 확장하고, 메쉬의 색상 속성을 사용하여 클릭하면 색상이 자주색으로 변경됩니다.

이제 이 상자의 두 가지 예를 원본 장면에 넣습니다.

```xml
export default function App() {
  return (
    <Canvas>
      <ambientLight intensity={0.5} />
      <spotLight position={[10, 10, 10]} angle={0.15} penumbra={1} />
      <pointLight position={[-10, -10, -10]} />
      <Box position={[-1, 0, 0]} />
      <Box position={[1, 0, 0]} />
    </Canvas>
  )
}
```

상자 중 하나는 x축의 `1` 위치에, 다른 하나는 음의 x축의 `-1` 위치에 배치된다. 다음과 같은 모습입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/cubes.png?resize=512%2C317&ssl=1)

좋아요, 하지만 우리는 큐브들을 계속해서 회전시키고 또한 일정한 주파수로 위아래로 움직이게 함으로써 조금 더 흥미롭게 만들 수 있습니다. 그러기 위해서는 리액션 3섬유에서 use 프레임을 가져와야 한다. 이것은 우리가 원하는 계산을 모든 프레임 연산에서 적용할 수 있게 해주는 중요한 고리입니다. 바로 이 경우에 우리가 원하는 것입니다. 따라서 앞에서 생성한 Box 구성 요소에 다음과 같은 코드 라인을 추가합니다.

```js
useFrame(state => {
  const time = state.clock.getElapsedTime();
  mesh.current.position.y = mesh.current.position.y + Math.sin(time*2)/100;
  mesh.current.rotation.y = mesh.current.rotation.x += 0.01;
})
```

후크는 내부에서 변경되는 것이 프레임률(초당 60회)로 지속적으로 업데이트되도록 합니다. 이 경우 x-회전 및 y-회전 값을 지속적으로 업데이트하여 방향 변화 효과와 함께 이 회전을 제공한다. 또한 박스는 수직축을 따라 튕기는 등 조화로운 방식으로 움직입니다. 훨씬 낫군!

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/b2.gif?resize=606%2C358&ssl=1)

그것은 3섬유 기능이 어떻게 반응하는지 이해하는 데 필요한 모든 개념을 이해하는 데 도움이 되는 기본적인 예였다. 이제 좀 더 복잡하고 흥미로운 것에 대해 알아보겠습니다.

## 빌딩 맨 위

반응 섬유 3개 구성요소에 대한 사전 지식 및 구성요소의 기본 사용법을 통해 이제 보다 발전된 예를 만들 수 있습니다. 우리는 인기 있는 안드로이드 게임 스택을 위한 조잡한 버전의 UI를 구축하려고 노력할 것입니다. 다음은 렌더링된 UI의 스크린샷입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/stack2.png?resize=1048%2C866&ssl=1)

이는 애플리케이션의 UI 클론일 뿐입니다. 즉, 타일이 스택에 추가될 때 점수가 증가합니다(타일 균형 검사 없음). 코드로 이 작업이 어떻게 이루어지고 있는지 확인해 보겠습니다. 다음은 코드 샌드박스에 대한 링크입니다.

다음은 `App.js` 파일의 기본 JSX 코드입니다.

```xml
<Canvas
  onClick={() => generateNewBlock()}
  camera={ position: [0, 7, 7], near: 5, far: 15 }
  onCreated={({ gl }) => gl.setClearColor('lightpink')}>
  <ambientLight intensity={0.5} />
  <pointLight position={[150, 150, 150]} intensity={0.55} />
  <spotLight position={[10, 10, 10]} angle={0.15} penumbra={1} />
  {boxes.map((props) => (
    <Box {...props} />
  ))}
  <Box position={[0, -3.5, 0]} scale={[3, 0.5, 3]} color="hotpink" />
  <Html scaleFactor={15} class="main">
    <div class="content">{`Score: ${boxes.length}`}</div>
  </Html>
</Canvas>
```

앞의 예와 가장 큰 차이점은 렌더링되는 상자가 현재 `박스` 배열에서 온다는 것이다. 캔버스를 클릭할 때 새로 추가할 상자에 해당하는 소품이 박스 배열로 밀어넣는다. 또한, 우리는 캔버스 안에서 점수를 렌더링하는 데 사용되는 HTML 섹션을 볼 수 있습니다.

새 블록을 추가하고 모든 이전 블록을 중지하는 코드 로직이 이 기능에 포함되어 있습니다.

```js
function generateNewBlock() {
  const total = boxes.length
  const color = colors[getRandomInt(6)]
  let newBoxes = boxes.map((props) => ({ ...props, move: false }))
  newBoxes.push({ position: [getRandomInt(3), total * 0.5 - 3, 0], color: color, move: true })
  setBoxes([...newBoxes])
}
```

최상위 블록이 다음 블록으로 이동하도록 `use Frame`을 구현하는 방식에도 변화가 있다.

```coffeescript
let direction = 0.01
useFrame(() => {
  if (mesh.current.position.x > 1) {
    direction = -0.01
  } else if (mesh.current.position.x < -1) {
    direction = 0.01
  }
  if (props.move) {
    mesh.current.rotation.y = mesh.current.rotation.y += 0.01
    mesh.current.position.x = mesh.current.position.x + direction
  }
})
```

이 모든 코드들은 게임과 비슷한 큐보이드 블록으로 이 스택을 최고의 효과로 만들기 위해 필요한 모든 코드들이죠.

## 반응-세-섬유 물리

방금 본 두 가지 예에서 우리는 `프레임 사용` 후크를 통해 모든 동작을 직접 시뮬레이션하고 있다. 그러나, 예를 들어, 게임에서와 같이, 시간과 관련하여 자동으로 변화하기 위해 구성요소의 특정 특성이 필요할 때(중력 하에서 아래로 가속하는 것과 같이) 사용 사례가 있습니다. 여기서 물리 시뮬레이션이 나타납니다. 캔버스에 물리학을 포함시킴으로써, 우리는 캔버스 안에 존재하는 모든 요소들이 기능할 특정한 지상 규칙을 시행할 수 있다. 여기 반응 섬유로 물리학을 설명하는 코드 샌드박스가 있습니다. 우리는 캔버스 컴포넌트 내부의 물리학을 시뮬레이션하기 위해 `use-cannon` 라이브러리를 사용할 것이다. 우리는 도서관에서 Physics, useSphere, useBox를 수입한다.

```coffeescript
import { Physics, useSphere, useBox } from 'use-cannon'
```

현재 예제에서는 다음과 같이 여러 개의 공이 땅에 떨어지는 것을 시뮬레이션하려고 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/useSphere.gif?resize=512%2C308&ssl=1)

이 예에서 볼 수 있는 지면은 boxBufferGeometry의 예이며 볼은 우리가 이전에 본 적이 있는 spereBufferGeometry의 예다. 하지만, 이러한 것들에 물리 법칙을 적용하기 위해서는, 우리는 이와 동등한 물리적인 요소들을 이 메시에 붙여야 합니다. 이 작업은 useSphere 및 useBox 방식으로 수행됩니다. 방법은 다음과 같습니다.

```js
function Ball(props) {
  const { args = [0.2, 32, 32], color } = props
  const [ref] = useSphere(() => ({ args: 0.2, mass: 1 }))

  return (
    <mesh ref={ref}>
      <sphereBufferGeometry args={args} />
      <meshStandardMaterial color={color} />
    </mesh>
  )
}
```

이 코드 조각은 한 줄을 제외하고 이전에 했던 것과 유사합니다.

```coffeescript
const [ref] = useSphere(() => ({ args: 0.2, mass: 1 }))
```

이 선은 0.2에 해당하는 크기와 1에 해당하는 질량의 물리학계에 구를 만들고 있다. 이 API는 우리가 가지고 있는 구면 메쉬에 부착해야 하는 ref를 반환합니다. 접지는 다음과 같은 방법으로 생성됩니다.

```undefined
function Ground(props) {
  const { args = [10, 0.8, 1] } = props
  const [ref, api] = useBox(() => ({ args }))

  useFrame(() => {
    api.position.set(0, -3.5, 0)
    api.rotation.set(0, 0, -0.08)
  })

  return (
    <mesh ref={ref}>
      <boxBufferGeometry args={args} />
      <meshStandardMaterial color={'green'} />
    </mesh>
  )
}
```

중력이 작용하지 않도록 지면에 질량을 할당하지 않고 고정된 위치와 회전(틸트)도 설정합니다. 일단 지면과 공을 갖게 되면, 물리 법칙이 작용하기 위해서, 그것들은 물리학 제공자 안에 놓여야 합니다.

```js
<Physics 
  gravity={[0, -26, 0]} 
  defaultContactMaterial={ restitution: 0.6 }
>
  {balls.map((props) => (
    <Ball {...props} />
  ))}
  <Ground />
</Physics>
```

물리 구성요소가 중력과 복원력을 규정하는 두 개의 소품을 어떻게 채택하는지 주목하십시오. 여기:

- 중력(gravity)은 x, y, z 방향의 가속도를 결정하는 3D 어레이다. 여기서 y 방향으로 하향력을 시뮬레이션하는 음의 26으로 구성됨
- 환원(restimation)은 물리학계의 물체들 사이에서 충돌할 때마다 손실되는 에너지의 양을 결정하는 계수이다.

또한, 우리는 클릭할 때마다 캔버스에 구를 만드는 생성기 함수를 만듭니다.

```js
function onCanvasClicked(e) {
  let newBalls = [...balls]
  const color = colors[getRandomInt(6)]
  newBalls.push({ color: color })
  setBalls([...newBalls])
}
```

이것이 필요한 모든 설정입니다. 이를 통해 캔버스에 클릭이 등록될 때마다 물리학계에 새로운 구체가 추가되고, 구성된 중력으로 인해 가속 상태에서 지상으로 진행되어 지면/구체와 충돌한 다음 지면의 경사를 따라 계속 이동하게 된다. 그것은 반응-세-섬유 기능을 사용하여 브라우저에서 실제 물리학을 시뮬레이션할 수 있는 방법의 예이다.

## 성과

WebGL 라이브러리는 지난 몇 년 동안 성능에 관한 한 크게 개선되었습니다. 3.js는 이러한 이득을 활용하며 API에 동일한 속도를 제공한다. 그리고 리액트 3섬유 성능은 3.js와 GPU에 의해 병목 현상을 일으킨다. 즉, 리액트 3섬유 자체는 렌더링에 관한 한 어떠한 병목 현상도 일으키지 않는다는 뜻이다.

공식 페이지에서 인용:

> 렌더링 성능은 최대 3.js 및 GPU입니다. 구성 요소는 추가 오버헤드 없이 React 외부의 렌더 루프에 참여합니다. 리액션은 구성요소 트리를 만들고 관리하는 데 매우 효율적이며, 잠재적으로 규모에 따라 수동/임피던스 애플리케이션을 능가할 수 있습니다.

## 결론

이미 React를 사용하여 간단한 웹 앱을 만들고 있고 3D 방식을 모색하고 있다면, 리액트 3섬유는 바로 실행 솔루션이 됩니다.

이점은 다음과 같습니다.

- 개선된 WebGL 라이브러리의 성능
- 직접 사용할 수 있는 3.js API 및 아티팩트의 범위
- React 에코시스템을 떠날 필요 없음

이러한 이점을 염두에 두고, 리액트-세-파이버는 브라우저에 의해 우리가 이용할 수 있는 두 개의 차원을 렌더링할 때 최소한 시도하고 탐구할 수 있는 가치 있는 후보입니다. 그래도 아직 납득이 가지 않는다면 코드 샌드박스에서 이 멋진 예들을 더 확인해 보세요. 건배!