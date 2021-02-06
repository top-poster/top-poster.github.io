---
layout: post
title: "DX를 개선하기 위해 Docker 및 Docker Composite와 함께 Node.js 사용"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/node-js-docker-docker-compose-dx.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/node-js-docker-docker-compose-dx.png?fit=730%2C487&ssl=1)

지난 5년 동안 도커와 노드.js는 모두 큰 인기를 끌었다. 이 단계별 가이드에서는 Node.js를 도커와 함께 사용하여 개발자 경험을 개선할 수 있는 방법을 확인할 것이다.

이 튜토리얼에서는 `도커 빌드`를 효율적으로 사용하는 방법과 원활한 현지 개발 환경을 달성하기 위해 도커 컴포지트를 활용하는 방법에 대해 자세히 설명합니다. 우리는 demo Express.js 애플리케이션을 예로 들겠습니다. 슬슬 출발 해야지.

## 전제조건

- Node.js 및 npm의 기본 사항에 대해 잘 알고 있어야 합니다. 우리는 최신 LTS 버전인 노드 14를 사용할 것입니다.
- Express.js 프레임워크의 기본 사항에 유의해야 합니다.
- 도커에 대한 실무 지식이 있어야 해요.

이 튜토리얼에서는 셸이 있는 Linux 또는 macOS와 같은 유닉스 계열 시스템에서 실행되는 명령을 사용합니다. 이것은 우리가 앱의 설정에 직접 뛰어들 수 있는 축약된 포스트가 될 것입니다. 만약 여러분이 그렇게 마음이 내키면 도커와 노드로 읽어보는 것을 원할 것입니다.

당신은 내가 공공 GitHub 저장소에서 앱을 만든 방법을 여섯 번의 꺼내기 요청 순서로 분석할 수 있다.

## Express 생성기를 사용하여 새 Express.js 프로젝트 생성

데모 응용 프로그램을 생성하려면 Express 응용 프로그램 생성기를 사용합니다. 이를 위해 아래 npx 스크립트를 실행합니다.

```xml
npx express-generator --view=pug --git <app-name>
```

실행한 명령을 분석하기 위해 Express 생성기에 Express 앱을 생성하도록 요청했습니다. --view=pug는 생성자에게 Pug view 엔진을 사용하도록 하고 --git 매개 변수는 .gitignore 파일을 추가하도록 요청합니다. 물론 app-name을(를) 응용 프로그램 이름으로 바꾸어야 합니다. 나는 nodejs-docker-express를 예로 사용하고 있다.

다음과 같은 내용을 제공합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/express-application-generator-output.png?resize=730%2C392&ssl=1)

## Express 앱 테스트

앱을 테스트하려면 먼저 `npm install`을 실행하여 필요한 모든 npm 모듈을 설치합니다. 그런 다음 아래 명령을 실행하여 앱을 시작합니다.

```undefined
DEBUG=nodejs-docker-express:* npm start
```

그런 다음 `nodejs-docker-express:server Listening on port 3000`과 같은 메시지가 표시됩니다. 위의 명령은 매우 간단합니다. 이 명령은 서버의 상세 디버그를 지시하는 "nodejs-docker-express:*it" 값을 가진 "DEBUG"라는 환경 변수를 사용하여 `npm start`를 실행했습니다.

Windows에 있는 경우 `설정 DEBUG=nodejs-docker-express:*

이제 브라우저를 열고 `http://localhost:3000`을 입력하여 아래 출력과 같은 출력을 확인하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/express-demo-app-render.png?resize=730%2C310&ssl=1)

만세! 맨 뼈 Express.js 앱이 이미 실행 중입니다. 이제 해당 명령줄 창에서 `Ctrl+c`로 서버를 중지할 수 있습니다. Node.jsexpress 응용 프로그램을 도킹하도록 하겠습니다.

## Docker 다단계 빌드를 사용하여 앱 도킹

우리의 애플리케이션을 컨테이너화 하는 것은 많은 우여곡절을 가지고 있다. 우선, 실행 중인 플랫폼에 관계없이 동일하게 동작합니다. Docker 컨테이너를 사용하면 AWS Fargate, Google Cloud Run 또는 사용자 자신의 Kubernetes 클러스터와 같은 플랫폼에 앱을 쉽게 배포할 수 있습니다.

