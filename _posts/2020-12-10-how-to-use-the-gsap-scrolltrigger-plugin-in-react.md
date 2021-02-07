---
layout: post
title: "반응에서 GSAP 스크롤 트리거 플러그인을 사용하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/Howtousethegsapscrolltriggerplugininreact.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/Howtousethegsapscrolltriggerplugininreact.png?fit=730%2C487&ssl=1)

스크롤 방식의 애니메이션 라이브러리를 사용하지 않고 웹 사이트에서 구현하기 위해 스크롤링이 복잡할 수 있습니다. 이러한 라이브러리는 스크롤에서 다양한 상호 작용을 만들고 사용자 환경을 개선하기 위한 간단한 인터페이스를 제공합니다.

수년 동안, 자바스크립트에서 스크롤링을 구동하는 스크롤 구동 애니메이션 라이브러리가 크게 향상되었다. 2013년 John Polacek의 SuperScrollorama 출시부터 2014년 Jan Paepke의 ScrollMagic, 2020년 GSAP의 Scroll Trigger 발표까지.

Scroll Trigger는 스크롤 방식의 애니메이션의 재창조이지만 보다 적합하고 사용자 친화적인 방법으로 스크롤하는 동안 GSAP 애니메이션의 흐름을 제어할 수 있는 기능을 제공합니다.

이 튜토리얼에서는 GSAP Scroll Trigger 플러그인, 스크롤에서 애니메이션을 트리거하는 데 사용하는 방법 및 이에 대한 사용 사례에 대해 알아봅니다. 이 과정에서 애니메이션에 GSAP를 사용하고 애니메이션을 트리거하는 Scroll Trigger(스크롤 트리거)를 사용하는 Retact(반응)에 랜딩 페이지를 구축합니다. 이 튜토리얼을 마치면 GSAP Scroll Trigger 플러그인의 기본 사항과 React에서 사용하는 방법을 이해할 수 있습니다.

## 전제조건

이 튜토리얼에서는 판독기에 다음이 있다고 가정합니다.

- 로컬 개발 시스템에 설치된 노드 > = 8.10
- 로컬 개발 시스템에 설치된 npx 5.2 이상
- GSAP를 사용하여 요소를 애니메이션하는 방법에 대한 기본 이해
- HTML, CSS, JavaScript 및 React에 대한 기본 지식

## GSAP 스크롤 트리거 플러그인 소개

GSAP는 GreenSock Animation Platform의 약어이다. DOM 요소, 캔버스, SVG, CSS, WebGL, 일반 자바스크립트 객체 등을 애니메이션으로 만들 수 있기 때문에 웹을 위한 최고의 애니메이션 라이브러리이다.

GSAP의 제작자들은 GSAP가 지구상에서 가장 빠른 완전한 기능을 갖춘 스크립팅 애니메이션 도구라고 강하게 믿고 있다.

Scroll Trigger는 GSAP를 기반으로 하며 몇 줄의 코드, 우수한 성능, 크로스 브라우저 호환성, GSAP 커뮤니티의 지원만으로 스크롤에서 이러한 흥미로운 GSAP 애니메이션을 트리거하는 데 사용할 수 있습니다.

## 스크롤 트리거에 대한 사용 사례

이 섹션에서는 스크롤 트리거의 중요성과 언제 사용해야 하는지 살펴보겠습니다.

아래 데모에는 세 개의 원이 있습니다. 세 번째 원은 GSAP와 함께 2초 동안 페이지의 x축을 따라 움직이기 위해 애니메이션화되었다. 세 번째 원이 언뜻 보이지 않을 수 있으므로 아래로 스크롤해야 합니다.

아래로 스크롤하면 페이지의 해당 섹션에 도달하기 전에 세 번째 원이 이미 "x 축"으로 이동했음을 알 수 있습니다. 그래 나도 알아. 그건 멋지지 않아.

좋은 소식은 Scroll Trigger(스크롤 트리거)는 스크롤하는 동안 사용자가 지정된 뷰포트에 도달했을 때 애니메이션을 트리거할 수 있도록 하는 기능으로 이 문제를 해결한다는 것입니다.

## GSAP Scroll Trigger 플러그인의 가능성

스크롤 트리거를 사용하여 수행할 수 있는 몇 가지 작업은 다음과 같습니다.

- 스크롤에 아무거나 애니메이션(DOM, CSS, SVG, WebGL 및 Canvas)
- 재생 상태 전환 또는 애니메이션 스크러빙
- 다른 화면에서 자동으로 크기 조정
- 수직 및 수평 스크롤 지원
- 요소를 제자리에 고정하는 기능

