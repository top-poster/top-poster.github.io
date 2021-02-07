---
layout: post
title: "History API를 사용하여 JavaScript 라우터 구축"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/JavaScript-History-API.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/JavaScript-History-API.png?fit=730%2C487&ssl=1)

이 기사에서는 클라이언트측 라우팅 시스템을 구축하겠습니다. 클라이언트 측 라우팅은 페이지의 URL이 변경되더라도 전체 페이지 다시 로드가 발생하지 않는 응용프로그램을 탐색하는 라우팅의 한 종류입니다. 대신 새 내용을 표시합니다.

이를 구축하려면 `index.html` 파일을 지원하는 간단한 서버가 필요합니다. 준비됐어? 시작하자.

먼저 새 node.js 애플리케이션을 설정하고 프로젝트 구조를 생성합니다.

```undefined
npm init -y
npm install express morgan nodemon --save
touch server.js
mkdir public && cd public
touch index.html && touch main.js file
cd ..
```

npm init 명령을 실행하면 `패키지`가 생성됩니다.json 파일입니다. 서버 운영 및 경로 로깅에 사용되는 익스프레스, 모건 등을 설치하겠습니다.

또한 `server.js` 파일과 보기를 기록할 공개 디렉토리도 만들 것입니다. 파일을 변경하면 응용 프로그램을 다시 시작하는 데몬은 없습니다.

## 서버 설정

`server.js` 파일을 수정하여 Express를 사용하여 간단한 서버를 생성하겠습니다.

```php
const express = require('express');
const morgan = require('morgan');
const app = express();

app.use(morgan('dev'));
app.use(express.static('public'))

app.get('*', (req, res) => {
    res.sendFile(__dirname + '/public/index.html')
})
app.listen(7000, () => console.log("App is listening on port 7000"))
```

이제 nodemon server.js를 실행하여 애플리케이션을 시작할 수 있습니다. HTML을 위한 간단한 상용판을 만들어 보겠습니다.

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Javascript Routing</h1>
    <div id="app">
    </div>

    <script src="main.js"></script>
</body>
</html>
```

여기서는 언제든지 DOM을 조작할 수 있도록 `main.js` 파일을 연결합니다.

## 라우팅 시스템 구현

main.js 파일로 이동하여 라우터 로직을 모두 작성합시다. 모든 코드는 window.onload로 싸여 웹 페이지가 모든 콘텐츠를 로드한 후에만 스크립트를 실행할 수 있습니다.

다음으로, 두 개의 매개 변수가 있는 함수인 라우터 인스턴스를 만듭니다. 첫 번째 매개 변수는 경로 이름이 되고 두 번째 매개 변수는 정의된 모든 경로를 구성하는 배열이 됩니다. 이 경로에는 경로 이름과 경로의 두 가지 속성이 있습니다.

```js
window.onload = () => {
// get root div for rendering
    let root = document.getElementById('app');

  //router instance
    let Router = function (name, routes) {
        return {
            name,
            routes
        }
    };

 //create the route instance
    let routerInstance = new Router('routerInstance', [{
            path: "/",
            name: "Root"
        },
        {
            path: '/about',
            name: "About"
        },
        {
            path: '/contact',
            name: "Contact"
        }
    ])

}
```

현재 페이지의 경로 경로를 가져오고 경로를 기준으로 템플릿을 표시할 수 있습니다.location.pathname은 페이지의 현재 경로를 반환하며, DOM에 이 코드를 사용할 수 있습니다.

```js
 let currentPath = window.location.pathname;
    if (currentPath === '/') {
        root.innerHTML = 'You are on Home page'
    } else {
        // check if route exist in the router instance 
        let route = routerInstance.routes.filter(r => r.path === currentPath)[0];
        if (route) {
            root.innerHTML = `You are on the ${route.name} path`
        } else {
            root.innerHTML = `This route is not defined`
        }
    }
