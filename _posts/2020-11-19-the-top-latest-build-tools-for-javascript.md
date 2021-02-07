---
layout: post
title: "JavaScript용 최신 빌드 도구"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/js-build-tools-parcel-snowpack.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/js-build-tools-parcel-snowpack.png?fit=730%2C487&ssl=1)

## 도입

지난 수년간 Google Closure Tools 시대부터 현재까지 다양한 JavaScript 빌드 도구가 등장했습니다. 특히 빌드 시간, 속도, 사용자 지정, 구성 및 확장성과 관련하여 JavaScript 빌드 도구가 크게 향상되었습니다.

이 기사에서는 JavaScript 에코시스템에서 사용하는 최신 빌드 도구에 대해 살펴보겠습니다.

## 전제조건

이 튜토리얼의 경우 다음이 필요합니다.

- Node.js 10x 이상
- PC에 설치된 Yarn / npm 5.2 이상
- JavaScript에 대한 기본 지식 및 빌드 도구의 작동 방식

## 롤업.js

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/rollup.js.png?resize=730%2C476&ssl=1)

공식 웹사이트에 따르면, "롤업은 작은 코드 조각들을 라이브러리나 응용 프로그램과 같이 더 크고 복잡한 것으로 컴파일하는 자바스크립트용 모듈 번들러이다." 2018년 12월 첫 버전이 출시됐지만 2019년 후반까지 인기를 끌지 못했다.

현재 버전 2에서는 롤업이 JavaScript 패키지 및 모듈을 구축하는 표준이 되었습니다. 전 세계 여러 기관과 개인 개발자들이 채택하고 있습니다.

### 롤업.js 사용 찬성

다음은 롤업이 JavaScript 개발자에게 제공하는 몇 가지 멋진 기능입니다.

- 빠른 빌드
- 쉽게 학습하고 시작할 수 있을 뿐만 아니라 JavaScript 패키지를 게시할 수 있습니다.
- 코드 분할
- 웹 팩에 비해 구성이 더 작고 간편함
- JavaScript 라이브러리에 적합합니다.
- 사용자 지정 플러그인 지원과 함께 더 나은 빌드 및 사용자 지정을 위한 플러그인
- 더 작은 번들 크기를 제공하고 클린 코드를 생성합니다.
- ES 모듈에 대한 놀라운 지원 제공

### Rollowing.js 사용에 대한 반대 사항

- 비동기/대기 시 즉시 지원 없음

### Rollowing.js를 시작하는 중

롤업을 시작하려면 다음 명령을 실행하여 시스템에 패키지를 설치해야 합니다.

```coffeescript
npm install --global rollup
```

이 명령은 롤업을 전체적으로 설치하므로 다음에 필요할 때 컴퓨터에 롤업을 다시 설치할 필요가 없습니다.

필요한 경우 다음 명령을 실행하여 시스템의 특정 프로젝트에 대해 로컬로 설치할 수 있습니다.

npm

```undefined
npm install rollup --save-dev
```

실을 잣다

```undefined
yarn -D add rollup
```

롤업을 전체적으로 설치한 경우 개발 환경에 따라 다음 명령을 실행하여 롤업을 빨리 시작할 수 있습니다.

브라우저의 경우:

```css
rollup <your application entry point> --file bundle.js --format iife
```

이 명령은 애플리케이션 항목 파일(예: main.js 또는 script.js)의 코드를 컴파일하여 해당 코드를 단일 파일(bundle.js)로 가져온 다음 스크립트 태그의 IIFE(Immediate Imported Function Expression)로 브라우저에 제공합니다.

Node.js의 경우:

```css
rollup <your application entry point> --file bundle.js --format cjs
```

이 명령은 공통의 응용 프로그램 입력 파일에 코드를 컴파일합니다.JS 형식(예: main.js 또는 script.js)은 Node.js에 의해 인식되고 실행될 수 있도록 단일 파일(bundle.js)로 가져오는 것을 포함한다.

브라우저 및 Node.js의 경우 모두:

```undefined
rollup <your application entry point> --file bundle.js --format umd --name "myBundle"
```

