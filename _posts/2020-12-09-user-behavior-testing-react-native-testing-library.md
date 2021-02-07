---
layout: post
title: "반응 네이티브 테스트 라이브러리를 사용한 사용자 동작 테스트"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/behavior-testing-react-native-testing-library.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/behavior-testing-react-native-testing-library.png?fit=730%2C487&ssl=1)

프런트 엔드 개발에서 사용자 행동을 테스트하는 것은 훌륭한 제품을 제공하는 데 매우 중요합니다. RNTL(React Native Testing Library)을 사용하면 React Native 앱에서 사용자 이동 경로를 테스트할 수 있습니다. 구현 세부사항에 대한 테스트의 영향을 받지 않도록 하는 훌륭한 API를 갖추고 있습니다.

이 튜토리얼이 끝날 때까지, 여러분들도 이 기쁨을 느낄 것이라고 약속합니다. 그리고 저는 여러분이 그곳에 가는 데 너무 많은 시간을 들이지 않을 것입니다. 이 데모의 전체 코드는 GitHub에서 찾을 수 있습니다.

## 반응 네이티브 테스트 라이브러리 시작

우선, 우리의 시험을 즐거운 경험으로 만들기 위해 사용할 라이브러리를 추가하는 것부터 시작해보자.

```css
yarn add -D @testing-library/react-native
```

여기서 주목해야 할 한가지는 RNTL은 Jest에 의존하는 반응 테스트 라이브러리 위에 포장된 것이다. 이 조합은, 제 겸손한 생각이지만, 천생연분이며, 시험 경험을 놀랍게 만들어줍니다.

### '버튼' 구성 요소 테스트

먼저 테스트 구성 요소에 대해 잘 알고 있는 경우 이 섹션을 건너뛸 수 있습니다. 우리는 RNTL의 작동 방식과 우리가 하고 있는 일에 대해 조금 더 친숙하게 만들기 위해 간단한 버튼 구성 요소를 테스트할 것입니다. `Button` 구성 요소는 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/button-component-code.png?resize=730%2C396&ssl=1)

다음은 `Button` 구성 요소의 전체 코드입니다. 사용자가 누를 때 온프레스(on Press) 방식을 불러야 한다는 것을 시험해 본다. 테스트 결과는 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/button-test-suite.png?resize=730%2C524&ssl=1)

