---
layout: post
title: "Netlifify에 React 앱을 배포하는 3가지 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/react-netlify-logos.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/react-netlify-logos.png?fit=730%2C487&ssl=1)

지난 몇 년 동안 Netlifify는 많은 개발자들의 마음과 마음에 쉽게 파고들었습니다. 일부 개발자는 Netliify가 소프트웨어 개발 및 배포에 있어 지금까지 발생한 일 중 최고라고 생각합니다.

Netliify는 놀라운 사이트를 구축하는 가장 빠른 방법이라고 자부합니다. 생산성에 큰 도움이 될 수 있습니다. Netlifify에는 이 튜토리얼의 앵커 역할을 할 수 있는 인기 있는 태그 라인이 있습니다.

> "전 세계에 배포하기 위해 노력하십시오."

### 전제조건

여기서 목표는 Netlifify를 사용하여 React 앱을 배포하는 방법에 대해 자세히 살펴보는 것입니다. 이 튜토리얼을 따르기 위해서는 다음 사항에 대한 실무 지식이 필요합니다.

- 대응 애플리케이션 구축
- 버전 제어(Github, Gitlab, Bitbucket 등)
- 넷라이프

### Netliify는 무엇을 제공해야 하는가?

Netlifify는 대부분의 기능이 완전히 무료라는 사실 외에도 다음을 제공하여 구축을 간소화합니다.

- 사용하기 쉬운 UI
- 몇 초 이내에 구축
- Rolling update(업데이트 롤링) - 사이트 업데이트 중에도 다운타임 없음
- 사이트의 이전 작업 버전으로 롤백
- 버전 제어를 통한 지속적인 통합 및 지속적인 구축
- 사용자 데이터 수집을 위한 즉각적인 양식
- 서버리스 작업 등을 위한 기능 넷라이프

## 배포를 위해 React 애플리케이션 준비

Netlifify에 애플리케이션을 배포하는 방법에는 세 가지가 있으며, 이 방법이 오늘 자세히 설명하겠습니다.

각 방법의 세부 사항에 대해 자세히 알아보기 전에 모든 다양한 배포 방법에 적용되는 몇 가지 일반적인 단계를 살펴보겠습니다.

### 반응 앱 만들기

React는 사용자 인터페이스와 UI 구성 요소를 구축하기 위한 오픈 소스 프런트 엔드 자바스크립트 라이브러리이다.반응 앱을 만들기 시작하려면 `생성-반응-앱` 라이브러리를 사용하여 `테스트-넷플라이-배포`라는 앱을 만들 것입니다.

터미널에서 다음 명령을 실행합니다.

```undefined
npm install -g create-react-app
create-react-app test-netlify-deployment
cd test-netlify-deployment
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/create-test-netlify.png?resize=730%2C409&ssl=1)

위의 명령은 일부 정적 파일과 함께 간단한 대응 응용 프로그램을 만드는 데 필요한 모든 패키지를 설치하므로 작업할 수 있는 기반이 됩니다.

이 튜토리얼의 경우 이 샘플 앱을 그대로 배포합니다. 샘플 어플리케이션을 원하는 만큼 변경해 주세요.

### 빌드 디렉터리 만들기

다음으로, 프로덕션에서 사용할 수 있는 빌드 폴더에 응용 프로그램 빌드를 생성하려고 합니다.

이 작업을 수행하려면 아래 명령을 실행합니다.

```coffeescript
npm run build
```

이 명령은 전체 응용 프로그램의 축소 버전을 나타내는 빌드 폴더를 생성합니다. 이 폴더에는 응용 프로그램을 배포하는 데 필요한 모든 항목이 포함됩니다.

이제 React 앱을 배포할 준비가 되었으므로 이 앱을 원하는 버전으로 제어할 수 있습니다. 이 경우에는 Github를 선택하겠습니다.

### Github를 사용한 버전 제어

많은 버전 제어 시스템이 있지만, 이 튜토리얼을 위해 우리는 Github 버전 제어 시스템에 초점을 맞출 것이다. 가이드에 따라 Github를 시작하십시오.

원하는 이름으로 Github 저장소를 만들 것입니다. 그런 다음 아래 이미지와 같이 리액트 앱을 Github repo에 푸시합니다. 제 샘플 레포는 여기서 찾을 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/test-netlify-deployment-github.png?resize=730%2C517&ssl=1)

이 작업을 마치면 NetApp을 Netliify에 배포하는 다양한 방법에 대해 자세히 알아볼 수 있습니다.

## Netlifify에 React 애플리케이션을 배포하는 3가지 방법

### 방법 1: UI Netliify

이 방법은 명령줄 터미널을 사용하는 대신 Netliify 사용자 인터페이스를 사용하여 수동으로 애플리케이션을 배포하고 구성하는 개발자에게 가장 적합합니다.

#### Netlifify 계정을 Github 계정으로 연결

Netlifify 계정이 아직 없는 경우 여기서 무료 계정을 만들 수 있습니다.

이미 생성된 Github 계정으로 Netliify에 로그인하고 메시지에 따라 Netliify Auth를 인증합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/authorize-netlify-auth.png?resize=730%2C601&ssl=1)

Netlifify Auth를 승인하면 Github 팀 페이지로 리디렉션됩니다. 그런 다음 아래와 같이 Git에서 새 사이트를 만들어야 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/new-site.png?resize=730%2C421&ssl=1)

#### Netlifify를 인증하여 리포지토리 액세스

여전히 `새 사이트 만들기` 페이지에서 Netliify가 Github 저장소에 액세스할 수 있도록 허가하라는 메시지가 표시됩니다. 전체 GitHub 계정에 Netliify 액세스 권한을 부여하거나 페이지 하단의 "Configure Netliify on GitHub" 버튼을 클릭하여 Netliify 액세스 권한을 특정 리포지토리에 부여할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/Create-a-new-site.png?resize=730%2C565&ssl=1)

#### 빌드 옵션

React 애플리케이션이 호스팅되는 리포지토리를 선택하고 위의 단계에서 배포 분기를 선택한 후 사이트를 배포하는 데 필요한 기본 빌드 설정을 지정합니다.

이 페이지는 빌드 단계로 미리 채워져 있지만 응용 프로그램을 빌드하는 명령과 일치하는지 확인하십시오.

이 작업이 완료되면 good old` 버튼 `Deploy Site`를 클릭하고 Voila! 사이트 배포가 진행 중인 것을 볼 수 있습니다.

