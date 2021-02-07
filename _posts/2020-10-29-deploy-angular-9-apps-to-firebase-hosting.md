---
layout: post
title: "Firebase Hosting에 Angular 9+ 애플리케이션 배포"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/DeployAngular9appstoFirebasehosting.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/DeployAngular9appstoFirebasehosting.png?fit=730%2C487&ssl=1)

파이어베이스는 애플리케이션 개발 플랫폼입니다. 호스팅, 데이터베이스, 파일 스토리지, 분석 및 인증과 같은 서비스를 제공합니다. 플랫폼의 주요 제품 중 하나인 Firebase Hosting은 콘텐츠 전송 네트워크에서 사용자의 웹 자산을 호스팅합니다. Firebase Hosting을 사용하면 모바일 앱 페이지, 단일 페이지 애플리케이션, 정적 사이트, 진행형 웹 앱에서 마이크로서비스 및 API에 이르는 모든 종류의 프로젝트를 배포할 수 있습니다.

Angular 앱이 컴파일되면 몇 가지 단계를 거쳐 Firebase에 배포할 수 있습니다. 사이트를 배포하려면 Google Cloud Platform 프로젝트가 필요합니다. 이러한 프로젝트는 Firebase 서비스를 사용하며 이를 Firebase 프로젝트라고 합니다. 이 프로젝트는 Firebase 콘솔에 생성되지만 GCP 및 Google API 콘솔에서 액세스할 수 있습니다. 배포 시 사이트에는 firebaseapp.com 도메인과 web.app 도메인에서 두 개의 사용 가능한 하위 도메인이 자동으로 할당됩니다. 그러나 선택한 경우 배포된 사이트에 대해 고유한 도메인을 사용자 정의할 수 있습니다. 또한 각 사이트에 대해 무료 SSL 인증서가 프로비저닝됩니다.

Firebase Hosting을 사용하면 단일 프로젝트에 여러 사이트를 배포할 수 있습니다. 이 기능은 인증, 스토리지, API, 데이터베이스 등과 같은 Firebase 리소스 및 서비스를 공유하려는 경우에 특히 유용합니다. 예를 들어, 고객 대상 사이트는 하나의 프로젝트 아래에 있는 관리 사이트와 동일한 데이터베이스를 공유할 수 있습니다. 이 접근 방식을 사용하면 서로 다른 도메인에서 서로 다른 배포 구성으로 서로 다른 애플리케이션을 실행하는 동안 리소스를 공유할 수 있습니다. 그러나 이 설정은 데이터베이스와 같은 프로덕션 리소스를 조작할 수 있으므로 프로덕션, 스테이징, 개발 등과 같은 서로 다른 워크플로 환경에서는 권장되지 않습니다.

Firebase 클라우드 기능을 사용하면 Firebase에 Angular Universal 앱을 배포할 수 있습니다. Cloud Functions는 백엔드 코드를 실행하는 서버 없는 프레임워크입니다. HTTP 요청 및 기타 Firebase 기능과 같은 이벤트에 의해 트리거됩니다. 서버를 설정하고 관리하고 확장할 필요가 없습니다. Angular Universal 앱을 Firebase에 배포할 때 Cloud Function은 추가 개발 또는 구성 없이 서버 측 렌더링을 수행합니다.

이 기사에서는 단일 프로젝트를 사용하여 Angular 애플리케이션을 Firebase에 배포하는 방법에 대해 설명합니다. 다음으로, 하나의 프로젝트에 여러 Angular 앱을 배포하는 방법에 대해 살펴보겠습니다. 또한 클라우드 기능을 사용하여 Angular Universal 앱을 배포하는 방법에 대해서도 알아보겠습니다. 마지막으로, 다시 굴리는 방법에 대해 간략하게 설명하겠습니다.

## 전제조건

앱을 배포하려면 Firebase 계정이 필요합니다. Firebase는 로그인할 때 Google 계정을 사용해야 합니다. 계정이 아직 없는 경우 여기서 계정을 만들 수 있습니다. Firebase는 앱 배포에 활용할 수 있는 무료 스파크 계획을 제공합니다. 이 계획에 따르면 최대 10GB의 파일을 호스팅하고 월 125K 클라우드 기능 호출을 무료로 할 수 있습니다.