이러한 기능에 대한 자세한 내용은 Scroll Trigger 웹 사이트를 참조하십시오.

## 스크롤 트리거 기본 사항

스크롤 트리거를 사용하여 스크롤에서 애니메이션을 트리거하기 전에 기본 사항에 대해 알아봅니다.

> 이 튜토리얼은 독자가 GSAP를 사용하여 애니메이션을 만드는 방법을 공정하게 이해하고 있다고 가정하므로, 우리는 Scroll Trigger의 기본 사항만 다룰 것이다. 여기에서 GSAP를 통해 속도를 높일 수 있는 환상적인 리소스를 찾아보십시오.

이 섹션에서 학습할 스크롤 트리거 기본 사항은 이 튜토리얼의 뒷부분에 있는 프로젝트 작성에 사용됩니다. Scroll Trigger(스크롤 트리거) 속성 및 메서드의 전체 목록을 해당 문서에서 확인할 수 있습니다.

Scroll Trigger(스크롤 트리거)의 기본 사항을 이미 알고 있는 경우 이 섹션을 건너뛰고 프로젝트 섹션으로 바로 이동할 수 있습니다. 여기서 Action(반응)의 간단한 랜딩 페이지를 작성하고 Scroll Trigger(스크롤 트리거)를 사용하여 스크롤의 애니메이션을 트리거합니다.

### 방아쇠를 당기다

트리거 속성은 애니메이션이 시작될 지점을 지정하는 데 사용됩니다. `트리거` 속성을 추가하려면 아래 구문을 사용하십시오.

트리거: "트리거"

이 속성을 사용하면 이전 데모의 세 번째 원을 애니메이션의 트리거 포인트로 만들 수 있습니다. 즉, 지정된 요소의 뷰포트(이 경우 세 번째 원)에 도달할 때만 원이 x축 방향으로 이동한다는 의미입니다.

`#thirthCircle`을 트리거 포인트로 설정하고 변경된 내용이 있는지 살펴보겠습니다.

트리거: "#thirdCircle"

진실의 순간! 😱

이제, `제3의 원`이 보이는 대로 애니메이션을 시작한다는 것을 알 수 있다. 신나죠?

### 표식자

➡은 애니메이션으로 만들 요소의 시작과 끝 위치와 페이지 뷰포트를 볼 수 있는 개발용 기능입니다.

아래 구문을 사용하여 이전 데모에 `마커`를 추가해 보겠습니다.

➡: 참

트리거 요소(세 번째 원)로 스크롤하면 마커 덕분에 시작과 끝의 위치가 보인다.

### 출발하다

기본적으로 트리거 요소는 스크롤러의 뷰포트 하단에 들어가면 바로 애니메이션을 시작합니다.

그러나 Scroll Trigger `start` 속성을 사용하여 기본 `start` 위치를 변경할 수 있습니다.
아래 코드를 사용하여 이전 데모에서 애니메이션의 `시작` 위치를 변경합니다.

start: "top center"

- 여기서 상단(top)은 트리거 요소의 상단을 말하며, 이 경우 제3원(third Circle)이다.
- `center`는 웹 페이지의 중심을 나타냅니다.

상단, 하단, 중앙 등의 키워드를 활용해 애니메이션의 위치를 조절하는 것 외에 픽셀 값이나 백분율 값도 사용할 수 있다.

### 종지부를 찍다

시작 위치를 바꿀 수 있듯이 스크롤 트리거의 끝 속성을 사용하여 끝 위치를 변경할 수도 있습니다.

기본적으로 트리거 요소는 아래쪽이 스크롤러의 뷰포트 상단으로 들어가는 즉시 애니메이션을 중지합니다. 아래 코드를 사용하여 데모에서 애니메이션의 `끝` 위치를 변경해 보겠습니다.

end: "bottom top"

- 여기서 `하단`은 트리거 요소 `제3의 원`의 하단을 나타냅니다.
- 여기서 `top`은 웹 페이지의 맨 위를 나타냅니다.

### 문질러 닦다

`스크럽` 속성은 스크롤 위치를 애니메이션의 진행률로 연결합니다. 스크럽을 true로 설정하면 스크롤 막대가 시작 위치와 끝 위치 속성 사이에 있는 동안 스크러버 역할을 합니다.

demo에 `scrub:true` 구문을 사용하여 스크럽을 추가하여 보다 명확히 이해하도록 하겠습니다.

