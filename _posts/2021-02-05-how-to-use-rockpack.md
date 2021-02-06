---
layout: post
title: "Rockpack 사용 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/how-to-use-rockpack.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/how-to-use-rockpack.png?fit=730%2C487&ssl=1)

간단히 말해 록팩은 스테로이드제를 이용한 `크레이(CRA)` 생성 반응 앱이다. 리액트 부트스트래핑 툴로서 무거운 리프팅 작업을 목표로 하며 리액트 애플리케이션 설정 과정을 간소화합니다.

Rockpack은 개발자가 서버측 렌더링, 번들링, 보풀링, 테스트, 로깅, 로컬라이징 등을 지원하여 Retact 애플리케이션을 부트스트랩할 수 있도록 지원합니다.

락팩을 가장 쉽게 생각할 수 있는 방법은 그것이 많은 유용하고 잘 생각되었으며 종종 추가가 필요한 "CRA"라는 것이다.

이 게시물에서는 Rockpack, 아키텍처, 모듈 및 Rockpack이 필요한 이유에 대해 알아보겠습니다.

## 락팩 모듈

Rockpack의 가장 좋은 점 중 하나는 모듈식 아키텍처를 통해 원하는 Rockpack 모듈만 사용할 수 있다는 것입니다.

이 섹션에서는 각 Rockpack 모듈에 대해 자세히 살펴보겠습니다.

## @락팩/레저

웹팩 기반의 리액트 번들러입니다. 필요한 로더와 플러그인으로 미리 구성되어 있으며, 즉시 모범 사례를 사용합니다.

#### 사용량:

이 모듈을 사용하려면 다음 단계를 실행하십시오.

- 모듈 설치:
#NPM
npm install @rockpack/save --save-dev
#야른
실 추가 @rockpack/dev --dev
- 다음 코드를 사용하여 루트 디렉터리에 `build.js` 파일을 생성하십시오.
const {frontendCompiler} = require@rockpack/request`;

프런트 엔드 컴파일러(;
- `build.js`를 실행합니다.
#개발
교차 Env NODE_ENV= 개발 노드 빌드
#프로덕션
교차 Env NODE_ENV= 프로덕션 노드 빌드

프로덕션 빌드를 실행하면 프로덕션 최적화 기능을 갖춘 축소된 버전의 앱이 생성됩니다.

Rockpack 컴파일러는 수많은 멋진 기능을 가지고 있다. 다음은 그 중 일부입니다.

- TypeScript 지원
- nodemon, Liveroad, 소스 맵을 사용하여 빌드 nodejs 스크립트 지원
- 반응 최적화
- SEO 최적화
- 이미지 축소
- 그래프QL 지원(webpack-graphql-loader)

목록은 계속됩니다. 여기서 전체 목록을 얻을 수 있습니다.

## @락팩/레저

이 중요한 라이브러리는 Rockpack 프로젝트에 대한 SSR(Universal Server-side rendering) 지원을 제공합니다. 렉스(thunk, sagas), 아폴로, 뫼브스 등 다양한 솔루션과 연동돼 SSR를 보편적으로 지원하는 것으로 알려졌다.

즉, React는 SSR를 지원하지만 비동기 작업을 고려하지 않고 이를 수행합니다. @rockpack/usr은 SSR 동안 비동기식 작업에 대한 지원을 추가합니다.

React 상대와 유사한 일부 API(사용자 지정 후크)를 제공합니다. 이 중 일부는 usr state와 usr effect이다. 이는 useState와 use Effect 후크와 유사하다. 그러나 useUsrState와 useUsrEffect는 비동기식 운영과 SSR를 지원한다.

#### 사용량:

`@rockpack/usr`을 사용하려면 아래 단계를 따르십시오.

- 모듈 설치:
#NPM
npm install @rockpack/saving --save
npm install @rockpack/save --save-dev

#야른
@rockpack/brokpack의 실 추가
실 추가 @rockpack/dev --dev
- 다음과 같은 `SSR` 지원이 포함된 후크 가져오기 및 사용:
`반응`에서 반응을 가져옵니다.
`@rockpack/ussr`에서 {ussrState, ussrEffect} 가져오기;

ConstateUsers = 비동기식() => {
// 일부 비동기 작업 수행
}

내보내기 const App = () => {
const [Users, setUsers] = useUsrState({users: [] });
Usr Effect(sync) 사용(sync) = {
constallUsers = waitgetUsers(;
사용자 설정(모든 사용자);
});
{}을(를) 반환하다
// 일부 작업을 수행하여 사용자 목록 렌더링
};
};
- `client.jsx` 구성:
`반응`에서 반응을 가져옵니다.
`hydrate-dom`에서 {hydrate} 가져오기;
`@rockpack/usr`에서 createUsr을 가져옵니다;
{App}을(를) `./App`에서 가져옵니다.

const [Usr] = createUsr(창)입니다.소련_DATA);
수분을 공급하다
<서스르>
<
</Usr>,
document.getElementById(`root`)
);

줄:

```js
const [Ussr] = createUssr(window.USSR_DATA); 
```

서버에서 실행된 상태를 클라이언트와 연결하여 `ussrState`가 올바르게 작동하도록 합니다.

또한 SSR 응용 프로그램을 부트스트랩하기로 선택하면 기본적으로 `build.js`와 `server.jsx`(Koa를 사용하는) 파일이 설정됩니다. 이러한 파일은 서로 작동하도록 미리 구성되어 있으므로 더 이상 편집할 필요가 없습니다.

여기서 `@rockpack/usr` 모듈 사용에 대한 자세한 내용을 확인할 수 있습니다.

## @락팩/레저

여기에는 Jest와 일부 권장 모듈 및 추가 기능이 사용됩니다. Best Practice로 구성된 강력한 테스트 제품군을 갖추고 있습니다. 그리고 TypeScript와 babel을 완벽하게 지원합니다.

#### 사용량:

이 모듈을 사용하려면 다음 단계를 따르십시오.

- 설치:
#NPM
npm install @rockpack/save --save-dev

#야른
실 추가 @rockpack/dev --dev
- 프로젝트 루트에 `test.js`를 생성하고 다음 코드를 추가하십시오.
constests = required@rockpack/request`;

