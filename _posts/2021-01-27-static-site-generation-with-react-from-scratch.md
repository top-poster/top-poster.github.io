---
layout: post
title: "React를 사용하여 처음부터 정적 사이트 생성
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-21-at-2.58.15-PM.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-21-at-2.58.15-PM.png?fit=730%2C487&ssl=1)

어떤 프레임 워크가 가장 좋은 컴포넌트 모델인지 물어 보면 망설이지 않고 React라고 말하고 싶습니다.
 

여기에는 여러 가지 이유가 있습니다.
 한편으로 React는 많은 의식과 타협없이 구성 요소를 만듭니다.
 이 모델은 React와 거의 독립적입니다.
 새로운 JSX 팩토리를 사용하면 TypeScript 또는 Babel에서 React를 가져 오는 것을 볼 수 없습니다.
 

분명히 React에서만 모든 프론트 엔드 애플리케이션을 빌드 할 때 한 가지주의 사항이 있어야합니다.
 React가 실제로 프론트 엔드를 수행하는 올바른 방법인지에 대한 명백한 종교적 논의 외에도 React가… 결국…
 

JavaScript 라이브러리이며 실행하려면 JavaScript가 필요합니다.
 이것은 더 긴 다운로드 시간, 더 부풀어 오른 페이지 및 아마도 그다지 좋지 않은 SEO 순위를 의미합니다.
 

## JavaScript없이 React를 사용하기위한 옵션
 

그렇다면 상황을 개선하기 위해 무엇을 할 수 있습니까?
 초기 반응은 다음 중 하나 일 수 있습니다.
 

> "예, 대신 Node.js 서버를 사용하고 SSR도 수행하겠습니다."
“이미 정적 마크 업을 생성하는 것이 있지 않습니까?
 저 개츠비는 누구죠?”
“흠 나는 모든 것을 원하지만 분명히 Gatsby와 같은 작업을 수행하는 SSR 프레임 워크가 좋을 것입니다.
 이것이 Next.js 레벨 맞죠?”
“너무 복잡하게 들리 네요.
 다시 지킬을 쓰자.”
"왜?!
 모든 페이지를 SPA로 제공하는 것은 멋진 2021 년입니다. "
 

아마도 당신은 이들 중 하나와 동일시하는 경향이 있습니다.
 개인적으로 문제에 따라 후자의 두 가지 중 하나를 선택합니다.
 물론 React로 서버 측 렌더링 (SSR)을 꽤 많이했지만 추가 복잡성이 그만한 가치가 없다는 사실을 자주 발견했습니다.
 

마찬가지로, 나는 실제로 개츠비를 싫어하는 몇 안되는 사람 중 하나 일 수 있습니다.
 적어도 제게는 거의 모든 것을 지나치게 복잡하게 만들었고 많은 이득을 보지 못했습니다.
 

그래서 이건가요?
 물론 다른 방법이 있습니다.
 응용 프로그램을 견고한 아키텍처에 배치하면 약간의 스크립트를 작성하고 실제로 SSG (static site generation)를 직접 수행 할 수 있습니다. Gatsby 나 다른 어떤 것도 필요하지 않습니다.
 이것은 정교하지 않을 것입니다.
 그럼에도 불구하고 우리는 좋은 이득을보아야합니다.
 

이제 우리는 여기서 실제로 무엇을 기대하고 있습니까?
 

## 올바른 기대치 설정
 

이 게시물에서는 React를 사용하여 만든 페이지를 완전히 사전 생성 된 정적 사이트 세트로 변환하는 간단한 솔루션을 구축 할 것입니다.
 우리는 여전히 이것을 수화하고 우리의 사이트를 동적으로 유지할 수 있습니다.
 

우리의 목표는 초기 렌더링 성능을 향상시키는 것이 었습니다.
 Lighthouse 테스트에서 우리는 우리 자신의 홈페이지가 우리가 바라는 것처럼 항상 잘 인식되지 않는 것을 보았습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Lighthouse-Test-Rendering-Performance.png?resize=500%2C119&ssl=1)

우리가하지 않을 것은 정적 페이지를 최적화하여 JavaScript의 아주 작은 조각 만 가지도록하는 것입니다.
 우리는 항상 완전한 JavaScript로 페이지를 수화 할 것입니다 (여전히 지연로드 될 수 있지만 나중에 더 자세히 설명합니다).
 

