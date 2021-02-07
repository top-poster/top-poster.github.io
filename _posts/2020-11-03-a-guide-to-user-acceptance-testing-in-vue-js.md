---
layout: post
title: "Vue.js의 사용자 수락 테스트 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/Vute-testing.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/Vute-testing.png?fit=730%2C487&ssl=1)

## 승인 테스트란?

수용 테스트(UAT)는 소프트웨어나 애플리케이션이 소유주의 요구 사양을 충족하는지 여부를 판단하는 데 사용되는 테스트 프로세스 또는 기법이다. 이 프로세스의 목적은 비즈니스 사양에 대한 애플리케이션 규정 준수를 평가하고 사용자가 보는 방식으로 애플리케이션을 확인하는 것입니다.

### 시스템 테스트 유형과 사용자 승인 테스트의 차이점

단위시험

단위 시험에는 코드를 쉽게 시험할 수 있는 작은 부분으로 분해하는 것이 포함된다. 이러한 작은 부품을 단위라고 합니다. 단위는 단순한 함수에서 복잡한 알고리즘(Vue의 구성 요소)에 이르기까지 다양하다. 코드에서 간단한 `곱하기 함수`를 테스트하는 것이 한 예입니다. 테스트 결과가 예상 출력을 반환하는 것이 중요하다. 우리의 경우 입력한 숫자의 곱셈이다. 이것은 보통 응용 프로그램에서 수행된 첫 번째 테스트입니다.

적분검사

통합 테스트의 목적은 다른 구성 요소(하위 구성 요소)를 통합하는 구성 요소(상위 구성 요소)가 올바르게 작동하는지 확인하는 것입니다. 통합 테스트는 구성 요소 간의 데이터 흐름을 검사하여 애플리케이션의 모든 구성 요소가 필요에 따라 협업하는지 확인하는 데 사용됩니다. 이 테스트는 일반적으로 유닛 테스트가 완료된 후 수행됩니다.

단대단(E2E) 테스트

E2E 테스트는 응용 프로그램을 단위로 분류할 필요가 없는 응용 프로그램 테스트의 한 유형입니다. 대신 응용 프로그램을 전체적으로 테스트합니다. 이는 시스템의 다양한 구성 요소 간에 데이터 무결성을 유지하고 애플리케이션의 흐름이 처음부터 끝까지 필요에 따라 작동하도록 하기 위한 것입니다. 이 테스트는 주로 통합 테스트 후에 수행됩니다.

합격시험

수락 테스트는 앱이 클라이언트의 요구 사항을 충족하는지 확인합니다. 이 테스트에서는 E2E(End-to-End) 테스트와 함께 전체 애플리케이션을 테스트합니다. 그러나 E2E 시험 방법과 달리, 이 시험은 고객의 요구 사항에 관한 시스템 인증만을 위한 것이다. 그것은 외관상의 오류, 철자상의 실수 또는 시스템 테스트에 초점을 맞추지 않는다. 이 테스트는 일반적으로 응용 프로그램에서 수행된 마지막 테스트입니다.

### 사용자 승인 테스트 유형

작동 테스트

운용시험은 개발자가 응용 프로그램의 기능을 확인하기 위해 실시하는 합격시험의 일종이다. 이러한 유형의 테스트를 생산 허용 테스트라고도 합니다. 이 프로그램은 응용 프로그램이 사용자에게 발송될 준비가 되었는지 확인하는 데 사용됩니다.

컴플라이언스 테스트

컴플라이언스 테스트는 대부분 규제 법률에 부합하는지 확인하기 위해 애플리케이션에서 실행됩니다. 이러한 유형의 시험의 목적은 응용 프로그램이 관리법을 준수하는지 확인하는 것이다.

계약시험

계약 테스트는 제품 소유자와 개발 팀 간에 이루어집니다. 이러한 유형의 시험은 계약에 열거된 모든 규격이 애플리케이션에 통합되도록 하기 위해 조정된다. 이 시험은 계약에 열거된 기능을 확인하고 응용 프로그램이 규격을 어떻게 달성할 수 있는지 관찰한다.

