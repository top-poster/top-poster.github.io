---
layout: post
title: "모듈을 사용하여 TypeScript의 코드 구성"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/typescript-module.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/typescript-module.png?fit=730%2C487&ssl=1)

TypeScript에 대해 제가 정말 좋아하는 것 중 하나는 모듈형 프로그래밍 아키텍처를 지원하는 기능입니다. 수년 동안 설계 패턴과 모범 사례에 대한 열렬한 관심을 가지고 JavaScript 애플리케이션을 구축한 결과, 소프트웨어의 구조와 재사용 가능성을 고려할 때 ES6 모듈 패턴이 필수적이라는 사실을 알게 되었습니다.

이 기사에서는 모듈, 모듈 내용 및 모듈을 사용하여 TypeScript 코드의 구성을 개선하는 방법에 대해 설명합니다.

## 모듈이란 무엇인가?

모듈(Module)은 컴퓨터 프로그래밍에서 코드의 가독성, 구조 및 시험성을 향상시키기 위해 사용되는 일종의 설계 패턴이다. 이들은 TypeScript를 포함한 많은 프로그래밍 언어에서 지원된다. 간단히 말해서, 모듈은 독립적인 프로그램 기능을 가진 작은 코드 조각이다.

### 왜 우리는 모듈이 필요한가?

일반적인 소프트웨어는 새로운 기능이 도입됨에 따라 점차적으로 성장한다. 코드베이스를 구조화하고 보존하는 것은 처음에는 지루한 작업이 될 수 있지만, 미리 작업을 수행하는 것은 다음에 코드베이스에서 작업할 때 상황을 더 쉽게 만든다.

우리의 코드를 구조화하지 않으면 소프트웨어의 일부가 깊이 얽힐 수 있어 코드를 재사용하고 코드 조각을 분리하여 보는 것이 더욱 어려워진다. 모듈은 코드를 분리하여 재사용 및 테스트하기 위해 액세스 가능하도록 유지하는 데 매우 유용한 도구입니다.

코드 및 관련 기능을 위한 조직 도구로서 모듈을 통해 다음을 수행할 수 있습니다.

- 코드를 캡슐화합니다.
- 현재 모듈이 작동하기 위해 필요한 모듈 목록을 명시적으로 정의합니다.
- 자체 포함 블록으로 코드 구성
- 코드를 쉽게 테스트합니다.
- 공용 API 정의
- 그리고 더!

다음 섹션에서는 TypeScript와 함께 모듈을 사용하는 방법에 대해 설명합니다.

## TypeScript 모듈 탐색

이제 소프트웨어 개발에서 어떤 모듈이 있는지 공유하게 되었으므로 TypeScript 모듈 및 모듈 사용 방법에 대해 자세히 알아볼 수 있습니다.

TypeScript가 자바스크립트의 상위 집합이기 때문에, 대부분의 개념은 ES6에서 도입된 자바스크립트 모듈 패턴을 포함하여 자바스크립트에서 파생된다. 또한 자바스크립트 ES6 모듈과 동일한 내보내기 및 가져오기 키워드를 사용한다.

### 내보내기 구문 사용

TypeScript에서 코드 조각은 모듈 내부에 남아 있으므로 내보낼 때까지 모듈 외부에서 액세스할 수 없습니다. 내보낸 코드는 노출됩니다.

다음은 `내보내기`를 사용하는 예입니다.

```js
// module-1.ts 

//Private variable
let myApiKey: string = "Secret"; 

//Public variable
export const myPublicKey: string = "Public"; 

export enum MutationType {
  CreateTask = 'CREATE_TASK',
  SetTasks = 'SET_TASKS',
  RemoveTask = 'REMOVE_TASK',
  EditTask = 'EDIT_TASK',
}

// exported interface 
export interface Mutation{ 
  content: string; 
  type: MutationType; 
}

// private function
function logToConsole(mutation: Mutation): void {
    switch (mutation.type) {
        case MutationType.CreateTask:
            console.log(mutation.content);
    ...
        default:
            console.error(mutation.content);
    }
    console.log(message);
}

// exported function
export function log(mutation: Mutation): void {
    logToConsole(mutation);
}
```

