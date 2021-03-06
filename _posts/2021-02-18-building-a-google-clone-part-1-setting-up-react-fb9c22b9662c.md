---
layout: post
title: "Google 복제 작성 - 1부: 반응 설정"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Google clone](https://miro.medium.com/max/1200/1*BotAIpotlmG2qqgE6wWfUQ.gif)

개발자 역량을 한 단계 끌어올리시겠습니까? 아니면 미래의 고용주에게 깊은 인상을 주고 싶으십니까? 그럼 이 구글 클론을 만드세요!

이 4부작 시리즈에서는 완벽하게 작동하는 구글 검색 클론을 처음부터 구축하는 방법을 보여드리겠습니다. 막히더라도 걱정하지 마세요! 각 섹션 끝에 전체 코드를 제공하겠습니다. 또한 본 튜토리얼의 마지막 편에서는 GitHub의 전체 소스 코드에 액세스할 수 있도록 하겠습니다.

# 시작하기 전에

시작하기 전에 사용할 기술을 살펴보겠습니다.

이 튜토리얼에는 Visual Studio Code를 사용하겠지만, 코드 편집기는 어떤 것이든 사용할 수 있습니다. 저는 개인적으로 VS Code를 좋아합니다. 왜냐하면 V Code는 코딩이 훨씬 쉬워지기 때문입니다.

이제 그만 얘기해요. 우리 암호해!

# 1. 반응 앱 설정

먼저 Retact 앱을 만들고 설정합니다.

이 스크립트를 실행하여 웹 팩 및 Babel 구성을 처리할 필요 없이 새 React 응용 프로그램 시작 프로그램을 만들고 설정합니다. 끝의 점은 새 폴더를 만들지 않고 Retact 프로젝트를 동일한 폴더에 만들고 싶다는 것을 나타냅니다.

![Command in terminal](https://miro.medium.com/max/1200/1*mE47_0tz0B470bvdUJ26Yg.png)

이제 React 애플리케이션 시동기를 정리하여 Google 클론부터 시작하겠습니다.

![Files in public folder](https://miro.medium.com/max/438/1*9ALeFz16IrvsySh392sTTw.png)

![Files in src folder](https://miro.medium.com/max/450/1*aWjlQfY9Wp9VMVDvV13dOQ.png)

이제 터미널에 npm start를 입력하고 애플리케이션을 시작할 수 있습니다.

![React template](https://miro.medium.com/max/1160/1*hbIwNxFIFmVzbXYls5xH_Q.png)

React 템플릿이 드디어 설정되었으며 클론을 빌드할 준비가 되었습니다!

# 2. 홈페이지 설정

src 폴더에 `구성요소`와 `페이지`라는 두 개의 추가 폴더를 만들어 환경을 구성해보자. 구성요소 폴더에는 검색 줄과 같은 재사용 가능한 구성요소가 포함됩니다. 페이지 폴더에는 홈페이지와 검색 페이지가 포함됩니다.

![Two new folders in src folder](https://miro.medium.com/max/446/1*W8NhOTfl-7bBIcS-zXoP7A.png)

홈페이지부터 시작해보자!

헤더:

![Header](https://miro.medium.com/max/1396/1*ra6faoa3WC8rdG9FXmZb5w.png)

본문:

![Body](https://miro.medium.com/max/1392/1*ougAKK8-kaeBrJ8sabFPqA.png)

# 3. 홈페이지 헤더 작성

아이콘과 아바타를 렌더링하려면 재료 UI를 설치해야 합니다.

```js
npm 설치 @material-ui/core @material-ui/contract
```

```js
"@material-ui/core"에서 {Avatar} 가져오기;
"@material-ui/icons/Apps"에서 AppsIcon 가져오기;
```

```js
npm 설치 반응 영역
```

```js
ract-router-dom에서 {Link} 가져오기;
```

`<Link>` 구성 요소를 작동시키려면 React Router를 설정해야 합니다.

```js
"react-router-dom"에서 라우터, 라우팅, 스위치로 {BrowserRouter} 가져오기;
```

이제 `App.js` 파일은 다음과 같습니다.

우리는 방금 홈페이지의 헤더를 만들었습니다!

![Header](https://miro.medium.com/max/1396/1*ra6faoa3WC8rdG9FXmZb5w.png)

# 4. 홈페이지 로고 추가

이제, 로고 추가와 스타일링을 통해 홈페이지 본문을 만들어 보겠습니다.

마지막 `Home.css` 파일은 다음과 같습니다.

이제 `Home.js` 파일은 다음과 같습니다.

# 결론

이 부분은 여기까지입니다. 지금까지 수행한 작업을 요약해 보겠습니다.

다음 기사에서는 `<Search/>` 구성 요소를 만들어 구글 홈페이지를 완성하겠습니다. 채널 고정!