RNTL은 속성을 사용하여 구성 요소를 잡을 수 있는 훌륭한 API를 제공합니다. 구성 요소를 잡는 방법은 여러 가지가 있지만 `Get By`라는 느낌이 든다.TestId`를 사용하면 테스트 코드가 UI 및 텍스트 변경에 대해 보다 쉽게 액세스할 수 있습니다.

`fireEvent` API를 사용하면 사용자 행동을 테스트할 수 있는 많은 상호 작용을 조롱할 수 있다. 여기서는 모의 방법을 만들어 사용자가 버튼을 누를 때 한 번만 호출해야 한다고 주장했습니다. 우리는 우리의 흐름을 더 테스트하기 위해 비슷한 구조를 사용할 것입니다.

다음은 예상되는 출력과 함께 이 테스트를 실행하는 방법입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/button-component-test-output.png?resize=730%2C302&ssl=1)

좋아요, 이제 몇 가지 기본적인 테스트를 했고 상황이 어떻게 변할지 잘 알고 있어요.

### 로그인 양식 테스트

한 걸음 더 나아가서 몇 가지 실제 사례를 실천해 봅시다. 사용자로부터 e-메일 및 비밀번호 입력을 받아 on Submit 방식으로 전달하는 양식을 테스트하겠습니다. 또한 사용자가 비밀번호를 입력하지 않은 경우 유효성 검사 오류를 발생시킨다고 주장하겠습니다. 구성 요소는 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/form-component-code.png?resize=730%2C537&ssl=1)

첫 번째 사용 사례를 테스트해 보겠습니다. 구성 요소에 "암호 필요" 오류가 표시되어야 합니다. 테스트 결과는 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/form-component-test.png?resize=730%2C551&ssl=1)

여기서는 상태에 영향을 미치는 조치를 테스트합니다. `setState`는 React에서 비동기적이므로 우리는 비동기식으로 이 테스트를 실행해야 하며, RNTL은 이를 위한 `waitFor` API를 제공한다. 또한 입력 구성 요소의 텍스트를 변경하는 데 동일한 `fire event` API를 사용했다.

다음 사용 사례로 넘어가면 e-메일 및 비밀번호로 `제출`을 호출할 것으로 예상됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/form-component-test-mock-argument.png?resize=730%2C687&ssl=1)

우리는 예상 산출량을 확실히 얻기 위해 모의 함수의 주장을 주장함으로써 한 걸음 더 나아가고 있다. EmailPasswordForm 구성 요소를 위한 전체 테스트 세트입니다.

## 테스트 흐름 구축

이제 자신감을 얻었으니 로그인 흐름을 모두 테스트해 보겠습니다. 이것은 단순히 테스트 방법만을 부르는 것이 아닙니다. 로그인에는 일반적으로 토큰을 반환하는 일부 네트워크 API 호출이 있으며 나중에 사용할 수 있도록 해당 토큰을 로컬 스토리지에 저장합니다.

### 모킹 API

우리의 테스트를 서버/네트워크와 독립적으로 만들기 위해, 우리는 우리의 API 히트를 조롱할 것입니다. 이것은 훌륭한 오픈 소스 패키지인 "petch-mock-jest"로 가능합니다. 모든 `페치` 호출을 가로채서 지정된 응답으로 해결합니다. 이를 통해 모든 성공 사례와 오류 사례를 테스트할 수 있습니다. 코드베이스에 추가합시다.

```undefined
yarn add -D fetch-mock-jest
```

다음은 우리가 사용할 가장 간단한 모킹입니다. 이것은 예상되는 응답으로 로그인 API 호출을 조롱합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/mocking-login-api-call.png?resize=730%2C194&ssl=1)

### Async 스토리지 조롱

대응 네이티브 앱은 수많은 네이티브 브리지를 사용하여 플랫폼별 기능에 액세스합니다. 우리의 경우 로그인 API 응답이 로컬 스토리지에 저장되었는지 확인해야 합니다.

RNTL은 우리가 장치나 에뮬레이터에 의존하지 않고 테스트를 실행할 수 있도록 이러한 네이티브 브리지를 조롱할 수 있게 해준다. 이러한 라이브러리를 조롱하기 위해 앱에서 사용하는 모든 네이티브 라이브러리를 조롱하기 위해 코드화할 파일 `jest.mock.js`를 추가할 것이다. 다음은 모의 파일입니다.

```coffeescript
jest.mock('@react-native-community/async-storage', () => ({
  setItem: jest.fn(),
}));
```

여기서 주목해야 할 한 가지 중요한 점은, 우리는 라이브러리에서 사용하는 모든 방법을 조롱해야 한다는 것이다. 이러한 방법은 우리의 테스트 환경에 추가되지 않기 때문이다.

이제 `jest.configs.js` 파일에서 이 파일을 참조합니다.

```java
module.exports = {
  ...
  setupFiles: ['./jest.mock.js']
  ...
};
```

이 파일을 `setupFiles` 어레이에 추가하면 Jest에게 테스트 제품군을 실행하기 전에 이 파일을 실행하도록 지시합니다.

## 전체 테스트 제품군

이제 조롱거리가 준비되었으니, 우리의 전체 흐름을 어떻게 테스트할 것인지 살펴보도록 하겠습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/full-login-screen-test-flow.png?resize=730%2C895&ssl=1)

걱정하지 마세요. 소화가 어렵지 않고 길기만 해요. 우리는 이 일의 대부분을 이미 알고 있다.

우선, 우리는 이 테스트 스위트가 시작되기 전에 우리의 "fetch mock"을 확실히 하기 위해 "before All" 후크에 "fetch mock"을 추가했다. 여기서는 화면 구성 요소를 조롱된 탐색 방법으로 렌더링했습니다. 이렇게 하면 앱에서 화면이 렌더링됩니다.

모든 사용자 상호 작용을 실행한 후, 우리는 우리가 예상한 대로 일이 발생한다고 주장하기 시작한다. 다음은 NAT의 기대 사항입니다.

- API를 예상값으로 호출합니다.
- API 응답이 수신되면 앱이 홈 화면으로 이동합니다.
- `jwtToken`이 로컬 저장소에 저장됩니다.

이러한 테스트가 모두 통과되면 로그인 화면이 예상대로 작동합니다. 로그인 흐름 테스트가 완료된 것으로 표시됩니다.

## 사전 테스트 작업: 보풀링

테스트 제품군의 수와 크기가 증가함에 따라 테스트를 실행하는 데 시간이 좀 더 걸릴 수 있습니다. 테스트를 실행하기 전에 코드를 보풀로 처리하면 명백한 버그가 있는 코드에서 테스트를 실행하지 않습니다. 우리는 사전 테스트 스크립트에 보풀 작업을 추가할 것입니다. 다음과 같은 모습입니다.

```undefined

