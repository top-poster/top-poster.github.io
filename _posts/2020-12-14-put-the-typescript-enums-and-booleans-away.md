---
layout: post
title: "TypeScript 열거형 및 부울 제거"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/putthetypescriptenumsandbooleansaway.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/putthetypescriptenumsandbooleansaway.png?fit=730%2C487&ssl=1)

내가 TypeScript의 용감한 새로운 세계에서 가장 먼저 좋아한 것 중 하나는 TypeScript 열거형이었다. 나는 이전에 C#에서 그것들을 사용했고 안심할 수 있는 친근함을 느꼈다.

열거형은 숫자 또는 문자열 형식을 취할 수 있는 명명된 상수 집합입니다. 저는 항상 문자열 기반 열거형을 사용했는데, 이 열거형은 이 게시물의 모든 예에 사용할 것입니다.

```undefined
enum State {
  on = 'ON',
  off = 'OFF,
};
```

## 부적절한 사용

나는 @reduxjs/toolkit가 악명높은 Redux 보일러 플레이트를 완화하기 전에 Redux 동작의 문자열 유형과 같은 모든 종류의 부적절한 장소에 열거형을 사용했다.

```undefined
enum AuthActionTypes {
  SetForcePasswordChange = "SET_PASSWORD_CHANGE"
}

interface ForcePasswordChange {
type: AuthActionTypes.SetForcePasswordChange;
}

export const forcePasswordChange = (): ForcePasswordChange => ({
  type: AuthActionTypes.SetForcePasswordChange
});
```

저의 동기는 성가신 문자열 오타 오류를 피하는 것이었고, 이 요구 조건에서는 효과가 있었습니다.

## 새 망치로 무장한 모든 것이 못처럼 보인다.

저는 건축 시간과 런 타임에 존재하는 새로운 매력적인 프랑켄슈타인 건축물과 함께 제 자신감이 커지면서 더 모험적이 되었습니다.

Enum은 유한 상태 시스템에서 상태를 모델링하는 데 탁월한 선택으로 보였다.

그 당시에는 TypeScript의 가장 뛰어난 기능 중 하나가 누락되어 있다는 사실을 몰랐습니다. 즉, 상호 배타적인 상수 집합을 확인하는 것보다 훨씬 더 많은 기능을 사용할 수 있습니다.

```undefined
enum AuthenticationStates {
  unauthorised = "UNAUTHORISED",
  authenticating = "AUTHENTICATING",
  authenticated = "AUTHENTICATED",
  errored = "ERRORED",
  forcePasswordChange = "FORCE_PASSWORD_CHANGE"
}
```

위의 예에서는 인증 워크플로우를 모델링하는 `AuthenticationStates` 열거형이 있습니다.

사용자가 인증되지 않음 상태로 시작한 후 인증으로 전환됩니다. 신중하게 조작된 열거형을 사용하면 사용자가 한 번에 둘 이상의 모순된 상태에 있을 수 없습니다. 예를 들어 인증 및 인증은 허용되지 않습니다.

## 주스가 더 필요해요.

그 후, 예를 들어 오류가 발생하면 실제 `Error` 객체가 무엇인지 알아야 한다는 등의 추가 데이터가 필요하다는 것을 알게 되었습니다.

처음에는 다음과 같은 새로운 요구 사항을 모델링했습니다.

```undefined
enum AuthenticationStates {
  unauthorised = "UNAUTHORISED",
  authenticating = "AUTHENTICATING",
  authenticated = "AUTHENTICATED",
  errored = "ERRORED",
}

type State = {
  current: AuthenticationStates;
  isLoading: boolean;
  authToken?: string;
  error?: Error;
};

const current: State = {
  kind: AuthenticationStates.authenticated,
  isLoading: false,
  authToken: 'token',
  error: undefined
};
```

나는 각 인증 상태가 동일한 유형의 필드를 가질 수 있도록 부지런히 보장했습니다.

이 접근 방식의 문제는 각 상태가 동일한 필드를 가지며 버그를 복사하고 붙여넣기 시작할 수 있기 때문에 버그의 훌륭한 원천이 될 수 있다는 것입니다.

그런 다음 대수 데이터 유형으로 알려진 구별된 결합을 발견했다.

## 구별된 결합 a.k.a 대수 데이터 유형

파티에서 사람들에게 깊은 인상을 주고 싶다면, 매일 대수적 데이터 유형을 사용한다고 말하는 것은 보장된 홈런이다!

TypeScript에서는 `Authentication` 열거형과 매우 유사한 문자열 유니언을 생성할 수 있습니다.

```bash
type AuthenticationStates =
  | "UNAUTHORISED"
  | "AUTHENTICATING"
  | "AUTHENTICATED"
  | "ERRORED";
```

