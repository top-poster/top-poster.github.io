---
layout: post
title: "TypeScript에서 기본 제공 유틸리티 유형 사용"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/02/builtin-utility-types-typescript.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/builtin-utility-types-typescript.png?fit=730%2C487&ssl=1)

TypeScript는 프로그래밍 언어 중에서 가장 강력한 유형 시스템 중 하나를 가지고 있는데, 이는 대부분 JavaScript에서 수행할 수 있는 모든 동적 작업을 수용하도록 발전했기 때문입니다.

조건, 조회 및 매핑된 유형과 같은 기능을 포함하면 TypeScript에서 몇 가지 고급 유형 함수를 쓸 수 있습니다.

## 유형함수란 무엇인가?

TypeScript를 사용하면 기존 유형에서 유형 별칭을 생성할 수 있습니다. 예를 들어:

```undefined
// Example
type Str = string; 

// Usage
let message: Str = 'Hello world';
```

TypeScript 유형 별칭도 일반 항목을 지원합니다. 제네릭은 전통적으로 다른 유형을 기반으로 한 유형을 제한하는 데 사용됩니다. 예를 들어 `value`, `setValue` 쌍 및 제네릭 유형은 `setValue`가 항상 `value`에서 사용되는 것과 동일한 유형으로 호출되도록 하기 위해 사용됩니다.

```js
// Example
type ValueControl<T> = {
  value: T,
  setValue: (newValue: T) => void,
};

// Usage 
const example: ValueControl<number> = {
  value: 0,
  setValue: (newValue) => example.value = newValue,
};
```

위의 예에서 우리는 `ValueControl`의 유형을 전달할 수 있습니다. 이 예제에서는 `숫자` 유형(즉, `값 제어 <`)을 전달하고 있습니다.

TypeScript의 진정한 강력한 기능은 일반 함수에 전달된 형식이 조건(조건 유형 포함)과 매핑(맵핑 유형 포함)에서 사용될 수 있다는 것이다. 다음은 조건을 사용하여 유형에서 `null` 및 `undefined`를 제외하는 유형의 예입니다.

```undefined
/**
 * Exclude null and undefined from T
 */
type NoEmpty<T> = T extends null | undefined ? never : T;

// Usage 
type StrOrNull = string | null;
type Str = NoEmpty<StrOrNull>; // string
```

그러나 TypeScript에는 여러 가지 편리한 기본 제공 유틸리티 기능이 포함되어 있으므로 이러한 기본 수준 기능을 사용할 필요는 없습니다.

사실, 우리 타입은 "No Empty <"입니다.T >은 이미 TypeScript의 일부로 제공됩니다(`Non Nullable`이라고 함).T> 그리고 우리는 아래와 같이 다룬다.) 이 기사에서는 이러한 유형의 기능을 실제 예제와 함께 사용하여 이러한 기능을 사용하려는 이유를 살펴봅니다.

## TypeScript의 기본 제공 유형 함수

TypeScript 4.0은 추가 패키지 없이 TypeScript에서 사용할 수 있는 기본 제공 유형 기능입니다.

### '일부'T>

부분 입력 유형 T의 모든 구성원을 선택사항으로 표시합니다. 다음은 단순 `점` 유형의 예입니다.

```bash
type Point = { x: number, y: number };

// Same as `{ x?: number, y?: number }`
type PartialPoint = Partial<Point>;
```

일반적인 사용 사례는 변경하려는 속성의 하위 집합만 제공하는 많은 상태 관리 라이브러리에 있는 업데이트 패턴입니다. 예를 들어:

```cpp
class State<T> {
  constructor(public current: T) { }
  // Only need to pass in the properties you want changed
  update(next: Partial<T>) {
    this.current = { ...this.current, ...next };
  }
}

// Usage
const state = new State({ x: 0, y: 0 });
state.update({ y: 123 }); // Partial. No need to provide `x`.
console.log(state.current); // Update successful: { x: 0, y: 123 }
```

