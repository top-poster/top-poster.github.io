---
layout: post
title: "2021년 Retrazy Loading Library를 위한 최고의 선택"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/reactlazyloadcomponentsfor2021.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/reactlazyloadcomponentsfor2021.png?fit=730%2C487&ssl=1)

느린 로딩은 React 애플리케이션을 최적화하여 로드 시간을 단축하는 방법 중 하나가 되었습니다. 이 방법은 현재 보이지 않거나 필요한 웹 페이지의 일부분을 로드하지 못하도록 하는 방법입니다. 웹 및 모바일 애플리케이션을 최적화하기 위한 설계 방법입니다.

React 에코시스템에는 느린 로드를 통해 애플리케이션을 최적화하는 데 도움이 되는 여러 패키지가 있습니다. 이 게시물에서는 Ract 애플리케이션을 느리게 로드하는 데 사용할 수 있는 몇 가지 주요 선택 사항을 강조합니다.

## 반동 부하

React-lazyload는 React 애플리케이션에서 React 구성 요소 및 이미지를 느리게 로드하기 위한 패키지입니다.

반응 지연 로드를 시작하기 위해 패커(Packager)를 사용하여 다음을 설치합니다.

```coffeescript
npm i react-lazyload
```

이제 구성 요소를 느리게 로드할 수 있습니다.

```xml
import React from "react";
import LazyLoad from "react-lazyload";

export default function App() {
  return (
    <div className="list">
      <LazyLoad height={250} once>
        <img src="https://placedog.net/500/280" alt="dog" />
</LazyLoad>
      <LazyLoad height={250} once>
        <img src="https://placedog.net/500/300" alt="dog" />
      </LazyLoad>
      <LazyLoad height={250} offset={150}>
        <img src="https://placedog.net/500/300" alt="happy dog" />
      </LazyLoad>
    </div>
  );
}
```

위에서는 `LazyLoad` 구성 요소를 LazyLoad를 원하는 모든 요소 또는 컨텐츠에 포장을 합니다. 요소의 높이는 높이 속성을 사용하여 설정됩니다. 원스(once)는 리액션 게으른 로드의 속성으로, 콘텐츠가 표시되면 리액션 게으른 로드에 의해 관리되지 않는다는 의미입니다.

반응 지연 부하에 대한 몇 가지 장점은 다음과 같습니다.

- 모든 게으른 구성 요소에 대해 이벤트 수신기가 2개뿐이므로 성능이 향상됩니다.
- 일회성 게으른 로드와 연속 게으른 로드 모드를 지원하므로 구성 요소의 필요에 따라 모드를 선택할 수 있습니다.
- 장식품을 지원한다.
- 서버측 렌더링을 지원합니다.

리액션 지연 로드는 일부 단점이 있기 때문에 완벽한 라이브러리가 아닙니다. 그 중 하나는 프로젝트의 사용에 따라 스냅샷 테스트를 중단하는 경우가 있으며 프로젝트에 추가할 경우 추가 DOM 노드를 추가할 수 있다는 것입니다.

반응 부하 문서를 보면 반응 부하에 대한 자세한 내용을 볼 수 있습니다. 코드 샌드박스 예는 여기에서 확인할 수 있습니다.

## 반응-충돌-부하-이미지-구성 요소

Retact-Lazy-Load-image-component는 Retact 구성 요소 및 이미지를 느리게 로드하는 데 사용되며 요소가 언제 뷰포트에 들어가는지 결정하는 교차로 관찰자를 위한 지원을 자랑하며 서버측 렌더링(SSR)과도 호환됩니다.

사용하려면 먼저 아래 명령을 사용하여 npm을 통해 설치하십시오.

```coffeescript
npm i --save react-lazy-load-image-component
```

용도는 다음과 같습니다.

```js
import React from 'react';
import { LazyLoadImage } from 'react-lazy-load-image-component';

const ReactImage = ({ image }) => (
  <div>
    <LazyLoadImage
      alt={image.alt}
      src={image.src} />
    <span>{image.caption}</span>
  </div>
);

export default ReactImage;
```

위의 코드에서 "Ract-Lazy-Load-Image"에서 "LazyLoad Image"를 가져오고 "LazyLoadImage" 구성 요소를 감싸면 다른 이미지 속성이 소품으로 전달됩니다.

반응-래지 로드-이미지 구성 요소의 장점은 다음과 같습니다.