Angular CLI도 설치해야 합니다. 우리가 탐색할 배치 접근 방식은 @각선/화재 라이브러리에 의존한다. Angular CLI와 함께 빌드, 로컬 미리 보기 및 배포에 사용합니다. Angular CLI가 없는 경우 다음을 실행하여 설치할 수 있습니다.

```coffeescript
npm install -g @angular/cli
```

마지막으로 Firebase CLI 도구를 설치해야 합니다. 이를 통해 Firebase 계정을 인증 및 액세스하고 프로젝트 및 배포 대상을 생성할 수 있습니다. 설치하려면 다음을 수행합니다.

```coffeescript
npm install -g firebase-tools
```

## Firebase CLI를 사용하여 로그인 및 프로젝트 생성

배포하려면 먼저 CLI 도구를 Firebase 계정에 로그인해야 합니다. 실행:

```undefined
firebase login
```

`firebase login` 명령은 CLI 사용 및 오류 보고 정보를 수집하도록 요청할 수 있습니다. 원하는 대로 대답하세요. 이 명령은 로그인할 Google 계정을 선택하라는 브라우저로 리디렉션합니다. 그런 다음, Firebase CLI에서 Google 계정에 액세스하고 Firebase 계정과 관련된 일부 작업을 수행할 수 있도록 승인하라는 메시지가 표시됩니다. 허용할 경우 아래 스크린샷과 같이 "Firebase CLI 로그인 성공" 대화 상자가 표시됩니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/firebaselogin-nocdn-1024x788.png)

그런 다음 Angular 앱을 배포할 Firebase 프로젝트를 생성해야 합니다. 이 작업은 다음과 같이 실행합니다.

```css
firebase projects:create -n <YOUR_PROJECT_DISPLAY_NAME>
```

프로젝트의 표시 이름과 함께 `-n` 플래그를 전달합니다. 고유한 프로젝트 ID를 입력하라는 메시지가 표시됩니다. 이 ID는 Firebase에 생성된 모든 프로젝트에서 고유해야 합니다. 고유하지 않으면 프로젝트 생성에 실패합니다. 성공하면 다음과 같은 "프로젝트가 준비되었습니다!"라는 메시지가 표시됩니다.

이 명령을 실행하여 프로젝트가 성공적으로 생성되었는지 확인할 수 있습니다. 액세스 권한이 있는 모든 Firebase 프로젝트가 나열됩니다.

```cpp
firebase projects:list
```

firebase projects:create 명령을 사용하여 생성한 프로젝트에는 기본적으로 Cloud Logging이 연결되어 있지 않습니다. 프로젝트 설정 > 통합에서 이러한 종류의 프로젝트를 Firebase 콘솔의 클라우드 로깅에 연결할 수 있습니다.

## 프로젝트에 단일 Angular 앱 배포

먼저 각 소방서 라이브러리를 아직 설치하지 않은 경우 응용 프로그램에 추가하는 것부터 시작하십시오. 응용 프로그램의 루트 디렉터리에서 다음을 실행하십시오.

```css
ng add @angular/fire
```

필요한 종속성이 설치되면 Angular(각각) 앱과 연결할 Firebase 프로젝트를 선택하라는 메시지가 표시됩니다. 위에서 만든 프로젝트를 선택하십시오.

이 단계를 완료하면 `.firebaser`와 `firebase`라는 두 개의 파일이 생성됩니다.json. 모든 호스팅 및 프로젝트 구성은 `firebase`에 저장됩니다.json의 파일 .firebaser` 파일은 기본 프로젝트를 지정합니다. 이 파일은 "firebase use" 명령에서 프로젝트 간을 빠르게 전환하는 데 사용됩니다. 또한 `angular.json` 파일의 설계 도구 구성 섹션이 업데이트됩니다. @각선/fire:deploy를 빌더로 지정하는 deploy 대상이 포함된다. 마지막으로 `패키지`입니다.json과 json은 최신 종속성을 반영하여 업데이트된다.

컴파일된 앱이 배포되면 어떤 모습일지 미리 보려면 --prod(프로드)와 --watch(시계) 플래그로 구축한다. --prod 플래그는 빌드 구성이 프로덕션 대상에 대한 구성이어야 함을 지정합니다. `--watch` 플래그는 파일을 변경할 때 앱을 다시 빌드해야 함을 나타냅니다.

```undefined
ng build --prod --watch
```

앱을 서비스하려면 아래 명령을 실행하고 `http://localhost:5000`으로 이동하여 확인하십시오.