### '필수'T>

Required(필수)는 Partial(일부)과 반대입니다.T > 입력 유형 `T`의 모든 구성원을 선택사항으로 하지 않습니다. 다시 말해서, 그것은 그들을 요구하게 만든다. 다음은 이러한 변환의 예입니다.

```bash
type PartialPoint = { x?: number, y?: number };

// Same as `{ x: number, y: number }`
type Point = Required<PartialPoint>;
```

사용 사례는 유형이 선택적 구성원을 가지고 있지만 코드의 일부분을 모두 제공해야 하는 경우입니다. 선택적 구성원을 구성할 수 있지만 내부적으로 초기화하여 모든 코드 검사를 처리할 필요가 없습니다.

```js
// Optional members for consumers
type CircleConfig = {
  color?: string,
  radius?: number,
}

class Circle {
  // Required: Internally all members will always be present
  private config: Required<CircleConfig>;

  constructor(config: CircleConfig) {
    this.config = {
      color: config.color ?? 'green',
      radius: config.radius ?? 0,
    }
  }

  draw() {
    // No null checking needed :)
    console.log(
      'Drawing a circle.',
      'Color:', this.config.color,
      'Radius:', this.config.radius
    );
  }
}
```

### 읽기 전용<T>

입력 유형 `T`의 모든 속성을 `읽기 전용`으로 표시합니다. 다음은 이러한 변환의 예입니다.

```bash
type Point = { x: number, y: number };

// Same as `{ readonly x: number, readonly y: number }`
type ReadonlyPoint = Readonly<Point>;
```

이것은 편집을 방지하기 위해 개체를 고정하는 일반적인 패턴에 유용합니다. 예를 들어:

```js
function makeReadonly<T>(object: T): Readonly<T> {
  return Object.freeze({ ...object });
}

const editablePoint = { x: 0, y: 0 };
editablePoint.x = 2; // Success: allowed

const readonlyPoint = makeReadonly(editablePoint);
readonlyPoint.x = 3; // Error: readonly
```

### 픽<T, 키>

T에서 지정된 키만 선택합니다. 다음 코드에는 키 `x` | `y` | `z`가 있는 `포인트3D`가 있으며 키 `x` | `y`만 선택하면 포인트2D를 생성할 수 있다.

```bash
type Point3D = {
  x: number,
  y: number,
  z: number,
};

// Same as `{ x: number, y: number }`
type Point2D = Pick<Point3D, 'x' | 'y'>;
```

이 기능은 위의 예에서 보았던 것과 같은 개체의 하위 집합을 `Point2D`를 생성하여 얻는 데 유용합니다.

더 일반적인 사용 사례는 단순히 관심 있는 속성을 얻는 것입니다. 아래에서는 모든 CS 속성에서 폭과 높이를 확인할 수 있습니다.

```js
// All the CSSProperties
type CSSProperties = {
  color?: string,
  backgroundColor?: string,
  width?: number,
  height?: number,
  // ... lots more
};

function setSize(
  element: HTMLElement,
  // Usage: Just need the size properties
  size: Pick<CSSProperties, 'width' | 'height'>
) {
  element.setAttribute('width', (size.width ?? 0) + 'px');
  element.setAttribute('height', (size.height ?? 0) + 'px');
}
```

### 레코드<키, 값>

키에서 지정한 멤버 이름 집합을 지정하면 각 멤버가 `값` 유형인 유형이 생성됩니다. 이를 입증하는 예는 다음과 같습니다.

```coffeescript
// Same as `{x: number, y: number}`
type Point = Record<'x' | 'y', number>;
```

모든 유형의 구성원이 동일한 값을 가질 때, 레코드를 사용하면 모든 구성원이 동일한 값 유형을 갖는 것이 즉시 분명하므로 코드를 더 잘 읽을 수 있습니다. 위의 점 예에서 약간 볼 수 있습니다.

멤버가 많을 때는 레코드가 더 유용하다. 다음은 레코드를 사용하지 않는 코드입니다.