- 구성 요소의 사용자 지정 자리 표시자를 지정할 수 있습니다.
- beforeLoad 및 afterLoad에 대한 이벤트를 지정할 수 있습니다.
- 서버측 렌더링 구성요소 및 응용프로그램과 호환됩니다.
- TypeScript 선언과 호환됩니다.
- 블러 및 불투명도 전환과 같은 가시적 효과와 함께 내장됩니다.
- 사용자 지정 가능한 임계값이 특징입니다.

다른 것과 마찬가지로, 반응-래지 로드-이미지 구성 요소에도 일부 백사이드(backside)가 있으며, 그 중 일부는 다음과 같습니다.

- 모든 이미지가 한 번에 로드되고, 반응-부하-이미지 구성 요소가 뷰포트에 표시되는 모든 이미지를 로드합니다. 즉, 이미지 갤러리가 있으면 모든 이미지가 웹 페이지의 보이는 부분에 로드됩니다.
- 효과가 작동하지 않음 - 이 패키지를 사용하여 효과를 실행하려면 여기서처럼 효과 CSS를 가져와야 합니다.
- 반응 `setState` 후크는 장착 또는 장착 구성 요소에서만 작동할 수 있습니다.

해당 설명서에서 반응-라지-로드-이미지-구성 요소에 대해 자세히 알아볼 수 있습니다.

## 반동 하중

문서에 따르면, 반응-래지-로드는 예측 가능한 방식으로 내용 로드를 지연시키는 데 도움이 되는 사용하기 쉬운 반응 구성요소입니다. 스크롤바와 같은 스크롤 용기 내부에 자동 구성 요소가 포함되어 있습니다.

리액턴트 로드(react-contract-load)는 다음을 처음 설치할 때 사용할 수 있습니다.

```undefined
npm install --save react-lazy-load
```

리액션-래지 로드를 사용하여 구성 요소를 로드하려면 구성 요소를 `LazyLoad` 구성 요소로 포장합니다.

```xml
import React from 'react';
import LazyLoad from 'react-lazy-load';

const App = () => {
  <div>
    List of cat images
    <div className="filler" />
    <LazyLoad height={500} offsetVertical={200}>
      <img src='http://placekitten.com/200/300' alt='cat' />
    </LazyLoad>    
    <LazyLoad height={500} offsetTop={200}>
      <img src='http://placekitten.com/200/300' alt='cat' />
    </LazyLoad>
    <div className="filler" />
    <LazyLoad height={480} offsetHorizontal={50}>
      <img src='http://placekitten.com/200/300' alt='cat' />
    </LazyLoad>
    <div className="filler" />
    <LazyLoad height={500} offsetTop={200}>
      <img src='http://placekitten.com/200/300' alt='cat' />
    </LazyLoad>
    <div className="filler" />
    <LazyLoad height={480} offsetHorizontal={50}>
      <img src='http://placekitten.com/200/300' alt='cat' />
    </LazyLoad>
    <div className="filler" />
```

React에 대한 대부분의 게으른 로드 라이브러리와 유사하게, "LazyLoad" 구성 요소는 "LazyLoad" 구성 요소를 사용하여 게으른 로드를 수행할 요소를 결정합니다.

반응 속도 로드에는 다음과 같은 몇 가지 이점이 있습니다.

- 빠르다!
- IE8+에 대한 기본 지원
- 다른 제품과 비교했을 때, 반응 지연 부하가 작은 번들 크기(6kb 축소)
- 디바운스 기능 내장 지원

반응-래지 부하의 일부 단점은 반응-래지 부하로 오인될 수 있다는 사실, 또 다른 하나는 `래지 부하`를 사용하여 최적화 가능한 각 구성 요소와 요소를 포장해야 하는 정도이다.

여기서 반응 부하에 대해 자세히 알아볼 수 있습니다.

## 개츠비 이미지

개츠비 이미지는 개츠비 그래프QL 쿼리와 원활하게 작동하도록 설계된 신속한 이미지 최적화 라이브러리이다. 고급 이미지 로드 기능을 통해 웹 페이지에 대한 이미지를 느리게 로드할 수 있습니다.

개츠비 이미지를 사용하려면 다음을 설치합니다.

```coffeescript
npm install gatsby-image
```

개츠비 이미지는 시작 파일에 따라 `개츠비`와 `개츠비-개츠비`가 필요하며, 이를 설치하여 개츠비 구성 파일에 추가해야 합니다.

```undefined
npm install gatsby-transformer-sharp gatsby-plugin-sharp
```

아래의 개츠비 구성에 추가합니다.

