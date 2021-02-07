---
layout: post
title: "링귀지를 이용한 반응에서 국제화를 설정하는 방법.js"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/Untitled-design-12.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/Untitled-design-12.png?fit=730%2C487&ssl=1)

## 개요

국제화(i18n) 및 현지화는 응용 프로그램의 범위를 확장하기 위해 소프트웨어 개발에 매우 중요합니다. 때로는 특정 언어가 내장된 앱을 만들어야 하는 경우도 있습니다. 예를 들어, 대부분의 정부 웹사이트 페이지는 그들 나라의 지역 언어와 국가 언어로 번역된다.

대부분의 웹사이트가 자바스크립트에 의존하고 있는 상황에서, 애플리케이션의 국제화를 처리하기 위해 많은 인기 있는 자바스크립트 라이브러리가 설치되어 있는 것은 놀랄 일이 아니다. 가장 잘 알려진 라이브러리 중 일부는 다음과 같습니다.

- 링귀리.js
- 포맷JS
- i18 다음
- 글로벌화
- i18n

이 글의 목적을 위해 Lingui.js 사용법을 설명하겠습니다. 이것은 네이티브 자바스크립트 애플리케이션뿐만 아니라 가장 잘 알려진 자바스크립트 라이브러리 중 하나인 리액트에서도 국제화를 처리하는 강력한 라이브러리이다. Lingui.js는 5KB의 가벼운 크기로 초보자들도 쉽게 시작할 수 있습니다.

## 링귀지를 소개합니다.js

Lingui.js는 비교적 새로운 국제화 라이브러리이지만, react-intland i18 next와 같은 인기 있는 경쟁사 라이브러리에서는 사용할 수 없는 몇 가지 기능을 제공한다. 예를 들어 Lingui.js는 구성 요소의 메시지 구문을 생성하는 데 사용할 수 있는 매크로로 꽉 차 있습니다. Lingui.js의 다른 두드러진 특징은 다음과 같다.

- 성능에 최적화됨(크기가 5KB에 불과함)
- React 및 React Native 애플리케이션과 쉽게 통합
- Spectrum뿐만 아니라 GitHub에서도 커뮤니티가 활동하고 있습니다.
- Lingui.js는 단계별 튜토리얼 및 API 참조와 함께 철저한 문서를 제공합니다.
- UI 변환을 읽기 쉽게 만드는 ICU 메시지 형식을 사용합니다.
- 많은 경쟁 라이브러리에 없는 리치 텍스트 지원 제공
- Lingui.js는 사용자 인터페이스를 변환하는 전체 프로세스를 능률화하는 자체 CLI를 가지고 있습니다.

일반적으로 Lingui.js는 모든 유형의 정적 애플리케이션에 통합될 수 있다. 채팅 시스템이나 온라인 게임과 같은 실시간 응용 프로그램이 있다면 Lingui.js는 리소스를 많이 사용하는 응용 프로그램을 위해 효율적으로 설계되었기 때문에 좋은 선택이 될 수 있습니다. 예를 들어 Caliopen(통합 메시징 플랫폼)은 Lingui.js를 적극적으로 사용하고 있다.

이 기사에서는 Return 애플리케이션 내에 Lingui.js를 통합하는 샘플 프로젝트를 진행할 것입니다. 보다 쉽게 하기 위해, 우리는 react-app 패키지를 사용할 것입니다.

## 전제조건

이 튜토리얼을 수행하려면 시스템에 Node.js가 이미 설치되어 있어야 합니다. 최신 버전은 npm 및 npx와 같은 툴과 함께 제공되므로 별도로 설치할 필요가 없습니다.

## 시작 중

첫 번째 단계는 최신 버전의 앱 패키지를 로컬 컴퓨터에 설치하는 것입니다. 이렇게 하려면 시스템의 원하는 폴더 내에서 콘솔/터미널을 엽니다. 그런 다음 다음 명령을 실행합니다.

```undefined
npx create-react-app my-app
```

모든 종속성을 설치하고 기본 React 앱을 설정하는 데 시간이 걸리므로 조금만 기다려 주십시오. 설정이 완료되면 다음 명령을 실행합니다.

```bash
cd my-app
```

이제 프로젝트 폴더 안에 들어가서 Lingui.js를 설치할 준비가 됩니다.