```

전체 ` 패키지를 볼 수 있습니다.json의 파일입니다.

## 테스트 후 작업: 리포터 추가

차트와 대시보드가 성능을 시각화하는 것을 싫어하는 사람은 누구입니까? 여기서, 우리는 Jest 테스트 리포터를 통합할 것이며, 이 리포터는 우리의 테스트 실행의 완전한 모습을 우리에게 전달할 것이다. 이를 통해 어떤 테스트가 중단되고 있는지, 각 테스트를 완료하는 데 걸리는 시간을 모니터링할 수 있습니다.

저희 프로젝트에 리포터 추가:

```undefined
yarn add -D jest-html-reporters
```

다음은 이 리포터를 `jest.configs.js` 파일에 추가해 보겠습니다. 이 방법은 다음과 같습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/adding-reporter-configs.png?resize=730%2C410&ssl=1)

테스트를 실행할 때마다 이 리포터를 자동으로 열어 보겠습니다. 간단합니다. 테스트 후 스크립트에 패키지의 업데이트로 추가합니다.json:

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/post-test-script-reporter.png?resize=730%2C253&ssl=1)

다음은 저희 프로젝트에 대한 테스트 보고서입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/project-test-report.png?resize=730%2C333&ssl=1)

여기 보고서의 .html 파일이 있습니다.

## 사전 커밋 후크 추가

대규모 애플리케이션(및 7일마다 작업하는 사이드 프로젝트)의 경우 코드 품질을 유지하기 위해 확실한 검사를 하는 것이 좋습니다. 이러한 점검은 당사 저장소에 도착한 모든 PR이 표준에 부합하고 정의된 기준을 통과하도록 하기 위해 시행되어야 합니다.

사전 커밋 후크는 여기에서 유용합니다. 이 훅은 매 번 저지르기 전에 작동한다. 테스트 작업을 추가하여 어떤 커밋도 테스트를 위반하지 못하도록 할 수 있습니다. 우리는 여기서 허스키(husky)를 사용하고 있는데, 이것은 커밋 후크를 추가하는 것이 좋습니다. 마지막으로 추가할 라이브러리:

```undefined
yarn add -D husky
```

여기에 첫 번째 사전 커밋 후크를 설정하겠습니다. npx는 다음을 지원합니다.

```undefined
npx husky add pre-commit "yarn test" # will create .husky/pre-commit file
```

이 명령을 실행하면 `.husky/pre-commit` 파일이 생성됩니다. 커밋 전에 실행할 배시 스크립트입니다.

때때로 목록 태스크에는 명령을 실행할 때 파일을 변경하는 `--fix` 옵션이 있습니다. 우리의 경우, 위 섹션에 Jest 리포터를 추가했는데, 테스트를 진행하면서 새로운 보고 파일이 작성될 것이다. 사전 커밋 후크가 실행된 후 이러한 변경 사항을 커밋에 모두 추가하기 위해 사전 커밋 파일을 업데이트합니다. 다음과 같은 모습입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/pre-commit-file-update.png?resize=730%2C458&ssl=1)

## 오늘은 여기까지!

예, 완벽한 테스트 에코시스템을 구축했을 때 어떤 기분이 드는지 알고 있습니다. 코드가 너무 빨리 깨지지 않을 것이라는 확신과 여유로움이 더욱 강해집니다.

React Native Testing Library를 사용하면 모범 사례를 쉽게 따르고 테스트 환경을 즐겁게 유지할 수 있습니다. 사용자가 프로그램과 상호 작용하는 것과 동일한 방식으로 프로그램을 테스트할 수 있습니다. 모든 사용 사례를 다루면 많은 자신감을 얻을 수 있습니다. 앱의 등급이 올라갈 것이라고 믿는 것이 좋습니다.

여기서 앞으로 Detox를 통해 엔드 투 엔드 테스트를 수행할 수 있습니다. 이는 알림, 딥 링크, 정교한 사용자 상호 작용 등 기본 API가 필요한 사례를 테스트하는 데 도움이 됩니다.