```coffeescript
plugins: [`gatsby-transformer-sharp`, `gatsby-plugin-sharp`]
```

다음은 `개츠 바이 이미지`로 구성 요소를 최적화하는 모습입니다.

```js
import React from 'react'
import { graphql } from 'gatsby'
import Img from "gatsby-image"

export default ({ data }) => (
  <div>
    <h1>Hello</h1>
    <Img fixed={data.file.childImageSharp.fixed} />
  </div>
)

export const query = graphql`
  query {
    file(relativePath: { eq: "blog/avatars/kyle-mathews.jpeg" }) {
      childImageSharp {
        fixed(width: 125, height: 125) {
          ...GatsbyImageSharpFixed
        }
      }
    }
  }

```

쿼리에서 이미지 처리 사양을 정확하게 정의하기 위해 "childImageSharp" 속성을 사용하므로 웹 페이지의 UI 변경 사항을 업데이트하는 데도 사용됩니다. 개츠비 이미지에 대한 자세한 내용은 여기서 확인할 수 있습니다.

다음은 리액션 응용 프로그램에서 개츠비 이미지가 제공하는 몇 가지 이점입니다.

- 높이와 너비가 고정된 영상에 맞게 최적화할 수 있습니다.
- 컨테이너를 넘어 확장되는 이미지에 최적화될 수 있습니다.
- 파란색 효과와 느린 로드 이미지를 자동으로 추가합니다.
- 빠르다!

고도로 최적화된 개츠비 이미지는 애플리케이션에 의해 비개츠로 고전하는 것과 같은 몇 가지 단점을 가지고 있지만, 그래프QL에 대한 기본적인 이해가 없는 사람은 누구나 사용하기 어려울 수 있다.

## 게으름을 피우다

uselazy는 Ract 구성 요소 및 이미지를 느리게 로드하고 코드 분할하기 위한 React 라이브러리입니다. use lazy 핸들 동적 및 명명된 가져오기 모두.

대부분의 다른 라이브러리를 패키지 관리자를 사용하여 설치할 수 있는 것처럼 느릿느릿합니다.

```coffeescript
npm install uselazy
```

또는 실 사용:

```undefined
yarn add uselazy
```

### 사용법

```php
/// navbar.jsx

import React from 'react';

const Navbar = () => <p> React is awesome </p>

export default Navbar;


/// Menu.js

import React from 'react';

export const Menu = () = <h2> This is a menu page </h2>


/// App.js

import React, { useState } from 'react';
import useLazy from 'uselazy';

const imports = [() => import('./Navbar'), () => import('./Menu')];

const App = () => {
   const [shouldImport, setShouldImport] = useState(false);
   const { isLoading, result: Components } = useLazy(
      useMemo(() => imports, []),
      shouldImport,
  );

 return (
    <div>
      <h1>I'm very lazy </h1>
      <button onClick={() => setShouldImport(!shouldImport)}>Click menu</button>

      {isLoading && <span>some spinner</span>}

      {Components && Components.map(Component => <Component />)}
    </div>
  );
};
```

위 코드에서 use lazy는 use lazy 내 use effect의 종속성으로 추가된다.

아래에는 리액션 응용 프로그램에서 `유용한 사용`을 사용할 때의 몇 가지 이점이 포함되어 있습니다.

- 동적 및 명명된 가져오기에 대한 기본 지원
- 매우 작은 번들 크기(31kb)

여기서 게으름에 대해 더 많이 배울 수 있습니다.

## 요약

성능을 위해 구성 요소와 이미지를 느리게 로드하여 React 애플리케이션을 최적화하는 것은 오늘날 개발에서 매우 중요하다. 이러한 라이브러리를 사용하면 Relect 앱의 성능을 더욱 빠르게 하고 전반적인 사용자 환경을 개선할 수 있습니다. 개츠비 이미지 및 반응 게으름 같은 라이브러리는 이미지 갤러리 응용 프로그램에 더 적합하고, uselazy와 같은 다른 라이브러리는 동적 및 명명된 가져오기가 많은 구성 요소 및 React 프로젝트에 더 적합하다. 교차로 관찰자가 있는 리액션-래지-로드-이미지 구성 요소와 같은 라이브러리는 TypeScript와 매우 호환되는 특성으로 인해 TypeScript 중심 프로젝트에 이상적이지만 모든 이미지를 동시에 로드하기 때문에 갤러리 응용 프로그램의 성능에 어려움을 겪을 것이다.