```

경로 인스턴스에 경로가 정의되어 있는지 확인하기 위해 `currentPath` 변수를 사용합니다. 경로가 존재하는 경우 간단한 HTML 템플릿을 렌더링합니다. 그렇지 않으면 페이지에 `이 경로가 정의되지 않았습니다`라고 표시됩니다.

원하는 유형의 오류를 언제든지 표시하십시오. 예를 들어, 경로가 없는 경우 홈 페이지로 리디렉션할 수 있습니다.

### 라우터 링크 추가

페이지를 탐색하기 위해 라우터 링크를 추가할 수 있습니다. Angular와 마찬가지로 이동하고자 하는 경로의 값을 갖는 `routerLink`를 통과시킬 수 있습니다. 이를 구현하려면 `index.html` 파일에 몇 가지 링크를 추가하십시오.

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <nav>
        <button router-link="/">Home</button>
        <button router-link="/about">About</button>
        <button router-link="/contact">Contact</button>
        <button router-link="/unknown">Error</button>
    </nav>
    <h1>Javascript Routing</h1>
    <div id="app">
    </div>

    <script src="main.js"></script>
</body>
</html>
```

우리가 전달한 `라우터 링크` 속성을 주목하라. 이것이 우리가 라우팅에 사용할 것이다.

변수 저장소의 모든 `라우터 링크`를 생성하여 배열에 저장합니다.

```js
let definedRoutes = Array.from(document.querySelectorAll('[router-link]'));
```

라우터 링크를 배열에 저장한 후 반복하여 "내비게이션()" 함수를 호출하는 클릭 이벤트 수신기를 추가할 수 있습니다.

```js
 //iterate over all defined routes
    definedRoutes.forEach(route => {
        route.addEventListener('click', navigate, false)
    })
```

### 탐색 기능 정의

탐색 기능은 Javascript History API를 사용하여 탐색합니다. history.pushState() 메서드는 브라우저의 세션 기록 스택에 상태를 추가합니다.

버튼을 클릭하면 해당 버튼의 라우터 링크 속성이 수신되고 `history.pushState()`를 사용하여 해당 경로로 이동한 다음 렌더링된 HTML 템플릿을 변경합니다.

```js
  // method to navigate
    let navigate = e => {
        let route = e.target.attributes[0].value;

        // redirect to the router instance
        let routeInfo = routerInstance.routes.filter(r => r.path === route)[0]
        if (!routeInfo) {
            window.history.pushState({}, '', 'error')
            root.innerHTML = `This route is not Defined`
        } else {
            window.history.pushState({}, '', routeInfo.path)
            root.innerHTML = `You are on the ${routeInfo.name} path`
        }
    }
```

navlink에 `routeInstance`에서 정의되지 않은 라우터 링크가 있는 경우 푸시 상태를 `error`로 설정하고 템플릿에 `This route is defined`로 렌더링합니다.

그런 다음 경로를 별도의 파일에 저장하여 코드를 더 깔끔하게 만들고 오류가 있을 경우 디버깅하기 쉽게 만드는 방법을 고려해야 합니다. 이제 `routes.js` 파일을 생성하고 경로 생성자 및 라우터 인스턴스를 이 새 파일로 추출합니다.

```js
//router instance
let Router = function (name, routes) {
    return {
        name,
        routes
    }
};
let routerInstance = new Router('routerInstance', [{
        path: "/",
        name: "Root"
    },
    {
        path: '/about',
        name: "About"
    },
    {
        path: '/contact',
        name: "Contact"
    }
])

export default routerInstance
```

이 파일을 내보내면 다른 JavaScript 파일에 액세스할 수 있습니다. main.js 파일로 가져올 수 있습니다.

```coffeescript
import routerInstance from './routes.js'
```

오류가 발생합니다. 수정하려면 index.html 파일의 스크립트 태그를 다음과 같이 수정하십시오.

```xml
<script type="module" src="main.js"></script>
```

모듈 유형을 추가하면 모듈 외부에서 액세스할 수 있는 변수와 기능이 지정됩니다.

## 결론

Vanilla JavaScript에서 라우팅 시스템을 구현하는 방법을 이해하면 개발자들이 Vue.js Router와 같은 프레임워크 라우팅 라이브러리로 더 쉽게 작업할 수 있다. 여기에 있는 저희 코드는 한 페이지 응용 프로그램에서 재사용할 수 있으며, 프레임워크 없이 작업할 때 완벽합니다. 소스 코드를 가져오려면 GitHub를 확인하십시오.