이렇게 하면 UMD 형식의 응용 프로그램 코드가 브라우저와 Node.js 모두에 대해 단일 파일(bundle.js)로 컴파일됩니다.

롤업을 로컬로 설치한 경우 프로젝트의 루트 폴더에서 다음 명령을 실행하여 롤업을 시작할 수 있습니다.

npm:

```undefined
npx rollup --config
```

실:

```undefined
yarn rollup --config
```

### 마감사상

롤업은 현재 사용 가능한 다른 빌드 도구처럼 완전히 새로운 것은 아니지만 많은 JavaScript 개발자가 이를 채택했습니다. 자바스크립트 패키지나 라이브러리를 위한 빌드 도구나 번들이 필요하다면 롤업.js를 사용하는 것이 최선이라고 생각합니다.

프로젝트에서 롤업.js를 사용하는 방법에 대한 자세한 내용은 여기서 자습서와 공식 설명서를 참조하십시오.

## 소포

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/parcel.png?resize=730%2C472&ssl=1)

Parcel은 생태계에서 가장 최신의 자바스크립트 번들러 중 하나이다. 공식 문서에서는 이를 "개발자 경험으로 차별화된 웹 애플리케이션 번들러"라고 설명한다.

2019년에 출시된 소포체는 속도가 빠르며 롤업.js 및 웹팩과는 달리 아무런 구성도 필요하지 않다. 기본적으로 플러그 앤 플레이 번들로, 이 게시 당시 버전 2, 베타 1에 포함되어 있습니다.

구획은 프로젝트 파일과 종속성을 모두 가져와서 변환한 다음 코드를 실행하는 데 사용할 수 있는 더 작은 파일 조각으로 병합하여 코드를 묶습니다.

v2 릴리스의 설명서에서는 "언어 또는 툴 체인에 관계없이 모든 코드를 위한 컴파일러"라고 한다.

### 구획 사용 찬성

- 엄청나게 빠른 빌드
- 훌륭한 개발자 경험
- 제로 구성 - 설치 및 시작만 하면 됩니다.
- 플러그인이 필요하지 않지만 플러그인 지원
- 사용자 지정 플러그인 지원
- 멀티 코어 처리
- Rust와 같은 하위 수준 언어 지원 및 WASM(Web Assembly)으로 컴파일되는 모든 언어 지원
- 포장 개봉 후 핫 모듈 교체(HMR) 지원
- 상자에서 코드 분할 지원
- 녹 및 WASM과 같은 저수준 언어 지원
- 즉시 응답 및 유형 스크립트 지원

### 구획 사용에 대한 반대

- 최신 ES7 기능(구축)을 사용하려고 할 때 몇 가지 문제가 발생합니다.
- 개발자가 Babel용 추가 플러그인을 포함할 때 복잡성이 높습니다.

### 구획 시작

구획은 제로 구성 빌드 도구이므로 쉽게 시작할 수 있습니다. 먼저 다음 명령을 실행하여 프로젝트에 구획을 설치합니다.

실:

```undefined
yarn global add parcel-bundler
```

npm:

```coffeescript
npm install -g parcel-bundler
```

프로젝트를 프로젝트(또는 전역)에 설치한 후 다음 명령을 실행하여 프로젝트를 번들할 수 있습니다.

```xml
parcel <your application entry point>
```

구획은 빠른 개발 서버를 제공하며, 변경 시 응용프로그램 코드를 자동으로 재구성합니다. 또한 개발 속도를 높일 수 있도록 핫 모듈 교체를 지원합니다.

### 생산용 건물

다음 명령을 실행하여 Parcel을 사용하여 프로덕션용 응용 프로그램 코드를 빌드할 수 있습니다.

```xml
parcel <your application entry point>
```

### 마감사상

소포는 대부분의 작업에 플러그인을 사용할 필요가 없는 훌륭한 컴파일러입니다. 버전 2가 공개되면 게임 체인저가 됩니다.

Parcel의 제로 구성 특성 및 Rust 및 Web Assembly와 같은 로우 레벨 언어에 대한 지원을 통해 자바스크립트 에코시스템뿐만 아니라 다용도로 사용할 수 있습니다.

프로젝트에서 구획을 사용하는 방법에 대한 자세한 내용은 여기서 공식 설명서를 참조하고 여기서 아직 개발 중인 v2 설명서를 참조하십시오.

