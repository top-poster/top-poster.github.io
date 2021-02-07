---
layout: post
title: "코드 모드를 사용하여 대응 버전 업그레이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/react-codemod.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/react-codemod.png?fit=730%2C487&ssl=1)

대규모 코드베이스를 리팩토링하는 것은 지저분하고, 비용이 많이 들고, 지루할 수 있기 때문에 많은 엔지니어링 팀들은 특히 바람직하지 않지만 여전히 "효과"인 패턴을 다룰 때 이러한 작업을 수행할 가치가 없다고 판단합니다.

예를 들어, 바람직하지 않은 패턴은 여러 가지 방법으로 분할하는 것이 더 나은 함수일 수 있습니다. 몇 가지 선택적 매개 변수와 함께 사용하기 훨씬 더 쉬운 함수이거나, 현재 사용하는 부울 값 대신 더 많은 정보를 포함하는 구조를 반환하는 함수일 수 있습니다.

## 코드 모드 소개

대규모 코드베이스 리팩터를 지원하기 위해 구축된 파이썬 툴인 코드모드를 입력한다. 인간의 감독 및 때때로 수동 개입이 여전히 필요하지만, 프로세스를 부분적으로 자동화할 수도 있습니다.

페이스북에서 개발자들이 구축한 이 소프트웨어는 오픈소스 소프트웨어로 출시되었으며, 성숙된 코드베이스에 대한 전면적인 변화를 건전하고 체계적으로 도입하고자 하는 사람이라면 누구나 활용할 수 있다.

다음과 같이 즐겨찾기 Python 패키지 관리자를 사용하여 코드 모드를 설치할 수 있습니다.

```undefined
pip install codemod
```

sudo를 사용하여 시스템 전체에 패키지를 설치할 수도 있습니다.

```undefined
sudo -H pip install codemod
```

코드 모드의 유용성을 간단한 예를 들어 설명하겠습니다. 우리는 `font` 태그의 사용을 비난하고 싶다고 말한다. 명령줄을 가동하고 다음을 실행합니다.

```xml
codemod -m -d /Users/janedoe/reponame --extensions php,html \ 
'<font color="?(.?)"?>(.*?)</font>' \ 
'<span style="color: \1;">\2</span>'
```

