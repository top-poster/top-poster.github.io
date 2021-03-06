---
layout: post
title: "스켈레톤 로딩 화면 사용 방법, 이유 및 방법"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![wireframe diagram of a LinkedIn screen](https://miro.medium.com/max/2800/1*kjiP2MlAkAuAiuYOr6BwLA.gif)

Reddit, Discord, Medium 및 LinkedIn의 공통점은 무엇입니까? 애플리케이션에는 골격 로드 스크린이라는 것을 사용합니다.

스켈레톤 스크린은 본질적으로 애플리케이션의 와이어프레임입니다. 응용 프로그램이 마지막으로 로드될 때까지 와이어 프레임은 자리 표시자입니다.

다음은 일반적으로 스켈레톤 로딩 화면의 모습입니다. 기존의 로드 스피너를 대체하여 사용하는 방법에 주목하십시오.

![animation of a skeleton loading screen: a wireframe briefly appears, followed by the content](https://miro.medium.com/max/1800/1*ApCTZAEu9MX_nIow2K6y3w.gif)

골격 로드 화면은 기본적으로 원래 레이아웃과 가장 유사합니다.

이를 통해 사용자는 화면에서 어떤 일이 발생하는지 알 수 있습니다. 응용 프로그램이 부팅되고 콘텐츠가 로드되는 것으로 사용자가 해석합니다.

# 스켈레톤 로딩 화면이란?

스켈레톤 화면은 페이지의 레이아웃을 모방한 사용자 인터페이스의 버전입니다.

![a diagram that shows screen content on the right and the corresponding wireframe on the left](https://miro.medium.com/max/960/1*iDHhu6voeuFA3lCJWX3Cow.gif)

스켈레톤 로드 화면에는 페이지가 로드되고 사용 가능해짐에 따라 실제 내용과 유사한 모양으로 페이지가 표시됩니다.

# 스켈레톤 로딩 화면을 사용하는 이유

빈 페이지를 표시하는 대신 콘텐츠 로드를 표시하면 응용 프로그램의 응답 속도가 빨라지는 것처럼 느껴집니다.

직접 메시지 불일치: 다음은 스켈레톤 로딩 스크린이 적합한 경우의 또 다른 주요 예입니다.

![an example of a Discord skeleton loading screen](https://miro.medium.com/max/2068/1*zJQn3MTtgruOJi0kfwO7Og.png)

로드 스피너를 보여주는 대신 애플리케이션을 시작하고 탐색할 때 진행 상황이 발생한다는 것을 사용자가 볼 수 있는 스켈레톤 화면을 보여줄 수 있다.

다음은 다음 프로젝트에서 스켈레톤 로딩 화면을 사용하는 것을 고려하는 몇 가지 이유입니다.

# 반응 하중-골격 사용 방법

여러 가지 솔루션이 있지만, 오늘은 리액트 로딩 스켈레톤 오픈 소스 패키지를 사용하겠습니다.

![screenshot from react-loading-skeleton on GitHub](https://miro.medium.com/max/5304/1*8yxG4ip875lOf2J66iq4-Q.png)

# 지금 출발해!

일어나서 달리는 것은 꽤 간단하고 간단하다. 다른 패키지와 마찬가지로, npm 또는 Narn을 통해 반응 로드 스켈레톤을 설치할 수 있습니다.

## 실

```js
실을 덧대어 리액트 로딩을 하다.
```

## npm

```js
npmi 반응 적재 장치
```

# 블로그 사후 대응 구성 요소

글의 제목 자막 본문 등을 소품으로 받아들이는 기초 블로그 글을 만들어 보자.

소품만 부품으로 전달하는 대신 || 연산자를 사용하겠습니다. 이 연산자는 or check와 같은 역할을 합니다.

제목이 없으면 스켈레톤 로더를 표시합니다. 제목이 있으면 제목을 표시합니다.

구성 요소가 로드되는 동안 컨텐츠 대신 사용자의 구성 요소에 직접 사용하도록 설계된 `스켈레톤`을 기억하십시오.

# 블로그 게시물 렌더링

그런 다음 래퍼 구성 요소 내에 있는 블로그 게시 구성 요소를 가져옵니다.

세 가지 속성을 가진 블로그 게시 개체를 만듭니다. 블로그 게시물 오브젝트를 블로그 게시물 구성요소에 전달합니다.

데이터 가져오기를 가장하기 위해 `효과` 후크를 사용하는 방법에 주목하십시오. setTimeout을 사용한 덕분에, 우리는 블로그 게시물에 소품 전달을 잠시 연기할 것이다.

이 작업은 마치 데이터를 잠시 가져오는 것처럼 수행되고 완료되면 블로그 게시물에 데이터를 전달합니다. API에서 데이터를 가져오는 것과 같습니다.

참고: React Hooks를 처음 접하는 경우 앞에서 작성한 "React Hooks 제거" 기사를 확인하십시오.

이제 스켈레톤 스크린을 잠시 동안 볼 수 있습니다. 잠시 후, 우리는 블로그 게시물을 볼 것입니다.

![animation of a skeleton loading screen: a wireframe briefly appears, followed by the content](https://miro.medium.com/max/1800/1*ApCTZAEu9MX_nIow2K6y3w.gif)

# 프로필 이미지를 모방할 원 그리기

원 모양의 골격 자리 표시자의 경우 원 속성을 참으로 설정합니다.

```js
<골격 원={true} 높이={50} 폭={50} / >
```

![animation of a skeleton loading screen: a wireframe briefly appears, followed by the content, including a profile circle](https://miro.medium.com/max/1604/1*zGZeC5rFUl9iOisUnlY6hg.gif)

블로그 포스트 구성 요소의 전체 코드 샘플입니다.

아래 데모 응용 프로그램으로 재생해 보십시오.

# 결론

읽어주셔서 감사합니다! 새로운 것을 배웠기를 바랍니다. 해피 코딩!

아, 그리고 만약 당신이 최신 정보를 알고 싶다면, 제 뉴스레터는 그것을 위한 훌륭한 자료입니다.

![Image for post](https://miro.medium.com/max/2400/1*mxMY5TSx9Pvw06XqLxT_Ig.png)