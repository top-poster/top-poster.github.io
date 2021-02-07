---
layout: post
title: "싱글 스파를 통해 마이크로 프런트 엔드 애플리케이션 생성"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/puzzle-spa.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/puzzle-spa.png?fit=730%2C487&ssl=1)

지난 몇 년간 마이크로 서비스에 대한 많은 논쟁이 있었습니다. 이 토론의 결과로, 모든 사람들은 이제 모든 프로젝트에 맹목적으로 이 개념을 채택한 결과뿐만 아니라 이 개념을 둘러싼 찬반 양론을 알게 되었다.

음, 같은 논쟁이 마이크로 프런트 엔드에도 존재한다. 용어 자체는 많은 것을 말합니다. 그것은 여러분에게 여러분의 앞 끝을 이치에 맞는 더 작은 조각들로 쪼개어낼 수 있는 능력을 줍니다. 이렇게 하면 독립적이기 때문에 이러한 구성 요소를 테스트, 구축, 개발 및 배치할 수 있습니다.

이 용어는 2016년 유명한 ThinkWorks Tech Radar에서 처음으로 공개 어휘로 들어갔다. 이를 통해 백엔드에서 프런트 엔드로 개념이 확장되었습니다.

오늘날 수많은 프런트엔드 개발자들이 동일한 프로젝트 또는 어느 시점에 통합되어야 하는 프로젝트를 진행하고 있습니다. 궁극적으로 개발자는 사용자가 여러 사람이 만든 많은 부분(구성 요소)으로 전체 응용프로그램을 만들 수 있는 프레임워크나 라이브러리를 사용할 수 있기를 원합니다.

구성요소보다는 전체 응용프로그램이 퍼즐 조각처럼 통합됩니다.

여기서는 마이크로 프런트 엔드에 대해 자세히 볼 수 있습니다. 하지만 이 글에서는 실용적인 측면에 초점을 맞추겠습니다. 다시 말해, 우리는 싱글 스파와 리액션을 사용하여 처음부터 마이크로 프런트 앱을 만드는 방법을 다룰 것입니다.

Single-spa는 프런트 엔드 마이크로 서비스(즉, 또 다른 일반적인 용어)를 위한 JavaScript 라우터입니다.

모든 유형의 프레임워크(Vue, Angular, React, 일반 HTML/JS/CSS 등) 위에 구축된 여러 애플리케이션을 통합하여 마이크로 프런트 엔드의 구성을 단순화합니다. 좀 더 자세히 살펴봅시다!

### 우리가 만들 앱

이 기사의 마지막에는 여기에 제시된 단계를 자세히 따라 마이크로 프런트 엔드 애플리케이션을 구축할 수 있습니다.

이 애플리케이션은 다음과 같이 구축됩니다.

화면을 섹션으로 강조 표시했습니다. 각 섹션은 서로 다른 응용 프로그램에 해당합니다.

- 앱 헤더 + 메뉴: `logrocket-single-spa-navbar` 응용 프로그램에서 관리합니다.
- 집 본문: `로그로켓-싱글스파 홈페이지` 앱에서 관리
- 사용자 정보 페이지: `logrocket-single-spa-about page` 앱에서 관리
- 연락처 페이지: `logrocket-single-spa-contact page` 앱에서 관리

이는 프레임워크가 여러 앱을 단일 앱으로 통합하는 방법을 살펴보는 좋은 예입니다.

페이지의 바닥글은 변경되지 않으므로 루트 응용 프로그램에 그대로 둡니다.

## 설정 및 앱 생성

Seagate는 단일 Spa CLI 생성 툴을 사용하여 단일 Spa 프로젝트를 자동 생성합니다. 웹 팩, Babel, Jest 및 작업할 프런트 엔드 프레임워크(Ract, Angular 등)의 구성을 관리합니다.

설치하려면 다음 명령을 실행합니다.

```coffeescript
npm install --global create-single-spa
```

그런 다음 다음과 같이 루트 프로젝트를 생성합니다.

```cpp
// Creating the folder for all projects
mkdir logrocket-single-spa
cd logrocket-single-spa

// Creating the root app folder
mkdir logrocket-single-spa-root
cd logrocket-single-spa-root

// Creating the single-spa app
npx create-single-spa
```