페이지를 스크롤하여 자세히 보면 아래로 스크롤할 때 트리거 요소(`세 번째 원`)가 x축을 따라 이동하지만 다시 위로 스크롤하면 원래 위치로 돌아가 스크러버 역할을 합니다.

지금까지 Scroll Trigger를 통해 페이지가 특정 뷰포트로 스크롤될 때 애니메이션을 트리거할 수 있습니다. 스크롤 트리거의 기본 사항에 대해서도 설명했습니다. 이제 반응 앱에서 스크롤 트리거를 사용하는 것으로 넘어갑시다.

## 반응 앱에서 스크롤 트리거 사용

아싸! 우리 모두가 기다려온 시간이 왔다. 이 섹션에서는 Relect(반응)가 포함된 기본 랜딩 페이지를 작성하고 랜딩 페이지를 애니메이션하며 Scroll Trigger(스크롤 트리거)를 사용하여 애니메이션을 트리거합니다.

GitHub의 코드 및 코드 저장소에서 데모를 확인하십시오.

이 섹션은 이해하기 쉽도록 여러 단계로 구분되어 있습니다.

### 1단계: 반응 프로젝트 생성

먼저 Create React App을 사용하여 React 애플리케이션을 회전시켜 보겠습니다.

npx create-discreate-approlly

위의 명령어는 우리를 위해 `스크롤리`라고 불리는 Retact 애플리케이션을 만들 것이다. 응용 프로그램이 성공적으로 생성되면 `cd 스크롤리`를 사용하여 해당 저장소로 전환하고 아래 명령을 실행하십시오.

npm start

Retact 앱이 올바르게 생성되었다면 `localhost:3000`으로 이동할 때 브라우저 창에 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/save-to-reload.png?resize=730%2C363&ssl=1)

### 2단계: React 앱에 콘텐츠 추가

즐겨찾는 IDE(통합 개발 환경)에서 새로 만든 Retact 앱을 열고 아래 코드를 App.js 파일에 붙여넣습니다.

```xml
import './App.css';
import workout from "./workout.svg";
import greensocklogo from "./greensocklogo.svg";
import happy from "./happy.svg";

function App() {
  return (
   <div className="App">
      <div className="first">
        <h1>ScrollTrigger</h1>
        <p className="first-paragraph">
          is the coolest Greensock plugin.
          <span role="img" aria-label="celebrating">
            🥳
          </span>
        </p>
        <div className="logo-main">
          <img src={workout} id="workout-logo" alt="workout" />
        </div>
      </div>
      <div className="second">
        <div className="logo-main">
          <img src={greensocklogo} id="gsap-logo" alt="greensocklogo" />
        </div>
      </div>
      <div className="third">
        <p>
          <span className="line" />
        </p>
        <div className="logo-main">
          <img src={happy} id="happy-logo" alt="happy" />
        </div>
      </div>
    </div>
  );
}
export default App;
```

위의 코드는 랜딩 페이지를 애니메이션화하는 데 사용할 기본 기능 리액트 구성 요소입니다.

이제 코드를 조금씩 설명하겠습니다.

```coffeescript
import './App.css';
import workout from "./workout.svg";
import greensocklogo from "./greensocklogo.svg";
import happy from "./happy.svg";
```

여기서는 랜딩 페이지를 작성하는 데 필요한 외부 파일을 모두 가져왔습니다.

위의 코드 조각으로 가져온 SVG 이미지는 여기에서 찾을 수 있습니다. 이러한 파일을 React 앱에 추가하려면 각 SVG 이미지(workout.svg, greensocklogo.svg 및 happy.svg)에 대한 파일을 생성하고 여기에 SVG 코드를 복사한 다음 생성한 파일에 붙여넣으십시오.

```xml
function App() {
  return (
   <div className="App">
      <div className="first">
        <h1>ScrollTrigger</h1>
        <p className="first-paragraph">
          is the coolest Greensock plugin.
          <span role="img" aria-label="celebrating">
            🥳
          </span>
        </p>
        <div className="logo-main">
          <img src={workout} id="workout-logo" alt="workout" />
        </div>
      </div>
      <div className="second">
        <div className="logo-main">
          <img src={greensocklogo} id="gsap-logo" alt="greensocklogo" />
        </div>
      </div>
      <div className="third">
        <p>
          <span className="line" />
        </p>
        <div className="logo-main">
          <img src={happy} id="happy-logo" alt="happy" />
        </div>
      </div>
    </div>
  );
}

export default App;
```