시험(;)
- 다음 명령을 사용하여 테스트를 실행합니다.
노드 tests.js

#개발
노드 tests.js --watch

## @락팩/코드스타일

모범 사례를 사용하여 구성한 Eslint입니다.

#### 사용량:

이 모듈을 사용하려면 다음 단계를 따르십시오.

- 설치:
#NPM
npm install @rockpack/codstyle --save-dev

#야른
@rockpack/codstyle --dev 추가
- 다음 코드를 사용하여 `eslintrc.js` 파일을 생성합니다.
const {rockConfig} = require@rockpack/codstyle`;

module.disc = rockConfig({)
`no-plus`: `error`
});
- 모든 구성을 재정의하려면 아래 나온 것처럼 `rockConfig()` 함수에 1초만 할애하면 됩니다.
module.disc = rockConfig({}, {})
플러그인: [
`새 플러그인`
]
});

이 모듈에 대한 자세한 내용은 여기를 참조하십시오.

## @락팩/레저

여기가 바로 사물들이 정말 흥미로워지고 락팩이 빛나는 곳입니다. `@rockpack/logger` 모듈은 모든 사용자 작업(누른 버튼, OS 정보, 디스플레이 등)을 기록하는 고급 로깅 시스템입니다. 그런 다음 디버깅 섹션에서 이러한 작업을 분석할 수 있습니다.

#### 사용량:

이 모듈을 사용하려면 다음 단계를 따르십시오.

- 설치:
#NPM
npm install @rockpack/saving --save

#야른
@rockpack/brokpack의 실 추가
- 아래와 같이 `Logger Container` 구성 요소의 `App.js` 구성 요소를 포장합니다.
...
<로거 컨테이너
회기하다ID = {window.세션 ID}
제한 = {75}
getCurrentDate={() => dayjs(.format) »YYYY-MM-DD HH:mm:ss`)}
stdout={ShowMessage}
오류 = {stackData => sendToServer(stack)에서}
onPrepareStack={stack => stack.language = window.language}
>
<
</로거 컨테이너>
...

위의 코드 조각은 `exported` 익명 함수의 반환 값이어야 합니다. `@rockpack/logger` 모듈의 전체 설정과 사용은 다소 까다롭다.

## @rockpack/localazer

get text를 사용하는 고급 로컬라이저 모듈입니다. 그것은 우리의 응용 프로그램을 다른 언어와 지역에 쉽게 적응시킬 수 있는 (현지화) 방법을 제공한다.

#### 사용법

- 설치:
#NPM
npm install @rockpack/localaser --save
npm install @rockpack/save --save-dev

#야른
실 추가 @rockpack/localaser
실 추가 @rockpack/dev --dev
- 그런 다음 아래와 같이 `로컬라이제이션 옵서버` 구성 요소의 `App.js`를 포장해야 합니다.
`@rockpack/localaser`에서 {localizationObserver}을(를) 가져옵니다.

구성 루트 = () = > {
답례하다
< 로컬라이제이션 관찰자
현재 언어= {this.state.currentLanguage}
언어 = {this.state.content}
>
<
</로컬라이제이션 관찰자>
)
}

@rockpack/localaser 모듈의 설정과 사용법은 다소 까다롭다.

## 왜 락팩이죠?

주목할 만한 락팩 대안으로는 넥스트.js와 크리에이트 리액트 앱이 있다. 그러나 락팩은 여러 가지 면에서 경쟁사보다 뛰어나다.

