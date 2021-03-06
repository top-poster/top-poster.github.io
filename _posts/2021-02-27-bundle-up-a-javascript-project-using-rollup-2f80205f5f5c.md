---
layout: post
title: "롤업을 사용하여 JavaScript 프로젝트 번들"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![A tree in autumn.](https://miro.medium.com/max/8064/1*u_k2nR83IFCfc2KnctBfhA.jpeg)

롤업은 작은 코드 조각들을 라이브러리나 응용 프로그램과 같이 더 크고 복잡한 것으로 컴파일하는 자바스크립트용 모듈 번들러이다. GitHub에는 19,000개 이상의 스타가 있으며 npm 레지스트리에서 매주 300만 건 이상의 다운로드가 있다.

시장에 출시된 주요 번들의 npm 동향은 다음과 같습니다.

![Bundler popularity: webpack, Rollup, Parcel, Browserify](https://miro.medium.com/max/1762/1*FRegiOJkR7QDsIYnZdvrGg.png)

롤업은 두 번째로 가장 인기 있는 번들러입니다. 앱에는 웹팩을, 라이브러리에는 롤업을 사용하는 것이 통념이다. 그것은 오늘날에도 여전히 사실이다. 그러나 웹 팩과 롤업이 서로의 기능을 구현함에 따라 두 번들 사이의 선이 흐리게 됩니다.

웹 팩과 마찬가지로 롤업은 가파른 학습 곡선으로 복잡합니다. 롤업 프로젝트를 빌드하는 간단한 예부터 살펴보겠습니다.

# HTML 프로젝트

우리는 반응성 메뉴를 위한 작은 HTML 프로젝트를 만들었습니다. 앱이 모바일로 작동하면 다음과 같은 화면이 다양한 경우에 표시됩니다.

![Lorem ipsum text on a mobile web page for a wiki-like entry on “Flower.”](https://miro.medium.com/max/1714/1*7fzx9hlqLKFrZE428Df_CA.png)

맨 왼쪽 화면이 시작 페이지입니다. 콘텐츠가 표시 영역보다 길기 때문에, 나머지 콘텐츠(중앙 화면)를 보려면 아래로 스크롤해야 합니다. 오른쪽 상단 모서리의 햄버거 메뉴를 클릭하면 수직 메뉴(오른쪽 화면)가 표시됩니다.

최소 너비인 `768px`의 바탕 화면에 앱이 있으면 다음과 같은 수평 메뉴가 표시됩니다.

![On a desktop monitor, the webpage shows the navigation horizontally.](https://miro.medium.com/max/2264/1*eSIKJrHi_Xus4vMdE53EhQ.png)

소스 코드는 이 GitHub 저장소에 있으며 다음은 폴더 구조입니다.

```js
$ ls -R
images index.class style.class
./오렌지:
꽃.jpg 햄버거svg
./src:
index.js
```

이것은 몇 개의 이미지, 스타일시트, 자바스크립트 코드를 포함하는 `index.html`을 가진 전형적인 HTML 프로젝트이다.

7호선은 style.css의 연결선이다.

8번 라인에는 JavaScript 코드가 포함되어 있습니다.

14호선은 꽃 JPG입니다.

17호선은 좁은 화면에 표시되는 햄버거 SVG입니다.

프로젝트는 VSCode의 라이브 서버로 시작할 수 있습니다.

![Launching in VSCode.](https://miro.medium.com/max/876/1*WfHr8pMJtdlLPnVzm6JEcw.png)

# 실시간 로드를 사용한 롤업

모듈 번들은 작은 자바스크립트 코드 조각들을 스타일 시트와 이미지와 함께 라이브러리나 응용 프로그램과 같이 더 크고 복잡한 것으로 컴파일하기 위해 필요하다. 롤업을 사용하여 JavaScript 프로젝트를 번들링합니다. 첫 번째는 npm 프로젝트를 관리하는 것입니다.

```js
$npm in-y
/Users/fuje/rollup-example/package에 기록되었습니다.죤슨:
{
"이름": "dism-dism",
"버전": "1.0.0",
"description": ","
"main": "index.js",
"flict": {
"test": "echo \"Error: no test 지정됨\"
```

생성된 `패키지`에서.json, 롤업 명령을 실행할 스크립트를 추가합니다. 또한 devDependencies 섹션을 생성하여 개발용 롤업 및 플러그인 패키지를 다운로드한다.

라인 7에서는 시계 모드의 구성에 따라 롤업을 시작하기 위해 `시작` 스크립트가 추가됩니다.

14호선은 롤업 패키지이다.

라인 15는 롤업 번들을 라이브로 다시 로드하는 `롤업-플러그인-라이브 재로드` 플러그인입니다.

16번 라인은 라이브 로딩을 제공하는 개발 서버를 시작하는 `롤업 플러그인-서브` 플러그인입니다.

시작 스크립트는 롤업 구성을 기반으로 하므로 프로젝트에 `rollup.config.js`가 필요합니다.

라인 5는 `입력` 항목을 `src/index.js`로 지정합니다.

7-14행은 `rollup-plugin-serve`를 구성합니다.

8번 라인은 브라우저 모드에서 실행됩니다.

9번 라인은 콘솔에서 상세 출력을 활성화합니다.

라인 10은 폴더, 현재 디렉터리 및 파일을 서비스할 `dist`를 지정합니다.

11호선에서는 폴백 페이지를 오류 페이지(404)가 아닌 index.html(`200`)로 설정합니다.

12, 13호선은 개발 서버를 localhost:3000으로 설정했다.

15호선은 `롤업-플러그인-라이브 재로드` 감시 폴더를 구성한다.

라인 17-21은 출력 객체의 위치와 형식을 정의합니다.

라인 18은 출력 파일을 `dist/bundle.js`로 지정합니다.

라인 19는 출력 형식을 ife로 지정합니다. 이것은 즉시 호출되는 함수 표현식(IIFE) 형식이며, 다른 문서에서 설명합니다.

라인 20은 별도의 소스 맵 파일인 `dist/bundle.js.map`을 생성하도록 구성합니다. 부울 값 외에도 `inline`으로 설정할 수도 있습니다.

또한 `sourcemap`을 동적으로 활성화할 수 있습니다.

`패키지`로요.json, 스크립트를 다음과 같이 변경합니다.

```js
"start": "rollup -c-w --Environment BUILD:development"
```

그런 다음 출력을 동적으로 설정할 수 있습니다.

```js
출력: {}
파일: 'dist/dist.js',
이름: '➡' ,
형식: 'ilife',
sourcemap:process.env.빌드 === '개발',
}
```

이 롤업 구성에 따라 `dist/bundle.js`가 생성됩니다. 다음을 포함하도록 `index.html`을 수정합니다.

```js
<script src="dist/discr.js" script/script >
```

프로젝트의 루트에서 `npm start`를 실행합니다. 롤업 프로젝트가 실행 중입니다.

`src/index.js`입니다.

생성된 `dist/bundle.js`입니다.

라인 1은 라이브 로드 스크립트입니다.

선 2-20은 IIFE를 정의한다.

21호선은 소스 맵에 대한 링크입니다. 출력일 경우.sourcemap은 `dist/dist.js`에 속하며, sourcemap은 `dist/dist.js`에 속하게 됩니다.

# 나무 흔들기

트리 흔들기는 코드 최적화 기법이다. 롤업은 코드를 분석하여 라이브러리 또는 응용 프로그램을 실행하는 데 필요한 원본만 포함합니다.

src/index.js에 printCount(17-21) 함수를 추가해 보겠습니다.

이는 생성된 `dist/bundle.js`에 반영됩니다.

src/index.js에서 printCount(16행)의 사용을 설명하면 생성된 dist/bundle.js에는 printCount가 포함되지 않습니다.

트리 흔들기는 롤업의 시그니처 기능입니다. 롤업이 더 작고 빠른 번들을 만들 수 있습니다. ES2015 모듈 구문(예: import 및 export)의 정적 구조에 의존한다.

# CSS 플러그인

우리는 `index.html`에 스타일시트를 포함시켰습니다. 이 포함은 롤업 플러그인인 `롤업 플러그인-포스트cs`로 대체될 수 있으며, 롤업 플러그인과 포스트CSS 간의 원활한 통합을 제공한다.

postcs와 롤업플러그인 postcs는 패키지에 포함될 필요가 있다.json:

```js
"devDependencies": {
"postcs": "^8.2.4",
"fileng-disc-postcs": "^4.0.0",
...
}
```

`rollup-plugin-postcs` 구성을 `rollup.config.js`(3행 및 8행):

`index.html`에서 `style.css` 링크를 제거합니다.

```js
<link rel="stylesheet" href="style.discount" / >
```

대신 style.css를 `src/index.js`(줄 1)로 가져옵니다.

그러면 다음과 같은 `dist/bundle.js`가 생성됩니다.

라인 2-30은 HTML 파일에 스타일시트를 주입하는 기능을 정의합니다.

32호선은 모든 스타일시트 내용을 캡처합니다.

라인 33은 스타일시트를 주입하는 기능을 호출합니다.

# 이미지 플러그인

우리는 `index.html`에 두 개의 이미지를 포함시켰습니다. 이 포함은 시작 시 이미지를 사용할 수 있는 편의를 제공하는 롤업 플러그인 `@rollup/plugin-image`로 대체될 수 있습니다.

@rollup/plugin-image 패키지는 `package`에 포함되어야 한다.json:

```js
"devDependencies": {
@https/https-image,
...
}
```

`rollup.config.js`(4행 및 10행)에 `@rollup/plugin-image` 구성을 추가합니다.

이미지에 대해 `index.html` 조정(10행 및 13행):

flower.jpg 및 hamburger.svg를 src/index.js(라인 2, 3)로 가져옵니다.

그러면 다음과 같은 `dist/bundle.js`가 생성됩니다.

35호선은 41호선에서 사용되는 꽃 JPG이다.

37호선은 43호선에서 사용되는 햄버거 SVG 파일이다.

# 터서 및 클리너 플러그인

`disc-disc-terser`는 생성된 번들을 최소화하기 위한 플러그인입니다. 생산 코드를 최소화하는 것이 좋습니다.

➡-➡-➡는 재구성 전에 디렉토리를 정리하는 플러그인이다. 디렉토리를 정리하는 것이 좋습니다.

이 두 플러그인을 사용하려면 다음 패키지가 `devDependencies`의 일부여야 합니다.

```js
"devDependencies": {
롤업-트레이닝-롤링-롤링-트레이닝,
롤업식 터어,
...
}
```

그런 다음 `rollup.config.js`에서 사용할 수 있습니다.

# 결론

롤업은 두 번째로 가장 인기 있는 번들로, 특히 라이브러리 번들에 능숙하다.

우리는 6개의 플러그인을 도입했습니다. 마지막 코드는 이 리포지토리에서 캡처됩니다.

롤업 기능을 확장할 수 있는 플러그인이 많이 있습니다. 또한 고유한 플러그인을 만들 수 있습니다.

지금까지 롤업은 우리 프로젝트에 잘 맞습니다. 웹 팩과 롤업은 서로 경쟁 상대입니다. 앞으로 더 많은 기능과 효율성을 제공할 수 있기를 바랍니다.

읽어주셔서 감사합니다. 이게 도움이 됐으면 좋겠어요. 여기 제 다른 매체 출판물을 보실 수 있습니다.