도커 파일로 시작하겠습니다. 도커 파일은 도커 이미지가 빌드되는 Blueprint입니다. 빌드된 이미지가 실행 중일 때 컨테이너라고 합니다. 이 게시물은 이 세 용어의 차이점을 설명하는 데 도움이 됩니다. 다음은 간단한 시각적 설명입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/dockerfile-flow-diagram.jpg?resize=730%2C236&ssl=1)

이 과정은 매우 간단합니다. 우리는 도커 파일에서 도커 이미지를 만듭니다. 실행 중인 도커 이미지를 도커 컨테이너라고 합니다.

### 기본 단계 설정

Docker 파일이 어떻게 표시되는지 확인할 시간입니다. 보너스로, 멀티 스테이지 빌드를 활용하여 보다 빠르고 효율적으로 빌드할 수 있습니다.

```coffeescript
FROM node:14-alpine as base

WORKDIR /src
COPY package*.json /
EXPOSE 3000

FROM base as production
ENV NODE_ENV=production
RUN npm ci
COPY . /
CMD ["node", "bin/www"]

FROM base as dev
ENV NODE_ENV=development
RUN npm install -g nodemon && npm install
COPY . /
CMD ["nodemon", "bin/www"]
```

위의 도커 파일에서는 다단계 빌드를 사용한다. 기본, 생산, 개발의 세 단계로 구성되어 있습니다. 기본 단계에는 개발 및 생산에서 공통되는 것들이 있습니다. 그래픽으로 다음과 같이 표현할 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/dockerfile-stages.jpg?resize=628%2C374&ssl=1)

유산을 생각나게 하나요? 이것은 도커 이미지에 대한 일종의 상속입니다. 우리는 슬림한 생산 단계와 기능이 풍부하고 개발에 중점을 둔 개발 단계를 사용하고 있습니다.

차근차근 살펴봅시다.

```undefined
FROM node:14-alpine as base
```

먼저, 우리는 도커에게 마지막 LTS 버전인 공식 도커 노드 알파인 이미지 버전 14를 사용하라고 말한다. 이 이미지는 DockerHub에서 공개적으로 사용할 수 있습니다. 우리는 공식 Node.js Docker 이미지의 알파인 변형을 사용하고 있는데, 메인 이미지의 경우 345MB에 비해 40MB가 조금 안 되기 때문이다.

이 도커 파일은 다단계 빌드를 사용하기 때문에 우리는 또한 `기준`을 지정한다. 이름 지정은 사용자의 몫입니다. 빌드 프로세스 후반에 "확장"되기 때문에 이 베이스를 사용하고 있습니다.

```undefined
WORKDIR /src
COPY package*.json /
EXPOSE 3000
```

`WORKDIR`는 설정 후 실행되는 후속 `RUN` 명령의 컨텍스트를 설정합니다. 우리는 소포만 복사한다.json과 package-lock.json 파일은 컨테이너에 저장함으로써 더 나은 도커 빌드 캐슁으로 더 빠른 빌드를 얻을 수 있습니다.

다음 줄은 컨테이너의 3000번 항구를 노출하는 것이다. 기본적으로 Node.js Express 웹 서버가 실행되는 포트입니다. 위의 단계는 개발 단계와 생산 단계 모두에 공통적으로 적용됩니다.

이제 생산 목표 단계가 어떻게 구축되는지 살펴볼 수 있습니다.

### 생산단계 설정

```coffeescript
FROM base as production
ENV NODE_ENV=production
RUN npm ci
COPY . /
CMD ["node", "bin/www"]
```

생산 단계에서는 베이스 스테이지로 중단했던 부분을 계속합니다. 여기 라인이 도커에게 베이스에서 시작하라고 지시하기 때문입니다. 이어서 도커에게 환경변수 `NODE_ENV`를 `생산`으로 설정해 줄 것을 요청한다.

이 변수를 프로덕션으로 설정하면 성능이 3배 향상되고 캐시된 보기와 같은 다른 이점도 있다고 합니다. npm install을 실행하면 기본 종속성만 설치되고 dev 종속성은 제외됩니다. 이러한 설정은 프로덕션 환경에 적합합니다.

다음으로 npm install이 아닌 npmci를 운영한다. npmci는 지속적인 통합과 구축을 목표로 한다. 사용자 중심의 일부 기능을 우회하기 때문에 npm 설치보다 속도가 훨씬 빠르다. npmci는 package-lock.json 파일이 있어야 작동합니다.

