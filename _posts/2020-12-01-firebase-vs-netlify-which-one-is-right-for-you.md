---
layout: post
title: "사격장 대 사격장 Netliify: 어떤 것이 당신에게 적합합니까?"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/firebasevsnetlify.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/firebasevsnetlify.png?fit=730%2C487&ssl=1)

기업들은 매일 점점 더 많은 클라우드 호스팅 서비스를 채택하고 있습니다. 특히, 더 나은 신뢰성과 함께 비용 효율적인 솔루션을 제공하기 때문입니다. Firebase와 Netliify는 이 범주에서 가장 쉽고 강력한 도구 중 하나입니다.

두 제품 모두 애플리케이션을 쉽게 배포할 수 있는 기능이 포함되어 있습니다. 따라서, 본 가이드에서는 고객의 사용 사례, 장점, 단점, 그리고 언제 다른 것을 선호해야 하는지 알아보겠습니다. 우리는 또한 파이어베이스와 Netliify에 프로젝트를 설치 및 배치할 것이다. 각 서비스의 사용 방법에 대한 기술적 세부 사항을 이해하는 데 도움이 될 것입니다.

## 파이어베이스

Firebase는 강력한 서비스형 백엔드(BaaS)를 제공합니다. 기능이 풍부한 앱을 즉시 개발할 수 있도록 도와줍니다. 거대 기술업체인 구글의 지원을 받아 안전하게 인프라에 의존할 수 있다.

### 특징들

- 실시간 데이터베이스
- 데이터 동기화
- 기계 학습 도구
- 한 번의 클릭으로 Google Analytics 통합
- 여러 인증 방법(예: 이메일/암호, 소셜 미디어 앱, 전화, 익명 등)

### 프로스

- 방화벽에는 신속한 애플리케이션 개발을 위한 모든 기능이 포함되어 있습니다. 최소 실행 가능한 제품(MVP)을 생성하는 것이 매우 적합합니다.
- A/B 테스트 수행 능력
- 간편한 앱 내 및 클라우드 메시징 추가
- 인공지능을 사용하여 사용자 행동 예측
- 중요한 코드를 작성하지 않고 파일 업로드 및 검색 처리
- 실시간 데이터베이스보다 훨씬 빠른 클라우드 FireStore 소개
- Google 클라우드 플랫폼을 통해 모든 앱을 쉽게 확장

### 콘스

- 애플리케이션 확장 및 데이터베이스 마이그레이션 처리 및 캐싱 관리와 관련된 가파른 학습 곡선

## 넷라이프

Netlifify는 개발자 커뮤니티에서 JamStack의 엄청난 성공으로 인기를 끌었다. 휴고, 지킬 등과 같은 정적 사이트 생성기를 사용하여 생성되는 정적 웹 사이트를 호스팅하는 데 주로 사용됩니다.

### 특징들

- 정적 웹 사이트 호스팅
- 지속적인 통합 및 지속적인 구축
- 소스 제어 시스템과의 통합
- 이미지, 비디오 및 문서 즉시 최적화
- 끌어서 놓기 기능을 사용하여 배포
- 사용자 인증에 대한 기본 지원

### 프로스

- 분할 테스트를 통해 새로운 기능 또는 다양한 설계 테스트
- 플러그인을 사용하여 빌드 워크플로우 사용자 지정
- Netlifine 대시보드 내에서 바로 양식 제출 수집

### 콘스

- 콘텐츠를 동적으로 표시할 수 없음
- Firebase와 달리 정적 웹 사이트만 지원합니다.

## Firebase를 사용하여 작업관리 목록 웹 사이트 만들기

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/todolist.png?resize=1366%2C625&ssl=1)

## Firebase 프로젝트 설정

우선 Gmail 계정에 로그인한 다음 Firebase 콘솔로 이동합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/welcometofirebase.png?resize=1366%2C625&ssl=1)

계속하려면 "프로젝트 만들기" 버튼을 클릭하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/createanewprojectinfirebase.png?resize=1366%2C625&ssl=1)

프로젝트 이름을 입력하라는 메시지가 표시됩니다. 원하는 대로 입력하지만 이 튜토리얼을 위해 "작업관리 목록"을 입력합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/turnoffgoogleanalytics.png?resize=1366%2C625&ssl=1)

Firebase는 Google Analytics와 쉽게 통합할 수 있습니다. 생산 환경에서는 활성화하는 것이 좋습니다. 하지만 지금으로서는 그럴 필요가 없습니다. 따라서, 아래 스크린샷에 언급된 것처럼 간단히 끄십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/createproject.png?resize=1366%2C625&ssl=1)

