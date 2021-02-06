---
layout: post
title: "Rust에서 '스팩' 번들로 JavaScript 빌드 속도 향상"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/swc-js.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/swc-js.png?fit=730%2C487&ssl=1)

대부분의 자바스크립트 번들러와 트랜스필러는 자바스크립트 자체에서 작성되기 때문에 효율성이 떨어지는 경향이 있다. 그러나 번들러와 트랜스필러가 파이썬이나 러스트와 같이 더 빠르고, 더 잘 컴파일되고, 최적화된 언어로 작성된다면, 그것들은 더 빨리 작동할 수 있다.

이 기사에서는 Rust의 swc-project와 함께 `스팩` 번들을 사용하여 자바스크립트 빌드 속도를 높이는 방법에 대해 논의한다.

## swc프로젝트란?

`swc 프로젝트`는 러스트(Rust)로 쓰여진 번들러와 트랜스필러 모음집이다. swc-project는 Typescript, JSX, TSX 및 기타 버전의 JavaScript를 전송할 수 있습니다. Babel과 마찬가지로 SWC는 소스 코드를 읽는 도구이다. 그러나 SWC는 Babel보다 약 20배 더 빨리 동작한다.

swc는 `.swcrc` 파일을 사용하여 구성할 수 있습니다. 여기서 다른 구성을 참조하십시오.

## swc를 통해 '스팩'으로 전환

`스팩`은 `swc`를 통해 번역할 수 있도록 지원하는 JS 번들러다. 데드코드 제거와 트리 흔들기를 지원하는 웹팩의 대안이다.

Deno는 최근 "스팩"을 기반으로 한 새로운 타입스크립트 번들링 및 전송 옵션을 추가했다.

내부적으로는 swc_foller상자(swc_foller상자)에 의존하고 있다.

```java
const { config } = require("@swc/core/spack");
const path = require("path");
module.exports = config({
 // starting point
  entry: {
    web: path.join(__dirname, "src", "index.ts"),
  },
 // output file
  output: {
    path: path.join(__dirname, "dist"),
  },
  module: {},
});
```

## 번들러 및 트랜스필러 구성

spack은 transpiler의 경우 .swcrc 파일을 사용하고 bundler의 경우 spack.config.js 파일을 사용하여 구성됩니다.

작동 방식을 보려면 `spack-demo`라는 디렉터리를 만드십시오. `spack-demo` 폴더에서 demo용으로 `index.js` 파일과 여러 파일을 추가합니다. 디렉토리에 `spack.config.js` 및 `.swcrc` 파일을 추가합니다.

파일 구조는 이와 같아야 합니다.

```css
├── index.js
├── spack.config.js
└── .swcrc
```

npmi @swc/core @swc/cli @swc/helpers를 사용하여 필수 패키지를 설치합니다.

번들의 경우: `spack.config.js`

```java
// importing config creator
const { config } = require("@swc/core/spack");

// import path module
const path = require("path");

// export config
module.exports = config({

// start file
  entry: {
    build: path.join(__dirname, "index.js"),
  },

// output file
  output: {
    path: path.join(__dirname),
  },
  module: {},
});
```

번역용: `.swcrc`

```cpp
{
    "jsc": {
// transpile to es2015
        "target": "es2015",
        "parser": {
// file use ecmascript
            "syntax": "ecmascript",
        }
    }
}
```

## '스팩'을 사용하여 JavaScript 번들

이전에 추가한 보일러 플레이트 코드는 React.js 및 Node.js와 같은 대부분의 코드 베이스에 필요한 기본 번들링 및 전송에 사용될 수 있다.

데모의 경우 index.js를 사용하여 `mod`라는 디렉토리를 생성하고 루트에서 `index.js`를 가져옵니다. 파일 구조는 이와 같아야 합니다.

```css
├── index.js
├── mod
│   └── index.js
├── spack.config.js
└── .swcrc
```

`mod/index.js` 및 `index.js`에는 `스팩` 번들 기능을 보여주기 위한 더미 코드가 포함되어 있습니다.

`mod/index.js`

```coffeescript
export const sum=(a,b)=>a+b;
```

`index.js`

```coffeescript
import {sum as add} from "./mod"
console.log(add(1,2));
```

