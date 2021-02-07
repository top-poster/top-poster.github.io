---
layout: post
title: "작동 중인 복구: 재사용 가능한 코드 블록 구성요소 작성"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/react-recoil-in-action.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/react-recoil-in-action.png?fit=730%2C487&ssl=1)

이 튜토리얼에서는 블로그 또는 개발자 문서 프로젝트에 대한 다국어 지원을 통해 코드 조각 블록을 작성하는 방법에 대해 설명합니다. VSCode 인터페이스와 비슷하게 보이도록 코드 블록을 스타일링하겠습니다. 또한 React Recoil을 사용하여 사용자의 언어 기본 설정을 기억함으로써 UX를 개선할 수 있습니다.

## 세우다

먼저 이 튜토리얼에서는 반응을 기본적으로 이해하고 있다고 가정합니다. 그런 다음 노드가 설치되어 있는지 확인해야 합니다. 버전 8이면 됩니다. 마지막으로, 후속 조치를 원하시면 시작 레포에 대한 링크가 있습니다.

복제된 프로젝트는 다음과 유사한 폴더 구조를 가져야 합니다.

```undefined
|----- src
  |----- components
    |----- Codeblock
      |----- Codeblock.jsx
      |----- Codeblock.scss
    |----- CodeGroup
      |----- CodeGroup.jsx
      |----- CodeGroup.scss
  |----- views
    |----- Home.jsx
    |----- About.jsx
  |----- utils
  |----- store
      |----- index.js
  |----- index.js
  |----- App.js
  |----- App.scss
```

다음과 같은 노드 종속성이 미리 설치되어 있으며, starter repo는 create-react-app에서 부트스트래핑됩니다.

- `node-contract`: 스타일링을 하는 동안 바닐라 CSS 대신 SCSS를 쓰는 데 도움이 됩니다.
- 프리즘 리액트렌더: 경량 프리즘 리액트 래퍼. 이를 통해 구문 강조 표시가 있는 사용자 지정 코드 블록을 구축할 수 있습니다.

## ReactRecoil을 사용하여 코드 조각 구성 요소 빌드

코드 조각 구성 요소는 전체 사이트에서 재사용할 수 있습니다. 코드 조각들을 자주 공유해야 하는 사용자 정의 개발자 문서 또는 기술 블로그가 완벽한 사용 사례입니다.

가독성을 높이기 위해 논리를 두 가지 요소로 나눕니다.

- `코드 스니펫`: 코드를 표시하고 코드 구문 강조 표시를 처리합니다.
- `코드그룹`: 다국어 지원을 추가하는 동안 `코드스냅` 구성 요소를 포장합니다.

아래 코드로 `CodeSnipet.jsx` 파일을 편집합니다.

```js
import React from 'react';
import Highlight, { defaultProps } from 'prism-react-renderer';

// Import popular vscode nightOwl theme
import theme from 'prism-react-renderer/themes/nightOwl';
import './CodeSnippet.scss';

const supportedSyntax = {
  curl: 'bash',
  node: 'js',
  js: 'js',
  ruby: 'rb',
  php: 'php',
};

const Codeblock = ({ code, syntax, className }) => (
  <Highlight
    {...defaultProps}
    theme={theme}
    code={code.trim()}
    language={supportedSyntax[syntax]}>
    {({ style, tokens, getLineProps, getTokenProps }) => (
      <pre className={`c-pre ${className}`}>
        {/* Tokens are equivalent to each row/line of code text */}
        {tokens.map((line, index) => (
          <div
            className={'c-line'}
            key={index} {...getLineProps({ line, key: index })}>
            <span className="c-line-number">{index + 1}</span> {/* Show code line number */}
            <span className="c-line-content">
              {/* Show code snippet for that line */}
              {line.map((token, key) => (
                <span key={key} {...getTokenProps({ token, key })} />
              ))}
            </span>
          </div>
        ))}
      </pre>
    )}
  </Highlight>
);
export default Codeblock;
```

위의 코드는 프리즘-리액트-렌더(prism-react-renderer)의 하이라이트(highlight)와 디폴트(default props)를 확장한 것이다. 하이라이트(Highlight)는 리액트(Retact) 구성 요소로, 가장 중요한 것은 다음과 같습니다.

- `code`(문자열): 표시할 텍스트/코드 조각
- `언어`(문자열): 이것은 프리즘-리액트-렌더 언어별 구문 하이라이트를 설정하는 데 도움이 된다. 지원되는 언어 목록은 프리즘 문서를 참조하십시오.
- `➡`(객체): 이 선택적 매개 변수를 사용하여 프리즘에서 기본 코드 테마를 재정의할 수 있습니다. 우리의 경우 5행의 `밤새` 테마로 재정의한다.

기본 Props 개체는 하이라이트에 필요한 기타 모든 소품을 처리하므로 변경할 필요가 없습니다. 지원되는 소품 전체 목록은 설명서를 참조하십시오.