SPA의 정적 사전 렌더링이인지 된 성능에 도움이 될 수 있지만 성능 최적화에 초점을 맞추지는 않습니다.
 성능 최적화에 관심이있는 경우 웹팩을 사용한 성능 최적화에 대한이 심층 가이드를 참조해야합니다.
 

## 기본 React 애플리케이션
 

이 기사의 상용구로서 우리는 상당히 단순하지만 매우 일반적인 종류의 React 애플리케이션을 사용합니다.
 많은 개발 종속성을 설치합니다 (예, 트랜스 파일 목적으로 TypeScript를 사용합니다).
 

```coffeescript
npm i webpack webpack-dev-server webpack-cli typescript ts-loader file-loader html-webpack-plugin @types/react @types/react-dom @types/react-router @types/react-router-dom --save-dev
```

물론 일부 런타임 종속성은 다음과 같습니다.
 

```coffeescript
npm i react react-dom react-router-dom react-router --save
```

이제 애플리케이션을 번들링하기 위해 적절한`webpack.config.js`를 설정했습니다.
 

```coffeescript
module.exports = {
  mode: 'production',
  devtool: 'source-map',
  entry: './src/index.tsx',
  output: {
    filename: 'app.js',
  },
  resolve: {
    extensions: ['.ts', '.tsx', '.js'],
  },
  module: {
    rules: [
      { test: /\.tsx?$/, loader: 'ts-loader' },
      { test: /\.(png|jpe?g|gif)$/i, loader: 'file-loader' },
    ],
  },
};
```

이것은 TypeScript 및 파일을 자산으로 지원합니다.
 더 큰 웹 앱의 경우 다른 많은 것이 필요할 수 있지만 데모에는 충분합니다.
 

이 문제가 해결되는 것을 확인하려면 약간의 콘텐츠가있는 페이지도 추가해야합니다.
 구조에서는`index.tsx` 파일을 사용하여 모든 항목을 집계합니다.
 이 파일은 다음과 같이 간단 할 수 있습니다.
 

```coffeescript
import * as React from 'react';
import { BrowserRouter, Route, Switch } from 'react-router-dom';
import { render } from 'react-dom';

const HomePage = React.lazy(() => import('./pages/home'));
const FirstPage = React.lazy(() => import('./pages/first'));
const NotFoundPage = React.lazy(() => import('./pages/not-found'));

const App = () => (
  <BrowserRouter>
    <Switch>
        <Route path="/" exact component={HomePage} />
        <Route path="/first" exact component={FirstPage} />
        <Route component={NotFoundPage} />
    </Switch>
  </BrowserRouter>
);

render(<App />, document.querySelector('#app'));
```

이 접근 방식의 문제점은 모든 새 페이지에 대해이 파일을 편집해야한다는 것입니다.
 `src / pages` 디렉토리에 페이지를 저장하는 규칙을 사용하면 더 나은 것을 찾을 수 있습니다.
 두 가지 옵션이 있습니다.
 

- webpack의`require.context` 마법을 사용하여 디렉토리를 "읽고"사용할 수있는 동적 목록을 가져옵니다.
 
- 지연 로딩 및 경로를 제공하는 특수 컴파일 시간 생성 모듈을 사용하십시오.
 

두 번째 접근 방식은 경로가 페이지의 일부가 될 수 있다는 큰 이점을 제공합니다.
 이 방법을 사용하려면 다른 웹팩 로더가 필요합니다.
 

```coffeescript
npm i parcel-codegen-loader --save-dev
```

이제`index.tsx` 파일을 다음과 같이 리팩터링 할 수 있습니다.
 

```coffeescript
import * as React from 'react';
import { BrowserRouter, Route, Switch } from 'react-router-dom';
import { render } from 'react-dom';
import Layout from './Layout';

const pages = require('./toc.codegen');
const [notFound] = pages.filter((m) => m.route === '*');
const standardPages = pages.filter((m) => m !== notFound);

const App = () => (
  <BrowserRouter>
    <Layout>
      <Switch>
        {standardPages.map((page) => (
          <Route key={page.route} path={page.route} exact component={page.content} />
        ))}
        {notFound && <Route component={notFound.content} />}
      </Switch>
    </Layout>
  </BrowserRouter>
);

render(<App />, document.querySelector('#app'));
```