```bash
type PageInfo = {
  id: string,
  title: string,
};

type Pages = {
  home: PageInfo,
  services: PageInfo,
  about: PageInfo,
  contact: PageInfo,
};
```

다음은 `기록`을 사용하는 코드입니다.

```undefined
type Pages = Record<
  'home' | 'services' | 'about' | 'contact',
  { id: string, title: string }
>;
```

### 제외<T, 제외>

T에서 제외된 타입은 제외된다.

```js
type T0 = Exclude<'a' | 'b' | 'c', 'a'>; // 'b' | 'c'
type T1 = Exclude<'a' | 'b' | 'c', 'a' | 'b'>; // 'c'
type T2 = Exclude<string | number | (() => void), Function>; // string | number
```

가장 일반적인 사용 사례는 개체에서 특정 키를 제외하는 것입니다. 예를 들어:

```bash
type Dimensions3D = 'x' | 'y' | 'z';
type Point3D = Record<Dimensions3D, number>;

// Use exclude to create Point2D
type Dimensions2D = Exclude<Dimensions3D, 'z'>;
type Point2D = Record<Dimensions2D, number>;
```

또한 이 옵션을 사용하여 다른 바람직하지 않은 멤버(예: `null` 및 `미정의`)를 조합에서 제외할 수도 있습니다.

```js
type StrOrNullOrUndefined = string | null | undefined;

// string
type Str = Exclude<StrOrNullOrUndefined, null | undefined>;
```

### Non Nullable<T>

T형에서 null과 undefined는 제외된다. Exception <, null | defined >와 같은 효과를 가진다.

빠른 JavaScript Premier: nullable은 null 값(즉, `null` 또는 `undefined`)을 할당할 수 있는 값입니다. 따라서 Nullable은 null 값을 허용하지 않아야 합니다.

다음은 Exclude(제외)와 동일한 예제입니다. 하지만 이번에는 Non Nullable(누룰 수 없음)을 사용합니다.

```coffeescript
type StrOrNullOrUndefined = string | null | undefined;

// Same as `string`
// Same as `Exclude<StrOrNullOrUndefined, null | undefined>`
type Str = NonNullable<StrOrNullOrUndefined>;
```

### 추출<T, 추출>

T에서 추출된 타입을 추출한다. 제외할 유형(`제외`)을 지정하는 대신 유지/추출할 유형(`추출`)을 지정하므로 `제외`와 반대로 볼 수 있습니다.

```js
type T0 = Extract<'a' | 'b' | 'c', 'a'>; // 'a'
type T1 = Extract<'a' | 'b' | 'c', 'a' | 'b'>; // 'a' | 'b'
type T2 = Extract<string | number | (() => void), Function>; // () => void
```

추출물은 두 가지 유형의 교차점으로 생각할 수 있습니다. 이는 아래에서 공통 요소 `a` | `b`를 추출하는 경우이다.

```bash
type T3 = Extract<'a' | 'b' | 'c', 'a' | 'b' | 'd'>; // 'a' | 'b'
```

`추출물`의 한 가지 사용 사례는 다음과 같은 두 가지 유형의 공통 기반을 찾는 것이다.

```undefined
type Person = {
  id: string,
  name: string,
  email: string,
};

type Animal = {
  id: string,
  name: string,
  species: string,
};

/** Use Extract to get the common keys */
type CommonKeys = Extract<keyof Person, keyof Animal>;

/** 
 * Map over the keys to find the common structure 
 * Same as `{ id: string, name: string }`
 **/
type Base = {
  [K in CommonKeys]: (Animal & Person)[K]
};
```

### Omit<T, Keys>

유형 `T`에서 `키`로 지정된 키를 생략합니다. 예는 다음과 같습니다.

```bash
type Point3D = {
  x: number,
  y: number,
  z: number,
};

// Same as `{ x: number, y: number }`
type Point2D = Omit<Point3D, 'z'>;
```

