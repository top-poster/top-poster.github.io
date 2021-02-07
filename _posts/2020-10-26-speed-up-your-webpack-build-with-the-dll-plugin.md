---
layout: post
title: "DLL 플러그인을 사용하여 웹 팩 빌드 향상"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/webpack.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/webpack.png?fit=730%2C487&ssl=1)

자바스크립트 개발자로서, 프런트엔드 자산을 React와 함께 번들링하거나 일부 TypeScriptNode.js 코드를 전송하면서 웹 팩을 접할 수 있는 충분한 기회가 있었을 것입니다.

대부분의 경우 웹 팩과 직접 상호 작용할 필요가 없습니다. 대신 빌드 도구의 종속성으로 웹 팩과 간접적으로 상호 작용합니다. 그러나 이러한 빌드 도구를 개발하거나 웹 팩 구성을 관리하는 경우 이 튜토리얼을 통해 빌드 시간을 향상시킬 수 있습니다.

우리는 DLL 플러그인을 사용할 것이다. 이 플러그인은 웹 팩이 설명서에 "로드 시간을 획기적으로 개선할 것"을 약속한다.

## 그것은 어떻게 작동하나요?

DLL 플러그인은 다음 두 가지를 생성합니다.

- manifest.json 파일
- 자주 변경되지 않는 모듈 번들

DLL 플러그인을 사용하지 않으면 웹 팩은 수정 여부에 관계없이 코드베이스의 모든 파일을 컴파일합니다. 이것은 컴파일 시간을 필요 이상으로 길게 만드는 효과가 있다.

그러나 웹 팩에 거의 변하지 않는 라이브러리(예: `node_modules` 폴더의 라이브러리)를 다시 컴파일하지 말라고 지시할 수 있는 방법이 있습니다.

DLL 플러그인이 여기에 들어옵니다. 거의 변경하지 않는 코드(예: 벤더 라이브러리)를 번들링하고 다시 컴파일하지 않으므로 빌드 시간이 크게 단축됩니다.

DLL 플러그인은 `manifest.json` 파일을 생성하여 이를 수행합니다. 이 파일은 가져오기 요청을 번들 모듈에 매핑하는 데 사용됩니다. 다른 번들의 모듈로 가져오기 요청을 하면 웹 팩은 해당 모듈에 대한 `manifest.json` 파일에 항목이 있는지 확인합니다. 만약 그렇다면, 그것은 그 모듈을 만드는 것을 건너뜁니다.

## 개요

DLL 플러그인은 공급업체 번들처럼 거의 변경되지 않는 코드 번들에 사용해야 합니다. 따라서 별도의 웹 팩 구성 파일이 필요합니다. 여기에서 벤더 번들을 생성하는 방법에 대해 알아 보십시오.

이 튜토리얼에서는 두 개의 웹 팩 구성을 사용합니다. 이러한 이름은 webpack.config.js 및 webpack.vendor.config.js로 지정됩니다.

`webpack.config.js`는 비메모 코드, 즉 자주 수정되는 코드에 대한 기본 구성입니다.

`webpack.discount.config.js`는 `node_discount`의 라이브러리처럼 변경되지 않은 번들에 사용됩니다.

DLL 플러그인을 사용하려면 다음 두 개의 플러그인이 적절한 웹 팩 구성에 설치되어 있어야 합니다.

DllReferencePlugin → `webpack.config.js`
DllPlugin → `webpack.dpack.config.js`

5.x는 아직 베타 버전이기 때문에 웹 팩 버전 4.x를 사용할 것입니다. 그러나 둘 다 유사한 구성을 공유합니다.

### DLL 플러그인 구성('webpack.vendor.config.js')

DLL 플러그인에는 다음과 같은 필수 옵션이 있습니다.

- `name`: DLL 함수 이름입니다. 그것은 아무거나 부를 수 있다. 우리는 이것을 `vendor_lib`라고 부를 것이다.
- 경로: 출력된 매니페스트 json 파일의 경로입니다. 절대적인 길임에 틀림없어. 루트 디렉토리의 "빌드" 폴더에 저장합니다. 이 파일은 `벤더-매니페스트.json`으로 불린다.

경로를 지정하려면 다음과 같이 `path.join`을 사용합니다.

```undefined
path.join(__dirname, 'build', 'vendor-manifest.json')
```

webpack.vendor.config.js 파일에서 `output.library`가 DLL 플러그인 `name` 옵션과 동일한지 확인하십시오.

원하는 만큼 진입점을 포함합니다. 이 예에서, 저는 몇 개의 정말 무거운 도서관을 포함시켰습니다. 이 플러그인을 사용하는 동안에는 출력 폴더가 중요하지 않습니다.

이제 `webpack.vendor.config.js`의 모양을 살펴보겠습니다.