본질적으로, Lingui.js의 개발자들은 전체 라이브러리를 다른 모듈로 나누었다. 현재로서는 CLI, 매크로, React에 관심이 있습니다. 우리는 또한 Babel의 이전 버전을 지원하기 위해 Babel 컴파일러 코어와 Babel-babel compiler core와 babel-bridge가 필요하다.

React 프로젝트에 다음과 같은 모든 종속성을 설치하겠습니다.

```coffeescript
npm i --save-dev @lingui/cli @lingui/macro @babel/core babel-core@bridge
npm i --save @lingui/react
```

## Lingui.js 구성

메시지 파일의 위치, 사용할 형식 및 기본 UI 언어를 알려 Lingui.js를 설정할 때입니다. 이렇게 하려면 프로젝트 루트 내에 .linguirc라는 파일을 만들어야 합니다. 이 파일의 내용은 다음과 같아야 합니다.

```undefined
{
    "localeDir": "src/locales/",
    "srcPathDirs": ["src/"],
    "format": "minimal",
    "sourceLocale": "en"
}
```

src 디렉터리는 메시지의 원본 파일을 보관하는 반면 Lingui.js는 이러한 파일에서 메시지를 추출하여 언어별 폴더에 씁니다. 각 언어(예: 영어, Urdu, 중국어 등)는 `src/locales/` 폴더 내에 자체 하위 폴더를 가집니다.

또한, 메시지 번역을 위해 올바른 형식을 선택하는 것이 중요합니다. 사용 가능한 형식에는 다음이 포함됩니다.

- ➡i: 각 메시지는 다음과 같이 미리 정의된 형식으로 반환됩니다.
- 최소: 각 메시지는 다음과 같은 간단한 JSON 형식으로 반환됩니다.
- po: 각 메시지가 파일로 반환됩니다.

Lingui.js는 po 형식을 권장합니다. 일반적으로 개인 선호도와 현재 응용 프로그램의 설정에 따라 다릅니다. 우리들 대부분은 다른 사람들보다 `최소`(즉, JSON) 형식이 읽기 쉽다고 생각할 수 있다. 이 튜토리얼에서는 `최소` 형식을 사용합니다. 각 형식에 대한 자세한 내용은 문서의 이 섹션을 참조하십시오.

## 패키지를 업데이트합니다.json의

이 때 일부 스크립트를 `패키지`에 추가해야 합니다.json의

```undefined
{
    "scripts": {
      "add-locale": "lingui add-locale",
      "extract": "lingui extract",
      "compile": "lingui compile"
    }
}
```

위의 명령으로, 우리는 새로운 로케일을 생성할 수 있을 뿐만 아니라 우리의 응용 프로그램에서 현재 로케일을 컴파일하고 추출할 수 있을 것이다.

## 사용자 인터페이스 언어 추가

`add-locale` 스크립트를 실행하면 사용자 인터페이스에 대해 다른 언어를 만들 수 있습니다.

```coffeescript
npm run add-locale en zh ur
```

이를 위해 공식 IANA 웹 사이트에서 지원되는 언어 코드를 볼 수 있습니다.

## 웹 페이지 만들기

CRA 앱은 이미 기본 홈페이지와 함께 제공됩니다. 이 파일은 `/src/App.js`에서 찾을 수 있습니다. 이 파일을 다음과 같이 수정합니다.

```js
import React, { useState, useEffect } from 'react';
import { Trans } from '@lingui/macro';

import LanguageSelector from './components/LanguageSelector';

function App({ language, onLanguageChange }) {

  return (
    <div className="App">
      <LanguageSelector
        language={language}
        onChangeLangage={onLanguageChange}
      />
      <header className="App-header">
        <h1><Trans>Name of Countries</Trans></h1>
      </header>
      <ul>
        <li><Trans>United States</Trans></li>
        <li><Trans>Finland</Trans></li>
      </ul>
    </div>
  );
}

export default App;
```

Language Selector 구성 요소를 사용하고 있다는 사실을 알고 계십니까?

이를 통해 사용자 인터페이스의 언어를 동적으로 변경할 수 있습니다. 간단한 HTML 선택 드롭다운을 사용합니다. 이 구성 요소는 다음과 같습니다.