특정 속성을 전달하기 전에 개체에서 생략하는 것은 JavaScript에서 일반적인 패턴입니다.

Omit 유형 함수는 이러한 변환에 주석을 달 수 있는 편리한 방법을 제공합니다. 예를 들어, 로그인하기 전에 PII(전자 메일 주소 및 이름과 같은 개인 식별 가능 정보)를 제거하는 것이 일반적입니다. `Omit`로 변환에 주석을 달 수 있습니다.

```undefined
type Person = {
  id: string,
  hasConsent: boolean,
  name: string,
  email: string,
};

// Utility to remove PII from `Person`
function cleanPerson(person: Person): Omit<Person, 'name' | 'email'> {
  const { name, email, ...clean } = person;
  return clean;
}
```

### '매개변수<함수>

함수 유형이 주어지면 이 유형은 함수 매개 변수의 유형을 튜플로 반환합니다. 다음은 이러한 변환을 보여 주는 예입니다.

```js
function add(a: number, b: number) {
  return a + b;
}

// Same as `[a: number, b: number]`
type AddParameters = Parameters<typeof add>;
```

매개 변수`를 TypeScript의 인덱스 조회 유형과 결합하여 개별 매개 변수를 가져올 수 있습니다. 첫 번째 매개 변수의 유형도 가져올 수 있습니다.

```js
function add(a: number, b: number) {
  return a + b;
}

// Same as `number`
type A = Parameters<typeof add>[0];
```

`매개변수`의 주요 사용 사례는 기능 매개변수의 유형을 캡처하여 코드에서 사용하여 유형 안전을 보장할 수 있는 기능입니다.

```undefined
// A save function in an external library
function save(person: { id: string, name: string, email: string }) {
  console.log('Saving', person);
}

// Ensure that ryan matches what is expected by `save`
const ryan: Parameters<typeof save>[0] = {
  id: '1337',
  name: 'Ryan',
  email: 'ryan@example.com',
};
```

### ConstructorParameters<Class Constructor

이것은 우리가 위에서 본 `매개변수` 타입과 비슷하다. 유일한 차이점은 `Constructor Parameters`가 다음과 같은 클래스 생성자에서 작동한다는 것이다.

```undefined
class Point {
  private x: number;
  private y: number;
  constructor(initial: { x: number, y: number }) {
    this.x = initial.x;
    this.y = initial.y;
  }
}

// Same as `[initial: { x: number, y: number} ]`
type PointParameters = ConstructorParameters<typeof Point>;
```

물론 건설자 파라미터의 주요 사용 사례도 비슷하다. 다음 예에서는 이 값을 사용하여 초기 값이 `포인트` 클래스에서 허용되는지 확인합니다.

```coffeescript
class Point {
  private x: number;
  private y: number;
  constructor(initial: { x: number, y: number }) {
    this.x = initial.x;
    this.y = initial.y;
  }
}

// Ensure that `center` matches what is expected by `Point` constructor
const center: ConstructorParameters<typeof Point>[0] = {
  x: 0,
  y: 0,
};
```

### '반환 유형 < 함수'

함수 유형이 주어지면 함수에 의해 반환되는 형식을 가져옵니다.

```js
function createUser(name: string) {
  return {
    id: Math.random(),
    name: name
  };
}

// Same as `{ id: number, name: string }`
type User = ReturnType<typeof createUser>;
```

가능한 사용 사례는 파라미터로 본 경우와 유사합니다. 함수를 사용하여 다른 변수를 입력할 수 있도록 함수의 반환 형식을 가져올 수 있습니다. 이것은 위의 예에서 실제로 입증되었다.

또한 `ReturnType`을 사용하여 한 함수의 출력이 다른 함수의 입력과 동일한지 확인할 수 있습니다. 이는 React 구성 요소에 필요한 상태를 관리하는 사용자 지정 후크가 있는 React에서 일반적입니다.

```js
import React from 'react';