```undefined
firebase serve
```

앱을 배포하려면 다음을 실행합니다.

```undefined
ng deploy
```

이 명령은 응용 프로그램을 `dist/`로 빌드한 다음 결과 출력 파일을 배포합니다. 이 앱은 .web.app과 .firebaseapp.com 하위 도메인에 배포된다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/ngdeploy-nocdn.png)

## 단일 프로젝트에 여러 애플리케이션 배포

단일 프로젝트에 여러 앱을 배포하려면 각 앱에 해당하는 Firebase 사이트를 만들어야 합니다. Firebase 콘솔에서 프로젝트 개요의 호스팅 설정으로 이동합니다. 예를 들어, 한 프로젝트에 포트폴리오와 블로그를 구축하려고 한다고 가정해 보겠습니다. 우리는 두 개의 사이트를 만들 것이다. 하나는 blog-zc.web.app이고 다른 하나는 portfolio-zc.web.app이다.

- 그런 다음 위에서 설명한 대로 "@각/fire" 라이브러리를 각 프로젝트에 추가합니다(아직 사용하지 않은 경우).
- 뒷부분의 설명에 따라 각 응용 프로그램을 로컬에서 미리 봅니다.
- 각 앱이 각자의 하위 도메인에 배포되도록 하려면 각 앱에 대한 배포 대상을 설정해야 합니다. 배포 대상은 프로젝트 내의 Firebase 리소스를 나타내는 짧은 이름 식별자입니다. 배포 대상은 구성이 다른 Firebase 리소스 유형의 인스턴스가 여러 개 있는 경우에 특히 유용합니다. 사용자에 의해 설정됩니다.

이 경우 위에서 생성한 두 개의 서로 다른 Firebase Hosting 사이트에 대한 배포 대상을 설정합니다. `firebase target:apply < <target_name`을(를) 실행합니다. 여기서:

- `resource_type`은 호스팅, 스토리지 버킷, 데이터베이스,
- `target_name`은 배포 대상에 할당하는 이름입니다.
- `resource_name`은 Firebase Console의 리소스에 할당된 이름입니다(이 경우 호스팅 사이트 하위 도메인).

각 앱 루트 디렉토리에서 다음을 실행합니다.

```shell
# Run this in the portfolio app root directory
firebase target:apply hosting portfolio portfolio-zc
# Run this in the blog app root directory
firebase target:apply hosting blog blog-zc
```

이를 실행하면 .firebaser의 호스팅 항목이 새 호스팅 사이트를 포함하도록 수정됩니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/adddeploytarget-nocdn.png)

이제 각 앱을 배포하기만 하면 됩니다. 앱 루트 디렉토리에서 다음을 실행합니다.

```undefined
ng deploy
```

## Angular Universal 앱 배포

Angular Universal은 서버에서 Angular 앱을 렌더링합니다. 요청 시 클라이언트에 부트스트랩되는 정적 페이지를 앱에서 생성합니다. 이러한 방식으로 Angular 앱을 렌더링하기 위해 Firebase Cloud Functions와 Angular Universal의 Angular Express Engine 모듈을 사용합니다.

먼저 Angular Express Engine을 설치하는 것부터 시작합니다. 아직 설치되어 있지 않은 경우:

```css
ng add @nguniversal/express-engine
```

위의 명령을 실행하면 필요한 종속성이 설치되고, 클라우드 기능, 서버측 앱 모듈 및 서버 앱의 부트스트래퍼에서 실행되는 고속 웹 서버가 생성되며, 추가 앱 구성이 수정됩니다. 이 도식을 추가할 때 앱에서 수정할 내용에 대한 자세한 설명을 보려면 이 링크를 방문하십시오.