루트 앱(컨테이너 앱이라고도 함)은 다른 앱의 통합 방식을 관리하는 역할을 한다.

최신 명령을 실행할 때는 명령줄을 통해 여러 개의 추가 입력을 제공해야 합니다. 아래 그림에는 입력할 수 있는 각 값이 나와 있습니다.

옵션을 제대로 선택하지 않으면 오류가 발생할 수 있으므로 각 옵션에 유의하십시오.

예를 들어 두 번째는 `싱글 스파 루트 구성`이다. 이 옵션은 세 가지 옵션 중에서 선택해야 하며 현재 생성된 프로젝트가 루트 구성이라고 명시합니다.

다른 옵션을 선택하면 다른 마이크로 프런트 엔드 프로젝트의 질문도 변경됩니다. 그것은 실제 대화식 설문지처럼 작동합니다.

`조직명`에 제공할 가치는 편지 케이스에 따라 다르므로 각별히 유의하십시오. 여기서 우리는 `로그 로켓`이라는 단어 전체를 대문자로 쓰고 있다.

이제 루트 폴더로 다시 이동하여 다른 프로젝트를 생성하겠습니다. 다음은 탐색 모음 프로젝트를 만드는 명령입니다.

```bash
cd ..
mkdir logrocket-single-spa-navbar
cd logrocket-single-spa-navbar
```

```undefined
npx create-single-spa
```

구성 프로젝트가 아니기 때문에 다른 설정이 필요합니다. 아래 그림에는 세부 정보와 작성해야 하는 옵션이 나와 있습니다.

이 프로젝트 설정을 사용하면 사용할 프레임워크(React) 및 프로젝트 이름과 같은 이전 옵션보다 몇 가지 더 많은 옵션을 사용할 수 있습니다. 다시 말하지만, 낮은 케이스와 높은 케이스의 구별에 주의해야 합니다.

계속하기 전에 나머지 프로젝트에 대해 위의 단계를 반복해야 합니다.

- 로그로켓-싱글로켓-나브바
- 로그로켓-싱글로켓-싱글로켓
- 로그로켓-단일화 관련 페이지
- 로그로켓-단일접촉 페이지

각 프로젝트 설정을 완료하면 콘솔 로그 끝에 다음과 같은 출력이 표시됩니다.

로그에는 다음 두 가지 팁이 있습니다.

- Yarn을 통해 개별적으로 애플리케이션 시작
- 단일 스파 원격 놀이터에서 직접 테스트

Single-spa는 System.js를 사용하여 가져오기 맵을 만들기 때문에 네트워크를 통해 모듈을 쉽게 가져올 수 있으므로 이를 변수 이름에 매핑할 수 있습니다.

이렇게 하면 앱 콘텐츠를 모두 읽고 미리 보는 방법을 알 수 있습니다.

포트 충돌을 방지하려면 각 앱에 알릴 포트에 주의하십시오.

## 앱 구성

대부분의 변경 사항은 루트 앱에서 이루어집니다. 따라서 파일을 열고 이름이 `-root-config.js`로 끝나는 파일로 이동합니다.

여기에 일부 초기 구성이 표시될 수 있지만 다음과 같이 변경합니다.

```coffeescript
import { registerApplication, start } from "single-spa";

registerApplication({
    name: "@LogRocket/logrocket-single-spa-navbar",
    app: () => System.import("@LogRocket/logrocket-single-spa-navbar"),
    activeWhen: ["/"],
});

registerApplication({
    name: "@LogRocket/logrocket-single-spa-homepage",
    app: () => System.import("@LogRocket/logrocket-single-spa-homepage"),
    activeWhen: [(location) => location.pathname === "/"],
});

registerApplication({
    name: "@LogRocket/logrocket-single-spa-aboutpage",
    app: () => System.import("@LogRocket/logrocket-single-spa-aboutpage"),
    activeWhen: ["/about"],
});

registerApplication({
    name: "@LogRocket/logrocket-single-spa-contactpage",
    app: () => System.import("@LogRocket/logrocket-single-spa-contactpage"),
    activeWhen: ["/contact"],
});

start({
    urlRerouteOnly: true,
});
```

이 곳은 모든 마이크로 프런트 엔드 앱을 등록해야 하는 곳입니다. 4개니까 1개당 4개의 `등록신청서`가 있어야 해요.

