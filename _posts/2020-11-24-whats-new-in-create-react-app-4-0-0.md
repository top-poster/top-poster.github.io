---
layout: post
title: "react-app 4.0의 새로운 기능"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/createreactapp4.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/createreactapp4.png?fit=730%2C487&ssl=1)

반응 애플리케이션 생성은 단일 페이지 반응 애플리케이션(SPA)을 생성하는 데 권장됩니다. React에 의해 공식적으로 지원되며 아무런 구성 없이 현대적인 빌드 설정을 제공합니다.

이 기능을 사용하면 단일 명령으로 최신 React 앱을 부트스트랩할 수 있습니다. Create React App은 npm과 yarn을 모두 지원하므로 사용할 패키지 관리자에 따라 이 명령은 약간 달라질 수 있습니다.

새 React 앱을 생성하려면 아래 명령 중 하나를 실행하면 됩니다.

```undefined
npx create-react-app my-app
```

또는

```coffeescript
npm init react-app my-app
```

또는

```undefined
yarn create react-app my-app
```

또한 Create React App은 사전 구성 및 숨겨진 Webpack 및 Babel 설치 및 구성과 같은 까다로운 작업을 제거하여 코딩에 집중할 수 있습니다. 따라서 학습할 내용이 적어지고 반응 앱을 만들기 시작하기 위해 필요한 모든 작업은 위의 명령 중 하나를 실행하는 것입니다.

Create React App에는 Webpack 및 Babel에 대한 사전 구성이 함께 제공되지만 자체 구성을 사용하도록 강제하지는 않습니다. Create React App에서 `탈퇴`하고 사용자 지정 구성을 원하는 대로 추가할 수 있습니다.

버전 4.0은 몇 가지 흥미로운 기능을 갖춘 주요 릴리스입니다.

`create-react-app`을 이미 전체적으로 설치한 경우 다음을 실행해야 합니다.

```undefined
npm uninstall -g create-react-app
```

또는

```undefined
yarn global remove create-react-app
```

제거할 수 있습니다. 그런 다음 실행:

```undefined
npx create-react-app my-app
```

앞으로 npx는 항상 최신 버전을 사용해야 한다. 여기서 더 자세히 보실 수 있습니다. 이전 버전을 사용하는 프로젝트가 있는 경우 마이그레이션에 대한 다음 절의 지침을 볼 수 있습니다.

## 마이그레이션

버전 3.4.x에서 버전 4.0.0으로 마이그레이션하는 경우 프로젝트 내부에서 배출되지 않은 다음을 실행하십시오.

```css
npm install --save --save-exact react-scripts@4.0.0
```

또는

```css
yarn add --exact react-scripts@4.0.0
```

이렇게 하면 대응 스크립트가 원활하게 업데이트됩니다. 오류가 발생할 경우 다음을 실행하여 `node_modules` 폴더를 삭제하고 종속성을 다시 설치하는 것이 좋습니다.

```undefined
npm install or yarn install
```

이렇게 하면 오류 없이 마이그레이션이 완료됩니다. 다음 섹션의 새로운 기능으로 넘어가겠습니다.

## 새로운 기능

### 빠른 새로 고침

빠른 새로 고침은 이전 기능인 핫 재로드의 공식 리액션 구현입니다. 이것은 핫 재로드와 비슷하지만 훨씬 더 안정적이다.

빠른 새로 고침을 사용하면 아래 이미지와 같이 실행 중인 앱에서 React 구성 요소를 상태를 잃지 않고 편집할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/imagebyadamcharron.gif?resize=640%2C371&ssl=1)

간단히 말해 React 구성 요소를 실시간으로 조정할 수 있습니다. 리액트 핫 로더(react-hot-loader)와 비슷하다.

그러나 전작에 비해 패션 리프레쉬의 주요 장점 중 하나는 훅을 핫 재로드하고 주 보존을 가능하게 한다는 것이다.