여기서는 세 개의 자식 div 요소를 가진 부모 div 요소를 가지고 있습니다. 이 섹션은 랜딩 페이지에 표시되는 내용을 제어합니다. 마지막으로, 우리는 반응 앱의 다른 섹션에서 재사용할 수 있도록 `앱` 구성요소를 내보냈습니다.

SVG의 이미지를 올바르게 추가했다면 오류가 없을 것이고 랜딩 페이지는 지금 이렇게 보일 것입니다.

### 3단계: 랜딩 페이지 사용자 정의

보시다시피 저희 랜딩 페이지의 스타일이 맞지 않으니 CSS로 더 매력적으로 만드세요. 이렇게 하려면 아래 코드를 `App.css` 파일에 붙여넣으십시오.

```css
*{
  padding: 0%;
  margin: 0%;
  box-sizing: border-box;
}

.first{
  width: auto;
  height:500px;
  background-color:  #fdfffc;
  margin-top: 30px;
}

.second{
  width:auto;
  height: 400px;
  background-color: #e0fbfc;
}

.third{
  width: auto;
  height: 400px;
  background-color: #fdfffc;
}

.line {
  width: 100%;
  max-width: 1400px;
  height: 20px;
  margin-top:20px;
  position: relative;
  display: inline-block;
  background-color:#023047;
}

h1{
  text-align: center;
  padding-top: 90px;
  color: #023047;
  font-size: 60px;
}

.first-paragraph{
  text-align: center;
  color: #023047;
  font-size: 20px;
  font-weight: bold;
}

.logo-main{
  align-items: center;
  display: flex;
  justify-content: center;
}

#workout-logo{
  width: 500px;
  height: 300px;
  margin-top: 10px;
}


#gsap-logo{
  width: 500px;
  height: 300px;
  margin-top: 50px;
  padding-right: 80px;
}

#happy-logo{
  width: 500px;
  height: 300px;
}

.second-paragraph{
  font-size: 30px;
  margin-top: 40px;
}

.third-paragraph{
  font-size: 30px;
  margin-top: 40px;
}
```

방금 추가한 CSS로, 우리 앱은 이렇게 보일 거야.

### 4단계: 참조 사용 대상 요소

useRef() React Hook을 사용하여 랜딩 페이지에서 애니메이션하려는 요소를 공략해야 합니다.

아래의 업데이트된 코드를 `App.js` 파일에 붙여넣으십시오.

```xml
import {useRef} from "react";
import "./App.css";
import workout from "./workout.svg";
import greensocklogo from "./greensocklogo.svg";
import happy from "./happy.svg";

function App() {

  const ref = useRef(null);

  return (
    <div className="App" ref={ref}>
      <div className="first">
        <h1>ScrollTrigger</h1>
        <p className="first-paragraph">
          is the coolest Greensock plugin.
          <span role="img" aria-label="celebrating">
            🥳
          </span>
        </p>
        <div className="logo-main">
          <img src={workout} id="workout-logo" alt="workout" />
        </div>
      </div>

      <div className="second">
        <div className="logo-main">
          <img src={greensocklogo} id="gsap-logo" alt="greensocklogo" />
        </div>
      </div>

      <div className="third">
        <p>
          <span className="line" />
        </p>
        <div className="logo-main">
          <img src={happy} id="happy-logo" alt="happy" />
        </div>
      </div>
    </div>
  );
}

export default App;
```

다음은 코드 변경에 대한 설명입니다.

```coffeescript
import {useRef} from "react";
```

useRef() React Hook을 App.js로 가져왔습니다.

```undefined
const ref = useRef(null);
```

그런 다음 대상 요소(이 경우 상위 div 요소)의 참조를 저장하는 데 사용할 ref라는 변수를 만들었습니다.

```xml
<div ref={ref} className="App">
    ...
</div>
```

마지막으로, 우리는 돌연변이 `ref` 객체를 부모 `div` 요소로 전달하였다.

### 5단계: GSAP를 사용하여 리액션 애니메이션 적용

이제 우리가 애니메이션화하려는 요소를 타겟으로 삼았으니 GSAP로 애니메이션을 해보겠습니다.

먼저 터미널에서 아래 명령을 실행하여 대응 앱에 GSAP를 설치합니다.

npm 설치 gsap

응답 앱에 GSAP를 성공적으로 설치한 후 아래의 업데이트된 코드를 `App.js` 파일에 붙여넣으십시오.

