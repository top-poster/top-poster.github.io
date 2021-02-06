---
layout: post
title: "Vue.js와 함께 Pug.js 사용"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/02/using-pugjs-with-vuejs.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/using-pugjs-with-vuejs.png?fit=730%2C487&ssl=1)

이 Pug.js 튜토리얼에서는 Pug를 템플릿 엔진으로 사용하여 Vue.js를 백엔드 애플리케이션에 통합하는 방법을 보여 드리겠습니다.

Pug.js에 대한 소개는 "Pug 시작하기"를 참조하십시오.

## Pug.js란 무엇인가?

Pug.js는 Node.js와 같은 서버측 기술에서 HTML을 렌더링하도록 설계된 템플릿 엔진이다. 다른 자바스크립트 템플릿 엔진과 마찬가지로 Pug.js는 재사용 가능한 HTML 코드 작성과 동적 데이터 렌더링을 지원한다.

Vue.js는 웹 인터페이스와 단일 페이지 애플리케이션을 구축하는 데 사용되는 자바스크립트용 프로그레시브 프레임워크이다. 웹 인터페이스뿐만 아니라, Vue.js는 데스크탑과 전자 프레임워크의 모바일 앱 개발에도 사용된다.

## Vue.js와 함께 Pug를 사용하는 이유는 무엇입니까?

대부분의 백엔드 개발자는 Pug를 Vue.js와 함께 사용하는 것을 선택합니다. Pug는 구현 및 읽기가 훨씬 쉬우며 전체 구성이 필요하지 않기 때문입니다. 유효한 HTML은 또한 유효한 Vue.js 템플릿입니다. Vue.js 템플릿의 사전 프로세서로 Pug.js를 사용하면 기존 프로젝트를 마이그레이션하여 Vue의 반응성 기능을 활용할 수 있습니다.

Pug.js는 HTML로 컴파일되기 때문에 템플릿에 Express.js와 같은 백엔드 기술과 함께 자주 사용된다. 백엔드 개발자들과 달리, 대부분의 프런트엔드 개발자들은 Pug가 백색 공간에 민감하기 때문에 쓰기 및 유지보수에 어려움을 느끼며, 이는 어떤 태그가 서로 중첩되어 있는지 식별하기 위해 사용하는 것을 의미한다.

이 튜토리얼을 따르기 위해서는 JavaScript 및 Vue.js에 대한 이해와 VS Code와 같은 텍스트 편집기를 사용한 경험이 있어야 합니다.

## Node.js 프로젝트 설정

nodejs 프로젝트를 설정할 때, 우리는 `package`를 초기화해야 한다.json 파일은 npm in-y 명령을 사용하여 애플리케이션 종속성을 추적합니다.

다음으로, 루트 Node.js 파일이 될 `index.js` 파일을 생성하겠습니다. 서버에 Express.js를 사용하므로 Express를 설치하고 루트 `index.js` 파일로 가져온 다음 해당 인스턴스를 생성해야 합니다. 우리는 `3000`에서 실행되는 포트를 듣기 위해 이 인스턴스를 사용할 것이다.

### Node.js 앱에 Express.js 설치

Express.js를 설치하려면 터미널을 열고 `npmi express `--`save`를 입력하십시오.

```coffeescript
let express = require('express');
let app = express();

app.get('/', (req, res) => res.json({msg:"Hello World"}))

app.listen(3000, () => console.log('Test running'))
```

응용 프로그램을 실행하려면 터미널을 열고 `node index.js`를 입력하십시오. 이 명령은 터미널에 `테스트 실행 중`을 표시합니다. 즉, 서버가 실행 중임을 의미합니다. 이제 브라우저에서 `http://localhost:3000/`에서 애플리케이션에 액세스할 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/pugjs-hello-world-1.png?resize=720%2C143&ssl=1)

### Pug.js 설정 중

Pug를 템플릿 엔진으로 설정하려면 먼저 `npm install pug`를 실행하여 Pug를 설치하십시오.

Pug를 설치한 후에는 기본 Express 템플릿 엔진으로 설정하고 모든 템플릿이 정의될 디렉터리를 가리켜야 합니다. 다음과 같이 `index.js` 파일을 수정할 수 있습니다.

