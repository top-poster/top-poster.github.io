---
layout: post
title: "반응 스크린샷 테스트용 Jest 사용"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/Jest-_jest-image-snapshot-react-screenshot-test.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/Jest-_jest-image-snapshot-react-screenshot-test.png?fit=730%2C487&ssl=1)

스크린샷 테스트는 앱의 이전 UI와 새 UI 간의 스크린샷을 비교하는 것입니다. 일반적으로 두 가지 차이는 다음과 같이 빨간색으로 강조 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/screenshot-test.png?resize=730%2C183&ssl=1)

위의 스크린샷 중 하나는 텍스트의 색 변화로 인해 6414개의 다른 픽셀이 있기 때문에 테스트에 실패했습니다.

다른 모든 테스트에서는 브라우저에 표시된 이미지가 아닌 코드만 검사하기 때문에 현재 시각적 회귀 분석을 검사할 다른 방법은 없을 것입니다.

말하자면, 오늘날 React의 가장 인기 있는 두 가지 Jest 스크린샷 테스트 립은 jest-image-snapshot과 react-screen shot-test입니다.

## 농담-이미지-리액션-리액션-리액션-리액션-리액트-테스트

다음은 이러한 두 가지 테스트에 대한 간단한 소개와 비교입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/jest-vs-react-screenshot-testing-comparison.png?resize=495%2C230&ssl=1)

두 libs 모두 DevTools Protocol을 통해 헤드리스 크롬이나 크롬을 제어하기 위한 하이 레벨 API를 제공하는 Node.js 라이브러리인 Puppeteer를 사용하여 스크린샷을 찍을 수 있지만, jest-image-snapshot은 다른 스크린샷 도구에서도 작동할 수 있다.

jest-image-snapshot은 앱 측에 Puppeteer 구성이 필요한 반면, 반응 스크린-shot-test는 전체 프로세스를 캡슐화하여 스크린샷 테스트를 위한 자체 API를 제공한다.

이제 설정 및 사용 세부 정보 차이점에 대해 살펴보겠습니다.

## 설치

첫째, 국소 시험을 시작하려면 농담-이미지-스냅샷, 반응-스크린-샷 테스트 및 Puppeteer를 설치해야 한다.

```coffeescript
npm i jest-image-snapshot puppeteer
```

또는

```coffeescript
npm i react-screenshot-test puppeteer
```

## 세우다

Jest-image-snapshot을 설정하는 것은 Jest 기대치에 한 가지 추가 방법을 추가하는 것으로 요약된다.

```js
// setupTests.js

import { toMatchImageSnapshot } from "jest-image-snapshot";
expect.extend({ toMatchImageSnapshot });
```

반면 retact-screenhot-test 구성은 `jest.screenshot.config.js` 파일에 저장됩니다.

```java
// jest.screenshot.config.js

module.exports = {
  testEnvironment: "node",
  globalSetup: "react-screenshot-test/global-setup",
  globalTeardown: "react-screenshot-test/global-teardown",
  testMatch: ["**/?(*.)+(screenshot).[jt]s?(x)"],
  transform: {
    "^.+\\.[t|j]sx?$": "babel-jest", // or ts-jest
    "^.+\\.module\\.css$": "react-screenshot-test/css-modules-transform",
    "^.+\\.css$": "react-screenshot-test/css-transform",
    "^.+\\.scss$": "react-screenshot-test/sass-transform",
    "^.+\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga)$":
      "react-screenshot-test/asset-transform"
  },
  transformIgnorePatterns: ["node_modules/.+\\.js"]
};
```

babel.config.js와 쌍을 이루어야 합니다.

```java
// babel.config.js

module.exports = {
  presets: ["babel-preset-react-app"]
};
```

리액션 스크린샷 테스트 lib는 테스트 목적으로 리액션 앱의 일부를 빌드하지만 리액션 스크립트의 빌드 스크립트를 사용하지 않기 때문에 더 많은 구성이 필요합니다. 이 접근 방식은 일반 제스트 스냅샷 테스트와 마찬가지로 앱의 구성 요소를 격리하여 테스트할 수 있다.

## 농담-이미지-리액션-리액션-리액션-리액션-핫-테스트 예

농담-이미지-스냅샷 테스트는 다음과 같이 보입니다.

```undefined
// src/screenshot.test.js 

import puppeteer from 'puppeteer'

it('main page screenshot test', async () => {
    const browser = await puppeteer.launch({});
    const page = await browser.newPage();
    await page.goto('http://localhost:5000'); 

    const image = await page.screenshot();
    expect(image).toMatchImageSnapshot();
    await browser.close();
});
```

위의 코드는 표준 Puppeteer 워크플로입니다. 브라우저를 시작하고 페이지를 열고 탐색한 다음 마지막으로 스크린샷을 촬영합니다.

테스트와 관련된 유일한 코드 라인은 설정 섹션 `expect(image)`입니다.MatchImageSnapshot()을(를) 일치시키려면

이 테스트 코드 라인은 차이 감지 및 다른 이미지 생성, 또는 이전에 스크린샷을 촬영하지 않은 경우 새로운 스크린샷 생성을 담당합니다.

이제 테스트를 위한 자체 최소 및 편리한 API를 갖춘 반응 스크린샷 테스트를 살펴보겠습니다. 다음은 일반적인 테스트입니다.

