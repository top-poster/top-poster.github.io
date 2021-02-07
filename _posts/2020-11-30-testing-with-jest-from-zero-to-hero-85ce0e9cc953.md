---
layout: post
title: "농담 테스트: 주요 기능 및 사용 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2018/11/Testing-with-Jest.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/11/Testing-with-Jest.png?fit=730%2C487&ssl=1)

웹 개발자는 테스트를 통해 회귀를 방지하고 코드가 효과적으로 수행되도록 보장하므로 응용프로그램 작성에 있어 중요한 부분을 차지한다는 것을 알고 있습니다. 제스트를 시험 주자로 사용하는 것이 이것을 성취하는 한 가지 방법이다. 나는 꽤 오랫동안 제스트의 단골이었다. 원래는 다른 테스트 주자와 마찬가지로 사용했지만, 경우에 따라서는 생성-반응-앱의 기본 테스트 프레임워크이기 때문에 간단히 사용합니다.

오랫동안, 나는 제스트를 그것의 잠재력을 최대한 발휘하지 못했다. 이제, 왜 이것이 가장 좋은 테스트 프레임워크라고 생각하는지 보여드리겠습니다. 영원히.

## 제스트란 무엇인가?

제스트(Jest)는 페이스북이 구축한 자바스크립트 테스트 프레임워크이며 주로 리액트 기반 애플리케이션용으로 설계되었지만, 바벨, 자바스크립트, 노드, 앵글, 뷰와 함께 사용된다. NestJS와 GraphQL을 테스트하는 데도 사용할 수 있다. 제스트의 가장 좋은 특징 몇 가지를 살펴보자.

## 제스트의 주요 특징

### 제스트의 스냅샷 테스트

스냅샷은 무엇이며 스냅샷이 매우 유용한 이유는 무엇입니까?

이 기능을 처음 봤을 때 효소와 반응 단위 검사에 국한된 것이라고 생각했습니다. 하지만 그렇지 않아! 스냅샷은 UI의 변경 사항을 추적하는 데 사용됩니다. 스냅샷 테스트는 특정 시간에 구성 요소의 코드를 캡처하여 테스트와 함께 저장된 참조 스냅샷과 비교합니다. 직렬화 가능한 모든 개체에 스냅샷을 사용할 수 있습니다.

어디 한번 봅시다

함수가 일부 중첩된 데이터 구조를 가진 개체와 같이 사소한 값을 반환하는지 여부를 검정하려고 한다고 가정해 보십시오. 나는 이런 코드를 쓰는 내 자신을 여러 번 발견했다.

```java
const data = someFunctionYouAreTesting()
assert.deepEqual(data, {
  user: {
    firstName: 'Ada',
    lastName: 'Lovelace',
    email: 'ada.lovelace@example.com'
  }
  // etc.
})
```

그러나 일부 중첩된 속성이 기대했던 것과 완전히 다르다면… 오류가 발생하면 시각적인 차이를 찾아야 합니다!

```css
assert.js:83
  throw new AssertionError(obj);
  ^

AssertionError [ERR_ASSERTION]: { user:
   { firstName: 'Ada',
     lastName: 'Lovelace!',
     email: 'ada.lovelace@example.com' } } deepEqual { user:
   { firstName: 'Ada',
     lastName: 'Lovelace',
     email: 'ada.lovelace@example.com' } }
```

테스트 중인 함수가 (예를 들어, 임의 API 키를 생성할 때) 랜덤을 반환하는 경우 이 메커니즘을 더 이상 사용할 수 없습니다. 이 경우 필드별로 다음 필드를 수동으로 확인해야 합니다.

```java
const data = someFunctionYouAreTesting()
assert.ok(data.user)
assert.equal(data.user.firstName, 'Ada')
assert.equal(data.user.lastName, 'Lovelace')
assert.equal(data.user.email, 'ada.lovelace@example.com')
// and it goes on...
```

테스트 측면에서는 이것이 더 좋지만 훨씬 더 많은 작업이 필요합니다.

이러한 작업을 하는 자신을 발견하면 스냅샷을 매우 좋아하게 될 것입니다.

다음과 같은 내용을 쓸 수 있습니다.

```cpp
const data = someFunctionYouAreTesting()
expect(data).toMatchSnapshot()
```

…테스트를 처음 실행하면 수동으로 열고 검증할 수 있는 스냅샷 파일에 데이터 구조를 저장합니다. 테스트를 다시 실행할 때마다 Jest는 스냅샷을 로드하고 테스트에서 수신된 데이터 구조와 비교합니다. 차이가 있을 경우 Jest는 출력물에 컬러 디파일을 인쇄합니다. 대단해!

이제 전체 구조를 비교하지 않으려면 어떻게 해야 합니까? (일부 필드는 동적일 수도 있고 테스트에서 테스트로 변경될 수도 있기 때문입니다.) 문제 없어요.