## 스노우팩

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/snowpack.png?resize=730%2C424&ssl=1)

공식 웹사이트에 따르면, "스노우팩은 더 빠른 웹 개발을 위한 현대적인 프런트 엔드 빌드 도구이다. 개발 워크플로우에서 웹 팩이나 구획과 같이 더 무겁고 복잡한 번들러를 대신합니다."

스노우팩은 웹팩과 소포의 대체품으로 제작되었으며, 그것이 주요 판매 포인트입니다. 개발에서 번들링 과정을 건너뛰어 만들 것이 없어 개발이 빨라지는 것이 가장 큰 특징이다. 웹 팩, 롤업 및 구획보다 훨씬 빠른 빌드 도구입니다.

Snowpack은 버전 2.7입니다. Snowpack v2 릴리스에 대한 제 기사도 여기에서 읽을 수 있습니다.

### 스노우팩의 특징

- O(1) 제조 시간
- 프로덕션에 번들이 없습니다.
- 번개같이 빠른 빌드
- 핫 모듈 교체 지원
- TypeScript, JSX, CSS 모듈 등에 대한 기본 지원
- 간단한 툴링
- 개발자를 위한 앱 템플릿

### Snowpack 사용에 대한 반대 사항

- 기본 제공 프로덕션 번들러 없음

### 시작하기

Snowpack을 시작하려면 먼저 프로젝트에 Snowpack을 설치하고 개발 서버를 시작해야 합니다. 이 작업은 다음 명령을 실행하여 수행할 수 있습니다.

### 설치

npm:

```undefined
npm install --save-dev snowpack
```

실:

```undefined
yarn add --dev snowpack
```

공식 홈페이지에 따르면 스노팩은 전 세계적으로도 설치가 가능하지만, 로컬에서 설치하는 것을 추천합니다.

### 개발 서버 시작

다음 명령은 Snowpack 개발 서버를 시작하고 사이트를 로컬로 로드합니다.

```undefined
snowpack dev
```

### 생산용 건물

다음으로, 프로덕션용 애플리케이션을 구축하십시오.

```undefined
snowpack build
```

참고: Snowpack은 최적화된 버전의 코드를 제공하므로 프로덕션용 내장 번들과 함께 제공되지 않습니다. 프로덕션용 애플리케이션을 구축해야 하는 경우 플러그인을 통해 즐겨찾는 번들러에 쉽게 연결할 수 있습니다.

### 마감사상

Snowpack은 ES Module을 활용하여 신속하게 빌드를 제공하고 개발 중인 번들 프로세스를 건너뛰는 차세대 빌드 도구입니다. 또한 코드베이스가 성장하더라도 개발 환경을 더 빠르게 구축할 수 있도록 지원합니다.

프로젝트에서 Snowpack을 사용하는 방법에 대한 자세한 내용은 여기에서 공식 설명서를 참조하십시오.

## 에스테이트를 만들다

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/insure-esbuild-bundle.png?resize=730%2C487&ssl=1)

esbuild는 매우 빠른 차세대 자바스크립트 번들러이자 미니어처이다. 웹에서 배포할 자바스크립트 및 타입스크립트 코드를 패키징한다. 그것은 바둑으로 쓰여져 있어서 매우 빨랐다. 또한 소포와 웹팩을 포함한 다른 번들러보다 가볍습니다.

다음은 JavaScript 및 TypeScript 벤치마크 그래프입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/javascript-benchmark-build-times-graph.png?resize=730%2C401&ssl=1)

현재 버전 0.72에서 실험 중이며, 현재 활발히 개발되고 있다.

### esbuild의 특징

- 매우 빠른 빌드
- Typescript, JavaScript 및 JSON의 로더
- 미네이션
- 코드 분할 지원
- 번들링
- 나무 흔들기
- CJS, IIFE, ESM과 같은 다양한 출력 형식 지원
- 소스 맵 생성
- JSX 및 최신 JS 구문을 ES6으로 전송

### esbuild 사용의 반대

- 아직 실험 중이라 아직 주요 버전은 없습니다.
- HRM 없음

### 시작하기