Create React App 4.0.0은 react-refresh 패키지와 react Refresh 웹 팩 플러그인을 사용하여 이를 구현합니다.

### 대응 17(새로운 JSX 변환) 및 TypeScript 지원

Create React App 4.0.0은 새로운 JSX 변환 및 React 17에 대한 지원을 제공합니다. 새로운 JSX 변환은 JSX 구문을 변경하지 않지만 React를 크게 개선합니다.

브라우저가 JSX를 즉시 이해하지 못하기 때문에, 리액트 개발자들은 컴파일러에 의존하여 그들의 JSX를 브라우저가 이해할 수 있는 리액트 함수 호출로 변환한다. Babel 및 TypeScript와 같은 컴파일러는 대부분 이 용도로 사용됩니다.

또한 Create React App과 같은 툴킷도 JSX 변환과 함께 제공되며 4.0.0은 TypeScript 4.1.0을 통해 새로운 JSX 변환을 지원합니다.

새로운 JSX 변환으로 업그레이드하는 것은 선택 사항이지만 다음과 같은 몇 가지 흥미로운 이점이 있습니다.

- 새로운 JSX 변환을 사용하면 React를 가져오지 않고도 JSX를 사용할 수 있습니다.

알려진 대로 브라우저는 JSX를 이해하지 못하므로 자바스크립트 기능으로 컴파일해야 한다. 반응 코드가 다음과 같은 경우:

```js
import React from 'react';

function App() {
  return <p>Hi I am Lawrence Eagles</p>;
}
```

이전 `JSX` 변환은 이것을 다음과 같이 변환한다.

```php
import React from 'react';

function App() {
  return `('p', null, 'Hi I am Lawrence Eagles');
}
```

문제는 리액트.createElement 함수를 호출하여 리액트를 가져와야 한다는 점이다.

또한 React.createElement는 일부 성능 최적화 기술을 지원하지 않습니다. 따라서 JSX 변환의 구현은 개선될 여지가 있다.

다음 코드가 있는 경우 새로운 `JSX` 변환으로:

```js
function App() {
  return <p>Hi I am Lawrence Eagles</p>;
}
```

이를 통해 다음과 같은 이점을 얻을 수 있습니다.

```php
// Inserted by a compiler (don't import it yourself!)
import {jsx as _jsx} from 'react/jsx-runtime';

