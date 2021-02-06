---
layout: post
title: "TypeScript 4.1: 새로운 기능 및 개선 사항"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/typescript-4.1.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/typescript-4.1.png?fit=730%2C487&ssl=1)

TypeScript 4.1은 11월 말에 이 언어의 최신 안정적인 릴리즈가 되었다. 이 새로운 버전의 언어는 4.0 버전에 포함된 기능을 기반으로 개발자들이 더 정확한 방식으로 유형을 표현할 수 있도록 새로운 확인 플래그, 생산성 추적 및 버그 수정을 포함한 향상된 기능을 제공합니다.

이 기사에서는 이 릴리스에 포함된 일부 기능이 향후 코드 작성 방식에 어떤 영향을 미치는지 살펴봅니다.

## TypeScript 4.1로 업그레이드하는 방법

npm을 통해 TypeScript의 최신 버전을 설치하거나 업그레이드할 수 있습니다.

```shell
$ npm install -D typescript
```

설치를 건너뛰려면 TypeScript Playground를 통해 TypeScript의 새 기능을 사용해 볼 수 있습니다. TypeScript Playground는 최신 버전을 지원하므로 웹 브라우저를 종료하지 않고도 이 문서에 포함된 코드를 테스트할 수 있습니다.

이제 TypeScript 4.1에서 제공하는 몇 가지 가장 흥미로운 기능을 살펴보겠습니다.

## 템플릿 리터럴 유형

TypeScript에는 모든 문자열을 포함하는 문자열 유형과 특정 문자열에만 일치하는 문자열 리터럴 유형의 두 가지 문자열이 있습니다. 문자열 리터럴 유형과 조합하여 허용된 문자열 입력에 대해 보다 정확하게 지정할 수 있습니다. 다음을 살펴보십시오.

```bash
let a: string = 'Hello'
a = 'World' // works

let b: 'Hello' = 'Hello'
b = 'World' // Type '"World"' is not assignable to type '"Hello"'.

declare function selectOperatingSystem(os: 'windows' | 'linux' | 'macos'): void

selectOperatingSystem('windows'); // works
selectOperatingSystem('linux'); // works
selectOperatingSystem('android');
// Argument of type '"android"' is not assignable to parameter of type '"windows" | "linux" | "macos"'.
```

TypeScript 4.1을 사용하면 템플릿 리터럴 문자열 유형의 도움말로 연결된 문자열을 사용할 수 있습니다. 자바스크립트에서는 템플릿 리터럴과 동일한 구문을 사용하지만 형식 위치에서는 사용한다. 템플릿 리터럴 문자열 유형은 템플릿 문자열의 내용을 연결하여 새 문자열 리터럴 유형을 생성합니다.

```bash
type Name = 'John';
type Greeting = `Hello ${Name}`;
// Equal to: type Greeting = 'Hello John'
```

대체 위치에서 유니언 유형을 사용하면 템플릿 리터럴 유형의 검정력이 분명해집니다. 생성되는 유형은 가능한 모든 문자열 리터럴 조합입니다.

```bash
type Names = 'John' | 'Jake' | 'Sarah';
type Greeting = `Hello ${Name}`;
// Equal to: type Greeting = 'Hello John' | 'Hello Jake' | 'Hello Sarah'
```

또한 템플릿 리터럴에 여러 유니언 유형을 사용할 수 있으며, 이렇게 하면 결과 유형의 가능한 모든 조합과 함께 유니언을 생성할 수 있습니다.

```bash
type Suit =
  "Hearts" | "Diamonds" |
  "Clubs" | "Spades";

type Rank =
  "Ace" | "Two" | "Three" | "Four" | "Five" |
  "Six" | "Seven" | "Eight" | "Nine" | "Ten" |
  "Jack" | "Queen" | "King";

type Card = `${Rank} of ${Suit}`;

declare function printCard(c: Card): void

printCard('Ace of Spades'); // works
printCard('Ace of Diamond'); // error
```

편집기에서 `type` 위에 마우스를 놓으면 다음과 같이 표시됩니다.

이 기능이 활성화하는 훌륭한 사용 사례는 Express와 같은 Node.js 웹 프레임워크에서 경로 매개 변수를 구문 분석하는 기능이며, 여기서 자리 표시자를 사용하여 경로의 동적 부분(예: `ID`)을 나타내는 것이 일반적이다.

```coffeescript
app.get("/users/:userId", (req, res) => {
    const { userId } = req.params;
    res.send(`User ID: ${userId}`);
});
```

req.params 개체는 다음과 같은 서명이 있는 Params Dictonary입니다.

```undefined
interface ParamsDictionary {
    [key: string]: string;
}
```

TypeScript는 객체에 문자열 값이 있는 문자열 키가 있다는 것을 모두 알고 있습니다. 경로 경로에 지정되지 않은 매개 변수에 액세스하려고 하면 컴파일러가 `userId`만 존재할 수 있다는 것을 모르기 때문에 이 매개 변수를 허용합니다.

템플릿 리터럴 유형을 사용하여 대체 위치에서 추론하여 `req.params` 개체에 대한 보다 정확한 유형을 제공할 수 있다.