이제 모든 페이지는 번들링 중에 즉석에서 생성되는 모듈 인`toc.codegen`에 의해 완전히 결정됩니다.
 또한 페이지에 약간의 공유 구조를 제공하기 위해`Layout` 구성 요소를 추가했습니다.
 

`toc` 모듈의 코드 생성기는 다음과 같습니다.
 

```coffeescript
const { getPages } = require('./helpers');

module.exports = () => {
  const pageDetails = getPages();
  const pages = pageDetails.map((page) => {
    const meta = [
      `"content": lazy(() => import('./${page.folder}/${page.name}'))`,
      `${JSON.stringify('route')}: ${JSON.stringify(page.route)}`,
    ];

    return `{ ${meta.join(', ')} }`;
  });

  return `const { lazy } = require('react');
module.exports = [${pages.join(', ')}];`;
};
```

모든 페이지를 반복하고`route` 및`content` 속성이있는 객체 배열을 내보내는 새 모듈을 생성합니다.
 

이 시점에서 우리의 목표는이 간단한 (아직 완성 된) 애플리케이션을 사전 렌더링하는 것입니다.
 

## React를 사용한 SSG의 기본
 

React와 SSR을 알고 있다면 기본적인 SSG를 수행하는 데 필요한 모든 것을 이미 알고 있습니다.
 핵심은 `react-dom`의 `render`대신 `react-dom / server`의 `renderToString`함수를 사용합니다.
 레이아웃에 중첩 된 페이지가 있다고 가정하면 다음 코드가 이미 작동 할 수 있습니다.
 

```js
const element = (
  <MemoryRouter>
    <Layout>
       <Page />
    </Layout>
  </MemoryRouter>
);
const content = renderToString(element);
```

위의 스 니펫에서는 레이아웃이 `Layout`구성 요소에 완전히 제공된다고 가정합니다.
 또한 React Router가 클라이언트 측 라우팅에 사용된다고 가정했습니다.
 따라서 적절한 라우팅 컨텍스트를 제공해야합니다.
 다행히도 우리가`HashRouter`,`BrowserRouter` 또는`MemoryRouter`를 사용하는지 여부는 실제로 중요하지 않습니다. 모두 라우팅 컨텍스트를 제공합니다.
 

자, `페이지`란?
 `페이지`는 사전 렌더링 할 페이지를 실제로 표시하는 구성 요소를 의미합니다.
 모든 페이지에 가져오고 싶은 구성 요소가 더있을 수 있습니다.
 좋은 예는 일반적으로 다음과 같이 통합되는`ScrollToTop` 구성 요소입니다.
 

```xml
<Route component={ScrollToTop} />
```

그러나 이것은 정적 페이지가 동적이 될 때 수분 공급에 맡겨야 할 구성 요소입니다.
 이를 미리 렌더링 할 필요가 없습니다.
 

그렇지 않으면 응용 프로그램 자체는 일반적으로 다음과 유사합니다.
 

```xml
const app = (
  <BrowserRouter>
    <Route component={ScrollToTop} />
    <Layout>
      <Switch>
        {pages
          .map(page => (
            <Route exact key={page.route} path={page.route} component={page.content} />
          ))}
        <Route component={NotFound} />
      </Switch>
    </Layout>
  </BrowserRouter>
);

hydrate(app, document.querySelector('#app'));
```

이것은 정적 생성을위한 스 니펫에 매우 가깝습니다.
 가장 큰 차이점은 여기에 모든 페이지 (및 하이드레이트)를 포함하는 반면 위의 SSG 시나리오에서는 전체 페이지 콘텐츠를 고정 된 단일 페이지로 축소한다는 것입니다.
 

## SSG의 함정
 

글쎄, 지금까지 모든 것이 간단하고 쉽게 들리 죠?
 그러나 문제는 내부에 있습니다.
 

### 자산 지원
 

자산을 어떻게 참조합니까?
 다음과 같은 코드를 사용한다고 가정 해 보겠습니다.
 

```xml
<img src="/foo.png" />
```

이것은 우리 서버에`foo.png`가 존재한다면 작동 할 수 있습니다.
 전체 URL을 사용하는 것이 약간 더 안정적입니다 (예 :`http : // example.com / foo.png`).
 그러나 환경에 대한 유연성을 포기합니다.
 

어차피 웹팩과 같은 번 들러에 이러한 자산을 맡기는 경우가 자주 있습니다.
 이 경우 다음과 같은 코드가 있습니다.
 