배포에 성공하면 반환되는 테스트 도메인으로 사이트를 미리 볼 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/trusting-ride.png?resize=730%2C462&ssl=1)

#### 사이트 도메인 구성

Retact 앱 배포를 미리 보고 상태가 좋아지면 이제 애플리케이션에 적합한 도메인을 구성할 수 있습니다. 배포 탭에는 사이트 배포에 대한 세부 정보가 표시됩니다. 여기서 React 응용 프로그램에 사용할 하위 도메인을 등록합니다.

사전 등록된 도메인을 사용하려면 Netliify에서 해당 도메인이 우리의 도메인인지 확인하려고 합니다. 위의 이미지에서 사용자 지정 도메인 설정을 클릭하겠습니다. Netlifify가 구성하기 전에 사용자 지정 도메인을 확인하라는 메시지가 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/add-a-custom-domain.png?resize=730%2C398&ssl=1)

이 작업이 성공적으로 확인되면 구성된 도메인의 애플리케이션에 액세스할 수 있습니다.

### 끌어다 놓기 네트라이프

이 방법은 위에서 설명한 첫 번째 방법과 상당히 유사하다. 다만 드래그 앤 드롭 기능은 온라인 앱 기능을 활용한다는 점에서 독특하다. 이 제품은 특히 초고속 배포에 적합합니다.

#### Netlifify 계정에 로그인

우리는 위의 첫 번째 방법의 첫 번째 단계에 따라 우리의 Github 계정을 Netliify 계정에 연결할 수 있다. 그러나 이 경우 아래와 같이 빈 페이지가 표시됩니다. 이를 온라인 앱 기능이라고 합니다.

우리는 첫 번째 방법처럼 git에서 새로운 사이트를 만들지 않을 것입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/praise-adanlawo.png?resize=730%2C421&ssl=1)

#### 드래그 앤 드롭

그렇게 쉬워요!

최신 `빌드` 폴더가 포함된 Retact 앱이 있으므로 빌드 폴더를 위 빈 공간으로 끌어다 놓기만 하면 됩니다. 이 검은 공간은 달리 온라인 앱으로 알려져 있다. Netlifify는 우리에게 배포 마법을 부립니다.

#### 사이트 도메인 구성

이는 위에서 설명한 첫 번째 방법으로 사이트 도메인을 구성하는 것과 동일한 접근 방식을 따릅니다.

### Netliify CLI

이 방법은 UI 또는 끌어서 놓기 기능을 사용하는 대신 명령줄 터미널에서 배포를 실행하는 데 더 익숙한 개발자를 위한 것입니다.

#### Netlifify CLI 설치

