---
layout: post
title: "Next.js에서 영역 탐색"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/exploring-zones-next-js.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/exploring-zones-next-js.png?fit=730%2C487&ssl=1)

Next.js는 멋지고, 프런트 엔드 개발자들이 선호하는 도구가 되었습니다. Next.js의 발전은 놀라웠습니다. Next.js는 프로덕션 지원 애플리케이션을 빠르고 쉽게 작성할 수 있도록 도와주는 많은 기능을 가지고 있습니다.

이 프레임워크는 모든 경로가 페이지 폴더 내에 있을 것으로 예상한다. 그러나 실제 시나리오에서는 개별 페이지나 구성 요소를 작업하는 팀이 많이 있을 것이며, 단일 구성 요소를 사용하는 것을 현실적으로 기대할 수 없습니다.

이 문제의 해결책은 구역이다. Next.js 문서에 따르면 영역은 Next.js 앱의 단일 배포입니다.

next.js를 사용하면 개별 Next.js 앱을 배포하기 위한 여러 영역을 가질 수 있으며 모든 영역에 단일 URL에서 액세스할 수 있습니다. 이를 통해 서로 다른 팀이 개별적으로 앱의 일부를 구축하고 배포할 수 있지만, 사용자의 말단에는 하나의 완전한 애플리케이션으로 표시됩니다.

## Next.js 영역은 어떻게 작동합니까?

영역에는 HTTP 프록시를 기반으로 하는 Next.js의 특정 API가 없습니다. Vercel에 내장된 지원으로 여러 영역을 구현하거나 고유한 HTTP 프록시를 만들 수 있습니다. 그러나 Vercel은 사용자 지정 프록시에 대한 예를 제공하지 않고 플랫폼에 얼마나 간단한지 보여줌으로써 자체 배포를 판매하려고 합니다.

### Vercel 사용

Vercel을 사용하면 아주 간단합니다. 존에 대한 지원이 내장되어 있으며 개별 앱의 빌드 및 해당 앱의 서비스 경로를 `vercel.json`으로 나열하면 됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/zones-implementation-vercel.png?resize=730%2C561&ssl=1)

### 사용자 지정 프록시 사용

사용자 지정 프록시에서 Nginx 서버와 같은 프록시 서버를 만들 수 있습니다. 이 서버는 내부적으로 각 Next.js 앱으로 리디렉션됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/zones-implementation-http-proxy.png?resize=730%2C552&ssl=1)

구역을 사용할 때 고려해야 할 두 가지 주요 사항은 다음과 같습니다.

- next.js에는 각 영역 내에 여러 페이지를 추가할 수 있는 규정이 있지만 모든 영역에서 동일한 이름을 가질 수 있는 두 페이지는 없습니다. 따라서 모든 영역의 모든 페이지는 고유한 이름을 가져야 합니다.
- 정적 파일에서 충돌이 발생할 수 있는 위험을 실행합니다.

정적 파일 간의 충돌을 해결하기 위해 우리는 `자산 접두사`를 활용할 수 있습니다. 이렇게 하면 `assetPrefix`를 기반으로 하는 모든 정적 파일에 대해 고유 경로를 사용할 수 있습니다. 여기서 더 읽어보세요.

```java
module.exports = {
  assetPrefix: "about/",
}
```

## HTTP 프록시를 사용한 샘플 구현

#### 1. Next.js 앱 2개 생성

nextjs-zones라는 폴더를 새로 만들고 create-next-app을 사용하여 next.js 프로젝트 두 개를 부트스트랩합니다.

각 프로젝트 내부에 페이지를 만들어 보겠습니다.

```js
// home/pages/home.js
function Home() {
  return (
    <div>
      <h1>Home</h1>
    </div>
  );
}
export default Home;
```

```js
// about/pages/about.js
function About() {
  return (
    <div>
      <h1>About</h1>
    </div>
  );
}
export default About;
```

#### 2. 두 앱을 서로 다른 포트에서 독립적으로 실행

홈 → `localhost:3000`

→ `localhost:3001` 정보

#### 3. Nginx 역방향 프록시를 생성하여 실행합니다.

```undefined
# Sample nginx config

daemon off;
pid nginx.pid;

http {
    server {
        listen       8000;
        server_name  http://localhost:8000;
        proxy_buffering off;
        proxy_http_version 1.1;

        location / {
            proxy_pass    http://localhost:3000/home;
            proxy_redirect off;
        }
        location /about/ {
            proxy_pass    http://localhost:3001/about;
            proxy_redirect off;
        }
    }
}
events {
}
```

현재 Nginx 서버는 localhost:8000에서 실행되며 홈 애플리케이션을 직접 가리키고 있다. 마찬가지로 localhost:8000/about은 About 애플리케이션을 가리킵니다. 홈(Home)과 어바웃(About)이 서로 다른 애플리케이션으로 실행되지만 지금은 로컬 호스트(8000)를 통해 접속이 가능하다.

이것은 개념을 설명하기 위한 매우 순진한 구현이다. 보다 강력한 구현을 통해 Docker 컨테이너 내부의 빌드 파일을 역프로시싱할 수 있습니다.

## Vercel을 사용한 샘플 구현

#### 1. Next.js 앱 2개 생성

위와 동일한 단계를 수행합니다.

#### 2. 'vercel.json' 파일 생성

Vercel을 배포 플랫폼으로 사용하는 경우 즉시 사용할 수 있는 영역을 지원합니다. 데이터가 있는 `vercel.json` 파일을 생성해 봅시다.

```undefined
// A sample vercel.json
{
  "version": 2,
  "build": {
    "env": {
      "BUILDING_FOR_NOW": "true"
    }
  },
  "builds": [
    { "src": "about/package.json", "use": "@now/next" },
    { "src": "home/package.json", "use": "@now/next" }
  ],
  "routes": [
    { "src": "/about(.*)", "dest": "about$1" },
    { "src": "(.*)", "dest": "home$1" }
  ]
}
```

여기서는 SSG 영역과 Next 영역이 동일한 URL에서 작동할 수 있는 등 다양한 유형의 영역을 함께 결합할 수 있습니다. 위의 예에서는 `홈`과 `정보` 모두에 대해 `@now/next`를 사용하고 있으며, URL을 기반으로 로드할 빌드를 Vercel이 결정하는 데 도움이 되는 경로를 정의한다.

이 접근 방식은 애플리케이션을 더 크게 분리하여 Next.js를 사용하는 대규모 팀에게 많은 문제를 해결할 수 있다. 이 기능은 웹 팩 모듈 연합에서 훨씬 더 잘 사용할 수 있으며, 마이크로 프런트 엔드의 차세대 주요 기능이 될 수 있습니다.

더 나은 접근 방식이나 이데올로기가 있다면 댓글로 알려주세요.