```coffeescript
<img src={require('../assets/foo.png')} />
```

여기서`foo.png`는 빌드시 로컬로 확인 된 다음 해시, 최적화 및 대상 디렉터리로 복사됩니다.
 괜찮습니다.
 그러나 위에서 설명한 프로세스의 경우 Node.js의 모듈 만 요구하면 작동하지 않습니다.
 `foo.png`가 유효한 모듈이 아니라는 예외가 발생합니다.
 

이것을 해결하는 것은 실제로 복잡하지 않습니다.
 다행히 Node.js의 모듈에 대한 추가 확장을 등록 할 수 있습니다.
 

```js
['.png', '.svg', '.jpg', '.jpeg', '.mp4', '.mp3', '.woff', '.tiff', '.tif', '.xml'].forEach(extension => {
  require.extensions[extension] = (module, file) => {
    module.exports = '/' + basename(file);
  };
});
```

위의 코드는 파일이 이미 루트 디렉토리에 복사 될 것이라고 가정합니다.
 따라서`require ( `../ assets / foo.png`)`를` "foo.png"`로 변환합니다.
 

해싱 부분은 어떻습니까?
 해시가있는 자산 파일 목록이 이미있는 경우 다음과 같은 코드로 이동할 수 있습니다.
 

```undefined
const parts = basename(file).split('.');
const ext = parts.pop();
const front = parts.join('.');
const ref = files.filter(m => m.startsWith(front) && m.endsWith(ext)).pop() || '';
module.exports = '/' + ref;
```

이렇게하면 `foo.23fa6b.png`와 같이 `foo`로 시작하고 `png`로 끝나는 파일을 찾습니다.
 위와 같이 쉬십시오.
 

### TypeScript 지원
 

이제 일반 자산을 처리하는 방법을 알았으므로`.ts` 및`.tsx` 파일에 대한 처리 메커니즘도 제공 할 수 있습니다.
 결국 이것들은 메모리에서도 JS로 트랜스 파일 될 수 있습니다.
 그러나 `ts-node`가 이미 그렇게하고 있기 때문에이 모든 작업은 매우 불필요합니다.
 따라서 우리의 작업은 본질적으로 다음과 같이 축소됩니다.
 

```coffeescript
npm i ts-node --save-dev
```

그런 다음 다음을 사용하여 각 파일 확장자에 대한 핸들러를 등록합니다.
 

```undefined
require('ts-node').register({
  compilerOptions: {
    module: 'commonjs',
    target: 'es6',
    jsx: 'react',
    importHelpers: true,
    moduleResolution: 'node',
  },
  transpileOnly: true,
});
```

이제 이것은`.tsx` 파일로 정의 된 모듈 만 허용합니다.
 

### 지연 로딩
 

최적화 된 React SPA를 작성했다면 라우팅 수준에서 분할 된 애플리케이션도 번들로 제공합니다.
 즉, 모든 페이지는 자체 하위 번들을 갖습니다.
 따라서 코어 / 공통 번들 (일반적으로 라우팅 메커니즘, React 자체, 일부 공유 종속성 등을 포함)과 각 페이지에 대한 번들이 있습니다.
 페이지가 10 개라면 11 개 (또는 그 이상)의 번들이됩니다.
 

그러나 사전 렌더링 접근 방식을 사용하면 이것은 이상적이지 않습니다.
 결국, 우리는 이미 개별 페이지를 미리 렌더링하고 있습니다.
 페이지에 공유 할 하위 구성 요소 (예 :지도 컨트롤)가 있지만 너무 커서 자체 번들에 포함하고 싶은 경우에는 어떻게하나요?
 

즉, 다음과 같은 구성 요소가 있습니다.
 

```js
const ActualMap = React.lazy(() => import('./actual-map'));

const Map = (props) => (
  <React.Suspense fallback={<div>Loading Map ...</div>}>
     <ActualMap {...props} />
  </React.Suspense>
);
```

실제 구성 요소가 지연로드되는 위치입니다.
 이 시나리오에서 우리는 상당히 과감한 벽에 부딪 힐 것입니다.
 React는`lazy`를 사전 렌더링하는 방법을 모릅니다.
 