esbuild는 여전히 매우 실험적이지만 다음 명령을 실행하여 설치할 수 있습니다.

### 설치

npm:

```undefined
npm install --save-dev esbuild
```

실:

```undefined
yarn add --dev esbuild
```

다음 명령을 실행하여 esxbuild를 로컬로 설치할 수도 있습니다.

```coffeescript
npm install --global esbuild
```

### esbuild 사용

예를 들어, 다음과 같은 하나의 대응 파일이 있는 경우:

```js
//hello.tsx
import * as React from 'react'
import * as ReactDOM from 'react-dom'

ReactDOM.render(
  <h1>Hello, esbuild!</h1>,
  document.getElementById('root')
);
```

다음 명령을 실행하여 개발을 위해 번들을 제공할 수 있습니다.

```undefined
esbuild hello.tsx --bundle '--define:process.env.NODE_ENV="development"' --outfile=src.js
```

또한 다음과 같은 기능을 실행하여 프로덕션용으로 번들을 제공할 수 있습니다.

```undefined
esbuild hello.tsx --bundle '--define:process.env.NODE_ENV="production"' --outfile=build.js
```

### 마감사상

esbuild는 실험적인 새로운 자바스크립트 번들러이지만, 빠르고 TypeScript에 대한 즉각적인 지원을 제공한다.

esbuild에 대한 자세한 내용은 이 웹 사이트를 참조하십시오.

## 팩켐

팩켐(Packkem)은 Rust에 구현된 사전 컴파일된 자바스크립트 번들러이다. 또한 YAML/TOML과 같은 다른 파일 형식도 컴파일할 수 있다. 팩켐은 JSON 대신 YAML을 사용하여 구성 데이터를 구조화한다.

### 팩켐 사용 찬성

- 소포보다 2배 빠릅니다.
- 가볍고 효율적인 빌드 출력
- 간편한 구성
- 데이터 구조를 위한 YML
- 동적 가져오기를 통한 코드 분할 지원
- 더 나은 개발 환경을 위한 플러그인

### 팩켐 사용에 대한 반대

- 훨씬 더 자세한 API 시드
- 채택률이 낮다.

### 설치

Packem을 설치하려면 터미널에서 다음 명령을 실행합니다.

npm:

```coffeescript
npm install -g packem
```

실:

```undefined
yarn global add packem
```

### 구성 파일 생성

프로젝트에서 팩켐을 사용하려면 프로젝트의 루트 폴더에 `.packmrc` 파일을 생성해야 합니다. 이 파일에는 프로젝트에 필요한 모든 구성 옵션이 포함되어 있습니다. 이 파일에는 다음 필드를 정의해야 합니다.

```undefined
input: "./src/index.js"
output: "./dist/bundle.js"
format: "iife"
```

입력 필드는 번들 프로세스를 시작하는 필드이고 출력 필드는 마지막으로 연결된 번들이 끝나는 필드입니다. 입력 지점을 올바르게 만들려면 기존 파일(동적 파일 생성 없음)을 가리켜야 합니다. 형식 옵션은 JavaScript 파일을 처리할 형식을 유지합니다. 당신이 정의한 방법에 따라 팩켐이 나머지 프로세스를 처리합니다.

프로젝트를 빌드하려면 프로젝트의 루트 폴더에서 `packm`을 실행해야 합니다. 그럼 가시는 게 좋겠군요

### 마감사상

팩켐은 특히 러스트 기반이라 데이터 구조에 YML을 사용하기 때문에 깔끔한 빌드 도구입니다. 가볍기도 해요. 채택률은 여전히 낮지만 자바스크립트가 점차 Rust 기반 툴로 이동하고 있다는 점을 고려할 때 곧 더 널리 채택될 것이다.

## 결론

이 블로그 게시물에서는 많은 개발자들이 사용하는 다양한 JavaScript 빌드 도구에 대해 살펴봤습니다. 앞으로 빌드 툴은 더 빠른 빌드뿐만 아니라 더 나은 사용자 정의와 제로 프로토타이핑 및 구성, 더 많은 즉시 지원, 더 많은 Rust 중심 핵심 기술을 계속 보유할 것으로 예상됩니다.