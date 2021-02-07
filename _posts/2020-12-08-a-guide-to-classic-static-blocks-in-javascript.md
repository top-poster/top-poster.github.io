---
layout: post
title: "JavaScript의 기존 정적 블록에 대한 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/A_guide_to_classic_static_blocks_in_JavaScript.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/A_guide_to_classic_static_blocks_in_JavaScript.png?fit=730%2C487&ssl=1)

## 도입

JavaScript의 `클래스 정적 블록`을 사용하면 클래스 정의를 평가하는 동안 정적 초기화를 추가로 수행할 수 있습니다. 그러나 클래스 정적 블록은 정적 필드를 대체하기 위한 것이 아니라 정적 필드를 사용하여 달성할 수 없는 새로운 사용 사례를 제공하기 위한 2단계 제안에 여전히 포함되어 있다. 따라서 클래스 정적 블록은 객체 지향 프로그래밍(OOP) 자바스크립트를 훨씬 더 흥미롭고 강력하게 만든다.

자바나 C# 등 고전 상속을 사용하는 프로그래밍 언어는 이미 이런 구현이 있다. 자바에서는 정적 이니셜라이저를, C#에서는 정적 생성자를 의미한다.

자바스크립트의 OOP는 이러한 언어들과 달리 프로토타입 상속을 사용한다. 일반적으로 이와 같은 기능이 있으면 안 됩니다. 그러나 2015년 Ecmascript(es6) 수업의 등장으로 고전 상속에서 볼 수 있는 것과 유사한 기능이 구현되었다. 정적(static method)과 확장(extends) 등 일부는 이미 구현됐다. 그리고 이제는 정적 필드 프라이빗 필드 클래스 정적 블록 등 실험적인 기능이 더욱 많아졌다.

이러한 OOP JavaScript의 모든 위대한 발전에도 불구하고, 자바스크립트는 여전히 프로토타입 상속을 사용한다는 점을 유념하는 것이 중요하다. 따라서 이들 중 상당수는 합성 설탕에 불과하다.

> 합성 슈가(Syntatic sugar)는 오래된 연산을 수행하기 위해 시각적으로 호소력 있는 새로운 구문(종종 지름길)을 가리킨다. – 위키백과

다음 절에서 JavaScript에서 `class static blocks`의 구문과 의미론을 살펴보겠습니다.

## 클래스 정적 블록의 구문 및 의미론

#### 구문

다음은 제안된 구문입니다.

```cpp
class NewClass {
  static {
     // do something 
  }
}
```

#### 의미론

반품 내역서 없음:

```cpp
class NewClass {
  static {
    return // syntax error
  }
}
```

클래스 정의에는 `정적 블록 {}이(가) 하나만 있어야 합니다.

```cpp
class NewClass {
  static {}
  static {} // throws and error.
}
```

정적 블록 {}은(는) 클래스의 범위 내에 중첩된 새 변수 환경을 생성합니다.

```js
var age = 23
class NewClass {
  static {
      var age; // can still use age inside the static block 
              //because it creates as new lexical scope
      var name = "Lawrence Eagles"
  }
}

console.log(name) // reference error. 
                  // cannot access name inside the static block's lexical scope.
```

위의 코드에서 우리는 `변종`이 클래스와 동일한 범위로 선언되었지만, 여전히 `클래스 정적` 내부에 새로운 `나이` 변수를 생성한다는 것을 알 수 있다. 이는 `class static block {}`에 고유한 변수 환경이 있기 때문입니다.

그러나 로컬 범위 밖에 있는 클래스 정적 블록 {} 내에서 초기화된 var name(var name) 변수에는 액세스할 수 없습니다.

정적 블록 {}에는 장식기가 없어야 합니다. 아래와 같이 클래스 자체를 장식해야 합니다.

```java
@decorator // ok
class NewClass {
  @decorator // error. not allowed
  static {
  }
}
```

평가할 때 정적 블록 {}의 this 변수는 클래스의 생성자 함수를 가리킵니다.

여기서 의미론에 대해 더 자세히 알 수 있습니다.

## 정적 블록의 사례 사용

앞서 언급했듯이 정적 블록은 정적 필드나 정적 개인 필드의 대체물이 아니다.

그러나 아래 참조와 같이 더 많은 사용 사례를 가능하게 하기 위한 것입니다.

클래스 초기화 중 `문` 평가:

```cpp
class NewClass {
  static square = {L: 8, B: 6};
  static y;
  static z;
  // wrong code would throw an error
  try {
      // do something here
    }catch (error) {
      // handle error here
  }
}
```

위의 코드는 오류를 발생시킵니다. 우리는 수업 초기화 중에 `try…catch`라는 문장을 평가할 수 없다. try…catch라는 말은 반드시 학급의 선언 밖으로 옮겨져야 한다.

그러나 진술을 평가해야 할 경우(예: `시도`)catch)는 다음과 같이 클래스 초기화 시 `static block`static block`

```cpp
class NewClass {
  static square = {L: 8, B: 6};
  static y;
  static z;
  static {
    try {
      // do something here
    }catch (error) {
      // handle error here
    }
  }
}
```

아래 나온 것처럼 단일 값에서 두 개의 필드를 설정해야 하는 경우:

```cpp
class NewClass {
  static square = {L: 8, B: 6};
  static y;
  static z;
  NewClass.y = square.L // throws an error
  NewClass.z = square.B // throws an error
}
```

그러나 아래와 같이 정적 블록을 사용하여 값을 설정할 수 있습니다.

```cpp
class NewClass {
  static square = {L: 8, B: 6};
  static y;
  static z;
  static {
    NewClass.y = square.L // correct
    NewClass.z = square.B // correct
  }
}
```

`인스턴스 개인 필드`가 있는 클래스와 동일한 범위에서 선언된 다른 클래스 또는 함수 간에 정보 공유가 필요한 경우:

```coffeescript
let getName;
export class NewClass {
  #name
  constructor(devName) {
    this.#name = { data: devName };
  }
  static {
    // getName has privileged access to the private state (#name)
    getName = (obj) => obj.#name;
  }
}

export function getNameData(obj) {
  return getName(obj).data;
}
```

위에서 우리는 정적 블록이 (인스턴스) 또는 정적(static) 개인 상태에 대한 특권 액세스로 현재 클래스 선언의 맥락에서 문을 평가할 수 있음을 알 수 있다.

getName 기능은 `class static block {}에서 평가되지만 여전히 클래스의 `private state` 이름에 대한 권한을 가집니다.

여기서 `class static block {}의 사용 가능성에 대해 자세히 알아볼 수 있습니다.

## 결론

자바스크립트의 개발은 특히 OOP 자바스크립트에서 지속적으로 발전해왔다. 자바스크립트가 프로토타입상속 구현을 유지하고 있지만 많은 새로운 기능과 제안된 기능은 고전상속과 유사하다.

`클래스 정적 블록 {}도 다르지 않습니다. 이러한 발전은 이제 "원형 상속"이 자바스크립트 채택을 억제한다고 생각하는 더 넓은 범위의 개발자들에게 어필하기 때문에 언어에 도움이 된다.

마지막으로 클래스 정적 블록 {}은(는) OOP JavaScript에 강력한 추가 기능이지만 2단계 제안 기능입니다.