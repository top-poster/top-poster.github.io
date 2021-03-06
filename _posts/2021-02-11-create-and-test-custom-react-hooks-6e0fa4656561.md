---
layout: post
title: "사용자 정의 반응 후크 만들기 및 테스트"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Man holding camera lens](https://miro.medium.com/max/11200/0*zvlJFWz9U1bbAV0M)

리액트 훅스는 2018년 10월 리액트 콘프에서 세상에 소개되었으며, 리액트 릴리스 `16.8.0`의 일부로 2019년 2월에 출시되었다.

이제 더 이상 React with ES6 클래스를 사용할 필요가 없기 때문에 게임을 변경할 수 있습니다. 모든 것은 리액트 기능 컴포넌트를 통해 이루어질 수 있다.

React Hooks는 자산 업데이트 관리 방법 또는 상태 관리 방법을 대체합니다. 그것은 좀 더 선언적이고 단순한 접근법이다. 사용자 지정 후크를 만드는 것은 쉽고 직관적입니다.

# 컨텍스트의 일부

리액트 훅스 이전에는 렌더링 소품 방식이나 고차 부품 방식을 통해 부품 간 로직을 공유했다.

이 두 가지 접근 방식은 진부해 보이고 매우 지저분한 JSX 구문을 생성한다. 결국엔 "렌더 소품 콜백 지옥"이나 "래퍼 지옥"으로 끝날 수도 있습니다.

React Hooks를 사용하면 구성 요소의 로직을 더 나은 방식으로 재사용하고 내장할 수 있습니다. 우리는 관련된 논리를 촘촘히 결합할 수 있다. 후크는 테스트할 수 있고 합성할 수 있습니다.

이 기사에서는 다음과 같은 내용을 다룹니다.

취재할 게 많으니까, 시작하겠습니다!

# Next.js 앱을 만드는 중

모든 코드를 실행하기 위해 Next.js 응용 프로그램을 만들 것입니다.

먼저 다음 명령을 실행합니다.

```undefined
npx next-ap 생성
```

설정이 완료되면 다음 세 가지 명령을 사용할 수 있습니다.

```undefined
실 dev
개발 서버를 시작합니다.
실 체격
프로덕션용 앱을 빌드합니다.
실타래를 치다
프로덕션 모드에서 빌드된 앱을 실행합니다.
```

프로젝트에 TypeScript를 추가하겠습니다. next.js는 몇 가지 단계를 통해 즉시 실행 시간을 지원합니다. 자세한 내용은 공식 문서를 참조하십시오.

```undefined
tsconfig.json을 누릅니다.
```

TypeScript 종속성을 설치하겠습니다.

```undefined
yarn add --dev typescript @types/types/node
```

완료되면 다음을 실행합니다.

```undefined
npm 런 dev
```

이 명령은 프로젝트에서 TypeScript를 사용하고 있음을 감지하고 생성된 `tsconfig`를 구성합니다. 나중에 필요에 따라 사용자 정의할 수 있는 간단한 구성을 제공합니다. TypeScript가 완료되면 포트 3000에서 앱을 실행하며 https://localhost:3000에서 액세스할 수 있어야 합니다.

# 샘플 앱

간단한 React 확장 가능한 구성 요소를 생성하겠습니다. 이 구성 요소는 요약과 설명을 매개 변수로 사용합니다. `버튼`을 누른 경우에만 설명이 표시됩니다. 다시 누르면 숨겨집니다. 버튼은 토글의 역할을 합니다.

next.js는 파일이 `[name]과 일치할 경우 즉시 `css-in-js`를 지원합니다.module.dister` 패턴 Expand.module.css는 매우 간단합니다.

```undefined
.tv {.
패딩: 1렘 0;
}
```

참고: 일반적으로 `차단` 구성 요소는 별도의 파일에 생성해야 합니다. 저는 이것을 모두 하나로 하고 있어서 시각화하기 더 쉽습니다.

구성 요소의 작동 상태를 살펴보겠습니다.

![Component in action](https://miro.medium.com/max/1920/1*kWJpUACwG6dcARkN7Ejzhg.gif)

# 반응 후크의 리프레셔

Hooks에 대한 규칙을 빠르게 살펴보겠습니다.

이러한 규칙이 항상 존중되도록 하기 위해 다음 ESLINT 규칙을 사용할 수 있습니다.

```undefined
실 덧셈 --devlint-devlint-develt-develt-devel
```

사용:

```undefined
{
"s": [
// ...
"➡-➡"
],
"규칙": {
// ...
"react-hooks/rules-of-hooks": "error", // 후크의 규칙 확인
"Retact-hooks/Extustive-deps": "경고" // 효과 의존성 확인
}
}
```

사용자 지정 후크는 항상 기본 반응 논리를 재사용하여 구성 요소를 새로 고쳐야 합니다. 그 기본 훅을 후크 원시라고 생각할 수 있습니다. 다음은 다음과 같습니다.

사용자 정의 훅의 입력과 출력은 전적으로 당사에 달려 있습니다. 우리는 어떤 제약도 없다. 단지 관습일 뿐이다. React Hooks로 일관된 방식으로 사용자 지정 Hooks를 만들면 보다 직관적으로 사용할 수 있습니다.

# 사용자 지정 후크

리프레셔를 사용했으므로 이제 리액트 후크를 만들 준비가 되었습니다. 이 예에서는 `true/false` 상태 간에 전환할 후크를 만듭니다.

확장 가능한 구성 요소에 사용되는 토글 논리는 응용 프로그램의 다른 부분에서 쉽게 재사용될 수 있습니다.

```undefined
const [Show, setShow] = useState(false);
= 사용Callback(() = {}을(를) 변경합니다.
setShow(!show);
}, [Show, setShow];
```

이 논리를 포장할 사용자 정의 후크를 생성하겠습니다. 우리가 가장 먼저 해야 할 일은 이름을 붙이는 것이다. 관례에 따라 모든 후크는 use라는 접두어를 붙인다. 이름 지정 규칙을 사용하면 쉽게 찾을 수 있습니다. 우리는 그것들을 조건부로 사용할 수 없기 때문에, 그것들을 발견하기 쉬운 것이 필수적이다.

후크는 useToggle.ts로 명명할 것이다. 우리는 그것을 자체 훅 폴더에 넣을 것이다. 훅스는 자바스크립트 논리만 보유하게 될 것이기 때문에 우리는 그것이 `tsx` 확장을 하기 위해 필요하지 않다.

팁: `return [state, toggle]의 `as const`를 continuous로 표시 이렇게 하면 Type 스크립트가 이 배열의 고정 크기를 표시하므로 Type 스크립트는 `Tuple`을 사용하여 배열 항목 유형을 더 잘 노출할 수 있습니다.

확장 가능한 구성 요소에 사용하겠습니다.

# 사용자 지정 후크 테스트

이제 Hook을 테스트해 볼 차례입니다. 이것은 그것이 예상대로 작동하고 미래의 원자로에 의해 깨지지 않도록 확실히 합니다. 테스트에는 반응 테스트 라이브러리를 사용할 예정입니다.

먼저 lib 설치부터 시작하겠습니다.

```undefined
// 농담 및 해당 타이프 스크립트 도구 설치
@types/types/types를 추가하다.
// 반응 테스트 라이브러리 설치
실 덧셈 --dev @dev @devin-devin/d
// 반응 테스트 라이브러리를 위한 사용자 정의 jsdom 매치커
실 덧셈 --dev @devin-twin/twin-dom
// 반응 테스트 라이브러리의 사용자 이벤트 유틸리티
yarn add --dev @discret-disc/user-event
```

그런 다음 Jest를 구성해야 합니다.

```undefined
npx ts-vs 구성:init
```

Next.js 앱은 `tsconfig.json`에 `jsx: `preserve` 설정이 있어야 하고, Retact Testing Library에서는 작동하지 않으므로 사용자 지정 `tsconfig.jest.json`이 필요합니다. 하지만 걱정하지 마세요, 당신은 모든 것을 다시 선언할 필요는 없습니다. 확장을 사용할 수 있습니다.

```undefined
module.dister = {
사전 설정: 'ts-contract',
전역: {}
"ts-contract": {
// 사용자 지정 tsconfig 파일 사용
tsconfig: "tsconfig.discount.json"
}
}
};
```

그러면 `tsconfig.jest.json` 파일은 다음과 같습니다.

```undefined
{
"/tsconfig.json": "/tsconfig.json"
"compiler Options": {
"jsx": "jsx"
},
}
```

Hurray! Next.js + Typescript + Jest + React Testing Library로 준비되었습니다. 이제 구성 요소를 테스트하는 데 집중할 수 있습니다. React Testing Library 접근 방식은 사용자 동작을 기반으로 하므로 구성 요소에 대한 사용자 지정 Hook의 영향을 테스트합니다.

Target Component를 사용하여 사용자 지정 후크의 출력과 일치하는 요소를 표시하거나 숨깁니다. 가능한 경우 후크의 구현이 아닌 동작으로 후크를 테스트하려고 합니다. React Testing Library는 방법이 아닌 테스트 동작을 기반으로 구축되었습니다. 사용자가 어떤 메서드가 호출되었는지 상관하지 않는 경우, 왜 호출해야 합니까?

가능하지 않은 경우, 항상 반응 후크 테스트 라이브러리로 돌아갈 수 있습니다.

이제 결과를 보기 위해 `야크 농담`을 실행해 봅시다.

![Test results](https://miro.medium.com/max/876/1*lQcuiJfRSszqkxG2tlxjMw.png)

만세! 모두 예상대로 되고 있어!

# 마무리 중

![Sun beaming down on greenery](https://miro.medium.com/max/8318/0*_fxekubQ1ZrOa_xD)

기존 React Hooks를 결합하고 고객의 요구에 맞는 새로운 맞춤형 Hook을 만드는 것이 얼마나 쉬운지 확인했습니다. 가능성은 무궁무진하며, 기존의 "렌더 소품" 접근 방식을 대체하기에 좋습니다. 여러 구성 요소에서 일부 논리를 캡슐화하고 재사용할 수 있는 더 나은 도구를 사용할 수 있습니다.

테스트 측면에서는 모든 것을 설정한 후 후크를 테스트하는 것이 얼마나 쉬운지 확인했습니다. 그것은 마치 일반 부품을 시험하는 것과 같다. 후크는 많이 재사용되므로 적절히 테스트해야 합니다.

이 문서에서는 더 많은 테스트 통찰력을 확인할 수 있습니다.

더 많은 리액트 콘텐츠가 여러분의 뜻대로 될 것입니다. 건배!