운 좋게도 `lazy`를 재정 의하여 일부 자리 표시 자만 표시 할 수 있습니다 (여기에서 미쳐서 실제로 콘텐츠를로드하거나 미리 렌더링 할 수도 있지만 간단하게 유지하고 여기에서 지연로드되는 모든 항목이 실제로 지연로드되어야한다고 가정하겠습니다.
 나중에):
 

```coffeescript
React.lazy = () => () => React.createElement('div', undefined, 'Loading ...');
```

`renderToString`에서 사용할 수없는 다른 부분은`Suspense`입니다.
 음, 솔직히 이것의 구현은 해가되지 않을 것입니다.하지만 우리 스스로 해봅시다.
 

여기서 우리가해야 할 일은 `서스펜스`가 전혀없는 것처럼 행동하는 것입니다.
 따라서 제공된`children`을 사용하여 조각으로 교체합니다.
 

```coffeescript
React.Suspense = ({ children }) => React.createElement(React.Fragment, undefined, children);
```

이제 우리는 시나리오에 따라 그보다 훨씬 더 많은 작업을 수행 할 수 있지만 (심지어 원할 수도 있음) 지연로드를 매우 효율적으로 처리합니다.
 

### 다른 것들
 verified_user

`lazy`및 `Suspense`와 같은 것 외에 다른 부분도 React에서 누락 될 수 있습니다.
 좋은 예가`useLayoutEffect`입니다.
 그러나`useLayoutEffect`는 일반적으로 런타임에만 적용되므로이를 지원하는 쉬운 방법은 아예 피하는 것입니다.
 

아주 간단한 구현은 no-op 함수로 대체하는 것입니다.
 이런 식으로`useLayoutEffect`는 우리 방식을 방해하지 않으며 단순히 무시됩니다.
 

```coffeescript
React.useLayoutEffect = () => {};
```

우리가 `필요한`모듈 중 일부는 실제로 CommonJS 형식이 아닙니다.
 이것은 큰 문제입니다.
 작성 당시 Node.js는 CommonJS 모듈 만 지원합니다 (ES 모듈은 아직 실험 중입니다).
 

운 좋게도 `import ...`및 `export ...`문을 사용하여 ES 모듈을 지원하는 esm 패키지가 있습니다.
 큰!
 

`ts-node`와 마찬가지로 종속성을 설치하여 시작합니다.
 

```coffeescript
npm i esm --save-dev
```

그리고 실제로 사용합니다.
 이 경우 "normal" `require`를 새 버전으로 교체해야합니다.
 코드는 다음과 같습니다.
 

```undefined
require = require('esm')(module);
```

이제 우리는 ES 모듈도 지원합니다.
 누락 된 유일한 것은 일부 DOM 관련 기능 일 수 있습니다.
 

대부분의 경우 코드에 다음과 같이 조건문을 넣어야합니다.
 

```js
if (typeof window !== 'undefined') {
  // ...
}
```

우리는 또한 다른 글로벌을 위조 할 수도 있습니다.
 예를 들어, 우리 (또는 직접 또는 간접적으로 사용하는 일부 종속성)는`XMLHttpRequest`,`XDomainRequest` 또는`localStorage`를 참조 할 수 있습니다.
 이 경우`global` 객체를 확장하여 조롱 할 수 있습니다.
 

같은 논리로 `문서`또는 적어도 일부를 조롱 할 수 있습니다.
 물론 이미 대부분을 도와주는`jsdom` 패키지가 있습니다.
 하지만 지금은 간단하고 요점 만 설명하겠습니다.
조롱의 예 :
 

```coffeescript
global.XMLHttpRequest = class {};
global.XDomainRequest = class {};
global.localStorage = {
  getItem() {
    return undefined;
  },
  setItem() {},
};
global.document = {
  title: 'sample',
  querySelector() {
    return {
      getAttribute() {
        return '';
      },
    };
  },
};
```

이제 큰 문제없이 애플리케이션을 사전 렌더링 할 수있는 모든 것이 준비되었습니다.
 

## 모두 함께 가져 오기
 

위의 스 니펫을 일반화하고 사용할 수있는 방법에는 여러 가지가 있습니다.
 개인적으로 나는 `fork`를 사용하여 페이지별로 평가 / 사용되는 별도의 모듈에서 사용하는 것을 좋아합니다.
 이런 식으로 우리는 항상 병렬화 될 수있는 격리 된 프로세스를 얻습니다.
 

다음은 구현의 예입니다.
 