알파

알파 테스트는 개발 환경의 애플리케이션에 대해 수행되는 테스트로, 주로 직원들 사이에서 수행됩니다. 애플리케이션의 전반적인 기능은 알파 테스트에서 테스트됩니다. 테스터는 응용프로그램을 배치하기 전에 피드백을 보냅니다.

베타 테스트는 고객이 프로덕션 환경에서 애플리케이션을 테스트하는 것입니다. 이 테스트의 목적은 많은 고객이 실제 환경에서 성능과 확장성을 테스트하면서 동시에 제품을 사용할 수 있도록 하는 것입니다.

### 합격 시험의 중요성

지원서에서 합격 시험의 중요성은 아무리 강조해도 지나치지 않습니다. 애플리케이션에서의 합격 테스트의 핵심 중요성에 대해 살펴보겠습니다.

- 개발자, 고객 및 사용자 간의 협업 향상
- 최종 제품이 비즈니스 소유자의 기대에 부합하도록 보장합니다.
- 최종 제품에 버그가 없으며 올바른 사용자 환경을 제공합니다.

## Logrocket을 사용한 Vue App 수락 테스트

이 섹션에서는 확장성과 성능을 테스트하기 위해 실제 시나리오를 사용하여 Logrocket의 Vue.js 애플리케이션에 대한 베타 테스트를 수행하는 방법을 살펴본다.

전제조건

Vue.js 응용 프로그램에서 사용자 수락 테스트를 수행하려면 응용 프로그램이 온라인으로 호스팅된 것으로 가정합니다. 이 튜토리얼에서는 Netliify에서 Vue 애플리케이션 수락 테스트에 대해 설명합니다. Netlifify에서 응용 프로그램을 호스팅하는 데 도움이 필요하면 Netlifify에 대해 작성한 이 기사를 참조하십시오.

또한 애플리케이션을 배포할 준비가 되었거나 이미 배포되었다고 가정하겠습니다.

Logrocket을 사용하여 Vue 응용 프로그램에서 사용자 수락 테스트를 수행하려면 먼저 LogRocket에 등록해야 합니다. 이 링크를 따라 Logrocket 애플리케이션을 생성하십시오. 무료 시작 클릭:

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/logrocket-front-page.png?resize=730%2C369&ssl=1)

등록 페이지로 이동합니다. 저는 GitHub 페이지가 있어서 GitHub에 가입하는 것을 선호합니다. 원하는 방법을 선택하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/logrocket-sign-up.png?resize=730%2C989&ssl=1)

다음은 GitHub의 승인을 요청하는 메시지가 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/authorize-logrocket.png?resize=730%2C694&ssl=1)

그러면 아래 페이지가 표시됩니다. 계속을 클릭합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/welcome-to-logrocket.png?resize=730%2C258&ssl=1)

이 페이지의 양식을 적절하게 입력하십시오. 활성 전자 메일 주소를 사용합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/create-a-new-project.png?resize=730%2C475&ssl=1)

install Logrocket 섹션에서 NPM 대신 스크립트 태그를 사용하는 것을 선호합니다. 아래 이미지와 같이 `스크립트 태그 섹션`의 코드를 복사하겠습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/install-logrocket.png?resize=730%2C198&ssl=1)

LogRocket 대시보드 설정의 통합으로 이동합니다. 다음 명령을 사용하여 npm을 통해 `logrocket-vuex` 플러그인을 설치합니다.

```coffeescript
npm i --save logrocket-vuex
```

저장소에 LogRocket 플러그인을 추가하려면 대시보드에 표시된 대로 코드를 복사하여 Vuex 저장소에 추가하십시오.