각 정규식 일치에 대해 터미널에 컬러 디프가 표시됩니다. 그런 다음 변경 내용을 수락하라는 메시지가 표시됩니다(즉, `<font> 태그의 인스턴스를 `<span> 태그로 바꾸거나, 거부하거나, 편집기에서 지정된 줄을 편집하십시오.

이런 식으로 코드모드를 사용하여 코드베이스를 리팩터링하는 것은 좋아하는 워드프로세서에서 찾기-바꾸기를 수행하는 것과 비슷합니다.

## 사례 연구: 코드 모드를 사용하여 최신 React 버전으로 업그레이드

여러분과 여러분의 엔지니어 팀이 React를 사용하여 제품의 프런트 엔드를 구축했다고 가정해 보겠습니다. 잘 작동하고, 원활하게 작동하며, 이해관계자들은 현재 제품이 큰 차질 없이 그대로 유지되기를 원합니다.

제품을 빌드하기 시작했을 때 React의 최신 버전은 16.5였습니다. 그러나 이제 데이터, 이미지, 스크립트 및 기타 비동기식으로 로드된 요소를 포함한 특정 코드를 로드하고 그 동안 로드 상태(예: 스핀너 또는 화면의 일부 텍스트)를 지정할 수 있는 깔끔한 구성 요소인 서스펜스에 대해 알아보았습니다.

다음은 서스펜스 사용 사례를 보여 주는 간단한 예입니다.

```js
const demoPage = React.lazy(() => import('./demoPage'));
// Display a spinner while the page is loading 
<Suspense fallback={<Spinner />}> <DemoPage /> </Suspense>
```

문제는 서스펜스가 리액트 버전 16.6 이상에서만 사용할 수 있고 전체 제품이 버전 16.5에 내장되어 있다는 점입니다. 업데이트로 모든 것을 깨면 제품 소유자와 다른 이해 관계자들이 분노할 것은 말할 것도 없습니다.

그래서 상황은 절망적이다. 리액트가 완전히 쇄신하고 시간과 방대한 자원을 리팩터링에 쏟을 수밖에 없을 정도로 발전하기 전까지는 당신은 영원히 구버전으로 운명지어져 있을 겁니다. 그렇죠?

틀렸습니다. 코드 모드를 사용하면 관리 상태를 유지하고 변경 사항을 관리하면서 번거롭지 않게 업그레이드할 수 있습니다.

### jcodeshift 및 React-Code 모드 설치

페이스북은 개발자들이 리팩터를 시작하는 데 사용할 수 있는 여러 코드모드 스크립트를 제공했다.

### 업그레이드 버전 16.9에 대응

먼저 다음을 실행합니다.

```coffeescript
npm install --save react@latest
```

또는 실도 사용할 수 있습니다.

```undefined
yarn upgrade react@latest
```

좋아요! 이제 리액션 16.9가 설치되었고 서스펜스를 최대한 활용할 수 있습니다.

그러나 React 16.9에는 중단 변경 사항이 포함되어 있지 않지만 다음과 같이 이름을 바꾼 안전하지 않은 라이프사이클 방법이 몇 가지 있습니다.

- `component WillMount` → `Unsafe_component WillMount`
- `구성 요소 수신 예정` → `UNSafe_구성 요소 수신 예정`
- `componentWillUpdate` → `Unsafe_componentWillUpdate`

그래서 이제 어쩌지금은?

### 반응 코드 모드 설치

다음을 실행합니다.

```coffeescript
npm i react-codemod
```

이렇게 하면 React-Codemode가 설치되는데, 이 스크립트는 React APIs를 업데이트하기 위해 특별히 만들어진 jscodeshift와 함께 사용할 수 있다. 이 스크립트를 설치한 후에는 Facebook에서 제공하는 다양한 스크립트를 사용하여 클린 리팩터를 실행할 수 있습니다.

응용프로그램이 안전하지 않은 라이프사이클 방법을 많이 사용하는 경우 다음 이름과 일치하도록 세심하게 편집하는 대신 빠르고 편리한 리팩터링을 위해 이 코드 모드를 사용할 수 있습니다.

```undefined
cd your_app_respository
npx react-codemod rename-unsafe-lifecycles
```

위에서는 이전 라이프사이클 메서드 명명 규칙의 모든 항목을 찾아서 교체하는 데 사용할 수 있는 대화형 프롬프트가 시작됩니다.

## J 코드 시프트에 대한 빠른 소개

반응 코드 모드에 대한 논의는 jcodeshift를 도입하지 않고는 완료될 수 없다. React 16.9로 업그레이드하기 위해 jcodeshift를 사용할 필요는 없었지만, React Codemod 스크립트와 자바스크립트로 작업할 때 필수적인 툴킷입니다. 이 도구는 여러 JavaScript(또는 TypeScript) 파일에서 코드 모드를 실행하는 방식으로 작동합니다.

그러나 js코드 이동은 일반적인 코드 모드 찾기 및 교체 스크립트를 훨씬 능가하므로 개발자는 코드베이스에 대한 주요 업데이트를 수행하고 자동화할 수 있다. 업데이트에는 함수 서명을 변경한 다음 호출되는 함수의 모든 인스턴스에서 코드를 다시 쓰는 등의 작업이 포함될 수 있습니다.

명령줄에서 다음을 실행하여 jcodeshift를 설치할 수 있습니다.

```coffeescript
npm install -g jscodeshift
```

다음 예는 일부 변수 "foo"가 발생할 때마다 "bar"로 교체하는 간단한 사용 사례입니다.

```js
/**
 * Using jscodeshift to replace all occurrences of variable "foo" with "bar"
 */
module.exports = function(file, api) {
  return api
    .jscodeshift(file.source)
    .findVariableDeclarators("foo")
    .renameTo("bar")
    .toSource();
};
```

## 결론

React는 빠르게 움직이는 라이브러리로, 개발자 팀은 제품 및 코드베이스의 무결성을 유지하면서 업데이트를 계속하는 것이 중요합니다. 이 기사에서는 대규모 코드베이스를 리팩터링하고 업그레이드할 수 있는 대화식 방법을 제공하는 스크립트가 있는 파이썬 도구인 코드모드에 대해 살펴봤다.

또한 React API 업그레이드 전용 스크립트를 포함하는 npm 패키지인 React-Codemod도 검토했다. 마지막으로, 보다 강력한 자동화 기능을 갖춘 코드 모드를 기반으로 하는 툴킷인 jcodeshift에 대해 다루었다.

이러한 추가 툴을 활용하면 리팩토링 코드 및 React 버전 업그레이드를 쉽고 간편하게 수행할 수 있으므로, 팀이 코드베이스를 손상시키거나 개발을 지연시키지 않고도 최첨단 업데이트를 즐길 수 있습니다.