그 후, 우리는 `/src`에 코드를 복사한다. 이것은 우리의 workdir이기 때문이다. 이곳은 우리가 가지고 있는 커스텀 코드를 컨테이너에 복사하는 곳입니다. 따라서 Node 명령으로 `bin/www` 명령을 실행하여 웹 서버를 실행합니다.

다단계 빌드를 활용하고 있기 때문에 개발 단계에서만 개발에 필요한 구성요소를 추가할 수 있습니다. 그것이 어떻게 이루어졌는지 보자.

```coffeescript
FROM base as dev
ENV NODE_ENV=development
RUN npm install -g nodemon && npm install
COPY . /
CMD ["nodemon", "bin/www"]
```

생산과 유사하게, 개발은 또한 기초 단계에서부터 "확장"하고 있다. 이 단계에서 우리는 `NOD_ENV` 환경 변수를 `개발`로 설정하고 있다. 그 후에는 데몬을 설치하지 않습니다. 파일이 변경될 때마다 서버를 재시작하는 데몬이 없으므로 개발 환경이 훨씬 원활해집니다.

그런 다음 develocity가 있는 경우 develocity를 설치하는 npm 설치를 정기적으로 수행합니다. 현재 `패키지`에 있습니다.json, 편차 의존성은 없습니다. 예를 들어, 우리가 Jest로 우리의 앱을 테스트한다면, 그것은 개발의존성들 중 하나일 것이다. 두 명령이 `와 함께 결합됨`에 주목하십시오.

이전 단계와 마찬가지로 `/src`의 컨테이너에 코드를 복사합니다. 그러나 이번에는 개발 환경이기 때문에 각 파일 변경 시 웹 서버를 다시 시작하기 위해 데몬 없이 웹 서버를 실행합니다.

### .dockerignore를 무시하지 마세요!

.gitignore가 없으면 Git를 사용하지 않듯이 Docker를 사용할 때는 .dockerignore 파일을 추가하는 것이 좋습니다. .dockerignore는 Docker 이미지에 도달하지 않으려는 파일을 무시하는 데 사용됩니다. Docker 이미지를 작게 유지하고 관련 없는 파일 변경을 무시하여 빌드 캐시를 보다 효율적으로 유지할 수 있습니다. .dockerignore 파일은 다음과 같습니다.

```css
.git
node_modules
```

매우 간단합니다. 우리는 도커에게 호스트에서 도커 컨테이너로 `.git` 폴더와 `node_modules`를 복사하지 말라고 지시하고 있습니다. 컨테이너 안에서 npmci나 npm 인스톨을 하다 보면 일관성이 유지된다.

## Docker Composite 추가 - 빌드 대상을 잊지 마십시오.

현재 Node.js Express 앱을 Docker와 함께 실행하는 데 필요한 대부분의 사항이 있습니다. 다음으로 접착제로 붙여야 할 것은 도커 컴포지트입니다.

컴포지트를 사용하면 단일 또는 여러 개의 컨테이너로 애플리케이션을 쉽게 실행할 수 있습니다. 컨테이너를 제작하거나 실행하기 위해 매우 긴 명령을 기억할 필요가 없습니다. "docker-compose build"와 "docker-compose"를 실행할 수 있는 한 응용 프로그램은 쉽게 실행됩니다.

밝은 면에서는 Docker 설치와 함께 사전 설치됩니다. 개발 환경에서 주로 사용됩니다.

아래는 이 튜토리얼의 프로젝트 루트에 있는 "docker-compose.yml" 파일입니다.

```undefined
version: '3.8'
services:
  web:
    build:
      context: ./
      target: dev
    volumes:
      - .:/src
    command: npm run start:dev
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: development
      DEBUG: nodejs-docker-express:*
```

먼저, 우리는 도커 컴포지트 버전을 지정하는데, 우리의 경우, 도커 엔진 19.0.3에서 지원하는 최신 버전인 3.8이다. 이를 통해 다단계 도커 빌드를 사용할 수도 있습니다.

다음으로, 사용 중인 서비스를 지정합니다. 이 튜토리얼의 경우 web이라는 이름의 서비스가 하나만 있습니다. 현재 디렉터리의 빌드 `context`와 `target`의 중요한 빌드 매개 변수가 `dev`로 설정되어 있다. Docker에게 Devstage를 사용하여 Docker 이미지를 구축하려고 합니다.