LogRocket 계정을 생성했습니다. 다음 단계는 Ntlifify에 우리의 조각들을 추가하는 것이다. 이를 통해 다른 UAT 도구와 함께 당사 사이트와 상호 작용하는 사용자의 비디오를 볼 수 있습니다. 호스트 응용 프로그램에서 사이트 설정을 클릭합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/our-store.png?resize=730%2C270&ssl=1)

빌드 클릭

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/site-details.png?resize=730%2C442&ssl=1)

그런 다음 Post Processing(후 처리)을 클릭합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/continuous-deployment.png?resize=730%2C410&ssl=1)

그런 다음 `스냅펫 주입`과 `조각 추가`를 클릭합니다. 앞에 삽입을 머리말로 변경합니다. `스크립트 이름` 섹션에 이름을 추가합니다. LogRocket의 조각으로 HTML 섹션을 채웁니다. 마지막으로 저장 버튼을 클릭합니다.

Vue.js 응용 프로그램 및 Vuex 저장소를 보고 상호 작용합니다. 대시보드에서 새 세션 비디오를 확인할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/peppubuild.png?resize=730%2C261&ssl=1)

## 보너스

우리는 Postman과 함께 애플리케이션 API에 대한 알파 테스트를 수행하는 방법을 알아보려고 합니다. 요청을 백엔드로 보내기 전에 항상 API 끝점을 테스트하는 것이 좋습니다. API 끝점을 테스트할 때 오류가 있는지 미리 알 수 있습니다. Postman은 주로 API 호출을 테스트, 공유 및 문서화하는 데 사용되는 도구입니다.

프런트 엔드 애플리케이션과 백엔드 애플리케이션을 연결하기 전에 Postman을 통해 API 호출을 테스트하는 방법에 대해 알아봅니다.

먼저 작업 환경에 Postman을 설치해야 합니다. 우체부가 설치되어 있지 않으면 여기서 받을 수 있습니다.

단계에 따라 시스템에 우체부가 설치되도록 하십시오. 우리는 온라인 가짜 휴식 API를 사용하여 포스트맨 테스트를 진행할 것입니다.

Postman 환경을 시작한 다음 `요청 만들기`를 클릭합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/hey-there-night-owl.png?resize=730%2C357&ssl=1)

작업영역에서 요청 URL로 `https://jsonplaceholder.typicode.com/todos/1`을 입력하고 요청을 `GET`로 설정합니다.

테스트 클릭:

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/untitled-request.png?resize=730%2C373&ssl=1)

다음 코드를 테스트 작업 공간에 추가합니다.

```js
//Let postman test status code be 200
pm.test("status.code is 200", function () {

//If status does not return 200, test fails
       pm.response.to.have.status(200);
})
```

위의 주소로 이 코드를 실행하면 다음과 같은 응답이 나타납니다. GET 요청이 성공적이었기 때문입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/pm-test.png?resize=730%2C419&ssl=1)

주소를 잘못된 주소로 변경하면 해당 코드로 테스트를 통과하지 못할 수 있습니다. 아래 예제는 주소에서 O를 제거했으므로 코드 404를 반환합니다.

404는 200이 아니므로 테스트 상태가 실패합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/pm-response.png?resize=730%2C450&ssl=1)

## 결론

이 기사에서는 Vue.js 응용 프로그램과 Vuex 저장소의 사용자 수락 테스트에 대해 다루었다. LogRocket을 사용하여 비즈니스 소유자 요구사항 및 기능을 테스트했습니다. 우리는 또한 포스트맨과 함께 우리의 애플리케이션 API에서 알파 테스트를 수행하는 방법을 보았다.

당사는 승인 테스트 전에 귀하의 신청서에 대해 수행할 수 있는 다른 테스트와 함께 승인 테스트가 무엇인지 살펴봄으로써 이 기사를 시작했습니다. 또한 최종 사용자에게 애플리케이션을 발송하기 전에 승인 테스트의 중요성에 대해서도 이야기했습니다.

당신 시간에 감사드립니다. 트위터로 편하게 연락 주세요.