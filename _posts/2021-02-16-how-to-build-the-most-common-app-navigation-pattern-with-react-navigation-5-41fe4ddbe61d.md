---
layout: post
title: "반응 탐색 5를 사용하여 가장 일반적인 앱 탐색 패턴 구축"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Workflow on a wall](https://miro.medium.com/max/10368/0*oDgXSIVxtJgJuhs-)

React Native는 모바일 애플리케이션을 만드는 데 강력하고 널리 사용되는 도구가 되었습니다. 작성 당시에는 교차 플랫폼 애플리케이션을 구축하는 데 가장 많이 사용되는 라이브러리이다.

또한 저장소에 있는 애플리케이션(iOS 이상)을 살펴보면 수천 개의 애플리케이션에 걸쳐 반복되는 탐색 패턴을 볼 수 있습니다. 로그인 화면으로 시작한 다음 탭 또는 드로어 탐색에서 홈 화면으로 이동하는 앱을 말하는 것입니다. 탭 또는 서랍 탐색 내부의 각 화면에서 사용자는 일반적으로 스택 또는 모달 탐색을 사용하여 다른 화면으로 이동할 수 있습니다.

이 기사에서는 Retact Navigation 버전 5의 일부 기능을 사용하여 이 탐색 패턴을 구축하는 방법에 대해 설명합니다. 우리는 TypeScript를 사용하여 진행할 예정인데, 아직 주제에 대한 설명서가 많지 않기 때문에 몇 가지 문제가 발생할 수 있습니다.

이 프로그램은 다음과 같은 화면을 제공합니다.

![App screens](https://miro.medium.com/max/1490/1*4pyoxyJQp7vqNYq0U_paZg.png)

이 예에서는 서랍 탐색으로 쉽게 전환할 수 있지만 탭 탐색을 사용합니다.

# 준비 중

먼저 TypeScript 템플릿을 사용하여 React Native 애플리케이션을 생성합니다.

```js
npx 반응 네이티브 in NavigationCommonPattern --template 반응 네이티브 템플릿 유형 스크립트
```

그런 다음 다음 명령을 실행하여 Retact Navigation에서 프로젝트에 필요한 모든 종속성을 추가합니다.

```js
실 추가 @twin-navigation/twin @twin-navigation/stack @twin-navigation/하단-twin
실 add react-reimated react-reimated react-reimated react-remister-react-remister-remister-remister-remister-remission-remission-reafe-rea-reaimesa-
```

iOS 애플리케이션을 개발하는 경우 다음 명령을 실행하여 포드를 설치합니다.

```js
npx 포드 설치
```

마지막으로, Retact Navigation을 사용하려면 `index.js` 파일로 이동하여 Retact 가져오기 전이라도 이 줄을 맨 앞에 놓으십시오.

```js
'➡-➡-➡-➡-➡'를 가져옵니다.
```

이제 응용 프로그램의 코드 작성을 시작할 수 있는 모든 준비가 되었습니다.

# 인증 흐름

이 시점의 목표는 사용자가 올바르게 로그인한 경우에만 홈 화면으로 이동할 수 있는 두 개의 화면(로그인 및 홈)을 갖는 것입니다. 또한 앱을 열었을 때 사용자가 이미 로그인되어 있다면 홈 화면을 직접 보여드리겠습니다. 이를 위해 다음 구조에 따라 StackNavigator를 사용할 예정입니다.

![StackNavigator structure](https://miro.medium.com/max/2776/1*4g8BmOo3rfL7SIbyBudPoQ.png)

로그인 상태를 제어하기 위해 React Context를 사용하여 로그인하는 기능과 로그아웃하는 기능이 두 가지 있습니다. 다음 코드를 `AuthContext.tsx` 파일에 넣습니다.

그런 다음 다음 코드를 사용하여 로그인 및 홈 화면에 대한 두 개의 파일을 만듭니다.

우리가 만든 컨텍스트에서 함수를 어떻게 사용하고 있는지 주목하십시오.

이제 App.tsx 파일로 이동하여 모든 항목을 삭제합니다. 우리는 거기서 아무것도 사용하지 않을 것이다. 대신 다음 코드를 사용하십시오.

이 코드를 분석하겠습니다.

또한 정의되지 않은 화면 이름을 사용하려고 하면 TypeScript가 어떻게 시작되어 런타임 오류를 방지하는지 아래 이미지에서 확인할 수 있습니다.

![TypeScript helping us](https://miro.medium.com/max/2000/1*vz18qSOSpU6JqAf1ZgnOgA.png)

# 탭 탐색기 추가

이제 RootStack에서 Home(홈) 화면을 꺼내서 Settings(설정)이라는 새 화면과 함께 Tab Navigator(탭 탐색기)에 넣습니다. 그런 다음 TabNavigator를 루트 스택의 화면으로 사용할 것입니다. 이렇게 하면 다음과 같은 탐색 구조가 생성됩니다.

![Navigation structure](https://miro.medium.com/max/3600/1*Ue43-F0X-wFmsJC_01WwBw.png)

먼저 `Settings.tsx`라는 새 파일을 만들고 다음 코드를 붙여 넣어 간단한 텍스트만 포함된 Settings 화면을 만듭니다.

그런 다음 다음 다음 코드를 사용하여 `App.tsx` 파일을 업데이트합니다.

변경된 내용을 살펴보겠습니다.

전체 네비게이터를 화면으로 사용하므로 탭 내부의 모든 화면 헤더에는 상위 화면 이름(이 경우 `루트 탭`)이 지정됩니다. 실제 이름이나 원하는 이름을 사용할 수 있도록 26라인에 getTabHeaderTitle 기능을 추가하였습니다. 자세한 내용은 문서를 참조하십시오.

# 추가 화면 및 탐색 유형 추가

이 시점에서, 우리는 많은 애플리케이션을 위한 좋은 기반을 가지고 있습니다. 그러나 일반적으로 탭 화면(예: 홈 화면에서 세부 정보 화면으로)에서 다른 화면으로 이동하려고 합니다. 그것을 응용 프로그램에 포함시키자. 또한 세부 정보 화면에서 전체 화면 모형이 있는 편집 화면으로 이동합니다.

다음 사항을 변경하면 다음과 같은 탐색 구조가 만들어집니다.

![Nav structure](https://miro.medium.com/max/5250/1*uW3V5I0J6CkG6EjnOLWdNw.png)

이전과 같이 Details 및 Edit 화면에 대해 두 개의 파일을 더 만드는 것부터 시작합니다.

그런 다음 `App.tsx` 파일을 업데이트하여 다음 코드를 포함합니다.

변경된 내용을 살펴보겠습니다.

![Settings screen](https://miro.medium.com/max/866/0*aOUVZjRPHq5i8JR5.png)

자세한 내용은 문서에서 확인하십시오.

# 결과

코드를 완료한 후 응용 프로그램을 보고 탐색하는 방법은 다음과 같습니다.

![Final app](https://miro.medium.com/max/966/1*KqGi2ayBpDgmbI7nQW6ICg.gif)

# 결론

이 탐색 패턴의 코드를 작성하는 동안 볼 수 있듯이, 반응 탐색에서 가장 많이 사용되는 기능은 중첩된 탐색기입니다. 이 예제의 마지막에, 우리는 세 개의 중첩된 네비게이터의 깊이를 가지고 있었는데, 이것이 이상적입니다. 중첩된 네비게이터가 많을수록 코드 성능이 떨어지고 복잡해집니다.

공식 문서에서 읽을 수 있는 중첩 네비게이터를 사용할 때 몇 가지 단점과 권장 사항이 있습니다. 모든 것이 어떻게 돌아가는지 이해하기 위해 그것을 주의 깊게 살펴보도록 하세요.