---
layout: post
title: "JavaScript 클래스의 블리딩 에지"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/javascript-logo.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/javascript-logo.png?fit=730%2C487&ssl=1)

이 기사에서는 JavaScript 클래스의 출혈 가장자리를 살펴보겠습니다.

JavaScript 클래스는 특수한 유형의 함수입니다. 그러나 자바스크립트 클래스는 키워드로 선언되고 식 구문으로 초기화된다는 점에서 일반적인 기능과 유사합니다.

JavaScript에는 원래 클래스가 없습니다. 클래스는 자바스크립트의 새롭고 개선된 버전(ECMAS스크립트 5가 이전 버전임)인 ECMAS스크립트 6(es6)의 도입과 함께 추가되었다.

일반적인 JavaScript 클래스는 기본 생성자 메서드를 가진 개체입니다. JavaScript 클래스는 프로토타입에 기반하지만 표현 구문을 사용합니다.

클래스가 존재하기 전에는 프로토타입이 자바스크립트에서 클래스를 에뮬레이트하는 데 사용되었다. 프로토타입은 모든 JavaScript 기능과 개체에 연결된 기본 개체입니다. 추가 속성을 프로토타입에 연결할 수 있으므로 JavaScript 클래스를 에뮬레이트할 수 있습니다.

이에 대한 이해를 높이기 위해 나이와 이름의 두 가지 매개 변수가 있는 `카`라는 함수를 선언해 봅시다.

```js
function Car(){
    this.name = 'dragon';
    this.age = 3;
}
```

아래 코드 블록과 같이 프로토타입을 사용하여 추가 속성을 추가할 수 있습니다.

```bash
Car.prototype.color = 'white'
```

다음으로, 새 자동차 인스턴스를 만들어 보겠습니다.

```undefined
let Car1 = new Car()
```

이제 새로 추가된 속성을 콘솔에 기록합니다.

```css
console.log(Car1.color)
```

이 경우 자바스크립트 기능 카는 이름, 나이, 색상 등 세 가지 특성을 가진 클래스 역할을 한다. 즉, 자바스크립트는 프로토타입과 함께 제공되는 상속을 사용하여 클래스를 에뮬레이트합니다.

## ES6 클래스

자바스크립트에 클래스가 도입됨에 따라 ES6는 다른 객체 지향 프로그래밍 언어와 유사한 구문을 사용하여 훨씬 더 간결한 클래스 선언 방식을 제공하였다. ES5의 클래스 접근 방식과 달리, ES6는 후드 아래에서 클래스로 작업할 때 기능 키워드가 필요하지 않다. JavaScript는 여전히 클래스를 특수 유형의 함수로 간주합니다.

클래스와 함수의 주요 차이점은 호이스트(hoisting)이며, 함수와는 달리 자바스크립트 클래스는 액세스 전에 선언되어야 한다. 그렇지 않으면 오류가 발생합니다.

### 클래스 키워드

JavaScript는 클래스를 정의하는 기본 방법인 클래스 키워드를 제공합니다. 기존 프로토타입 상속 패턴에 대한 통사당 역할을 한다.

```coffeescript
//javascript class declaration
class Car  {
   //methods
}
```

위와 같이 클래스 키워드는 JavaScript 클래스가 정의되고 있음을 지정하는 데 사용됩니다.

더 많은 유연성을 위해 클래스 표현을 사용함으로써 클래스를 정의할 때 위와 다른 접근 방식을 따를 수 있습니다. 이렇게 하면 클래스의 이름을 지정하거나 이름을 지정할 수 있습니다.

```js
//unnamed javascript class expression
let Car = class {
    constructor(name, age){
        this.name = name
        this.age = age
    }
}
```

다음은 이름이 지정된 JavaScript 클래스 식의 예입니다.

```js
//named javascript class expression
let Car = class Car {
    constructor(name, age){
        this.name = name
        this.age = age
    }
}
```

### 생성자법

생성자 메서드는 클래스 개체를 초기화하는 데 사용되는 JavaScript의 특수 메서드입니다. 클래스가 시작되면 JavaScript에서 자동으로 호출됩니다. JavaScript 클래스에는 생성자 메서드가 하나만 있을 수 있습니다.