```coffeescript
import { useRef, useEffect } from "react";
import "./App.css";
import workout from "./workout.svg";
import greensocklogo from "./greensocklogo.svg";
import happy from "./happy.svg";
import { gsap } from "gsap";

function App() {
  const ref = useRef(null);
  useEffect(() => {
    const element = ref.current;
    gsap.fromTo(
      element.querySelector(".first-paragraph"),
      {
        opacity: 0,
        y: -20
      },
      {
        opacity:1,
        y: 0
      }
    );
  }, []);

  useEffect(() => {
    const element = ref.current;
    gsap.fromTo(
      element.querySelector("#gsap-logo"),
      {
        opacity: 0,
        scale: 0.2,
        y: -20
      },
      {
        opacity: 1,
        y: 0,
        scale: 1,
        duration: 1,
        ease: "none"
      }
    );
  }, []);

  useEffect(() => {
    const element = ref.current;
    gsap.from(element.querySelector(".line"), {
      scale: 0,
      ease: "none"
    });
  }, []);

  return (
    <div className="App" ref={ref}>
      <div className="first">
        <h1>ScrollTrigger</h1>
        <p className="first-paragraph">
          is the coolest Greensock plugin.
          <span role="img" aria-label="celebrating">
            🥳
          </span>
        </p>
        <div className="logo-main">
          <img src={workout} id="workout-logo" alt="workout" />
        </div>
      </div>

      <div className="second">
        <div className="logo-main">
          <img src={greensocklogo} id="gsap-logo" alt="greensocklogo" />
        </div>
      </div>

      <div className="third">
        <p>
          <span className="line" />
        </p>
        <div className="logo-main">
          <img src={happy} id="happy-logo" alt="happy" />
        </div>
      </div>
    </div>
  );
}

export default App;
```

다음은 App.js 파일의 기존 코드 변경에 대한 설명입니다.

```coffeescript
import { useRef, useEffect } from "react";
import { gsap } from "gsap";
```

use Effect() React Hook과 gsap을 App.js로 가져왔습니다.

```coffeescript
  useEffect(() => {
    const element = ref.current;
    ...
  }, []); 
```

우리는 세 아이 div 요소에 대해 세 가지 다른 `use Effect() 반응 후크를 초기화했다. 각 `use Effect() 후크` 내에서 `Element`라는 상수 변수를 생성하여 `useRef() React Hook:

```coffeescript
  useEffect(() => {
    const element = ref.current;
    gsap.fromTo(
      element.querySelector(".first-paragraph"),
      {
        opacity: 0,
        y: -20
      },
      {
        opacity:1,
        y: 0
      }
    );
  }, []);

  useEffect(() => {
    const element = ref.current;
    gsap.fromTo(
      element.querySelector("#gsap-logo"),
      {
        opacity: 0,
        scale: 0.2,
        y: -20
      },
      {
        opacity: 1,
        y: 0,
        scale: 1,
        duration: 1,
        ease: "none"
      }
    );
  }, []);

  useEffect(() => {
    const element = ref.current;
    gsap.from(element.querySelector(".line"), {
      scale: 0,
      ease: "none"
    });
  }, []);
```

세 개의 후크 각각에 GSAP `twen`을 추가하여 첫째 단락, 첫째 단락, #gsap-logo, .line의 세 아이 `div` 요소를 애니메이션화했다.

이번 업데이트로 랜딩 페이지는 이렇게 생겼습니다.

지금 랜딩 페이지를 보시면 첫 번째 단락, #gsap-logo 및 .line 하위 요소를 성공적으로 애니메이션화했음을 알 수 있습니다.

그러나 이 애니메이션은 페이지가 브라우저 창에 로드되면 애니메이션이 시작되어 현재 애니메이션을 볼 수 없습니다. 랜딩 페이지 끝에 있는 .line 요소의 애니메이션을 못 봤을 거예요.

스크롤 트리거 플러그인을 추가하여 애니메이션이 스크롤에서만 트리거되도록 합니다.

### 6단계: Scroll Trigger(스크롤 트리거)를 사용하여 스크롤에서 애니메이션 트리거

이제 Scroll Trigger 플러그인이 작동하는 것을 볼 시간입니다.

아래의 코드를 `App(.js)` 파일에 붙여넣으십시오.