`build.js`(이 파일은 `스팩`에서 생성됩니다.)

```js
var sum = function(a, b) {
    return a + b;
};
console.log(sum(1,2));
```

## 이전 JavaScript로 이동

spack은 .swcrc 파일의 target 키에 따라 여러 버전의 자바스크립트로 전송하는 데 swc를 사용한다. swc는 ES5, ES2015, ES2016, ES2018, ES2019, ES2020을 지원한다.

이전 데모에서는 최신 버전의 자바스크립트에서 지원되는 코드를 작성했지만, `스팩`을 사용하면 이전 브라우저에서 지원되는 이전 버전의 자바스크립트로 코드를 전송할 수 있다. 거의 모든 브라우저가 ES5 버전의 자바스크립트를 지원한다.

작동 방식을 확인하려면 target을 es5로 변경하고 mod/index.js에 새로운 코드를 추가합니다.

`mod/index.js`

```coffeescript
export const sum=(a,b)=>({...a,...b})
```

.swcrc

```cpp
{
    "jsc": {
// transpile to es5
        "target": "es5",
        "parser": {
// file use ecmascript
            "syntax": "ecmascript",
        }
    }
}
```

이제 `build.js` 출력 파일은 다음과 같습니다.

```js
function _defineProperty(obj, key, value) {
    if (key in obj) {
        Object.defineProperty(obj, key, {
            value: value,
            enumerable: true,
            configurable: true,
            writable: true
        });
    } else {
        obj[key] = value;
    }
    return obj;
}
function _objectSpread(target) {
    for(var i = 1; i < arguments.length; i++){
        var source = arguments[i] != null ? arguments[i] : {
        };
        var ownKeys = Object.keys(source);
        if (typeof Object.getOwnPropertySymbols === "function") {
            ownKeys = ownKeys.concat(Object.getOwnPropertySymbols(source).filter(function(sym) {
                return Object.getOwnPropertyDescriptor(source, sym).enumerable;
            }));
        }
        ownKeys.forEach(function(key) {
            _defineProperty(target, key, source[key]);
        });
    }
    return target;
}
var sum = function(a, b) {
    return _objectSpread({
    }, a, b);
};
console.log(sum({
}, {
}));
```

`build.js` 출력 파일을 자세히 보면 이제 이 파일에 새로운 JS 기능(예: 스프레드 연산자)을 지원하는 추가 코드가 포함되어 있음을 알 수 있습니다. 추가 코드는 모두 `스팩`에 의해 주입되어 ES5 버전의 자바스크립트만 지원하는 브라우저를 포함하여 우리의 코드가 구형과 신형 브라우저에서 완벽하게 실행될 수 있다.

## 여러 JavaScript 항목 번들

여러 번들을 처리할 때 공통 파일이나 모듈을 캐싱하고 중복 작업을 방지하여 빌드를 최적화할 수 있습니다. `스팩`은 여러 번들을 동시에 지원하며 번들에 공통 파일을 캐싱합니다.

`spack.config.js`

```undefined
const { config } = require("@swc/core/spack");
const path = require("path");
module.exports = config({
  entry: {
    web: path.join(__dirname, "src", "index.js"),
    node: path.join(__dirname, "src", "node.js"),
  },
  output: {
    path: path.join(__dirname, "dist"),
  },
  module: {},
});
```

## 번들링 유형 스크립트

Deno가 TypeScript의 전송 및 번들에 `spack`을 사용합니다. 그것은 `swc`를 이용한 TypeScript 전송 및 번들을 전적으로 지원한다. 컴파일된 swc는 TypeScript repo에 대한 모든 테스트를 통과한다. TypeScript는 `syntax` 키를 TypeScript로 변경하여 지원할 수 있습니다.

```cpp
{
    "jsc": {
// transpile to es5
        "target": "es5",
        "parser": {
// file use ecmascript
            "syntax": "typescript",
        }
    }
}
```

## 결론

스팩은 초기 단계지만 생산 준비가 되기 전까지는 러스트 swc-project에서 개발 환경 번들로 사용할 수 있다. Deno가 TypeScript 번들에 사용했던 것처럼 `spack`은 여러 기능을 가진 보다 강력한 번들러를 만들기 위한 기본 라이브러리로도 사용될 수 있다.