정의되지 않은 경우 JavaScript는 0 매개 변수가 있는 빈 생성자를 해당 클래스에 추가합니다.

다음은 생성자 메서드가 있는 클래스의 예입니다.

```js
//javascript class with a constructor
 class Car {
    constructor(name, age){
        this.name = name
        this.age = age
    }
}
```

위의 클래스에는 이름과 연령의 두 가지 매개 변수가 있는 생성자가 있습니다.

### 정적법

정적 메서드는 클래스의 인스턴스가 아닌 클래스 자체에서 호출되는 메서드입니다. 정적 메서드는 클래스의 인스턴스는 아니지만 기능 면에서 클래스와 관련이 있습니다.

다음은 정적 방법의 일반적인 예입니다.

```cpp
class Math {
    static add(number1 , number2){
        return number1 + number2
    }
}
```

그런 다음 아래와 같이 위의 정적 방법을 호출할 수 있습니다.

```undefined
let additionResult = Math.add(2,3)
```

정적 메서드 추가는 추가하는 것이 무엇을 의미하는지 보여줍니다.

### ES6 클래스 구문

일반적인 JavaScript 클래스에는 다음과 같은 구문이 있습니다.

```js
class Car {
    constructor(){
        //default operation
    }
    method(){
        //operation2
    }

}
```

## ES6 클래스 관련 문제

클래스는 JavaScript에서 특정 작업을 수행하는 간단한 방법에 대해 보다 복잡한 솔루션을 제공할 수 있습니다. 객체 지향 프로그래밍 언어에 대한 배경을 가진 사람들은 필요하지 않은 경우에도 클래스와 함께 간단한 작업을 수행하기로 결정할 수 있습니다.

일부 개발자들은 클래스를 추가하는 것이 자바스크립트의 독창성을 앗아갔고, 프로토타입을 사용하는 것이 클래스를 필요로 하는 작업을 수행하는 더 유연한 방법이었다고 주장할 수 있다. 다른 객체 지향 프로그래밍 언어의 클래스와 달리 자바스크립트는 개인 변수 선언과 같은 기본 클래스 기능을 제공하지 않기 때문이다.

ECMASCRIPT 2020은 이러한 문제 중 일부를 해결하는 것을 목표로 한다.

## ECMAScript 2020 클래스 추가

매년 JavaScript 사용자의 요구를 충족하기 위해 JavaScript에 추가 및 수정이 이루어집니다. 최신 수정사항은 ECMASCRIPT 2020에 있다. 2020년도의 클래스에 추가된 일부 항목에는 개인 클래스 변수와 정적 필드가 포함됩니다.

개인 클래스 변수: 변수 선언이 많은 대규모 코드베이스에서 작업할 때 클래스 내에서만 액세스할 수 있는 변수가 필요할 수 있습니다. 개인 클래스 변수가 이 문제를 해결합니다. 변수 이름 앞에 해시를 추가하면 개인 변수를 쉽게 만들 수 있습니다.

```cpp
 class Detail {
      #name = "steve"
     welcome(){
        console.log(this.#message)
      }
 }

 const detail_1 = new Detail()
   detail_1.welcome() 
```

위의 변수 `#name`은 `Detail` 클래스 내에서만 액세스할 수 있습니다.

정적 필드: 정적 필드에 대한 이해를 위해 아래 코드 조각에 대해 살펴보겠습니다.

```coffeescript
class Detail {
     #name = "steven"

     welcome(){
         console.log(this.#message)
     }
 }
```

이전 버전의 JavaScript에서는 새 클래스 인스턴스를 만들지 않고 `welcome` 메서드에 액세스하려는 시도가 불가능해 보입니다. 그러나 최신 추가 기능을 사용하면 인스턴스를 만들 필요 없이 이러한 방법에 액세스할 수 있습니다.

위 방법은 아래와 같이 액세스할 수 있습니다.

```cpp
 const DetailMethod = Detail.welcome()
```

## 결론

JavaScript 클래스는 프로토타입을 사용할 때 발생하는 몇 가지 문제를 해결합니다. 그들은 학급의 선언을 더 직접적으로 그리고 더 쉽게 만든다. 최신 ECMASCRIPT 2020은 더 많은 유연성을 추가함으로써 클래스를 더욱 쉽게 사용할 수 있도록 합니다.