```bash
type ExtractRouteParams<T extends string> =
  string extends T
  ? Record<string, string>
  : T extends `${infer Start}:${infer Param}/${infer Rest}`
  ? {[k in Param | keyof ExtractRouteParams<Rest>]: string}
  : T extends `${infer Start}:${infer Param}`
  ? {[k in Param]: string}
  : {};

type params = ExtractRouteParams<'/users/:userId/posts/postId'>;
// Same as: type params = { userId: string; postId: string; }
```

좀 더 자세히 설명하겠습니다.

- 첫 번째 경우는 T가 문자열 또는 임의일 경우 일치합니다. 이 경우 매개 변수에서 아무것도 추론할 수 없습니다.
- 두 번째 경우는 경로 시작 시 매개 변수와 일치하며 문자열의 더 짧은 섹션으로 반복적으로 호출됩니다.
- 세 번째 경우는 경로 끝에 있는 매개 변수와 일치합니다.
- 네 번째 경우에는 빈 객체가 반환됩니다.

이 패턴을 사용하면 예에서 설명한 것처럼 `/user/:userId`에서 올바른 유형의 `{userId: string}`을 쉽게 추출할 수 있습니다. 이 GitHub 저장소에서는 다른 사용 사례(또는 경우에 따라서는 상위 사용 사례)를 확인할 수명은 GitHub 저장소입니다.

## 인덱스 액세스 확인

인덱스 서명은 유형에 명시적으로 정의되지 않은 개체 속성에 액세스할 수 있도록 TypeScript 컴파일러에 지정하는 방법입니다.

```undefined
type Person = {
  name: string;
  email: string;

  // index signature
  [key: string]: string;
}

function logPerson(p: Person): void {
  console.log(p.name) // string
  console.log(p.email) // string
  console.log(p.nationality.toLowerCase()) // OK, nationality is a string
}

const person: Person = {
  name: "Jack",
  email: "jack@example.com",
};

logPerson(person);
```

명시적으로 나열된 속성을 제외하고 `사용자` 아래에 액세스하는 모든 속성은 `문자열` 유형을 가집니다. 이 기능의 문제는 인덱스 서명으로 확인되는 속성 액세스는 잠재적으로 `정의되지 않음`이지만 이 유형은 반영되지 않는다는 것입니다. 예를 들어 위의 블록에서 p.국적은 잠재적으로 정의되지 않지만 유형 정보는 문자열입니다.

TypeScript 4.1은 `noUnchecked`를 도입하여 이 바람직하지 않은 동작을 수정하는 방법을 제공합니다.인덱스 액세스 플래그입니다. 이 옵션을 선택하면 인덱스 서명으로 확인되는 모든 속성 또는 인덱스 액세스는 이제 지정된 유형 및 `정의되지 않음`의 유니언으로 확인됩니다. 아래 예제를 확인하십시오.

```js
function logPerson(p: Person): void {
  console.log(p.name) // string
  console.log(p.email) // string
  console.log(p.nationality.toLowerCase()) // Error when noUncheckedIndexedAccess is enabled
  //          ~~~~~~~~~~~~~ Object is possibly 'undefined'.
}
```

이제 오류를 수정하기 위해 유형 가드를 사용하여 유형 또는 Null이 아닌 어설션 연산자(`!`)를 좁힐 수 있습니다.

```coffeescript
if (p.nationality) {
  console.log(p.nationality.toLowerCase())
}

console.log(p.nationality!.toLowerCase())
```

`확인되지 않음`을 활성화하는 또 다른 효과IndexedAccess(인덱스 액세스) 플래그는 인덱스가 한계를 확인했더라도 어레이 요소 유형과 `정의되지 않음`의 결합도 생성한다는 의미입니다.

```js
let arr = [1, 2, 3, 4];
for (let i = 0; i < arr.length; i++) {
    const n: number = arr[i]; // Error
    // Type 'number | undefined' is not assignable to type 'number'.
    console.log(n);
}
```

`각각()`과 같은 배열 방법을 사용해도 동일한 오류가 발생합니다.

```coffeescript
let arr = [1, 2, 3, 4];
arr.forEach((e, i) => {
  const n: number = arr[i]; // Error
  // Type 'number | undefined' is not assignable to type 'number'.
  console.log(n);
});
```

물론 인덱스를 통해 액세스하지 않고도 위의 예에서 요소를 직접 사용할 수 있지만 이 사용 사례는 코드베이스에서 이 플래그를 활성화하는 효과를 보여 줍니다. 요소의 존재 여부를 확인하지 않고 배열에 인덱스 액세스를 수행하면 많은 오류가 발생할 수 있지만 이전과 마찬가지로 유형 가드 또는 Null이 아닌 연산자를 사용하여 오류를 해결할 수 있습니다.

참고: 이 동작은 `엄격한` 수준 옵션의 일부로 활성화되지 않으므로 `선택되지 않음`을 설정하여 명시적으로 활성화해야 합니다.구성 파일의 `compilerOptions` 아래에 있는 `true`로 인덱스된 액세스 옵션입니다.

## 매핑된 형식으로 키 재매핑