이제 "프로젝트 만들기" 버튼을 누르면 Firebase에서 새로운 프로젝트를 설정할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/newprojectisready.png?resize=1366%2C625&ssl=1)

프로젝트 개요를 보려면 "계속"을 클릭하십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/todolistsparkplan.png?resize=1366%2C625&ssl=1)

웹 사이트를 만들 예정이므로 코드 아이콘을 클릭하십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/registertheapp.png?resize=1366%2C625&ssl=1)

여기에서는 앱을 등록해야 합니다. 이렇게 하려면 사용자에게 친숙한 이름을 입력하고 "앱 등록"을 누릅니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/addfirebasesdk.png?resize=730%2C333&ssl=1)

이제 Firebase에서 자동으로 생성되는 코드 스니펫이 표시됩니다. 나중에 필요하므로 컴퓨터에 복사하십시오. 각 앱마다 고유한 몇 가지 중요한 세부 정보를 숨겼습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/openrealtimedatabase.png?resize=730%2C334&ssl=1)

앱을 등록한 후, 왼쪽 메뉴에서 "실시간 데이터베이스" 페이지를 열고 "데이터베이스 작성" 단추를 누릅니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/securityrulesforrealtimedatabase.png?resize=730%2C334&ssl=1)

팝업 창이 열리고 "테스트 모드에서 시작"을 선택한 후 "활성화"를 누르기만 하면 됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/realtimedatabasedata.png?resize=730%2C333&ssl=1)

이 때 소스 코드를 사용하여 이 NoSQL 데이터베이스에 쉽게 액세스할 수 있습니다. 그럼 이제 "작업 목록" 앱의 소스 코드를 작성하겠습니다.

여기서 코드를 찾을 수 있습니다.

### 프로젝트 실행

웹 브라우저 내에서 index.html 파일을 열기만 하면 됩니다. 이제 작업관리 목록 항목을 추가/제거할 수 있습니다.

## Netliify에 정적 웹 사이트 배포

### 코드 작성

이 섹션에서는 간단한 HTML 랜딩 페이지를 만든 다음 Netliify에 배포할 것입니다. 우리는 UI와 UX를 향상시키기 위해 부트스트랩, jQuery, fontwesome, 그리고 구글 폰트를 사용할 것이다.

여기서 코드를 찾을 수 있습니다.

### Netliify에서 프로젝트 업로드

이 때 정적 웹 페이지의 소스 코드를 배포할 준비가 되었습니다. 계속하려면 Netlifify에 계정을 만들고 전자 메일을 확인하십시오. 그런 다음 프로젝트를 업로드할 수 있는 대시보드에 도착합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/uploadprojectonnetlify.png?resize=1362%2C544&ssl=1)

넬리파이가 드래그 앤 드롭 기능을 추가해 이 과정을 한층 더 간소화했다는 점이 흥미롭다. 프로젝트의 루트 폴더를 끌어와 언급된 섹션 안에 놓기만 하면 됩니다. 업로드/빌드 프로세스가 자동으로 시작되고, 마지막으로 사용자의 웹 사이트가 즉시 활성화됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/automaticsubdomain.png?resize=1362%2C540&ssl=1)

기본적으로 Netliify는 자동으로 하위 도메인을 할당합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/sitepublished.png?resize=1366%2C625&ssl=1)

프로덕션 배포 섹션에서 볼 수 있듯이, 당사의 웹 사이트는 성공적으로 게시되었으며 온라인으로 볼 수 있습니다.

### 프로젝트 실행

방금 저장한 하위 도메인을 열기만 하면 됩니다. 이 튜토리얼에서는 내 이름을 사용하여 하위 도메인을 설정했습니다. 그래서 우리는 이 URL을 방문할 수 있습니다.

## 파이어베이스가 Netliify보다 나은가요?

동적 웹 사이트 또는 앱을 만들려면 Firebase를 사용하는 것이 좋습니다. 반면에 Netliify는 정적 웹 사이트를 호스팅하는 데 더 적합하다. 많은 스타트업이 빠른 애플리케이션 개발을 위해 파이어베이스를 사용한다. 그것은 그들이 처음부터 모든 것을 쓰지 않고 재빨리 그들의 아이디어를 시험할 수 있도록 도와준다. 마찬가지로 Netliify는 원활한 빌드 워크플로우를 제공하는 새로운 핫 및 트렌드 서비스입니다. GitHub와 같은 소스 제어 시스템과 쉽게 연결하여 각 커밋에 정적 웹 사이트 배포를 자동화할 수 있습니다.