> 팁: 불필요한 공백을 제거하려면 코드 문자열에 네이티브 JavaScript '.trim() 메서드를 사용하는 것이 좋습니다.

아래 코드로 `CodeGroup.jsx` 파일을 편집합니다.

```js
import React, { useState, useEffect, useRef } from 'react';

import CodeSnippet from '../CodeSnippet/CodeSnippet';
import './CodeGroup.scss';

const languageOptions = {
  curl: 'cURL',
  node: 'NodeJS',
  php: 'PHP',
};

const CodeGroup = ({ code }) => {
  const select = useRef();
  // Set the default language //
  const [language, setLanguage] = useState('curl');
  const handleChange = event => setLanguage(event.target.value);
  
  // Update the select value when langauge state changes
  useEffect(() => {
    select.current.value = language;
  }, [language]);

  return (
    <code className="c-code-group">
      <span className="c-dropdown">
        <select
          ref={select}
          name="codeSelect"
          defaultValue={language}
          onChange={handleChange}>
          {Object.keys(code).map((lang, index) => (
            <option
              key={index}
              value={lang.toLowerCase()}>{languageOptions[lang]}</option>)
          )}
        </select>
      </span>
      <>
        {Object.keys(code).map((lang, index) => (
          <CodeSnippet
            key={index}
            code={code[lang]}
            className={language === lang ? 'c-snippet c-snippet--is-active' : 'c-snippet'}
            syntax={lang} />
        ))}
      </>
    </code>
  );
};

export default CodeGroup;
```

CodeGroup` 구성 요소는 CodeSnipet 구성 요소를 감싸고 사용자 정의 코드 블록에 다국어 지원을 추가합니다.

CodeGroup` 구성 요소는 유형 객체의 `code` prop를 수락합니다. 이것이 우리가 표시하고자 하는 코드입니다. 개체 `key`는 언어 이름이며, 값은 해당 언어에 대해 표시할 해당 문자열입니다.

단순성을 위해 이러한 개체를 `utils/{page_name}Requests.js` 파일에 저장하고 필요할 때 가져옵니다. 다음은 샘플 코드 개체입니다. 다음 `utils/HomeRequests.js` 디렉토리에서 찾을 수 있습니다.

```php
const curl = `
curl -v -X POST https://api.sandbox.paypal.com/v2/invoicing/generate-next-invoice-number \\
-H "Content-Type: application/json" \\
-H "Authorization: Bearer Access-Token"
`;
const node = `
const https = require('https');
const options = {
  hostname: 'api.paystack.co',
  path: '/transaction/verify/:reference',
  method: 'GET',
  headers: {
    Authorization: 'Bearer SECRET_KEY'
  }
};
`;
const php = `
$stripe = new \Stripe\StripeClient('sk_test_BQokikJOvBiI2HlWgH4olfQ2');
$customer = $stripe->customers->create([
  'description' => 'example customer',
  'email' => 'email@example.com',
  'payment_method' => 'pm_card_visa',
]);
echo $customer;
`;
const code = {
  curl,
  node,
  php,
}
export default code;
```

다음은 앱의 작업 데모입니다.

위의 데모에서 코드 조각은 여러 페이지에서 작동합니다. 그러나 페이지를 탐색할 때 코드 언어 옵션이 재설정되므로 사용 편의성에 적합하지 않을 수 있습니다. 그곳이 코칠이 놀러온 곳이야.

## 반동으로 언어 기본 설정 저장

Recil은 Redux와 같이 가벼운 React 상태 관리 라이브러리입니다. Recoil은 간단한 API를 가지고 있으며, 후드 아래에서 React Hooks에 의해 구동된다. 복구 기능을 사용하면 여러 페이지에 걸쳐 언어를 유지하거나 사용자가 웹 사이트로 돌아올 때 선호하는 언어를 기억할 수 있습니다.

### Recoil 시작

먼저 yarn add Recoil을 실행하여 프로젝트에 Recoil을 추가합니다. `src/index.js` 파일에서 RecoilRoot를 가져오고 `App` 구성 요소를 `RecoilRoot`로 포장합니다.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom';
import { RecoilRoot } from 'Recoil';

import './index.css';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <BrowserRouter>
      <RecoilRoot> // Wrap the App component with RecoilRoot
        <App />
      </RecoilRoot>
    </BrowserRouter>
  </React.StrictMode>,
  document.getElementById('root')
);
```

리액트 리코일은 원자 및 셀렉터 모델을 기반으로 한다. Recoil에서 원자는 구성 요소가 가입할 수 있는 상태의 단위이며, 선택기는 상태를 동기식 또는 비동기식으로 업데이트하는 데 사용되는 순수한 함수이다. Redux와 같은 다른 상태 관리 라이브러리에 익숙하다면 원자는 상태 개체와 비슷합니다. 여기서 더 읽어보세요.

아래의 코드로 `store/index.js` 파일을 편집합니다.

```js
import { atom } from 'Recoil';

