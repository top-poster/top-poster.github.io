---
layout: post
title: "MobX 6.0의 새로운 기능"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/whatsnewinmobx6.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/whatsnewinmobx6.png?fit=730%2C487&ssl=1)

리액션에서 일반적으로 사용되는 라이브러리를 잘 알려면 많은 요구사항이 필요합니다. 특히 국정운영도서관의 경우 도서관이 구축되는 핵심 개념들이 개발자들의 프레임워크 채택 여부에 총체적인 차이를 가져올 수 있다.

MobX가 커뮤니티에 채택되어 많은 다른 프로젝트에 사용된 이유 중 하나는 MobX의 철학 때문이다. 매우 간단하고, 단순하고, 강력하며, 의견이 없는 라이브러리인 MobX는 시간이 지날수록 점점 더 좋아지고 있습니다. 거의 무료에 가까운 상태 관리 라이브러리는 개발자의 삶을 편리하게 해주며, 이는 Retact의 다른 기존 상태 관리 라이브러리보다 MobX의 주요 장점 중 하나입니다.

MobX는 React에서 가장 많이 사용되는 상태 관리 라이브러리 중 하나이며, 라이브러리를 더 단순하고 견고하며 확장하기 위해 몇 가지 변경 사항을 제공하는 새로운 버전을 가지고 있다. MobX는 React 앱 전용 라이브러리가 아니며 다른 자바스크립트 라이브러리 및 프레임워크에서도 사용할 수 있다.

MobX 6.0 버전의 새로운 기능에 대해 알아보고 이전 버전에서 최신 버전으로 코드를 마이그레이션하는 방법에 대해 알아보겠습니다.

## 뫼브엑스

아직 MobX를 사용해 보지 않으셨다면, MobX가 실제로 어떻게 작동하는지 염두에 두어야 할 컨셉을 보여드리겠습니다. 보다 확장성이 뛰어나고 강력하며 간편한 MobX를 제공하기 위해 Retact 애플리케이션에서 MobX를 사용하면 많은 이점이 있습니다. 의사로부터요.

> 애플리케이션 상태에서 파생될 수 있는 모든 것은 다음과 같아야 합니다. 자동으로

기본적으로 MobX에는 다음과 같은 네 가지 중요한 개념이 있습니다.

관찰자는 데이터 구조(클래스, 객체, 배열, 참조)를 추적하는 역할을 합니다. 매장의 값이 변경될 때마다 MobX는 다음을 위해 새로운 가치를 추적합니다.

```coffeescript
import { observable } from "mobx";
class Store {
  @observable counter = 0;
}
```

행동은 우리의 상태를 수정할 책임이 있다. 값을 업데이트하려면 항상 작업을 수행해야 합니다. 작업은 항상 이벤트, 버튼 클릭 또는 양식 제출 등에 따라 수행됩니다.

```coffeescript
import { observable } from "mobx";
class Store {
  @observable counter = 0;
  @action increment() {
    this.counter++;
  }

  @action decrement() {
    this.counter--;
  }
}
```

액션은 관찰 결과를 변경하고 수정하는 역할을 합니다. MobX에서는 기본적으로 작업 외부에서 상태를 업데이트할 수 없습니다. 이 규칙은 코드베이스 및 상태를 보다 예측 가능하고 버그가 없으며 견고하게 만듭니다.

계산된 값은 관측 가능한 정보를 가져오는 데 사용됩니다. 이러한 기능은 상태가 변경될 때마다 반환된 값도 함께 변경되므로 상태를 추적하는 기능입니다.

```coffeescript
import { observable } from "mobx";
class Store {
  @observable counter = 0;
  @action increment() {
    this.counter++;
  }

  @action decrement() {
    this.counter--;
  }

  @computed counterMoreThan10() {
    return this.counter > 10;
  }
}
```

반응은 계산된 값과 상당히 유사하지만, 다른 점은 부작용을 유발하고 관측 가능한 것이 변할 때 반응의 목표는 자동으로 부작용을 수행하는 것이다.

이제 MobX의 핵심 개념에 대해 조금 알게 되었으니, 6.0 버전에서 실제로 무엇이 바뀌었는지, API를 어떻게 사용하기 쉽고 강력해졌는지 알아보겠습니다.

## MobX 6.0

몇 달 전, MobX 6의 제안은 Michel Weststrate 도서관의 창안자에 의해 만들어졌다. 새로운 버전에 대한 많은 제안들 때문에, 새로운 MobX 버전에 무엇이 포함되어야 하는지, 무엇을 떨어뜨려야 하는지에 대해 많은 논의가 있었다.

새로운 MobX 6.0의 변경 사항과 새로운 기능은 다음과 같습니다.

## 장식가

Angular, NestJS, MobX와 같은 많은 프로젝트들이 자바스크립트 장식기를 사용하는 것으로 알려져 있다. 장식가들은 현재 2단계 제안 중이어서 조만간 지원이 되지 않을 것으로 보입니다.