Netlifify CLI를 설치하는 것은 매우 빠르고 쉽습니다. 아래 나온 것처럼 `npm install` 명령을 실행하는 것만큼 쉽습니다.

```coffeescript
npm install netlify-cli -g
```

#### CLI 인증

CLI를 설치한 후 작업 디렉토리로 이동하여 Netliify 계정에 대해 Netliify CLI를 인증하는 다음 명령을 실행합니다.

```bash
cd test-netlify-deployment 
netlify deploy
```

> 참고: 리디렉션에 대한 브라우저 창에서 팝업이 차단되지 않았는지 확인하십시오.

Netliify deploy 명령을 실행하면 Netliify CLI에 대한 권한을 요청하는 브라우저 창으로 리디렉션됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/authorize-application.png?resize=730%2C300&ssl=1)

#### 애플리케이션 배포

이제 CLI에 Netlifify 계정에 액세스할 수 있는 적절한 권한이 있으므로 애플리케이션을 배포할 수 있습니다.

몇 가지 명령줄 프롬프트를 통해 함께 살펴볼 수 있습니다. 장치의 화살표 키를 사용하여 프롬프트를 탐색할 수 있습니다.

CLI 프롬프트 1: 이 폴더는 아직 사이트에 연결되어 있지 않습니다. 당신은 무엇을 하고 싶습니까?

앱 승인 후 첫 번째 프롬프트입니다. 이 응용 프로그램을 기존 사이트에 배포할지 아니면 새 사이트에 배포할지 알고 싶어 합니다. 이 경우 새 사이트에 배포하기 때문에 "만들기"를 선택합니다.

CLI 프롬프트 2: 사이트 이름 선택(선택 사항)

여기서 사이트 세부 정보를 구성하거나 비워 두면 Netliify에서 임의의 이름을 지정할 수 있습니다. 여기서 사용하는 옵션에 관계없이 언제든지 나중에 업데이트할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/site-create.png?resize=730%2C112&ssl=1)

위의 이미지에서 Netliify가 사이트 이름에서 사용자 지정 도메인 URL을 어떻게 아름답게 생성하는지 볼 수 있습니다.

그러나 URL이 있더라도 아직 배포가 완료되지 않았습니다.

CLI 프롬프트 3: 경로 배포

React 앱 작업 디렉토리에서 netliify deploy 명령을 실행 중이기 때문에 배포 경로는 빌드 폴더입니다. 따라서 프롬프트에 대한 응답으로 빌드 디렉터리의 경로만 지정하면 됩니다.

빌드 폴더에만 응용 프로그램을 배포하는 데 필요한 프로덕션 준비 파일이 포함되어 있으므로 이 작업이 필요합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/deploy-path.png?resize=730%2C179&ssl=1)

이제 테스트할 수 있는 URL이 있습니다.

참고: URL이 Page Not found 오류를 발생시키는 웹 페이지로 리디렉션되는 경우 이는 잘못된 빌드 파일과 관련된 문제입니다. netlify deploy 명령을 다시 실행하고 그에 따라 빌드 파일을 업데이트하십시오.

> "초안 URL에서 모든 것이 좋아 보이면 "--prod" 플래그를 사용하여 기본 사이트 URL에 배포하십시오."

#### 운영 환경에 애플리케이션 배포

웹 사이트 초안 URL로 테스트한 후 위의 출력에서 명령을 실행하여 응용 프로그램을 라이브로 실행합니다.

```undefined
netlify deploy --prod
```

다시 한 번 빌드 디렉토리의 경로인 배포 경로를 지정해야 합니다. 배포에 성공하면 업데이트된 웹 사이트 URL이 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/logs.png?resize=730%2C190&ssl=1)

두 가지 중요한 사항은 위에 표시된 것처럼 Unique deploy URL과 Live URL입니다.

고유 배포 URL은 각 배포에 해당하는 URL을 나타냅니다.

라이브 URL은 프로덕션 웹 사이트 URL입니다.

이제 Live URL에서 React 애플리케이션에 액세스할 수 있습니다.

## 결론

Netlifify를 사용하여 React 애플리케이션 및 기타 애플리케이션을 구축하는 것은 대부분 플랫폼의 사용하기 쉬운 기능 때문에 원활한 프로세스입니다.

기본 배포 방법을 선택하고 요구 사항을 충족하면(예: CLI 설치, CLI가 기본 설정 방법인 경우) 일반적으로 애플리케이션을 60초 이내에 Netliify에 배포할 수 있으며, 모든 것이 동일합니다.

리액트 응용 프로그램을 만들고 저와 함께 배치해 주셔서 즐거우셨기를 바랍니다. 댓글란에 꼭 알려주세요.