```coffeescript
let express = require('express');
let pug = require("pug")
let path = require('path')
let app = express();

app.set(path.join(__dirname, './views'))
app.set('view engine', 'pug')


app.get('/', (req, res) => res.render('home'))

app.listen(3000, () => console.log('Test running'))
```

파일 및 디렉터리 경로 작업을 위한 유틸리티를 제공하는 Node.js `path` 모듈이 필요합니다.

모든 Pug 템플릿을 정의할 `views` 디렉토리를 생성하겠습니다. 경로 모듈을 사용하여 `views` 디렉토리를 대상으로 지정하고 모든 템플릿의 루트 디렉토리로 설정합니다.

그런 다음 `홈`을 만듭니다.pug` 파일을 `contract` 디렉토리 안에 넣고 다음을 추가합니다.

```css
.container
    h1 Hello World
```

"홈"을 렌더링하기 위해 경로를 변경했기 때문에 이제 어플리케이션이 브라우저에서 "Hello World"를 렌더링합니다.퍼그 템플릿 Pug가 들여쓰기 기능을 사용하여 서로 중첩된 태그를 알아냅니다.

## Vue.js와 함께 Pug 사용

Pug 템플릿에 Vue.js를 설정하려면 웹 팩을 설치하고 설정해야 합니다.

웹 팩은 기본적으로 모듈 번들러입니다. 주요 목적은 브라우저에서 사용하기 위해 JavaScript 파일을 번들링하는 것입니다.

웹 팩과 해당 CLI를 설치하려면 터미널을 열고 다음을 입력하십시오.

```coffeescript
npm i -D webpack webpack-cli
```

웹 팩을 설치한 후 Vue.js 부트스트랩을 지원하기 위해 몇 가지 다른 패키지를 설치해야 합니다.

- Vue.js 구성 요소를 단일 파일 구성 요소로 설정할 수 있는 웹 팩용 로더인 `vue-loader`
- Webpack5 구성의 호환성 문제를 해결하는 데 도움이 되는 vue-loader-plugin.
- 런타임 컴파일 오버헤드와 CSP 제한을 피하기 위해 Vue.js 2.0 템플릿을 렌더 함수로 미리 컴파일하는 데 사용되는 `vue-template-compiler`입니다.
- @import와 url을 import와 같이 해석하고 해결하는 ➡-➡

이러한 패키지를 설치하려면 터미널을 열고 다음을 입력하십시오.

```coffeescript
npm i -D vue-loader vue-loader-plugin vue-template-compiler css-loader
```

-D 플래그가 개발 종속성으로 이 패키지를 설치합니다.

다음으로, 우리는 ECMAScript 2015+ 코드를 현재 및 이전 브라우저와 환경에서 역호환 자바스크립트 버전으로 변환하는 데 주로 사용되는 툴체인 `babel`을 설치해야 한다. 이것을 설치하면, 우리는 자바스크립트 파일에 import, export와 같은 것들을 사용할 수 있다.

우선 npm을 이용한 babel-watch를 단말기에 npm-babel-watch를 설치한 뒤 개발 의존도로 babel-core와 babel-loader를 설치하기로 했다.

```coffeescript
npm i -D babel-core babel-loader
```

Vuejs 코드를 작성할 `클라이언트` 디렉토리를 생성하겠습니다. 디렉토리 내에서 `home.js` 파일을 작성하고 다음을 추가합니다.

```js
import Vue from "vue";
let app = new Vue({
    el: '#home',
    data: {
        names: ['Wisdom', "Ekpot", "Ubongabasi"]
    },
    methods: {
        logSomeThing() {
            return 'Hello Wisdom Ekpot'
        }
    },
    mounted() {
        console.log(this.logSomeThing());
    }
})
```

Vue.js에 익숙하다면 간단한 Vuejs 설정으로 인식해야 합니다. 여기서 뿌리는 id가 홈인 div를 겨냥하고 있다. 템플릿이 마운트되면 logSomething 메서드가 호출되며 methods 개체에 정의되어 있습니다.

이를 Pug 파일에 포함시키려면 몇 가지 웹 팩 구성을 추가해야 합니다. webpack.config.js 파일을 생성하고, 웹 팩을 가져오고, Vue.js에 대한 일부 구성을 작성하겠습니다.

먼저 웹 팩 `path` 및 `VueLoaderPlugin`을 가져옵니다.

```js
const webpack = require('webpack');
const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');
```

우리의 웹팩 구성은 `config` 객체로 포장될 것이다. 먼저 모드를 개발로 설정하고 엔트리 파일도 정의하겠습니다.

입력 파일을 설정한 후, 우리는 모든 변환된 코드가 저장되는 출력을 설정해야 합니다. 출력 속성은 웹 팩이 생성하는 번들을 어디에서 내보내는지와 이러한 파일들의 이름을 지정하는 방법을 알려준다. 기본적으로 웹 팩은 Vue.js 코드를 가져와서 바닐라 자바스크립트로 변환한다.

```undefined
const config = {
    mode: 'development',
    entry: {
        'home': './client/home.js',
    },
    output: {
        path: path.resolve(__dirname, 'public/js'),
        filename: '[name].bundle.js'
    },
}
```

컴파일된 코드를 저장할 `공용` 디렉토리를 생성합니다. 우리는 또한 규칙이 일치할 때 사용할 몇 가지 웹 팩 규칙을 마련해야 합니다. 이러한 규칙은 상위 규칙 조건이 일치하는 경우에만 평가됩니다. 각 중첩 규칙은 고유한 조건을 포함할 수 있습니다.

```css
 module: {
        rules: [
            {
                test: /\.vue$/,
                loader: 'vue-loader'
            },
            {
                test: /\.js$/,
                use: 'babel-loader',
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: [
                    'vue-style-loader',
                    'css-loader'
                ]
            }
        ]
    },
