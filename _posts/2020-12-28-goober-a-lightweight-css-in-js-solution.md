---
layout: post
title: "Goober: 경량 CSS-in-JS 솔루션"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/goobercssinjs.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/goobercssinjs.png?fit=730%2C487&ssl=1)

CSS는 원래 전체 웹 페이지를 스타일링하기 위해 만들어졌다. 그러나 시간이 지남에 따라 웹사이트의 복잡성이 증가하여 디자인을 관리하는 것이 매우 어려워졌습니다. CSS에는 코드를 별도의 청크로 나눌 수 있는 모듈 개념이 없습니다.

심지어 자바스크립트에도 처음에는 모듈이 없습니다. 그러나 시간이 지나면서 웹 개발은 상당히 진화했습니다. 이제 React 및 Vue.js와 같은 구성 요소 기반 라이브러리를 사용하여 웹 앱의 프런트 엔드를 설계한다. 그것은 CSS를 위한 유사한 솔루션의 필요성을 강조했다. 그러나 표준 CSS는 프로그래밍 개념을 제공하지 않습니다. 그래서 우리는 CSS-in-JS라는 솔루션을 사용한다.

감정 및 스타일 구성 요소 같은 인기 있는 CSS-in-JS 라이브러리가 있다. 하지만, 그들의 주된 문제는 그들이 약 10KB에서 20KB의 공간을 차지한다는 것입니다. 용량이 큰 파일은 페이지 로드 시간과 따라서 검색 엔진에서 웹 사이트의 랭킹에 상당한 영향을 미친다는 것을 알 수 있습니다. 그래서 크리스티안 보트는 가벼운 대안인 구버를 만들었습니다. 1KB 미만의 공간이 필요하므로 고성능 사이트에서 선호하는 옵션입니다.

### 구버의 특징

- 개발자들이 쩔쩔매게 하는 주요 특징은 그 크기다. 1KB 미만의 가벼운 풋프린트로 다른 CSS-in-JS 솔루션보다 돋보인다.
- 바닐라 자바스크립트뿐만 아니라 리액트, Vue.js, Angular, Svelte 등과 같은 프런트엔드 라이브러리/프레임워크와 작동하도록 설계되었다.
- 서버측 렌더링 지원
- 약 24명의 적극적인 기여자를 통해 커뮤니티 규모 확대
- CSS 속성을 사용자 지정하는 다양한 방법. 예를 들어, CSS 태그가 지정된 템플릿에 소품을 제공하거나 JSON과 함께 CSS를 사용하여 소품을 제공합니다. 여기서 중요한 점은 JSON/Object를 사용하여 CSS 코드를 작성하면 번들 크기가 크게 줄어든다는 것입니다.
- 스타일 태그를 추가할 대상 노드를 지정하는 기능
- 전체 문서와 특정 섹션에 대한 코드를 분리할 수 있습니다.
- 코드를 쉽게 재사용
- goober는 여러 구성 요소에서 애니메이션을 재사용할 수 있도록 하는 `키프레임`이라는 방법을 가지고 있다.
- tyled.tag와 같은 코드를 goober의 이해할 수 있는 형식으로 변환하는 babel 플러그인이 있다.
- 공식 플러그인을 사용하여 개츠비와 guber 통합
- CSS 코드를 구문 분석하는 기능
- goober에는 의사 선택자와 함께 중첩된 규칙이 있습니다. 중첩된 스타일 구성 요소도 있습니다.
- 그것은 우리가 스타일을 확장할 수 있게 해준다. 예를 들어, 다른 CSS 규칙 집합으로 덮어쓰거나 "as" prop를 사용할 수 있습니다.
- 미디어 쿼리(@media) 및 키 프레임(@keyframe) 지원
- 구버의 흥미로운 특징은 스마트(지루한) 클라이언트 측 수분 공급과 함께 제공된다는 것이다.
- CSS 코드가 모든 웹 브라우저에서 작동하는지 확인하는 유용한 자동 접두사. 이 기능을 벤더 접두사라고도 합니다. Goober 뒤의 팀은 자동 접두사를 처리하기 위해 별도의 패키지를 만들었습니다.