```js
const path = require('path');
const { readFileSync, readdirSync } = require('fs');
const { fork } = require('child_process');
const { getPages } = require('./files');

function generatePage(page, files, html, dist) {
  const modPath = path.resolve(__dirname, 'ssg-kernel.js');

  console.log(`Processing page "${page.name}" ...`);

  return new Promise((resolve, reject) => {
    const ps = fork(modPath, [], {
      cwd: process.cwd(),
      stdio: 'ignore',
      detached: true,
    });

    ps.on('message', () => {
      console.log(`Finished processing page "${page.name}".`);
      resolve();
    });

    ps.on('error', () => reject(`Failed to process page "${page.name}".`));

    ps.send({
      source: page.path,
      target: page.route,
      files,
      html,
      dist,
    });
  });
}

function generatePages() {
  const dist = path.resolve(__dirname, 'dist');
  const index = path.resolve(dist, 'index.html');
  const files = readdirSync(dist);
  const html = readFileSync(index, 'utf8');
  const baseDir = path.resolve(__dirname, '..');
  const pages = getPages();
  return Promise.all(pages.map(page => generatePage(page, files, html, dist)));
}

if (require.main === module) {
  generatePages()
    .then(
      () => 0,
      () => 1,
    )
    .then(process.exit);
} else {
  module.exports = {
    generatePage,
    generatePages,
  };
}
```

이 구현은 `node`를 통해 직접 lib로 사용될 수 있습니다.
 이것이 바로 `require.main`을 판별 자로 사용하여 두 경우를 구분하는 이유입니다.
 lib의 경우`generatePages`와`generatePage` 두 함수를 내 보냅니다.
 

여기에 남은 것은`getPages`의 정의뿐입니다.
 이를 위해 우리는 기본 React 애플리케이션에 대한 소개 섹션에서 지정한 정의를 사용할 수 있습니다.
 

`ssg-kernel.js` 모듈은 위의 코드를 포함합니다.
 포크 프로세스에 사용하기 위해 적절한 봉투에 포장하면 다음과 같이됩니다.
 

```js
// ...

process.on('message', msg => {
  const { source, target, files, html, dist } = msg;
  setupExtensions(files);

  setTimeout(() => {
    const { content, outPath } = renderApp(source, target, dist);
    makePage(outPath, html, content);

    process.send({
      content,
    });
  }, 100);
});
```

`setupExtensions`는`require`에 필요한 수정을 설정하는 반면`renderApp`은 실제 마크 업 생성을 수행합니다.
 `makePage`는`renderApp`의 출력을 사용하여 생성 된 페이지를 실제로 작성합니다.
 

이러한 설정을 사용하여 Lighthouse에서 확인한대로 웹 사이트를 사전 렌더링하고 상당한 성능 향상을 얻을 수 있습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Prerendered-Lighthouse-Performance-Boost.png?resize=600%2C169&ssl=1)

좋은 부작용으로, 우리 웹 사이트는 자바 스크립트가없는 사용자를 위해 어느 정도 작동합니다.
 전체 예제 애플리케이션은 GitHub에서 찾을 수 있습니다.
 

## 결론
 

훌륭한 정적 페이지를 만들기 위해 React를 사용하는 것은 실제로 큰 문제가 아닙니다.
 부풀고 복잡한 프레임 워크로 돌아갈 필요가 없습니다.
 이런 식으로 우리는 들어가는 것과 나가야하는 것을 아주 잘 제어 할 수 있습니다.
 

또한 Node.js, React의 내부 및 잠재적으로 우리가 사용하는 일부 종속성에 대해 상당히 많이 배웁니다.
 우리가 실제로 사용하는 것을 아는 것은 단순한 학문적 연습 이상이지만 버그 나 기타 문제가 발생하는 경우 매우 중요합니다.
 

이 기술을 사용하여 SPA를 미리 렌더링하면 성능이 향상 될 수 있습니다.
 더 중요한 것은, 우리 페이지가 더 쉽게 접근 할 수있게되고 SEO 목적으로 더 높은 순위가 매겨진다는 것입니다.
 

사전 렌더링이 빛나는 곳은 어디입니까?
 이것은 무의미한 연습 일 뿐입니 까? 아니면 React가 상호 작용없이 재사용 가능한 구성 요소를 작성하는 데 좋은 개발 모델이 될 수 있습니까?
 