export const codeLanguageState = atom({
  key: 'snippetLanguageState', // Unique state identifier
  default: 'curl', // Default state value
});
```

원자 함수는 JavaScript 개체를 수락합니다.

- `key`는 고유 식별자입니다.
- 이름에서 알 수 있듯이 `default`가 기본 상태입니다.

이제 Recoil의 use RecoilState 후크를 사용하여 코드 언어 상태 원자를 읽고 쓸 수 있다.

다음 업데이트를 사용하여 `CodeGroup.jsx`를 편집합니다.

```undefined
import React, { useEffect, useRef } from 'react';
+ import { useRecoilState } from 'Recoil';
import { codeLanguageState } from '../../store/index';

import CodeSnippet from '../CodeSnippet/CodeSnippet';
import './CodeGroup.scss';

const languageOptions = {
  curl: 'cURL',
  node: 'NodeJS',
  php: 'PHP',
};

const CodeGroup = ({ code }) => {
  const select = useRef();
  // Set default langauge using 
-  const [language, setLanguage] = useState('curl');
+  const [language, setLanguage] = useRecoilState(codeLanguageState);
  const handleChange = event => setLanguage(event.target.value);
  useEffect(() => {
    select.current.value = language;
  }, [language]);
  return (
    <code className="c-code-group">
      <span className="c-dropdown">
        <select
          ref={select}
          name="codeSelect"
          defaultValue={language}
          onChange={handleChange}>
          {Object.keys(code).map((lang, index) => (
            <option
              key={index}
              value={lang.toLowerCase()}>{languageOptions[lang]}</option>)
          )}
        </select>
      </span>
      <>
        {Object.keys(code).map((lang, index) => (
          <CodeSnippet
            key={index}
            code={code[lang]}
            className={language === lang ? 'c-snippet c-snippet--is-active' : 'c-snippet'}
            syntax={lang} />
        ))}
      </>
    </code>
  );
};
export default CodeGroup;
```

위의 코드 조각에서 기존 `CodeGroup.jsx` 파일에 두 가지 추가 작업을 수행합니다.

- Recoil의 `사용 Recoil State`(2호선) 확장
- useState 후크를 use RecoilState 후크로 교체하고 우리의 상태(이 경우 codeLanguageState 원자)를 매개 변수로 전달한다(18행).

Recoil의 use RecoilState API는 React의 useState Hook과 동일하며, 주요 차이점은 구성 요소 간에 상태를 공유할 수 있다는 것이다.

만세! 우리 앱은 이제 여러 페이지에 걸쳐 언어 기본 설정을 저장합니다.

만약 당신이 여전히 시작 파일을 따라가고 있다면, 축하드립니다. 마지막 단계는 /views 디렉토리에 있는 home.jsx와 payments.jsx 파일로 이동하여 codeSnipet 구성 요소를 홈 앤드 페이먼트 페이지로 가져오는 것이다.

라이브 데모입니다.

## 반동 시 세션 지속성

위의 데모에서는 페이지 간을 탐색할 때 언어가 지속되더라도 페이지를 새로 고치면 언어가 `curl`로 재설정된다는 것을 알 수 있습니다. Recil 및 Recil-persist(타사 라이브러리)를 사용하여 다음 세 가지 간단한 단계로 세션 지속성을 앱에 추가할 수 있습니다.

yarn add recil-persist를 실행하여 앱에 recil-persist를 추가합니다.

store/index.js 파일에서 `codeLanguageState`를 `persistence_로 업데이트합니다.불안정` 키:

```undefined
import { atom } from 'Recoil';
export const codeLanguageState = atom({
  key: 'snippetLanguageState',
  default: 'curl',
+ persistence_UNSTABLE: {
+   type: 'snippetLanguageState' // type should be the same as the atom key
+ },
});
```

다음을 사용하여 `index.js` 파일을 업데이트하십시오.

```undefined
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom';
import { RecoilRoot } from 'Recoil';
+ import RecoilPersist from 'recoil-persist';
import './index.css';
import App from './App';
+ const { RecoilPersist, updateState } = RecoilPersist();


ReactDOM.render(
  <React.StrictMode>
    <BrowserRouter>
-     <RecoilRoot>
+     <RecoilRoot initializeState={updateState}>
+       <RecoilPersist />
        <App />
      </RecoilRoot>
    </BrowserRouter>
  </React.StrictMode>,
  document.getElementById('root')
);
```

## 결론

이제 앱이 완료되었습니다. 새로 고친 후에도 사용자가 마지막으로 선택한 언어를 기억할 수 있을 만큼 스마트합니다. 여기서 한번 써보세요. 여기 Github repo에 대한 링크가 있습니다. 마지막 코드를 가지고 놀고 싶으시다면요.