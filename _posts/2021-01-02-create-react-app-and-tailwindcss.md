---
layout: post
title: "React에서 Tailwind CSS를 사용하여 create-react-app을 구성하는 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/07/tailwindcsscra.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/tailwindcsscra.png?fit=730%2C412&ssl=1)

최근에 CRA(create-react-app) 보일러 플레이트에 갇힌 리액트 프로젝트 부트에서 Tailwind CSS를 사용하려 했는데 CRA 추상화 구성으로 Tailwind CSS를 설정하는 데 어려움이 있었다. 사용자 지정 구성을 만들려면 CRA가 구성을 완전히 사용할 수 있도록 `배출`해야 하는데, 이는 훨씬 더 지루한 설정을 의미하기도 한다. 그리고 만약 어떤 것이라도 깨진다면... 당신은 당신 스스로 할 수 있습니다. 나는 그것을 약간 얼버무려 더 나은 방법을 찾았다.

이 게시물에서는 창조-리액트-앱을 꺼낼 필요 없이 당신의 리액트 프로젝트 안에서 Tailwind CSS가 작동하도록 하는 쉬운 방법을 보여드리겠습니다.

## 필수 구성 요소:

- hpm 작동 방법에 대한 지식
- Node.js 8.0 이상 및 npm 5.2 이상이 설치되어 있어야 합니다.
- 리액트 및 테일윈드 CSS의 기본 이해

## 시작 중

먼저 터미널을 열고 다음 명령을 입력하여 새 프로젝트를 만듭니다.

```shell
#Using NPM
$ npx create-react-app tailwindreact-app

#Using Yarn
$ yarn create react-app tailwindreact-app
```

이 부팅은 필요한 모든 구성 및 빌드 파이프라인(웹 팩, Babel)과 함께 새 Relect 앱을 트랩합니다.

앱 디렉토리에 `cd`를 입력합니다.

```bash
cd tailwindreact-app
```

다음으로 Tailwind를 설치합니다.

```shell
# Using npm
npm install tailwindcss --save-dev

# Using Yarn
yarn add tailwindcss --dev
```

기본 구성 비계 생성

```css
npx tailwind init tailwind.js --full
```

이 명령은 프로젝트의 기본 디렉터리에 `tailwind.js`를 생성하며, 파일에는 Tailwind의 모든 기본 구성이 들어 있습니다.

다음과 같이 Autopfixer 및 PostCSS-CLI를 설치합니다.

```undefined
npm install postcss-cli autoprefixer --save-dev
or
yarn add postcss-cli autoprefixer --save-dev
```

PostCSS 설명서에 명시된 대로:

PostCSS는 JS 플러그인으로 스타일을 변환하기 위한 도구이다. 이러한 플러그인은 CSS, 지원 변수 및 믹신, 향후 CSS 구문, 인라인 이미지 등을 전송할 수 있습니다.

Autoprefixer는 PostCSS 플러그인이지만 기본적으로 CSS를 구문 분석하고 컴파일된 CSS 규칙에서 불필요한 벤더 접두사를 추가/제거합니다. 또한 애니메이션, 전환, 변환, 그리드, Flex, Flexbox 등의 접두사를 추가할 수 있습니다.

## PostCSS를 구성하는 방법

수동으로 또는 다음 명령을 사용하여 기본 디렉터리에 PostCSS 구성 파일을 생성합니다.

```shell
$ touch postcss.config.js
```

PostCSS 파일에 다음 줄의 코드를 추가합니다.

```js
//postcss.config.js
 const tailwindcss = require('tailwindcss');
 module.exports = {
     plugins: [
         tailwindcss('./tailwind.js'),
         require('autoprefixer'),
     ],
 };
```

`src` 폴더 내에서 폴더를 만들고 이름을 `스타일`로 지정합니다. 여기서 모든 스타일을 저장할 수 있습니다. 이 폴더 내에 `tailwind.css`와 `index.css` 파일을 생성하십시오.

index.css 파일은 우리가 tailwind.css의 기본 스타일과 구성을 가져오는 반면 tailwind.css는 index.css의 컴파일된 출력을 포함할 것이다.

## 애플리케이션에 순풍 구성 요소, 유틸리티 및 기본 스타일을 주입하는 방법

다음을 `index.css` 파일에 추가하십시오.

```java
//index.css

@tailwind base;

@tailwind components;

@tailwind utilities;
```

@tailwind는 기본 스타일, 구성 요소, 유틸리티, 맞춤형 구성을 주입하는 데 사용되는 Tailwind 지시어다.

@tailwind base—이것은 Normalize.css와 몇몇 추가적인 베이스 스타일을 결합한 Tailwind의 베이스 스타일을 주입한다.

비행 전 적용되는 모든 스타일에 대한 전체 참조는 이 스타일시트를 참조하십시오.

`postcs-import`를 사용하는 경우 이 가져오기를 대신 사용하십시오.

```css
@import "tailwindcss/base";
```

`@tailwind components` — 테일wind 구성 파일에 정의된 플러그인으로 등록된 모든 구성요소(버튼 및 폼 요소 등 재사용 가능한 작은 스타일) 클래스를 삽입합니다.

`postcs-import`를 사용하는 경우 이 가져오기를 대신 사용하십시오.