```js
var webpack = require('webpack')
const path = require('path');
module.exports = {
    mode: 'development',
    entry: {
        vendor: ['lodash', 'react', 'angular', 'bootstrap', 'd3', 'jquery', 'highcharts', 'vue']
    },
    output: {
        filename: 'vendor.bundle.js',
        path: path.join(__dirname, 'build'),
        library: 'vendor_lib'
    },
    plugins: [
        new webpack.DllPlugin({
            name: 'vendor_lib',
            path: path.join(__dirname, 'build', 'vendor-manifest.json')
        })
    ]
}
```

### DllReferencePlugin('webpack.config.js') 구성

DllReferencePlugin에는 다음 두 개의 필수 필드가 있습니다.

- ➡: 빌드 폴더가 포함된 디렉터리의 절대 경로입니다. 이 튜토리얼의 경우 `__dirname`으로 두십시오.
- ➡: DLL의 매니페스트 json 파일에 대한 절대 경로입니다. 이것을 `path.join(__dirname, `build`, `vendor-manifest.json`)으로 설정합니다.

`webpack.config.js`는 다음과 같이 표시됩니다.

```js
const webpack = require("webpack")
var path = require("path");
// const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;
const SpeedMeasurePlugin = require("speed-measure-webpack-plugin");
const smp = new SpeedMeasurePlugin();
module.exports = smp.wrap({
  mode: 'development',
  entry: {
    app: ['./src/index.js']
  },
  output: {
    filename: 'main.bundle.js',
    path: path.join(__dirname, 'build')
  },
  plugins: [
    new webpack.DllReferencePlugin({
      context: __dirname,
      manifest: path.join(__dirname, 'build', 'vendor-manifest.json')
    }),
    // new BundleAnalyzerPlugin()
  ]
})
```

그러면 DLL 플러그인 설정이 완료됩니다.

### 번들 구성

#### DLL 'manifest.json' 생성

먼저 webpack.vendor.config.js 구성을 사용하여 webpack을 실행해야 합니다. 이 구성은 webpack.config.js가 작동하는 데 필요한 vendor.manifest.json을(를) 생성합니다. 이 빌드는 구성이 변경될 때 또는 벤더 번들의 라이브러리 버전이 변경될 때 모든 개발 세션을 시작할 때 수행될 수 있습니다.

이 스크립트를 `패키지`에 추가합니다.json의 파일 매니페스트 json 파일과 벤더 번들을 생성합니다.

```bash
"scripts": {
    "buildVendor": "webpack --config webpack.vendor.config"
}
```

이후 코드 변경 시 `webpack.config.js`만 사용하면 됩니다.

#### 기본 번들 빌드

그런 다음 기본 번들에 대한 빌드 스크립트를 추가합니다.

```bash
  "scripts": {
    "buildVendor": "webpack --config webpack.vendor.config",
    "build": "webpack --config webpack.config.js"
  }
```

## 벤치마크

플러그인을 테스트하기 위해 `src/index.js` 파일의 단순 Vue.js 앱을 인스턴스화했습니다. 일부 중량 의존성을 가져옵니다.

```js
import Vue from "vue"
import lodash from 'lodash'
import 'react'
import 'angular'
import 'bootstrap'
import 'd3'
import 'jquery'
import 'highcharts'
export default function createApp() {
  // vendor()
  const el = document.createElement("div")
  el.setAttribute("id", "app")
  document.body.appendChild(el)
  console.log("hello")
  new Vue({
    el: "#app",
    render: h => h("h1", "Hello world")
  })
}
document.addEventListener('DOMContentLoaded', () => {
  createApp()
})
```

웹 팩 구성에서 생성한 두 번들을 가져오려면 다음 스크립트 태그를 `index.html` 헤더에 추가해야 합니다.

```xml
<head>
  <title>Webpack DllPlugin Test</title>
  <script src="/build/vendor.bundle.js"></script>
  <script src="/build/main.bundle.js"></script>
</head>
```

speed-measure-webpack-plugin을 사용하여 번들을 테스트하면 다음과 같은 벤치마크가 제공됩니다.

사양: i5-6200U 8gbram 창 10

DllPlugin 사용(평균 3개 빌드)
공급업체 번들 구축:
*3370ms

기본 번들을 빌드하는 중:
146.6ms

DllPlugin 미포함(평균 3개 빌드)
공급업체 번들 구축:
3312ms

기본 번들을 빌드하는 중:
3583.6ms

코딩 세션 시작 시에만 벤더 번들을 빌드하고 한 세션에 대해 100번 언급을 다시 로드한다고 가정할 때, 기다리는 총 시간은 다음과 같습니다.

DllPlugin 사용
3370+(187.6*100) = 18030ms

DllPlugin 미포함
3312+(3583.6*100) = 361672ms

제작 시간이 95% 단축되었습니다! 놀라운 생산성 향상을 실현합니다.

## 결론

이러한 최적화는 프로덕션 빌드에 적용되지 않습니다. 개발 속도를 높이기 위해 지정된 번들만 캐시합니다.

튜토리얼 코드는 GitHubrepo를 참조하십시오.