### 가져오기 구문 사용

TypeScript에서는 `import`를 사용하여 모듈 외부에서 한 모듈의 노출된 코드 조각에 액세스할 수 있습니다.

다음은 예입니다.

```cpp
// my-module.ts
import { log, Mutation, MutationType, myPublicKey } from "./module-1";
console.log("Public key: ", myPublicKey);
const deleteTask: Mutation = {
    content: "Delete a task",
    type: MutationType.RemoveTask,
};
const createTask: Mutation = {
    content: "Create a task",
    type: MutationType.CreateTask,
};
log(deleteTask);
log(createTask);
```

TypeScript 모듈을 사용하여 클래스, type 별칭, var, let, const 및 기타 기호를 가져오고 내보낼 수 있습니다.

### 가져오기로 이름 바꾸기

ES6 모듈에서 매우 일반적인 개념은 import(가져오기)라는 이름을 바꾸는 것이다. TypeScript에서는 다음 구문을 사용하여 가져올 때 노출된 코드 조각의 이름을 변경할 수 있습니다.

```coffeescript
// my-module.ts
import { publicKey as publicApiKey } from './module-1"
```

또는 아래 구문을 사용하여 모듈의 모든 내용을 가져와 선택한 이름을 지정할 수 있습니다.

```coffeescript
// my-module.ts
import * as anyName from './module-1"
```

마지막으로 `esModule`을 설정합니다.intsconfig.json의 interop 옵션을 `true`로 설정하면 `Common`을 가져올 수 있습니다.아래 구문을 사용하는 JS` 모듈(ECMAScript 사양 준수):

```coffeescript
// my-module.ts
import foo from "someCommonJsModule";
```

새로운 TypeScript 3+ 프로젝트의 경우 `esModule`Interop` 설정은 자동으로 활성화됩니다.

### 내보내기로 이름 바꾸기

내보내기 문을 사용하여 다음 구문을 사용하여 노출된 코드 조각의 이름을 바꿀 수도 있습니다.

```undefined
interface Mutation{ 
  content: string; 
  type: MutationType; 
}

enum MutationType {
  CreateTask = 'CREATE_TASK',
  ...
}
// 1
export {
    Mutation
}
// Renaming export
export {
    Mutation as RenamedMutation
}
```

위의 예에서 첫 번째 내보내기는 원래 이름으로만 내보내고 두 번째 내보내기에서는 다른 이름으로 내보내며 키워드를 사용합니다.

### 기본 내보내기

JavaScript ES6 모듈과 마찬가지로 각 모듈에도 다음 구문을 사용하여 기본 내보내기가 가능합니다.

```js
// export default function
export default function log(mutation: Mutation): void {
    logToConsole(mutation);
}
```

기본 내보내기 구문은 다음과 같이 이름이 변경된 내보내기 및 `내보내기` 구문과 함께 사용할 수 있습니다.

```js
// export default function
export default function log(mutation: Mutation): void {
    logToConsole(mutation);
}

function saveMutation(mutation: Mutation): void {
    saveToLocalStorage(mutation);
}

export {
    saveMutation
}

// Renaming export
export {
    saveMutation as RenameSaveMutation
}
```

마지막으로 모듈에서는 `export =...`를 사용하여 단일 객체 또는 함수를 내보낼 수 있습니다.구문 예를 들어:

```js
//save-module.ts
function saveMutation(mutation: Mutation): void {
    saveToLocalStorage(mutation);
}
export = saveMutation;
```