```js
import React from 'react';

function LanguageSelector({ language, onChangeLangage }) {

  function handleChange(event) {
    event.preventDefault();
    onChangeLangage(event.target.value);
  }

  return (
    <div className="select">
      <select onChange={handleChange} value={language}>
        <option value="en">English</option>
        <option value="ur">Urdu</option>
        <option value="zh">Chinese</option>
      </select>
    </div>
  );
}

export default LanguageSelector;
```

마지막으로, 메인 `/src/index.js` 파일을 업데이트해야 합니다. 이 파일은 선택한 언어의 번역을 가져오고 전체 웹 페이지를 렌더링하는 역할을 합니다.

```js
import React, { useState } from 'react';
import ReactDOM from 'react-dom';
import { I18nProvider } from '@lingui/react';
import App from './App';

async function loadMessages(language) {
  return await import(`@lingui/loader!./locales/${language}/messages.json`);
}

function LocalizedApp() {
  const [catalogs, setCatalogs] = useState({})
  const [language, setLanguage] = useState('en');

  async function handleLanguageChange(language) {
    const newCatalog = await loadMessages(language);

    const newCatalogs = { ...catalogs, [language]: newCatalog };

    setCatalogs(newCatalogs);
    setLanguage(language);
  }

  return (
    <I18nProvider language={language} catalogs={catalogs}>
      <App
        language={language}
        onLanguageChange={handleLanguageChange}
      />
    </I18nProvider>
  );
}

ReactDOM.render(<LocalizedApp />, document.getElementById('root'));
```

## 소스 코드에서 메시지 추출

이제 일부 텍스트를 `Trans … </Trans` 매크로 안에 넣었으므로 소스 코드에서 모든 메시지를 추출한 다음 각 로케일에서 키-값 쌍을 만들어야 합니다. 키는 값이 특정 언어로 번역되는 ID로 작동합니다.

영어에서 키와 값 쌍은 동일하게 유지됩니다. 하지만 우리는 다른 언어들의 가치관을 바꿔야 합니다. 기본적으로 해당 값은 빈 문자열이 되며 변환을 수동으로 추가해야 합니다.

`npm run extract` 명령을 실행하여 소스 코드에서 메시지를 추출합니다.

## 사용자 인터페이스 변환

다음 두 국가의 목록을 표시하는 웹 페이지를 만들었습니다. 핀란드와 미국. 이제 사용자 인터페이스를 지원되는 모든 언어로 변환해야 합니다.

이전에 우리는 프로젝트에 영어, 중국어, 우르두어 로케일을 추가했습니다. 기본 언어는 영어이며, Lingui.js는 자동으로 영어 번역을 채울 만큼 충분히 똑똑합니다.

```undefined
{
  "Finland": "Finland",
  "Name of Countries": "Name of Countries",
  "United States": "United States"
}
```

이제 `/src/locals/ur/messages.json`을 열고 내용을 아래 코드로 바꿉니다.

```undefined
{
  "Finland": "فن لینڈ",
  "Name of Countries": "ممالک کا نام",
  "United States": "امریکہ"
}
```

마찬가지로 `/src/locales/zh/messages.json`을 열고 다음과 같은 번역으로 업데이트한다.

```undefined
{
  "Finland": "芬兰",
  "Name of Countries": "国家名称",
  "United States": "美国"
}
```

이 때, 우리는 우리의 응용 프로그램에서 사용할 수 있도록 번역을 컴파일할 준비가 되었습니다. 이렇게 하려면 compile 명령 `npm run compile`을 실행합니다.

## 프로젝트 실행

응용 프로그램을 실행하려면 `npm start` 명령을 실행합니다.

## 결론

함께, 우리는 Lingui.js를 React 앱과 통합하고 애플리케이션에 국제화 기능을 추가하는 것이 얼마나 쉬운지 배웠습니다.

Lingui.js의 가장 좋은 이점 중 하나는 모든 언어에 대한 변환 키-값 쌍을 자동으로 관리할 수 있다는 것이다. 이렇게 하면 런타임에서 오류를 방지하기 위해 빈 문자열로 메시지가 초기화됩니다. 그런 다음 다양한 키 값에 대한 특정 번역을 추가하고 응용 프로그램을 새 언어로 볼 수 있습니다.