```

기본적으로 웹 팩은 파일 확장자가 .vue인지 확인하고 vue-loader 플러그인을 사용하여 자바스크립트에 번들로 묶는다.

또한 모듈 해결 방법을 구성하려면 `resolve` 개체를 정의해야 합니다.

```bash
 resolve: {
        alias: {
            'vue$': 'vue/dist/vue.esm.js'
        },
        extensions: [
            '.js',
            '.vue'
        ]
    },
```

다음으로, Webpack 5에 대한 일부 구성을 처리하는 데 도움이 되는 VueLoaderPlugin의 새 인스턴스를 정의해야 합니다.

```coffeescript
 plugins: [
        new VueLoaderPlugin(),
    ],
```

이 작업을 수행한 후에는 `config` 개체를 내보내야 합니다.

```java
module.exports = config;
```

이 웹 팩 구성을 부트스트랩하려면 `패키지`에 스크립트를 추가해야 합니다.부트스트랩할 json 파일:

```bash
 "watch": "babel-watch  --watch -- index",
    "dev": "npm run watch & npm run client:build-dev ",
    "client:build-dev": "webpack --watch"
```

`client:build-dev` 명령은 부트스트랩을 시작하고 변경 사항이 있는지 파일을 모니터링합니다. 응용 프로그램을 시작하려면 터미널에서 `npm run dev`를 실행하십시오.

이제 우리는 `집`을 수정할 수 있다.`파일을 여기에 pug:

```bash
.container#home
    h1 Hello World
script(src="/js/home.bundle.js")
```

단말기에서 npm run dev를 실행하면 홈이 생긴다.bundle.js` 파일은 우리의 공용 디렉토리 안에 있으며, 이것은 우리의 변환된 코드이다.

브라우저 콘솔을 보면 `logSome`이 표시됩니다.인쇄된 사물(`)

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/vuejs-pug-template-1.png?resize=720%2C158&ssl=1)

위의 내용은 Vue.js가 Pug 템플릿에서 사용 중임을 나타냅니다.

마지막으로 `.gitignore` 파일을 생성하고 다음을 추가하는 것입니다.

```cpp
public/js
node_modules
```

이렇게 하면 `node_modules` 디렉토리와 `public` 디렉토리가 커밋되지 않습니다.

## 결론

백엔드에서 크고 복잡한 프런트엔드 작업을 수행할 때 Pug와 Vue.js를 함께 사용하면 개발 프로세스를 단순화하고 확장할 수 있습니다.

이 문서에 사용된 예제의 소스 코드는 GitHub에서 사용할 수 있습니다.