모든 앱에는 이름, 경로(이전에 이야기한 것처럼 SystemJS를 통해) 및 활성화/비활성화 조건이 있어야 합니다(`active`).언제).

이름은 일반적으로 `@OrganizationName/AppName` 패턴을 따르므로 앱을 만들 때 제공한 이름과 동일한 이름을 입력해야 합니다.

이 조건은 일반적으로 URL 경로 이름에 적용됩니다. 그렇기 때문에 정보 및 연락처 페이지의 경우 경로 이름만 제공하면 됩니다.

그러나 홈페이지 구성에 나와 있는 것처럼 자신만의 조건을 정의하고자 하는 경우가 있습니다. 거기에 부울 결과를 제공하면 됩니다.

`index.ejs` 파일로 이동하여 파일을 열고 다음 코드 조각을 `head` 태그 내 아무 곳에나 추가합니다.

```undefined
<% if (isLocal) { %>
    <script type="systemjs-importmap">
      {
        "imports": {
          "react": "https://cdn.jsdelivr.net/npm/react@16.13.1/umd/react.development.js",
          "react-dom": "https://cdn.jsdelivr.net/npm/react-dom@16.13.1/umd/react-dom.development.js",
          "@LogRocket/root-config": "http://localhost:9000/LogRocket-root-config.js",
          "@LogRocket/logrocket-single-spa-navbar": "http://localhost:9001/LogRocket-logrocket-single-spa-navbar.js",
          "@LogRocket/logrocket-single-spa-homepage": "http://localhost:9002/LogRocket-logrocket-single-spa-homepage.js",
          "@LogRocket/logrocket-single-spa-aboutpage": "http://localhost:9003/LogRocket-logrocket-single-spa-aboutpage.js",
          "@LogRocket/logrocket-single-spa-contactpage": "http://localhost:9004/LogRocket-logrocket-single-spa-contactpage.js"
        }
      }
    </script>
  <% } %>
```

이러한 가져오기 기능은 단일 스파에서 루트 앱으로 가져올 마이크로프론트를 파악해야 하는 필수 가져오기입니다.

그들은 또한 `이름:값`의 규칙과 문자 사례도 준수한다.

초기에는 React와 react-router 립도 수입하고 있습니다. 이는 다음 오류가 발생하지 않도록 하기 위한 것입니다.

모든 파일을 저장하고, 각 터미널에 정의된 포트에 주의를 기울여 모든 앱을 실행하고, 루트 파일을 시작합니다. 그런 다음 http://localhost:9000/ 주소에 액세스하여 다음 결과를 확인합니다.

시험해 보세요! 다른 URL로 이동하면 일부 텍스트가 표시되거나 사라집니다.

## 마이크로 프런트 엔드 앱

이제 앱의 콘텐츠를 하나씩 설정할 차례입니다. 나머지 설계에는 navbar가 필요하므로 먼저 navbar로 하자.

navbar 프로젝트 아래에서 `root.component.js` 파일을 열고 내용을 다음과 같이 변경합니다.

```js
import React from "react";

export default function Root(props) {
  return (
    <header className="masthead mb-auto">
      <div className="inner">
        <a className="blog-logo" href="/">
          <img
            style={ maxWidth: "40px" }
            src="https://single-spa.js.org/img/logo-white-bgblue.svg"
            alt="LogRocket Blog"
          />{" "}
          React + single-spa
        </a>
        <nav className="nav nav-masthead justify-content-center">
          <a
            className={`nav-link ${location.pathname === "/" && "active"}`}
            href="/"
          >
            Home
          </a>
          <a
            className={`nav-link ${location.pathname === "/about" && "active"}`}
            href="/about"
          >
            About Us
          </a>
          <a
            className={`nav-link ${
              location.pathname === "/contact" && "active"
            }`}
            href="/contact"
          >
            Contact
          </a>
        </nav>
      </div>
    </header>
  );
}
```

낯이 익은가? 왜냐하면 이 코드가 완전 리액트이기 때문이에요. React 구성 요소인데, 공통 라인 생성 설정에서 선택한 기술이기 때문입니다.

위치 라우터 개체와 함께 놀고 있습니다. 예, 모든 반응 파일 내에 자동으로 주입됩니다. 이렇게 하면 탐색 모음 메뉴의 링크를 사용자 정의할 수 있습니다.