`@import "tailwindcs/components";

구성 요소 가져오기 아래에는 사용자 정의 구성 요소 클래스를 추가할 수 있습니다. 기본 유틸리티보다 먼저 로드할 항목이 있으므로 유틸리티가 해당 클래스를 재정의할 수 있습니다.

예는 다음과 같습니다.

```undefined
.btn { ... }
.form-input { ... }
```

또는 전처리기 또는 `postcs-import`를 사용하는 경우:

```css
@import "components/buttons";
@import "components/forms";
```

`@tailwind 유틸리티` — 구성 파일을 기반으로 생성되는 모든 유틸리티 클래스(기본 유틸리티 및 사용자 유틸리티 포함)를 주입합니다.

postcs-import를 사용하는 경우 이 가져오기를 대신 사용하십시오.

```css
@import "tailwindcss/utilities";
```

유틸리티 가져오기 아래에는 Tailwind와 함께 개봉하지 않는 사용자 지정 유틸리티가 추가되어 있습니다.

다음은 예입니다.

```bash
.bg-pattern-graph-paper { ... }

.skew-45 { ... }
```

또는 전처리기 또는 `postcs-import`를 사용하는 경우:

```css
@import "utilities/background-patterns";
@import "utilities/skew-transforms";
```

Tailwind는 제조 시 이 모든 지시사항을 교환하고 CSS 생성으로 대체할 것이다.

## CSS 파일을 빌드하도록 React 앱을 구성하는 방법

npm start 또는 yarn start 명령을 실행할 때마다 스타일을 작성하도록 앱을 구성합니다.

패키지를 여십시오.json은 다음과 같이 파일링하고 "put"의 내용을 대체한다.

```bash
"scripts": {
  "build:style":"tailwind build src/styles/index.css -o src/styles/tailwind.css",
  "start": "npm run build:style && react-scripts start",
  "build": "react-scripts build",
  "test": "react-scripts test",
  "eject": "react-scripts eject"
},
```

## Tailwind CSS 스타일을 앱으로 가져오는 방법

`index.js` 파일을 열고 순풍 스타일을 가져옵니다.

```coffeescript
import './styles/tailwind.css';
```

프로젝트 루트 디렉토리에서 `index.css` 및 `app.css` 파일을 삭제하고 각각 `Index.js` 및 `App.js` 파일에서 해당 가져오기 문을 제거하십시오.

`index.js` 파일은 다음과 유사하게 표시되어야 합니다.

```js
// index.js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';
  .......
```

삭제 후에는 다음이 됩니다.

```js
//index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import * as serviceWorker from './serviceWorker';
```

삭제하기 전에 `App.js` 파일은 다음과 같아야 합니다.

```js
//App.js
import React from 'react';
import logo from './logo.svg';
import './App.css';
```

삭제 후에는 다음이 됩니다.

```js
//App.js
import React from 'react';
import logo from './logo.svg';
```

이러한 변경으로 인해 다음과 유사한 출력이 발생합니다.

구성이 올바르게 작동하는지 테스트하기 위해 간단한 로그인 양식을 만들어 보겠습니다.

`App.js` 파일을 열고 반환 기능 사이의 내용을 다음으로 교체합니다.

```xml
<div className="App" >
    <div className="w-full max-w-md bg-gray-800" >
      <form action="" className=" bg-white shadow-md rounded px-8 py-8 pt-8">
        <div className="px-4 pb-4">
          <label htmlFor="email" className="text-sm block font-bold  pb-2">EMAIL ADDRESS</label>
          <input type="email" name="email" id="" className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline border-blue-300 " placeholder="Johnbull@example.com"/>
        </div>
        <div  className="px-4 pb-4">
          <label htmlFor="password" className="text-sm block font-bold pb-2">PASSWORD</label>
          <input type="password" name="email" id="" className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline border-blue-300" placeholder="Enter your password"/>
        </div>
        <div>
          <button className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button">Sign In</button>
        </div>
      </form>
    </div>
  </div>
```

방금 우리가 한 것은 div에 w-full과 100%의 폭을 주고, max-w-md 이상의 화면도 최대 폭을 설정했다.

우리는 bg-화이트로 흰색 바탕을 주고 테두리 반경을 부여해 경계선(border), px-8(py-8), py-8(py-8)에 각각 x축, y축에 8px의 패딩(padding)을 더하고 pt-8(p8)은 형태 상단에 8px의 패딩을 추가했다.

우리는 라벨 요소에 텍스트-sm으로 8875rem의 폰트 사이즈를 추가하고 요소를 블록의 디스플레이로 만들고 폰트 가중치를 글꼴-볼드의 700으로 설정했다.

입력 요소에서 우리는 요소에게 `그림자`가 있는 상자 그림자를 주고 입력 요소에 대한 브라우저별 스타일링을 재설정하기 위해 `.expecture-none`을 사용했다.

우리는 `leading-tight`로 `1.25`의 `line high`를 추가하고 의사급 `focus`를 사용하여 `focus:outline-none`으로 중점 입력 요소의 브라우저별 개요를 제거하고 `focus:shadow-outline`으로 상자 그림자를 약간 추가했다.

당신은 이와 비슷한 결과를 얻어야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/form.png?resize=567%2C380&ssl=1)

=span data-mce-type="style=" 표시: 인라인 블록, 너비: 0px, 오버플로: 숨김, 선 높이: 0;"class="mce_SELRES_start"><span>

## 결론

이 게시물에서는 Tailwind CSS를 사용하도록 create-react-app을 구성하는 방법을 배웠습니다. 테일윈드에는 멋진 설명서가 있으니 자세한 내용은 확인해 보세요. 또한 GitHub에서 이 튜토리얼의 저장소를 체크 아웃하여 앱의 비계를 만들 수 있습니다.