첫째, 모든 Rockpack 애플리케이션은 기본적으로 다음을 지원합니다.

- 다른 파일 형식 가져오기(형식 목록)
- 이미지 최적화, SVG 최적화
- SVG 파일을 React 구성 요소로 로드하는 중
- CSS/SCSS/미만 모듈
- Babel 또는 TS, CSS/SCSS/미만 모듈에 대한 TS 지원
- 포스트CS 자동 고정 장치
- SEO 최적화, 반응 최적화
- 번들 분석기
- 그래프QL 지원

전체 목록은 여기에서 확인할 수 있습니다(이 목록은 모두 모범 사례를 사용하여 작성됨). Rockpack은 이러한 모든 기능을 즉시 제공함으로써 애플리케이션 설정 시간을 단축하는 것을 목표로 합니다. 이것은 앱의 크기와 개발자 스킬에 상관없이 가능합니다.

Rockpack을 사용하는 다른 이유는 다음과 같습니다.

- 모듈식이며 선택적으로 레거시 애플리케이션으로 가져올 수 있습니다.
- 사용하기 쉽고 매우 초보자 친화적인 API를 제공합니다(위에 나열된 기능으로 응용 프로그램을 부트스트랩할 수 있습니다).
- 매우 유연한 아키텍처를 갖추고 있으므로 선호하는 라이브러리를 사용하여 상태를 관리할 수 있습니다.
- 로깅 및 현지화를 위한 강력한 툴을 제공합니다.

Rockpack의 장점은 흥미롭고 훌륭합니다. 어떤 개발자도 이 멋진 라이브러리를 사용하는 데 관심이 있을 것입니다. 다음 섹션에서는 Rockpack을 사용하여 Retact 애플리케이션을 부트스트랩하는 방법에 대해 알아보겠습니다.

## Rockpack을 시작하는 방법

Rockpack을 시작하는 가장 쉬운 방법은 `@rockpack/starter` 모듈을 사용하는 것입니다. 이것은 Rockpack 프로젝트의 일부입니다.

다음과 같은 세 가지 유형의 응용 프로그램을 지원합니다.

- React Client Side Render(`React CSR`) – Create React App과 유사한 보일러 플레이트 코드를 제공합니다.
React Server Side Render(`React SSR Light Pack`) — 다음과 같은 상용구 코드를 제공합니다.
서버측 렌더링(`SSR`)
Koa를 Node.js 서버에 사용한다.
`@loadable/components`
React Server Side Render(`React SSR Full Pack`) — 다음과 같은 기본 플레이트 코드를 제공합니다.
React SSR 라이트 팩에 포함된 모든 기능
리액트 라우터
레덕스
레덕스가
반응-헬멧-아식스
Library 또는 React 구성 요소 - esm/cjs 빌드 및 축소 버전을 지원하는 React Library 또는 Component를 개발하는 데 필요한 효율적인 구성되는 웹 팩을 제공합니다.
- React Client Side Render(`React CSR`) – Create React App과 유사한 보일러 플레이트 코드를 제공합니다.
- React Server Side Render(`React SSR Light Pack`) — 다음과 같은 상용구 코드를 제공합니다.
서버측 렌더링(`SSR`)
Koa를 Node.js 서버에 사용한다.
`@loadable/components`
- 서버측 렌더링(`SSR`)
- Koa를 Node.js 서버에 사용한다.
- `@loadable/components`
- React Server Side Render(`React SSR Full Pack`) — 다음과 같은 기본 플레이트 코드를 제공합니다.
React SSR 라이트 팩에 포함된 모든 기능
리액트 라우터
레덕스
레덕스가
반응-헬멧-아식스
- React SSR 라이트 팩에 포함된 모든 기능
- 리액트 라우터
- 레덕스
- 레덕스가
- 반응-헬멧-아식스
- Library 또는 React 구성 요소 - esm/cjs 빌드 및 축소 버전을 지원하는 React Library 또는 Component를 개발하는 데 필요한 효율적인 구성되는 웹 팩을 제공합니다.

응용 프로그램을 부트스트랩하려면 다음 단계를 수행하십시오.

- 설치:
#NPM
npm install @rockpack/skip -g
- 앱 만들기:
Rockpack < your project name>
- 터미널 질문에 답하고 나머지는 Rockpack이 처리합니다. 모든 작업이 완료되면 다음을 수행해야 합니다.
- 프로젝트 시작:
cd < your project name>
# 달려라
npm start

## 결론

Rockpack은 잘 알려진 도서관이다. 나는 이미 그것을 사용할 생각에 들떠 있다.

Rockpack의 아름다운 점 중 하나는 Ract 코드를 쓸 수 있기 때문에 가파른 학습 곡선이 없다는 것입니다. 또한 상태 관리와 같은 경우 선호하는 라이브러리를 유연하게 사용할 수 있습니다.