```js
// src/index.screenshot.js

import React from "react";
import { ReactScreenshotTest } from "react-screenshot-test";
import "./index.css";
import App from "./index";

ReactScreenshotTest.create("App")
    .viewport("Desktop", {
        width: 1024,
        height: 768
    })
    .viewport("iPhone X", {
        width: 375,
        height: 812,
        deviceScaleFactor: 3,
        isMobile: true,
        hasTouch: true,
        isLandscape: false
    })
    .shoot("main page", <App />)
    .run();
```

이 접근 방식의 이점은 버튼 스타일과 크기 또는 모든 중단점에 대한 반응성 렌더링과 같이 가능한 많은 양의 상태를 가진 예에서 쉽게 확인할 수 있다.

예를 들어, 한 곳에 있는 모든 버튼 스타일에 대해 다음을 수행합니다.

```bash
ReactScreenshotTest.create("Buttons")
    .viewport("Desktop", {
        width: 1024,
        height: 768
    })
    .shoot("primary", <Button type="primary" />)
    .shoot("secondary", <Button type="secondary" />)
    .shoot("ghost", <Button type="info" />)
    .shoot("danger", <Button type="danger" />)
    .shoot("warning", <Button type="warning" />)
    .run();
```

한 곳에 5개의 중단점이 있는 경우:

```bash
ReactScreenshotTest.create("Header")
    .viewport("XS", {
        width: 365,
        height: 768
    })
    .viewport("S", {
        width: 768,
        height: 768
    })
    .viewport("M", {
        width: 1024,
        height: 768
    })
    .viewport("L", {
        width: 1200,
        height: 768
    })
    .viewport("XL", {
        width: 1600,
        height: 768
    })
    .shoot("responsive", <Header />)
    .run();
```

## 로컬에서 테스트 실행

jest-image-traft는 스크린샷 테스트를 실행하기 전에 "serve" 패키지를 사용하는 것과 같은 웹 서버를 사용하여 앱을 제공해야 한다. 이것은 서버용 콘솔 창과 테스트용 콘솔 창 두 개를 사용하여 로컬로 얻을 수 있으며, 또는 편의 패키지인 `시작-서버-앤-테스트`를 사용할 수도 있다.

최소 패키지.json은 다음과 같이 보여야 한다.

```undefined
{
  "dependencies": {
    "jest-image-snapshot": "latest",
    "puppeteer": "latest",
    "react": "latest",
    "react-dom": "latest",
    "react-scripts": "latest",
    "serve": "latest",
    "start-server-and-test": "latest"
  },
  "scripts": {
    "build": "react-scripts build",
    "serve": "serve -s build",
    "test": "react-scripts test",
    "test:jest-image-snapshot": "npm run build && start-server-and-test serve http://localhost:5000 test"
  }
}
```

`포장`에 따르면요.위의 json에서 스크린샷 테스트를 위해 실행해야 하는 유일한 명령은 다음과 같습니다.

```bash
npm run test:jest-image-snapshot
```

이제 반응 스크린샷 테스트 최소 `패키지`를 위해.json:

```undefined
{
  "dependencies": {
    "puppeteer": "latest",
    "react": "latest",
    "react-dom": "latest",
    "react-screenshot-test": "latest"
  },
  "scripts": {
    "test:react-screenshot-test": "SCREENSHOT_MODE=local jest -c jest.screenshot.config.js"
  }
}
```

`패키지`에 주목하십시오.json은 키가 작습니다. 테스트를 실행하기 위해 하나의 명령만 실행하면 됩니다.

```bash
npm run test:react-screenshot-test
```

## 요약

립 비교에서 분명한 승자는 없다: 반응 스크린샷 테스트와 농담 이미지 스냅샷 모두 그들만의 접근 방식을 사용하여 가치를 더한다.

e2e 테스트(예: 페이지의 텍스트 확인, 버튼 클릭 등)를 위해 프로젝트에서 Puppeteer를 이미 사용하거나 사용하려는 경우 jest-image-snapshot 패키지를 사용하는 것이 좋습니다.

켄트 C의 테스트용 금본위제입니다. "테스트가 소프트웨어 사용 방식과 비슷할수록 더 많은 자신감을 줄 수 있습니다."라는 Dodds가 여기에 완벽하게 적용됩니다.

한편, 반응 스크린샷 테스트 패키지의 가치는 앱의 모든 구성 요소를 하나의 테스트에서 다양한 옵션과 화면 해상도로 어떻게 보여야 하는지를 스크린샷과 쉽게 결합할 수 있다는 것이다.

만약 어떤 lib를 사용할지 결정하는 것이 어렵다면, 그들 둘 다 함께 잘 작동할 수 있다는 것을 명심하세요. 실제로 다음과 같은 CI 워크플로우 예제와 데모 프로젝트의 소스 코드가 있습니다.

```undefined
// .github/workflows/screenshot-tests.yml

name: Screenshot Tests

on:
    pull_request:
        branches:
            - main

jobs:
    screenshot:
        name: Main Page
        runs-on: ubuntu-16.04
        timeout-minutes: 5
        steps:
            - uses: actions/checkout@v2

            - name: Use Node.js
              uses: actions/setup-node@v1
              with:
                  node-version: "12"

            - name: Install Dependencies
              run: yarn

            - name: Jest Image Snapshot
              run: npm run test:jest-image-snapshot

            - name: React Screenshot Test
              run: npm run test:react-screenshot-test
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/react-screenshot-test.png?resize=730%2C259&ssl=1)

시험 잘 보셨어요!