## Netlifify 가격 책정

Netliify는 사업 규모에 따라 별도의 패키지를 가지고 있습니다. 예를 들어, 이러한 패키지를 제공합니다.

- 시동기
가격: 무료
- 가격: 무료
- 프로
가격: 회원/월당 $19
- 가격: 회원/월당 $19
- 비즈니스
가격: 회원/월 99달러
- 가격: 회원/월 99달러
- 엔터프라이즈
가격: 보통 매달 3,000달러부터 시작합니다. 그러나 웹 응용 프로그램에 따라 사용자 지정 계획을 보려면 해당 사용자에게 연락해야 합니다.
- 가격: 보통 매달 3,000달러부터 시작합니다. 그러나 웹 응용 프로그램에 따라 사용자 지정 계획을 보려면 해당 사용자에게 연락해야 합니다.

STARTER 패키지는 모든 사람에게 완전히 무료입니다. 대부분 개인/오픈 소스 프로젝트에 적합합니다. 주요 기능은 다음과 같습니다.

- GitHub와 연결하여 빌드 프로세스 자동화
- 쉽게 버전 변경

PRO 패키지는 중소기업 웹 사이트 또는 블로그용으로 특별히 설계되었습니다. START 패키지보다 약간 더 많은 트래픽을 처리할 뿐만 아니라 성능을 약간 향상시킬 수 있습니다. 필요한 경우 이 패키지를 선택할 수 있습니다.

- 로그인/가입 기능
- 알림 지원

마찬가지로, 비즈니스 및 엔터프라이즈 계획은 잘 구축된 웹 사이트에 권장됩니다. 그들은 훨씬 더 많은 특징과 통제력을 제공한다. 예를 들어 SAML을 사용하여 Single Sign-On 기능을 쉽게 추가하고 RBAC(역할 기반 액세스 제어)를 추가하며 자체 호스팅된 Git 저장소를 사용할 수도 있습니다.

## 소방서 가격

반면, Firebase는 Netliify에 비해 매우 단순한 가격 모델을 가지고 있습니다. 두 가지 계획만 제시합니다.

- 스파크 플랜
가격: 무료
- 가격: 무료
- 블레이즈 플랜
가격: 사용하는 것에 대해서만 지불
- 가격: 사용하는 것에 대해서만 지불

평소와 마찬가지로, Firebase의 기능을 무료로 사용해 볼 수 있는 스파크 플랜이 있습니다. 이 백엔드 서비스화(BaaS) 플랫폼에 익숙해질 수 있도록 도와줍니다. 나중에 Blaze Plan으로 업그레이드하여 고급 기능에 액세스할 수 있습니다.

## 비교

위의 학습 내용을 바탕으로 Firebase와 Nelify를 비교하여 특정 요구사항에 더 적합한 Firebase를 알아보겠습니다.

Firebase 사용:

- 동적 웹 사이트 - 예를 들어 웹 페이지를 생성하기 위해 데이터베이스에서 데이터에 액세스해야 하는 경우. 로그인/가입 시스템, 질문/응답 포럼, 소셜 미디어 앱, 게임 등이 될 수 있습니다.
- Android/iOS 앱 개발
- 인공지능 및 기계학습 알고리즘 처리

Netliify 사용:

- 정적 웹 사이트 또는 블로그. 그것들은 휴고, 지킬, 개츠비 등과 같은 도구를 사용하여 생성될 수 있다.
- 고성능. 정적 웹 사이트에서는 런타임에 아무것도 생성할 필요가 없고 표시만 하면 되기 때문입니다.
- 정적 웹 사이트를 쉽게 배포하거나 확장합니다. 전통적으로 대규모 정적 웹 사이트를 관리하는 것은 매우 어렵습니다. 그러나 Netliify는 모든 프로세스를 간단하고 쉽게 만듭니다.
- 버전 제어(특정 버전으로 업그레이드/다운그레이드). 여기서는 GitHub, GitLab, Bitbucket 등의 서비스에 연결할 수 있습니다.

여기까지입니다. 이제 Firebase와 Netlifify에 대해 잘 알고 계시기를 바랍니다. 끝까지 저를 따라오셨다면, 언제 그것들을 사용해야 하는지, 그리고 각각의 서비스로 웹사이트를 설정하는 방법을 아실 것입니다. 이제 두 플랫폼을 모두 사용해보고 다양한 기능을 살펴볼 차례입니다.