그 후 도커 볼륨을 지정합니다. Docker 컨테이너에 있는 `/src`로 호스트의 로컬 디렉토리 `/`에서 변경 내용을 복사하고 동기화하도록 지시합니다. 이것은 호스트 컴퓨터에서 파일을 변경할 때 유용하며, 컨테이너 내부에도 즉시 반영될 것입니다.

결과적으로, 우리는 `package`에 추가된 `npm run start:dev` 명령을 사용한다.json 파일:

```undefined
"start:dev": "nodemon ./bin/www"
```

그래서 우리는 데몬 없이 웹 서버를 시작하고 싶습니다. 개발 환경이기 때문에 각 파일 저장 시 서버를 재시작합니다.

다음으로, 컨테이너 포트 3000과 호스트 시스템의 포트 3000을 매핑합니다. 컨테이너를 만들 때 포트 3000을 노출시켰고, 웹 서버도 3000에서 실행됩니다.

마지막으로, 우리는 두 가지 환경 변수를 설정합니다. 첫째, 상세 오류를 보고 뷰 캐싱을 하지 않기 때문에 `개발`로 설정된 `NODE_ENV`이다. 그런 다음, 익스프레스 웹 서버가 모든 것에 대한 디버그 메시지를 출력하도록 하는 디버그를 `*`로 설정합니다.

## Docker 및 Docker Composite를 사용하여 앱 테스트

필요한 부품을 모두 설치했으니 이제 도커 이미지를 구축해 봅시다. 우리는 BuildKit로 도커 빌드를 최적화할 것이다. Docker 이미지는 BuildKit를 활성화하면 훨씬 빠르게 빌드됩니다. 다음 명령을 실행합니다.

```undefined
COMPOSE_DOCKER_CLI_BUILD=1 DOCKER_BUILDKIT=1 docker-compose build
```

여기서는 Composite에 BuildKit를 사용하여 Docker 이미지를 빌드하도록 지시합니다. 잠시 후 실행되어 아래와 같이 Docker 이미지를 구축해야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/docker-image-buildkit.png?resize=730%2C256&ssl=1)

Docker 이미지는 약 14초 만에 제작되었습니다. BuildKit를 통해 훨씬 더 빨라졌습니다. 이미지를 실행해 보겠습니다.

```undefined
docker-compose up
```

이 경우 다음과 같은 결과가 초래될 수명은 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/running-docker-image.png?resize=730%2C219&ssl=1)

그런 다음 브라우저에서 `http://localhost:3000`을 누르면 다음을 볼 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/running-express-app-docker.png?resize=730%2C310&ssl=1)

좋아! 우리 앱이 도커랑 잘 작동되고 있어. 이제 파일을 변경하여 올바르게 반영되는지 확인해 보겠습니다.

## 복구에 대한 파일 변경 노데몬에서 다시 시작

우리의 목표는 테스트로 "Welcome to Express"를 "Welcome to Express with Docker"로 변경하는 것입니다. 이렇게 하려면 line 6에서 routes/index.js 파일을 변경하여 아래와 같이 만들어야 합니다.

```bash
res.render('index', { title: 'Express with Docker' });
```

파일을 저장하는 즉시 웹 서버가 재시작되는 것을 볼 수 있습니다. 이것은 우리의 도커 볼륨과 노데몬이 예상대로 제대로 작동하고 있다는 것을 분명히 보여준다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/nodemon-restart-server.png?resize=730%2C80&ssl=1)

이 시점에서 `http://localhost:3000`을 실행하는 브라우저 탭을 새로 고치면 다음이 나타납니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/express-demo-app-render.png?resize=730%2C310&ssl=1)

Hurray! Docker Composite가 구성된 로컬 환경에서 Docker와 함께 Express 앱을 성공적으로 실행했습니다. 등을 한 번 두드려 보세요!

## 다음 단계 및 결론

Docker Composite는 여러 컨테이너를 시작하는 데 매우 유용합니다. 따라서 애플리케이션의 데이터 소스로 Mongo 또는 MySQL을 추가하려면 Docker-compose 파일에서 다른 서비스로 쉽게 수행할 수 있습니다.

이 튜토리얼의 목적상, 우리는 단일 컨테이너가 실행 중인 Node.js에만 초점을 맞출 것이다.

Node.js와 도커는 매우 잘 어울린다. 도커 컴포즈를 사용하면 개발 경험이 훨씬 원활해집니다. 이 튜토리얼을 빌드 베이스로 사용하여 Docker 및 Node.js를 사용하여 보다 고급 기능을 사용해 볼 수 있습니다. 해피 코딩!