## 벤치마크

그것의 시작 이후, goober는 개발자 커뮤니티로부터 상당한 적응을 보았다. 그것은 주버 뒤의 기여자들이 그것의 인기 있는 경쟁자들 사이에 실적을 비교하도록 격려했다.

그래서 그들은 괴저, 감정, 그리고 스타일리시한 요소들을 골랐다. 그런 다음 각 라이브러리에서 85개의 샘플을 실행하여 평균 작업 완료에 걸리는 시간을 알아봅니다.

벤치마크 결과는 매우 놀라웠습니다.

- 초당 21,469회의 처리되는 스타일 구성 요소
- gover 처리 속도 39,348 ops/s
- 초당 46,504회의 감정 처리

분명히, 여기서 승자는 감정 도서관입니다. 그러나 주목해야 할 점은 감정과 스타일의 구성요소가 잘 확립된 API를 가지고 있고 둘 다 200명 이상의 기여자를 가지고 있다는 것이다. 반면, goober는 CSS-in-JS 라이브러리 중 새로운 경쟁자이다. 그럼에도 불구하고, 그것은 속도 면에서 스타일링된 요소들을 능가하고 감정 라이브러리에 비해 아슬아슬하게 실행력을 준다.

## 괴버, 감정 및 스타일 구성 요소 비교

Goober에 대한 코드를 작성하기 전에, 인기 있는 경쟁사(즉, 감정 및 스타일 구성 요소)와 비교해 보십시오. 그러면 "왜 내가 괴버를 사용해야 하는가?"라는 당신의 질문에 대답할 것입니다.

위의 표에서 세 개의 CSS-in-JS 라이브러리가 모두 공통 기능을 가지고 있음을 알 수 있다. 그러나, 그 실행방식은 도서관의 성능을 결정한다. 감정은 둘 다보다 빠르지만 구버의 작은 발자국은 사용자에게 이점을 줄 수 있다.

## 괴버와 함께 시작하는 중

페이스북의 `크리에이트-리액트-앱` 프로젝트를 통해 리액트 앱을 빨리 설정해 봅시다. 이렇게 하려면 다음 단계를 수행하십시오.

먼저 "my-app" 폴더 내에 프로젝트를 만듭니다.

```undefined
npx create-react-app my-app
```

그런 다음 폴더 안으로 이동합니다.

```bash
cd my-app
```

이제 goober 라이브러리를 설치해야 합니다.

```coffeescript
npm install goober
```

이 시점에서, 우리는 gover와 React를 통합하기 위한 코드를 작성할 준비가 되었다. 먼저 하나의 제목과 세 개의 단락이 포함된 간단한 웹 페이지를 만들겠습니다.

우리가 직접 <h1> 태그를 목표로 하기 때문에 표제 스타일이 매우 간단하다. 반면 단락의 경우 모든 공통 코드를 한 번 정의하겠습니다. 그런 다음 각 단락마다 별도로 확장하십시오. src/index.js 내에 다음 코드를 붙여넣습니다.

```js
import React from "react";
import ReactDOM from "react-dom";
import { styled, setup } from "goober";

setup(React.createElement);

const Title = styled("h1")`
  font-weight: bold;
  color: #00f;
`;

const P = styled("p")`
  font-weight: bold;
`;

const P1 = styled(P)`
  color: #f00;
  font-style: italic;
  font-weight: normal;
`;

const P2 = styled(P)`
  color: pink;
  text-decoration: underline;
`;

const P3 = styled(P)`
  color: #bb0276;