function App() {
  return _jsx('p', { children: 'Hi I am Lawrence Eagles });
}
```

> 위의 코드 예는 문서의 예입니다.

원래 코드는 React를 가져올 필요가 없었고 여전히 정상적으로 작동했습니다.

또한 구성에 따라 새 `JSX` 변환은 앱 번들 크기를 줄일 수 있습니다.

### ESLINT 7 지원

리액션은 후드 아래에 있는 ESLint를 사용하여 코드를 보풀로 만듭니다. React는 JavaScript 라이브러리이고 JavaScript는 느슨하게 입력되기 때문에 중요합니다. 개발 중이나 컴파일 시간에 오류가 잡히지 않고 런타임에 오류가 발생한다는 의미다.

ESLint와 같은 린터는 개발 중에 코드 보풀과 오류 캐치를 위한 메커니즘을 제공합니다. 생명의 은인이 될 수 있어요. 또한 린터를 사용하여 사용자 정의 규칙을 작성하여 코딩 스타일을 적용할 수 있습니다. 그러나 코딩 스타일을 시행하려면 더 예쁜 스타일을 사용하는 것이 좋습니다.

Create React App 4.0.0은 새로 출시된 ESLint 7을 지원하기 위해 업데이트된 `eslint-config-react-app`과 함께 제공됩니다. 또 수입/무익명-기본수출(import/no-anonymous-default-export), 제스트(Jest), 리액트 테스트 라이브러리(react testing library)에 대한 새로운 규칙도 마련했다.

React App 생성. 기본 ESLint 규칙을 확장하거나 교체할 수도 있습니다. 하지만 이것은 나쁜 벌레의 알려진 원인이기 때문에 그것을 교체하는 것은 바람직하지 않다.

기본 ESLint 규칙을 확장하기 전에 주의해야 할 몇 가지 사항은 다음과 같습니다.

- `error`로 설정된 모든 규칙이 애플리케이션 빌드 프로세스를 중단합니다.
- TypeScript 작업 시 TypeScript 파일만 대상으로 하는 `override` 개체를 제공해야 합니다.
- 앞에서 설명한 대로 ESLint 규칙을 확장하고 대체하지 않음

다음은 확장 ESLint 규칙의 예입니다.

```undefined
{
  "eslintConfig": {
    "extends": ["react-app", "shared-config"],
    "rules": {
      "additional-rule": "warn",
      "indent": ["error", 4]
    },
    "overrides": [
      {
        "files": ["**/*.ts?(x)"],
        "rules": {
          "additional-typescript-only-rule": "warn",
          "indent": ["error", "tab"]
        }
      }
    ]
  }
}
```

위의 규칙은 간단히 다음을 수행합니다.

- 먼저 `react-app` 확장:
`공유`: ["공유-앱", "공유-구성"]
- `공유`: ["공유-앱", "공유-구성"]
- 빌드 프로세스가 중지되지 않도록 추가 규칙을 "경고"로 설정합니다.
"종속 규칙": "종속"
- "종속 규칙": "종속"
- 일관된 들여쓰기 스타일을 `4 공백`으로 설정합니다.
`➡`: ["오류", 4]
- `➡`: ["오류", 4]
- TypeScript를 사용 중인 경우 `override` 섹션에 TypeScript 관련 구성을 배치합니다.
"➡": [
{
"파일": ["**/*.ts?(x)"),
"규칙": {
"secondal-type 스크립트 전용 규칙": "second",
"tabs": ["error", "tab"] // 탭 들여쓰기를 적용합니다.
}
}
]
- "➡": [
{
"파일": ["**/*.ts?(x)"),
"규칙": {
"secondal-type 스크립트 전용 규칙": "second",
"tabs": ["error", "tab"] // 탭 들여쓰기를 적용합니다.
}
}
]

여러분은 또한 여기서 이것에 대해 더 많이 배울 수 있습니다.

## 기타 주목할 만한 변경사항

- `typescript flag` 및 `NODE_PATH`에 대한 지원이 손실됨

TypeScript를 Create React App 4.0.0에 추가하려면 다음을 수행합니다.

```undefined
npx create-react-app my-app --template typescript
```

대신 다음과 같이 하십시오.

```undefined
npx create-react-app hello-tsx --typescript
```

또한 `NODE_PATH`에 대한 지원은 `jsconfig.json`에서 기본 경로를 설정하는 것으로 대체되면서 중단되었다.

- 제스트 26으로 업그레이드
- 2019년 말에 수명을 다하여 더 이상 지원되지 않는 노드 8에 대한 지원 중단
- Workbox injectionManifest 플러그인으로 전환되어 자체 저장소에서 `PWA` 템플릿을 독립화함

## 결론

Create React App 4.0.0은 주요 릴리스이며 몇 가지 멋진 기능을 제공합니다. 내가 가장 흥분되는 것은 빠른 리프레시이다. 별도의 패키지 없이 이것을 갖게 되어 정말 좋습니다.

새로운 JSX 변환도 훌륭한 추가이지만 구문이 그대로 유지되기 때문에 개발자의 경험을 향상시키지는 못한다.

다른 업데이트도 멋지고 마이그레이션 프로세스가 매끄럽다는 것은 아름다운 일입니다. 하나의 명령을 실행하여 이러한 모든 업데이트를 가져올 수 있습니다.

마지막으로, 이 버전에 대한 자세한 내용은 여기에서 확인할 수 있습니다.