다음은 `export =...`를 사용할 때의 해당 `import` 구문입니다.` 구문:

```undefined
//use-save-module.ts
import saveMutation = require("./save-module");
saveMutation(mutationToBeSave);
```

그러나 이는 권장되는 접근 방식이 아닙니다. 이 가져오기 스타일을 모듈에서 노출된 코드를 가져올 수 있는 마지막 수단으로만 사용해야 합니다.

### 재수출

다른 모듈에서 내보낸 노출된 코드 조각을 다시 내보낼 수도 있습니다.

```coffeescript
// module-with-re-exports.ts 
export * from "./module-1";
```

앞의 코드 조각에서, 우리는 `module-1.ts`의 노출된 코드 조각을 모두 다시 내보내는 것 외에는 모듈에서 아무것도 하지 않았다.

## 배럴을 수출입과 함께 사용하는 방법

### 배럴이란 무엇인가?

배럴은 서로 다른 모듈의 수출을 단일 모듈인 index.ts로 롤업하여 수입을 단순화하는 데 사용되는 기법이다. 따라서 배럴은 하나 이상의 다른 모듈의 내보내기를 단순히 결합합니다. 배럴은 매우 유용합니다. 왜냐하면 어떤 코드를 사용하고자 하는지에 집중하게 해주기 때문입니다.

### 컨텍스트에서 배럴 이해

배럴은 노출 코드의 특정 부분을 쉽게 가져오기 위해 전술적으로 사용될 수 있습니다.

예를 들어, 아래 코드 조각은 내보내기 구문을 사용하여 `bookList`( 문자열 배열)를 반환하는 `books` 함수를 내보냅니다.

```cpp
//barrel/book-module.ts
const bookList: string = ["God of War", "Lord of the rings"]
export function books(): string[] {
  return bookList
}
```

마찬가지로 다음 코드 조각은 내보내기 구문을 사용하여 carList(줄의 문자열)를 반환하는 cars 함수를 내보낸다.

```cpp
//barrel/car-module.ts
const carList: string = ["Ferrari", "BMW"]
export function cars(): string[] {
  return carList
}
```

마지막으로, 다음 코드 조각은 배럴을 사용하여 서로 다른 모듈의 내보내기를 일반적으로 인덱스.ts라고 하는 단일 모듈로 롤업하는 방법을 보여준다. 이 예에서는 `./북-모듈`과 `./자동차-모듈`의 모든 수출품을 각각 재수출합니다.

```js
//barrel/index.ts
export * from './book-module';
export * from './car-module';
```

### 왜 배럴을 사용하는가?

배럴이 없다면 소비자는 다음과 같은 두 가지 수입 내역이 필요합니다.

```js
//barrel/usage.ts
import { books } from '@/barrel/book-module';
import { cars } from '@/barrel/car-module';
```

대신 배럴을 사용하면 다음과 같이 수입을 간소화할 수 있습니다.

```js
//barrel/usage.ts
import {book, car} from '@/barrel';
let allBooks = books(); //["God of War", "Lord of the rings"]
let allCars = cars(); //["Ferrari", "BMW"]
```

아래 코드 조각도 유효한 배럴 사용량입니다.

```js
//barrel/usage.ts
import {book, car} from '@/barrel/index.ts';
let allBooks = books(); //["God of War", "Lord of the rings"]
let allCars = cars(); //["Ferrari", "BMW"]
```

## 결론

모듈형 프로그래밍의 중요성은 일반적인 소프트웨어 개발에서 아무리 강조해도 지나치지 않으며, 이를 무시하고 우리 소프트웨어의 부분이 깊이 얽히게 하려는 유혹이 있다.

확장 가능하고 재사용 가능한 TypeScript 애플리케이션을 구축하려면 TypeScript 모듈을 활용하여 애플리케이션 구성을 개선하는 것이 좋습니다. 이렇게 하면 코드 재사용 가능성과 테스트 용이성이 향상되고 빌드에 대한 전체적인 구조를 개선하는 데 도움이 됩니다.