새로운 6.0 버전에서 가장 논쟁이 되는 점 중 하나는 다음과 같은 몇 가지 이점을 얻기 위해 장식가를 중단해야 한다는 것이었다.

- 표준 최신 JavaScript와의 호환성 향상
- 대부분의 설정(예: create-react-app)에서 문제 해결
- 번들 크기 축소

MobX에는 더 이상 장식기가 필요하지 않지만, 이전 버전에서는 여전히 라이브러리를 사용할 수 있지만, 많은 개발자들은 처음에 단지 장식기를 사용한다는 이유만으로 MobX를 좋아하지 않았다.

예를 들어, 이는 이전 버전의 MobX에서 장식기를 사용한 예입니다.

```coffeescript
import { observable } from "mobx";
class Store {
  @observable counter = 0;
  @action increment() {
    this.counter++;
  }

  @action decrement() {
    this.counter--;
  }

  @computed counterMoreThan10() {
    return this.counter > 10;
  }
}
```

다음은 6.0 버전의 MobX의 예입니다.

```js
import {
  observable,
  action,
  computed,
  makeObservable
} from "mobx";
class Store {
  counter = 0;
  constructor() {
    makeObservable(this, {
      counter: observable,
      increment: action,
      decrement: action,
      counterMoreThan10: computed
    });
  }
  increment() {
    this.counter++;
  }
  decrement() {
    this.counter--;
  }
  get counterMoreThan10() {
    return this.counter > 10;
  }
}
```

일부 개발자의 경우, 장식기 없는 구문이 더 깨끗하고 간단하며, 다른 개발자의 경우 개발자의 경험도 더 나빠졌습니다.

사실 `관찰 가능한` 유틸리티 기능은 통합이 쉬워져 더 이상 장식기 때문에 많은 다른 패키지를 다운로드하고 사용할 필요가 없어졌다.

make Observable 유틸리티 함수는 `composer` 방식으로 포장되어야 하며, 이를 관측할 수 있도록 국가의 각 속성에 대해 다음 주석을 사용하여 지정해야 한다.

```js
constructor() {
  makeObservable(this, {
    counter: observable,
    increment: action,
    decrement: action,
    counterMoreThan10: computed
  });
}
```

여전히 MobX에서 장식기를 사용할 수 있지만, 우리는 클래스에 생성자를 추가하고 관찰 가능(make Observable) 유틸리티 함수를 사용해야 하며 다음 두 번째 인수를 생략할 수 있다.

```java
class Store {
  @observable counter = 0;
  constructor() {
    makeObservable(this);
  }
  @action increment() {
    this.counter++;
  }
  @action decrement() {
    this.counter--;
  }
  @computed get counterMoreThan10() {
    return this.counter > 10;
  }
}
```

## 자동 관찰 가능

6.0 버전에서 소개된 `make`Auto Observable 유틸리티 기능은 make Observable 유틸리티 기능보다 훨씬 강력합니다.

특정 주석으로 재정의할 수 있지만 기본적으로 모든 속성을 관찰할 수 있습니다.

```undefined
class Store {
  counter = 0;
  constructor() {
    makeAutoObservable(this);
  }
  increment() {
    this.counter++;
  }
  decrement() {
    this.counter--;
  }
  get counterMoreThan10() {
    return this.counter > 10;
  }
}
```

메이크Auto Observable은 MobX의 새 버전에서 가장 중요하고 놀라운 변화 중 하나이다. 이를 통해 개발자는 상점 내부에 있을 수 있는 각각의 관측 가능한 값, 작업 및 계산된 값을 언급할 필요가 없어집니다.

## 프록시

MobX에는 몇 가지 구성 방법이 있으며, 일부 JavaScript 엔진에서 사용하기 위해서는 몇 가지 작업을 수행해야 합니다.

MobX 5.0에서는 프록시에 대한 지원이 필요했고, 이 요구 조건은 일부 자바스크립트 엔진, 특히 Internet Explorer와 React Native(엔진에 따라 다름)에 대한 지원 문제를 야기했다.

후드 아래에서 MobX는 프록시를 사용하여 어레이와 일반 개체를 관찰할 수 있도록 합니다. 문제는 프록시를 지원하지 않는 일부 JavaScript 엔진에 몇 가지 문제가 발생할 수 있다는 것입니다. MobX 6.0에서는 프록시가 계속 지원되고 요구되지만 이제 `configure` 기능을 사용하여 프록시를 사용하지 않도록 설정하는 방법이 있습니다.

```coffeescript
import { configure } from "mobx";
configure({     
  useProxies: "never"
})
```

## 결론

수년 동안 도서관을 유지하고 계속 업데이트하는 것은 쉬운 일이 아니며, MobX 커뮤니티는 그 공로를 인정받을 만하다. 라이브러리는 점점 더 좋아지고 있으며, 초기부터 "응용프로그램 상태에서 파생될 수 있는 모든 것은 파생되어야 한다. 자동으로".