```coffeescript
import { useRef, useEffect } from "react";
import "./App.css";
import workout from "./workout.svg";
import greensocklogo from "./greensocklogo.svg";
import happy from "./happy.svg";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
function App() {
  gsap.registerPlugin(ScrollTrigger);
  const ref = useRef(null);
  useEffect(() => {
    const element = ref.current;
    gsap.fromTo(
      element.querySelector(".first-paragraph"),
      {
        opacity: 0,
        y: -20
      },
      {
        opacity: 1,
        y: 0,
        scrollTrigger: {
          trigger: element.querySelector(".first"),
          start: "top top",
          end: "bottom center",
          scrub: true
        }
      }
    );
  }, []);
  useEffect(() => {
    const element = ref.current;
    gsap.fromTo(
      element.querySelector("#gsap-logo"),
      {
        opacity: 0,
        scale: 0.2,
        y: -20
      },
      {
        opacity: 1,
        y: 0,
        scale: 1,
        duration: 1,
        ease: "none",
        scrollTrigger: {
          trigger: element.querySelector(".first"),
          start: "top center",
          end: "bottom top",
          scrub: true
        }
      }
    );
  }, []);
  useEffect(() => {
    const element = ref.current;
    gsap.from(element.querySelector(".line"), {
      scale: 0,
      ease: "none",
      scrollTrigger: {
        trigger: element.querySelector(".third"),
        scrub: true,
        start: "top bottom",
        end: "top top"
      }
    });
  }, []);
  return (
    <div className="App" ref={ref}>
      <div className="first">
        <h1>ScrollTrigger</h1>
        <p className="first-paragraph">
          is the coolest Greensock plugin.
          <span role="img" aria-label="celebrating">
            🥳
          </span>
        </p>
        <div className="logo-main">
          <img src={workout} id="workout-logo" alt="workout" />
        </div>
      </div>
      <div className="second">
        <div className="logo-main">
          <img src={greensocklogo} id="gsap-logo" alt="greensocklogo" />
        </div>
      </div>
      <div className="third">
        <p>
          <span className="line" />
        </p>
        <div className="logo-main">
          <img src={happy} id="happy-logo" alt="happy" />
        </div>
      </div>
    </div>
  );
}
export default App;
```

다음은 코드 변경에 대한 설명입니다.

```coffeescript
import { ScrollTrigger } from "gsap/ScrollTrigger";
```

스크롤 트리거 플러그인을 `App.js`로 가져왔습니다.

```css
gsap.registerPlugin(ScrollTrigger); 
```

그런 다음 Scoll Trigger 플러그인을 등록했습니다.

```coffeescript
  useEffect(() => { 
    const element = ref.current;
    gsap.fromTo(
      element.querySelector(".first-paragraph"),
      {
        opacity: 0,
        y: -20
      },
      {
        opacity: 1,
        y: 0
        scrollTrigger: {
          trigger: element.querySelector(".first"),
          start: "top top",
          end: "bottom center",
          scrub: true
        }
      }
    );
  }, []);
```

각각의 use Effect() Hooks에서 "scrol 트리거"를 "gsapween"의 값 중 하나로 추가했습니다.

```css
scrollTrigger: {
    trigger: element.querySelector(".first"),
    start: "top top",
    end: "bottom center",
    scrub: true
}
```

이 경우 트리거 요소인 .first 요소를 추가했습니다. 그런 다음 애니메이션의 시작과 끝도 명시했다. 마지막으로 스크럽을 참으로 설정해 랜딩 페이지를 스크롤할 때 모든 애니메이션이 스크러버 역할을 할 수 있도록 했다.

업데이트된 변경사항으로 랜딩 페이지의 애니메이션은 웹 페이지를 스크롤하는 동안 지정된 뷰포트에 도달해야만 애니메이션을 시작합니다.

데모를 보려면 여기를 참조하십시오.

## 결론

이 튜토리얼에서 GSAP Scroll Trigger 플러그인에 대해 배울 수 있었기를 바랍니다. 이 튜토리얼에서는 Scroll Trigger(스크롤 트리거)를 사용할 수 있는 기본적인 가능성에 대해 설명하지만, Scroll Trigger(스크롤 트리거)를 사용하면 훨씬 더 많은 작업을 수행할 수 있습니다. GreenSock의 CodePen에 있는 데모에서 영감을 얻을 수 있습니다.

궁금하신 점이 있으시면 아래 댓글란에 남겨주시면 됩니다.

## 리소스 및 추가 읽기

- 그린소크 101, 페트르 티치
- GSAP 치트시트, Green Sock
- 스크롤 트리거 설명서, GreenSock
- 가장 일반적인 스크롤 트리거 오류, GreenSock