다음 코드는 동일한 루트 구성 요소 파일에 속하지만 이번에는 `홈 페이지` 프로젝트의 경우입니다.

```js
import React from "react";

export default function Root(props) {
  return (
    <section>
      <div className="homepage-hero" style={ margin: "5rem 0" }>
        <img
          style={ width: "100%" }
          src="https://blog.logrocket.com/wp-content/uploads/2019/05/logrocket-blog.jpg"
        />
      </div>
      <h1 className="cover-heading">Welcome to the micro-frontend world!</h1>
      <p className="lead">
        This is an example of how powerful micro-frontends can be!
        <br /> You may integrate all of your frontend apps, regardless of what
        frameworks they're built with.
      </p>
      <p className="lead">
        <a href="#" className="btn btn-lg btn-secondary">
          Learn more
        </a>
      </p>
    </section>
  );
}
```

새로운 기능은 없습니다. 단순하고 정적 Retact 구성 요소에 대한 전체 JSX 코드만 있으면 됩니다.

정보 및 연락처 페이지에도 동일하게 적용됩니다. 아래에서 두 가지 코드를 모두 확인할 수 있습니다.

```xml
// About page

import React from "react";

export default function Root(props) {
  return (
    <section>
      <div className="homepage-hero" style={ margin: "5rem 0" }>
        <img
          style={ width: "100%" }
          src="https://blog.logrocket.com/wp-content/uploads/2019/05/logrocket-blog.jpg"
        />
      </div>
      <h1 class="cover-heading">About Us!</h1>
      <p class="lead">
        Hey, what's up?
        <br />
        This is a page to talk about us.
      </p>
    </section>
  );
}

// Contact page
import React from "react";

export default function Root(props) {
  return (
    <section>
      <div className="homepage-hero" style={ margin: "5rem 0" }>
        <img
          style={ width: "100%" }
          src="https://blog.logrocket.com/wp-content/uploads/2019/05/logrocket-blog.jpg"
        />
      </div>
      <h1 className="cover-heading">Contact Us</h1>
      <p className="lead">
        Hey, what's up?
        <br />
        This is a page to contact us.
      </p>
    </section>
  );
}
```

## 약간의 디자인

페이지의 전체 설계를 사용자 정의해 보겠습니다. 이를 위해 우리는 부트스트랩을 사용할 것이다. 왜냐하면 이것은 빠르고 매우 간단한 라이브러리이기 때문이다.

여전히 `index.ejs` 페이지 내에 다음 두 줄을 헤드 태그에 추가합니다.

```xml
<!-- Bootstrap core CSS -->
<link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet" />
<link rel="stylesheet" href="https://getbootstrap.com/docs/4.0/examples/cover/cover.css" />
```

이것들은 부트스트랩의 CSS 파일의 CDN 링크이다. 이 예에서는 부트스트랩 JavaScript 파일이 필요하지 않습니다.

두 번째 CSS 파일은 이 앱에 사용할 부트스트랩에서 제공하는 멋진 예제 중 하나에 속합니다.

프로젝트를 시험해 볼 시간이다. 단일 스파의 또 다른 좋은 기능은 라이브 다시 로드가 기본적으로 사용 가능하므로 변경할 때마다 앱을 계속 재시작할 필요가 없다는 것입니다.

브라우저로 돌아가면 다음과 같이 새 설계가 적용될 수 있습니다.

## 결론

싱글 스파로 할 수 있는 게 훨씬 더 많아요. 이 프레임워크에는 개발용으로 선택한 프레임워크에 관계없이 기능 청크를 수동으로 만들 수 있는 프레임워크에 구애받지 않는 기능인 구획과 같은 몇 가지 추가 기능이 포함되어 있습니다.

옵션인 단일 스페어 레이아웃 패키지는 최상위 경로, 앱 및 DOM 요소에 대한 라우팅 API 제어 기능을 제공합니다.

Angular, AngularJS, Ember, 코드 게으름뱅이 로딩, 쉬운 코드베이스 재작성 등을 지원한다. 특징의 폭이 넓기 때문에, 나는 그것의 공식 문서에 대한 철저한 읽기를 강력히 추천하고 싶다.

여기서 이 예제의 전체 소스 코드에 액세스할 수 있습니다. 멋진 마이크로 프런트 엔드 프로그래밍을 경험하세요!