다음으로, 앱 종속성에 아직 속하지 않은 경우 Angular Firebase 라이브러리를 추가합니다.

```css
ng add @angular/fire
```

설치하는 동안 앱이 설정의 일부로 Angular Universal을 사용하고 있는지 확인합니다. 앱에서 사용하는 것이 감지되면 방화벽 기능으로 배포해야 하는지 여부를 묻습니다. 이 프롬프트에 yes를 입력합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/firebasefunctiondeployment.png?resize=730%2C141&ssl=1)

그런 다음 앱과 연결할 Firebase 프로젝트를 선택하라는 메시지가 표시됩니다. 계정에 액세스할 수 있는 프로젝트 목록에서 하나를 선택합니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/pleaseselectaproject-nocdn.png)

이 명령을 실행하면 `firebase`가 생성됩니다.지정한 구성에 따라 json 및 `.firebaser` 파일이 생성됩니다. 또 firebase 배포 빌더로 angle.json 파일을 업데이트하고 ssr 배포 옵션을 true로 설정한다.

앱을 배포하기 전에 미리 볼 수 있는 몇 가지 방법이 있습니다. 다음을 사용할 수 있습니다.

```coffeescript
npm run dev:ssr
```

이 메서드는 `ssr-dev-server` 빌더를 사용합니다. 이 방법으로 미리 볼 때 브라우저에서 `http://localhost:4200`에서 앱에 액세스할 수 있습니다.

배포 전에 미리 보기를 선택할 수도 있습니다. 이 두 번째 방법은 Firebase 기능 에뮬레이터를 사용하여 빠른 서버를 실행하여 앱을 렌더링합니다. 그러나 앱 미리 보기를 완료하면 배포를 취소해야 합니다. 명령을 실행하면 배포를 완료할지 여부를 묻는 메시지가 표시됩니다. 앱을 프로덕션에 배포할 준비가 될 때까지 이 프롬프트에 아니요로 대답합니다. 이 앱 미리 보기는 `http://localhost:5000`에서 볼 수 있습니다.

```undefined
ng deploy --preview
```

앱이 제대로 작동하면 다음을 배포합니다.

```undefined
ng deploy
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/Deploymentresults.png?resize=730%2C525&ssl=1)

버전 8보다 큰 Node.js를 실행하는 경우 무료 스파크 요금제에서 종량제 요금제 블레이즈 요금제로 전환해야 합니다. 이는 무료 스파크 계획이 Cloud Functions의 Node.js 8 런타임으로 제한되지만 Blaze 계획은 Node.js 10 이상의 런타임들을 지원하기 때문이다. 그러나 Blaze 계획에는 클라우드 기능을 위한 무료 계층이 특정 제한까지 있습니다. 이 링크에서 각 가격 책정 계획에 대해 자세히 알아볼 수 있습니다.

## 롤백스

호스트 사이트의 현재 릴리스에 문제가 있는 경우 Firebase 콘솔에서 이전 작동 버전으로 앱을 롤백할 수 있습니다. Firebase CLI 도구를 사용하여 이전 버전으로 롤백할 수 없습니다. 이전 버전으로 롤백하려면 Firebase 콘솔에 있는 프로젝트의 호스팅 페이지로 이동합니다. 수정할 사이트를 선택하고 릴리스 기록 테이블에서 롤백할 버전을 선택한 후 드롭다운 메뉴를 열고 롤백을 선택합니다.

## 결론

Firebase Hosting은 컴파일된 Angular 앱의 배포를 지원합니다. Angular Fire 라이브러리를 사용하면 프로젝트 설정과 Firebase의 사이트 배포에 필요한 많은 작업이 자동화됩니다. Angular CLI와 Angular Fire 라이브러리를 모두 사용하면 단일 프로젝트 및 서버측 렌더링된 앱의 여러 사이트처럼 복잡한 프로젝트 배포를 간소화할 수 있습니다. Firebase에서 호스팅하는 방법에 대해 자세히 알아보려면 이 링크를 방문하십시오.