나는 이것을 어떤 값이 허용되는지에 대한 더 나은 보증을 제공하는 더 강력한 문자열 매개 변수 유형으로 사용할 수 있다.

TypeScript의 조합은 원시적인 조합만이 아니라 많은 것을 조합할 수 있다. 우리는 노조의 각 요소와 같은 `친절한` 분야만 가지면 되는 노조를 만들어 세상을 더 나은 곳으로 만들 수 있다. `친절한` 분야가 차별자 역할을 할 것이다.

```bash
export type AuthenticationStates =
  | {
      kind: "UNAUTHORISED";
      context: {
        isLoading: false
      };
    }
  | {
      kind: "AUTHENTICATING";
      context: {
        isLoading: true;
      };
    }
  | {
      kind: "AUTHENTICATED";
      context: {
        isLoading: false;
        authToken: string;
      };
    }
  | {
      kind: "ERRORED";
      context: { isLoading: false; error: Error };
    };
```

위의 유형은 모두 아름다운 실행 문서이며 워크플로에서 사용 가능한 모든 상태를 한 눈에 볼 수 있습니다.

위의 예에서 판별자는 컴파일러가 좁은 범위를 입력하거나 변수의 정확한 결합 요소를 결정할 때 더 구체적인 규칙을 적용하기 위해 사용하는 `종류` 필드이다.

여기서 중요한 점은 각 유형별로 적절한 데이터만 사용 가능하다는 것입니다.

우리는 `auth`에 접근하려고 할 필요가 없다.현재 인증 상태가 아닌 경우 토큰.

매우 흥미로운 것은 컴파일러가 코드 검토 중에 JVM에 너무 많은 시간을 소비한 프로그래머보다 이러한 정확성 순서를 더 잘 적용할 수 있다는 것이다.

## 식별된 결합에 대한 유형 좁히기

다음은 TypeScript가 유니언의 판별기에서 범위를 좁힐 수 있는 방법에 대한 그림입니다.

```coffeescript
const transition = (state: AuthenticationStates) => {
  switch (state.kind) {
    case "UNAUTHORISED": {
      console.log(state.context.userName); // only available in UNAUTHORISED

      // this is hot!! the compiler will not allow us to access the authToken in this state
      console.log(state.context.authToken); // Property 'authToken' does not exist on type '{ isLoading: false; userName: string; password: string; }'
      break;
    }
    case "AUTHENTICATING":
      console.log(state.context.userName); // Property 'userName' does not exist on type '{ isLoading: true; }'.
      // Type 'false' is not assignable to type 'true'
      state.context.isLoading = false;
      // The only assignable value is true in this state
      state.context.isLoading = true;
      break;
    case "AUTHENTICATED":
      // here and only here do we have an authToken
      console.log(state.context.authToken);
      break;
    case "ERRORED":
      console.log(state.context.error);
      break;
  }
};
```

코드 샌드박스는 여기에서 확인할 수도 있습니다.

우리는 컴파일러가 kind 판별기 필드를 사용하여 형식이 좁아졌을 때만 특정 데이터에 액세스할 수 있다.

가장 좋은 예는 다음과 같습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/unathorised2.png?resize=1273%2C237&ssl=1)

auth를 사용하려고 하면 컴파일러에서 오류가 발생합니다.토큰이 잘못된 상태입니다.

## 부울이 상태를 모형화하지 않습니다.

부울을 사용하여 상태를 모델링하려는 모든 시도는 모순되는 변수와 스트레스 받는 개발자의 폭발로 실패할 것이다.

예를 들어, 이 방법을 시도했다면:

```java
const isAuthenticated: boolean = false;
const isErrored: boolean = false;
const isLoading: boolean = false;
```

이러한 어리버리들을 혼란스러운 논리로 결합하는 데 오래 걸리지 않을 것입니다.

```coffeescript
if (isErrored &&. isAuthenticated === false) {
  // do this
} else if (isLoading && is isErrored) {
  // do something else
```

## 대수학이 어디 있지?

이러한 화려하게 들리는 대수적 데이터 유형은 유형이 다른 유형으로 구성된다는 것을 나타내는 방법에 지나지 않는다. 바로 그거예요. 전혀 그렇게 멋지지 않아요.

## 에필로그

노동조합은 훌륭한 실행 문서 역할을 하며, 또한 자바스크립트 프로그래밍의 와일드 웨스트 환경에서 특히 널리 퍼져 있는 잘못된 프로그래머를 직선적이고 좁게 유지한다.

대수적 데이터 유형은 한동안 하스켈과 같은 기능적 언어로 존재해왔으며, TypeScript가 이들을 프런트엔드 개발자들의 위대한 미완성 상태로 이끌어냈다는 점이 흥미롭다.