```js
const data = someFunctionYouAreTesting()
expect(data).toMatchSnapshot({
  createdAt: expect.any(Date),
  id: expect.any(Number),
})
```

이들은 부동산 매치커라고 불린다.

하지만 더 있어요. 데이터 구조를 검증하는 이 방법에서 발견한 한 가지 문제는 스냅샷 파일이 테스트 코드와 분리되어 있다는 것입니다. 그래서 때때로 당신은 한 파일에서 다른 파일에 스냅 샷 뭐를 기대하고 있어 포함하고 있는지 확인하기 위해 뛸 필요가 있다. 문제없어! 스냅샷이 충분히 작으면 인라인 스냅샷을 사용할 수 있습니다. 다음 기능만 사용하면 됩니다.

```cpp
const data = someFunctionYouAreTesting()
expect(data).toMatchInlineSnapshot()
```

바로 그거야! 잠깐... 그런데 스냅샷은 어디 있죠?

스냅샷이 아직 없습니다. 테스트를 처음 실행할 때 Jest는 데이터 구조를 수락하고 스냅샷 파일에 저장하는 대신 코드에 저장합니다.

네, 그러면 검사 코드가 바뀌어 다음과 같은 결과가 나타납니다.

```coffeescript
const { someFunctionYouAreTesting } = require("../src/app");
test("hello world", () => {
  const data = someFunctionYouAreTesting();
  expect(data).toMatchInlineSnapshot(`
Object {
  "user": Object {
    "email": "ada.lovelace@example.com",
    "firstName": "Ada",
    "lastName": "Lovelace",
  },
}
`);
});
```

이건 내 마음을 아프게 하고, 난 그걸 좋아해. 코드를 매끄럽게 변경하는 개발 도구는 단순하고 우아한 솔루션으로, 다른 시나리오에서 매우 유용합니다. 브라우저의 구성요소를 시각적으로 편집할 수 있는 반응/각선/부 개발 모드를 사용하고 이러한 변경 사항에 맞게 코드가 업데이트된다고 가정해 보십시오.

그런데 테스트가 인라인 스냅샷을 사용할 만큼 충분히 작지 않은 경우에도 몇 가지 도움말을 볼 수 있습니다. 이 확장명에 Visual Studio Code를 사용하면 홀버에 스냅샷을 볼 수 있습니다(일부 제한이 있더라도 매우 유용함).

### 대화형 모드 사용

처음에는 대화형 모드가 많은 CLI 애플리케이션이 가지고 있는 일반적인 시계 기능을 나타내는 멋진 용어라고 생각했습니다. 하지만 그 후 나는 몇 가지를 배웠다.

농담은 Git과 Mercurial과 통합된다. 기본적으로 감시 모드는 마지막 커밋 이후 변경된 내용에 영향을 받는 테스트만 실행합니다. 이것은 멋지고 내가 더 많은 핵커밋을 쓰도록 만든다. 만약 여러분이 제스트가 어떤 시험이 커밋 변화에 의해 영향을 받는지 어떻게 알고 있는지 궁금하다면, 여러분은 혼자가 아닙니다.

Jest가 가장 먼저 하는 일은 테스트를 로드하여 애플리케이션의 소스 코드를 로드하고, 상호의존성 그래프를 생성하기 위한 요구 사항()과 가져오기를 구문 분석하는 것입니다.

그러나 Git 또는 Mercurial을 사용하는 것만이 매번 실행할 테스트 수를 제한하는 유일한 방법은 아닙니다. 소스 코드를 변경하고 실패한 테스트를 많이 볼 때 가장 간단한 테스트에 집중합니다. test.only를 사용하면 되는데, 더 좋은 방법이 있다(특히 test.only나 test.skip은 잊어버리고 코드에 남기 쉬우므로 test.only나 test.skip을 좋아하지 않는다).

"상호적인 방식"이 더 우아하고 편리합니다. 테스트 코드를 편집하지 않고 다른 방식으로 실행되도록 테스트를 제한할 수 있습니다.

어디 한번 봅시다

가장 간단한 것은 t를 치고 테스트 이름을 입력하는 것입니다. hello world(헬로 월드)를 테스트하는 경우 t를 클릭하고 hello world(헬로 월드)를 입력한 다음 Enter(입력)을 클릭합니다.