// Custom hook
function useUser() {
  const [name, setName] = React.useState('');
  const [email, setEmail] = React.useState('');
  return {
    name,
    setName,
    email,
    setEmail,
  };
}

// Custom component uses the return value of the hook
function User(props: ReturnType<typeof useUser>) {
  return (
    <>
      <div>Name: {props.name}</div>
      <div>Email: {props.email}</div>
    </>
  );
}
```

### '인스턴스 유형 or 클래스

InstanceType은 위에서 살펴본 ReturnType과 유사하다. 유일한 차이점은 `인스턴스 타입`이 클래스 생성자에서 작동한다는 것이다.

```js
class Point {
  x: number;
  y: number;
  constructor(initial: { x: number, y: number }) {
    this.x = initial.x;
    this.y = initial.y;
  }
}

// Same as `{x: number, y: number}`
type PointInstance = InstanceType<typeof Point>;
```

위의 포인트 클래스와 같은 정적 클래스에 이 기능을 사용할 필요는 없습니다. 그 이유는 다음과 같이 유형 주석 `점`을 사용할 수 있기 때문입니다.

```js
class Point {
  x: number;
  y: number;
  constructor(initial: { x: number, y: number }) {
    this.x = initial.x;
    this.y = initial.y;
  }
}

// You wouldn't do this
const verbose: InstanceType<typeof Point> = new Point({ x: 0, y: 0 });

// Because you can do this
const simple: Point = new Point({ x: 0, y: 0 });
```

그러나 TypeScript를 사용하면 동적 클래스를 만들 수도 있습니다. 예를 들어 다음 함수 `DisposibleMixin`은 클래스를 즉시 반환합니다.

```js
type Class = new (...args: any[]) => any;

// creates a class dynamically and returns it
function DisposableMixin<Base extends Class>(base: Base) {
  return class extends base {
    isDisposed: boolean = false;
    dispose() {
      this.isDisposed = true;
    }
  };
}
```

이제 `InstanceType`을 사용하여 `DispossiblePoint`를 호출하여 생성된 인스턴스 유형을 가져올 수 있습니다.

```js
type Class = new (...args: any[]) => any;

function DisposableMixin<Base extends Class>(base: Base) {
  return class extends base {
    isDisposed: boolean = false;
    dispose() {
      this.isDisposed = true;
    }
  };
}

// dynamically created class
const DisposiblePoint = DisposableMixin(class {
  x = 0;
  y = 0;
});

// Same as `{isDisposed, dispose, x, y}`
let example: InstanceType<typeof DisposiblePoint>;
```

## 결론

앞서 살펴본 바와 같이 TypeScript와 함께 제공되는 유틸리티 유형이 많이 있습니다. 이러한 정의 중 대부분은 직접 작성할 수 있는 간단한 정의입니다. 예를 들어, null을 제외시키고 정의되지 않은 경우 다음을 쉽게 직접 작성할 수 있습니다.

```undefined
// Your custom creation
type NoEmpty<T> = T extends null | undefined ? never : T;

// Usage 
type StrOrNull = string | null;

// string - People need to look at `NoEmpty` to understand what it means
type Str = NoEmpty<StrOrNull>; 
```

그러나 기본 제공 버전인 NonNullable(동일한 기능)을 사용하면 코드 가독성이 향상되어 TypeScript의 표준 라이브러리에 익숙한 사람들이 본문 T extenses null | defined ? never : T;를 구문 분석할 필요가 없습니다.

이는 아래에 설명되어 있습니다.

```undefined
// No need for creating something custom

// Usage 
type StrOrNull = string | null;

// string - People that know TS know what `NonNullable` does
type Str = NonNullable<StrOrNull>; 
```

궁극적으로는 사용자 지정 유형을 만들지 말고 가능하면 읽기 전용 / 부분 / 필수와 같은 기본 제공 유형을 사용해야 합니다. 코드 작성 시간을 절약할 뿐만 아니라, TypeScript 팀에서 지정한 유틸리티 이름을 지정할 필요가 없습니다.