매핑된 유형은 TypeScript에서 매우 중요한 도구입니다. 이러한 도구는 유형이 동기화된 상태를 유지하면서 복제를 제한하기 위해 다른 유형에서 유형을 가져오는 데 유용합니다.

다음은 다른 개체 유형을 기반으로 새 개체 유형을 생성하고 결과적으로 새 유형의 모든 속성을 null로 만드는 예입니다.

```undefined
type Person = {
  name: string;
  age: number;
}

// `Nullable<T>` is the same as T but each property is nullable
type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};

type NullablePerson = Nullable<Person>
// same as
//   type NullablePerson = {
//     name: string | null;
//     age: number | null;
//   };
```

이제 TypeScript 4.1에서 새 `as` 절로 개체 키를 다시 매핑할 수 있습니다. 이렇게 하면 기존 속성 이름에서 새 속성 이름을 만들 수 있어 유연성이 제공됩니다. 아래 예에서 개체 키를 다시 매핑하기 위해 템플릿 리터럴 유형을 사용하는 방법을 확인하십시오.

```coffeescript
type Company = {
  name: string;
  employees: number;
  ceo: string;
}

type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

type ComapanyAccessors = Getters<Company>
// same as
//   type ComapanyAccessors = {
//     getName: () => string;
//     getEmployees: () => number;
//     getCeo: () => string
//   };
```

보다시피, 키 자본화 도우미를 통해 키를 자본화했습니다. 비슷한 도우미로는 `자본화 해제`가 있다.T>taph` Uppercase.T>taph 및 "Lowercase >T>buit

## 재귀 조건부 유형

버전 2.8에서 조건부 유형이 처음 도입되었을 때, 당시에는 잘 지원되지 않았던 무한 재귀에 대한 보호 수단으로 비재귀로 제한되었다. TypeScript 4.1이 출시됨에 따라, 이제 분기 내에서 자신을 즉시 참조하는 조건부 유형을 표현할 수 있습니다. 아래 예제를 통해 `Waiting`이 어떻게 약속을 깊이 풀어내는지 확인하십시오.

```undefined
type Awaited<T> =
    T extends PromiseLike<infer U> ? Awaited<U> :
    T;

type P1 = Awaited<Promise<string>>;  // string
type P2 = Awaited<Promise<Promise<string>>>;  // string
type P3 = Awaited<Promise<string | Promise<Promise<number> | undefined>>>;  // string | number | undefined
```

TypeScript 컴파일러는 재귀 유형을 확인하는 데 더 많은 시간이 필요합니다. 내부 재귀 깊이 제한에 도달하면 컴파일 시간 오류가 발생합니다. 이러한 재귀 유형의 힘에도 불구하고 마이크로소프트는 복잡한 입력에 사용될 때 실패를 피하기 위해 책임감 있고 아껴야 한다고 경고한다.

## 반응 중인 JSX 공장

리액트 17의 곧 출시될 jsx 및 jsxs 공장 기능에 대한 지원은 jsx 컴파일러 플래그에 대한 react-jsx 및 react-jsxdev 옵션을 통해 이번 릴리스에 포함되었다. react-jsx 확장자는 생산 빌드를 위한 것이고, react-jsxdev는 개발을 위한 것이다.

```undefined
{
  "compilerOptions": {
      "module": "esnext",
      "target": "es2015",
      "strict": true,
      "jsx": "react-jsx"
  },
  "include": [ "./**/*" ]
}
```

TypeScript 유형 및 구문은 특히 React와 함께 사용할 때 따라가기 어려울 수 있습니다. 여기서 자세히 알아보십시오.

## 주목할 만한 언급

CheckJs 컴파일러 옵션을 설정하면 TypeScript 4.1에서 `allowJs`도 활성화됩니다. 이전에는 두 옵션이 중복되어 보이는 명시적으로 설정되어야 했다.

약속(void)을 일반형 인수로 설정하는 경우를 제외하고는 약속 생성자에서 해결()할 인수가 더 이상 선택적이지 않다는 점도 주목할 만한 개선사항이다.

예를 들어, 다음과 같은 코드를 작성할 때...

```js
function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve();
    }, 2000);
  });
}
```

… 다음과 같은 오류가 발생할 수 있습니다.

```bash
Expected 1 arguments, but got 0.
Did you forget to include 'void' in your type argument to 'Promise'?
```

수정 사항은 오류가 시사하는 바와 같이 `void` 일반 매개 변수를 추가하는 것입니다.

```js
function resolveAfter2Seconds() {
  return new Promise<void>(resolve => {
    setTimeout(() => {
      resolve();
    }, 2000);
  });
}
```

## 결론

TypeScript가 더 나은 개발자 경험과 더 단순하고 직관적인 API로 계속 발전하는 것을 보는 것은 고무적이다. 이 자료에는 4.1 릴리스의 주요 기능과 개선 사항만 자세히 설명되어 있으며 과거 TypeScript 기능은 자세히 나와 있지 않습니다. 모든 범위의 변경 사항이 궁금하신 경우 공식 릴리스 노트를 확인하십시오.

읽어줘서 고마워, 그리고 행복한 코딩!