음, 대부분의 경우에 효과가 있습니다. 만약 여러분이 (`안녕하세요 세계 2)`를 가지고 있다면, 그것은 여러분이 정규식을 입력했기 때문에 실행될 것입니다. 이것을 피하기 위해, 나는 보통 패턴의 끝에 "$"를 추가합니다.

데이터베이스에 적합한 통합 테스트가 많은 프로젝트에서 테스트 실행 속도가 여전히 느렸습니다. 그 이유는 무엇입니까?

테스트 이름으로 필터링해도 다른 모든 테스트에서 before() 및 after() 콜백이 모두 실행되지 않습니다. 일반적으로 통합 테스트에서 이러한 콜백은 데이터베이스에 대한 연결 열기 및 닫기 같은 중요한 기능을 제공합니다.

그래서 그것을 막기 위해서 나는 보통 파일 이름으로도 필터링을 한다. p(경로용)를 누르고 테스트가 포함된 파일 이름을 입력하십시오. 이제 테스트가 훨씬 빨리 실행된다는 것을 알 수 있습니다. t를 클릭하고 Enter 키를 눌러 필터를 청소하기만 하면 됩니다. p와 enter(입력)가 있는 파일 이름의 필터에서도 동일한 작업을 수행합니다.

또 다른 유용한 기능은 업그레이드입니다. 새 스냅샷이 정상이고 오래된 스냅샷이 오래된 것을 보면 업그레이드만 하면 스냅샷을 덮어씁니다!

두 가지 더 유용한 옵션은 모든 테스트를 실행할 수 있는 a와 실패한 테스트를 다시 실행할 수 있는 f입니다.

### 즉시 사용 가능

제가 좋아하는 또 다른 것은 Jest가 "배터리 포함" 프레임워크라는 것인데, 이것은 보통 플러그인과 라이브러리를 추가하여 공통 기능을 추가할 필요가 없다는 것을 의미합니다. 그냥 같이 보내요! 몇 가지 예:

- 몇 가지 기본 제공 리포터 또는 맞춤형 리포터 중에서 선택할 수 있는 기능으로 테스트에서 커버리지 보고서를 가져오려면 Jest를 호출할 때 커버리지를 추가하십시오. 범위 임계값을 설정하여 해당 임계값을 충족하지 못할 경우 테스트(및 CI)가 실패하도록 할 수도 있습니다. 이는 코드에서 양호한 테스트 범위를 유지하는 데 이상적입니다.
- 알림을 추가하면 테스트 주자가 완료되면 데스크톱 알림이 표시됩니다. 만약 수천 개의 시험이 있다면, 그들은 끝내는 데 시간이 걸릴 수 있습니다. 이 플래그를 추가하면 시간이 절약됩니다.
- 강력하고 유용한 주장을 쓰기 위해 프로젝트에 주장 라이브러리를 추가할 필요는 없습니다. 이미 내장된 광범위한 예상 기능을 통해 스냅샷 기능에서도 볼 수 있는 컬러 디핑과 같은 흥미로운 기능과 함께 사용할 수 있습니다.
- 기능이나 서비스를 조롱하기 위해 라이브러리가 필요하지 않습니다. 기능 및 모듈을 모의 실행하고 어떻게 호출되었는지 확인할 수 있는 유틸리티가 많습니다.

### VSCode로 Jest 테스트 디버깅

VSCode로 Jest 테스트를 디버깅하는 것은 매우 간단합니다.

디버거 탭으로 가서 작은 빨간 점이 있는 기어 아이콘을 클릭하세요. 이 아이콘을 클릭하고 Node.js 실행 파일을 만듭니다. 이제 내용을 아래에 나와 있는 레시피로 바꾸세요.

이 레시피는 두 가지 구성을 포함하는 별도의 레시피를 기반으로 합니다. 하나는 모든 테스트를 실행하기 위한 것이고 다른 하나는 현재 테스트 파일만 실행하기 위한 것입니다. 텍스트 편집기에서 테스트 이름을 선택하고 해당 구성만 실행할 수 있는 추가 구성을 추가했습니다.

코드나 테스트를 편집하고 저장하면 테스트가 매우 빠르게 다시 실행되도록 감시 플래그도 추가했습니다. 이것은 Jest가 기본 시스템(Node.js 런타임)이 아닌 자체적으로 테스트를 로드하기 때문에 가능하다.

```undefined
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Jest All",
      "program": "${workspaceFolder}/node_modules/.bin/jest",
      "args": ["--runInBand"],
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen",
      "windows": {
        "program": "${workspaceFolder}/node_modules/jest/bin/jest",
      }
    },
    {
      "type": "node",
      "request": "launch",
      "name": "Jest Current File",
      "program": "${workspaceFolder}/node_modules/.bin/jest",
      "args": ["${relativeFile}"],
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen",
      "windows": {
        "program": "${workspaceFolder}/node_modules/jest/bin/jest",
      }
    },
    {
      "type": "node",
      "request": "launch",
      "name": "Jest Selected Test Name",
      "program": "${workspaceFolder}/node_modules/.bin/jest",
      "args": ["${relativeFile}", "-t=${selectedText}$", "--watch"],
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen",
      "windows": {
        "program": "${workspaceFolder}/node_modules/jest/bin/jest",
      }
    }
  ]
}
```

## 결론들

많은 사람들이 생각하는 것에도 불구하고, 제스트는 단순한 테스트 주자가 아니라 다른 수준으로 테스트를 가져온 완전한 테스트 프레임워크이다. 강력하지만 사용하기 쉬우니까 한번 사용해 보세요. 후회하지 않을 거라고 약속할게요.