`;

function App() {
  return (
    <div className="App">
      <Title>Hello World</Title>

      <P1>This is paragraph # 1 that is designed with goober.</P1>

      <P2>This is paragraph # 2 that is designed with goober.</P2>

      <P3>This is paragraph # 3 that is designed with goober.</P3>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

이제 콘솔 창에서 아래 명령을 실행하여 이 코드를 테스트할 수 있습니다.

```coffeescript
npm start
```

다음과 같은 모양이 됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/helloworldgoober.png?resize=546%2C332&ssl=1)

### 위 코드에 대한 설명

우선 모든 것이 올바르게 작동하도록 필요한 패키지 리액트, 리액트 DOM, goober를 수입했다. 그런 다음 초기에는 setup() 방법을 호출해야 합니다. setup()은 styled() 함수를 사용하기 전에 호출되어야 합니다.

그 후, 우리는 태그가 붙은 템플릿 리터럴을 사용하여 표제와 단락에 대한 다른 CSS 규칙을 만들 것이다.

## 소품으로 스타일 사용자 지정

구버에서 스타일을 사용자 지정하는 다양한 방법이 있습니다. 그 중 하나는 소품을 사용하는 것입니다. 기본적으로 우리는 소품을 사용하여 원하는 값을 설정하고 원하는 `스타일링()` 함수의 템플릿 리터럴 내부에서 값에 액세스한다.

예를 들어:

```js
const Title = styled('h1')`
  color: ${props => props.textColor};
  font-size: 3rem;
`;

function App() {
  return (
    <div className="App">
      <Title textColor="red">Hello World</Title>
    </div>
  );
}
```

## 위 코드에 대한 설명

여기서, 우리가 `textColor`라고 불리는 소품을 추가하고 "red"라는 값을 할당하는 것을 볼 수 있습니다. 그런 다음 우리는 스타일링() 함수의 템플릿 리터럴 내부에 있는 소품을 사용하여 CSS의 "color" 속성에 값을 할당했다.

## 글로벌 스타일

goover는 `➡`라는 함수로 꽉 차 있다. 전체 문서에 적용될 전역 스타일을 지정하는 데 사용됩니다. 그것은 웹 디자인에서 다른 요소들 사이에 공통적으로 존재하는 많은 코드들이 꽤 유용하다.

예를 들어, 글로벌 기능은 전체 웹 페이지에 사용될 외부 글꼴을 포함하기에 매우 적합합니다. 대부분의 사람들은 "CSS 재설정" 규칙을 작성하는데도 이것을 사용한다. 우선 gover에서 glob을 수입해야 한다. 다음과 같은 경우:

```css
glob`
  body {
    margin: 0;
  }
`;
```

## 스타일.태그가 없습니다.

스타일 구성 요소 라이브러리에서 작업한 적이 있다면 `tyled.tag` 기능에 익숙할 수 있습니다. CSS-in-JS 솔루션으로 작업하는 개발자 사이에서 매우 인기가 있다.

goober는 기본적으로 이 형식을 지원하지 않습니다. 하지만, 기여자들과 그 팀은 바벨 플러그인을 개발했습니다. 이 플러그인은 모든 styled.tag 참조를 goober가 이해할 수 있는 형식으로 변환하는 데 사용됩니다.

## 결론

goober 개발의 주된 아이디어는 감정과 스타일 구성 요소 같은 인기 있는 CSS-in-JS 라이브러리를 위한 경량 대안을 도입하는 것이었다. 따라서 다음과 같은 시나리오에서 사용해야 합니다.

- 웹 페이지를 더 빨리 로드하려는 경우
- 웹 사이트는 많은 트래픽(즉, 매달 수백만 명의 사용자)을 수신합니다. 뉴스 웹사이트, 블로그, SaaS 애플리케이션 또는 소셜 미디어 네트워크와 같은 것. 구버는 1kB 미만이어서 경쟁사 대비 대역폭을 많이 섭취하지 않기 때문이다.

나는 그 팀이 경쟁사들이 가지고 있는 거의 모든 기능을 제공하는 것을 정말 잘했기 때문에 괴버의 이렇다 할 단점을 발견하지 못했다. 그리고 모두 아주 작은 코드베이스를 사용합니다.