---
layout: post
title: "반응에서 방사능 상태 시작"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/gettingstartedwithreadioactivestate.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/gettingstartedwithreadioactivestate.png?fit=730%2C487&ssl=1)

방사능 상태는 매우 반응적인 상태이다. 음영 또는 깊이가 변이되면 자동으로 렌더 업데이트를 트리거합니다. 이를 통해 상태 설정 필요성, 새로운 상태 생성의 번거로움, `스텔스 상태`를 다루는 불편함이 사라진다.

이용국 훅으로 복잡한 국가를 관리하려는 것은 상당한 노력이다. 이것은 리액트 커뮤니티에서 안티패턴으로 여겨진다.

또한 useState Hook은 새로운 상태가 설정된 후 "freshstate"에 즉시 액세스할 수 있는 규정을 제공하지 않는다. 그리고 상태가 렌더 업데이트 후에만 업데이트되기 때문에 우리는 여전히 `stale state`에 대처해야 한다.

우리는 방사능 상태를 통해 이러한 과제를 제거하고 성능을 향상시키며 새로운 기능을 즐깁니다.

## 시작 중

방사선 상태 사용을 시작하려면 다음 명령을 실행하여 패키지를 설치하십시오.

```undefined
npm i radioactive-state or yarn add radioactive-state
```

이를 통해 구성 요소에 방사능 상태를 생성할 수 있는 useRS Hook을 사용할 수 있습니다. 우리는 객체를 인수로 받아들이는 useRS Hook을 사용하여 상태를 초기화한다. 이 개체의 각 속성은 서로 다른 상태를 나타내며 상태를 설정할 필요 없이 음소거할 수 있습니다.

```cpp
const state = useRS({ count: 0 }); // initializes the state
```

이 상태를 음소거하려면 아래 구문을 사용합니다.

```js
const increment = () => state.count++; // increases the count
const decrement = () => state.count--; // decreates the count
```

### 카운터 구성 요소

```js
import "./styles.css";
import React from "react";
import useRS from "radioactive-state";
export default function App() {
  const state = useRS({ count: 0 });
  const increment = () => state.count++;
  const decrement = () => state.count--;
  return (
    <div className="app container d-flex flex-column justify-content-center align-items-center">
      <div className="mb-4">
        <span className="count">{state.count}</span>
      </div>
      <article className="d-flex">
        <button className="mx-2 btn btn-success btn-sm" onClick={increment}>
          increment count
        </button>
        <button className="mx-2 btn btn-danger btn-sm" onClick={decrement}>
          increment count
        </button>
      </article>
    </div>
  );
}
```

위는 버튼을 클릭할 때 카운트 값을 증가시키거나 감소시키는 간단한 카운터 구성요소입니다. 여기서 코드를 가지고 놀 수 있습니다.

이 간결한 정교함에 따라, 우리는 방사능 상태의 표면을 거의 긁어내지 못했다.

## 특징들

### useState와 달리 항상 새로운 상태

useState 후크를 사용하여 상태를 관리하는 경우. 렌더링 업데이트 후에만 구성 요소가 새로 고쳐집니다. 이로 인해 디버깅하기 어려운 일부 악성 버그가 발생할 수 있습니다. 몇 가지 예를 살펴보겠습니다.

```js
import React, { useState } from "react";
import "./styles.css";
const App = () => {
  const [count, setCount] = useState(0);
  const increment = () => {
    console.log("before: ", count);
    setCount(count + 1);
    console.log("after: ", count);
  };
  return (
    <div className="App">
      <div className="app
           container
           d-flex
           flex-column
           justify-content-center
           align-items-center"
      >
        <div className="mb-5">
          <span className="count">{count}</span>
        </div>
        <article className="d-flex">
          <button
            className="mx-2 p-3 btn btn-success btn-sm"
            onClick={increment}
          >
            increment count
          </button>
        </article>
      </div>
    </div>
  );
};
export default App;
```

위는 `count state`와 `count state`를 업데이트하기 위해 후드 아래의 `useState` setter 함수를 호출하는 증분 함수를 가진 단순한 구성 요소이다. 카운트의 현재 값이 UI에 표시됩니다.

카운트를 늘리면 UI에 반영되지만 콘솔에 기록된 카운트는 여전히 0입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/incrementcounter.png?resize=730%2C441&ssl=1)

콘솔에 기록된 카운트 값은 항상 `UI`의 값보다 1만큼 작습니다. 여기서 코드를 가지고 놀 수 있습니다.

`방사능 상태`에서는 이 문제가 발생하지 않습니다.

```js
import "./styles.css";
import React from "react";
import useRS from "radioactive-state";
const App = () => {
  const state = useRS({ count: 0 });
  const increment = () => {
    console.log("before: ", state.count);
    state.count++;
    console.log("after: ", state.count);
  };
  return (
    <div className="app
          container
          d-flex
          flex-column
          justify-content-center
          align-items-center"
    >
      <div className="mb-5">
        <span className="count">{state.count}</span>
      </div>
      <article className="d-flex">
        <button className="mx-2 p-3 btn btn-success btn-sm" onClick={increment}>
          increment count
        </button>
      </article>
    </div>
  );
};
export default App;
```

위는 동일한 앱이지만 useRS 후크를 사용하는 앱 구현이다. 이 문제는 방사능 상태 때문에 경험하지 못하고 있다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/beforeandafterincreementcounter.png?resize=730%2C440&ssl=1)

여기서 코드를 가지고 놀 수 있습니다. 위의 이미지에서 카운트 상태의 증가 전후의 값은 각각 0과 1임을 알 수 있다. 이것은 방사능 상태의 반응성 때문이다.

### 어떤 수준에서든 상태를 직접 변형하여 구성 요소를 업데이트합니다.

여기서 `방사능 상태`가 리액션의 `스텔스 상태` 문제를 어떻게 해결하는지 살펴볼 것이다.

```js
import "./styles.css";
import React, { useState } from "react";
export default function Example() {
  const [count, setCount] = useState(0);
  const lazyIncrement = () => {
    setTimeout(() => {
      setCount(count + 1);
    }, 3000);
  };
  return (
    <div className="app
          container
          d-flex
          flex-column
          justify-content-center
          align-items-center"
    >
      <div className="mb-5">
        <span className="count">{count}</span>
      </div>
      <article className="d-flex flex-column">
        <button
          className="mx-2 p-3 btn btn-success btn-sm"
          onClick={lazyIncrement}
        >
          increment count
        </button>
        <small className="m-2">
          <strong>Increment the count a number of times!</strong>
        </small>
      </article>
    </div>
  );
}
```

여기서 코드를 가지고 놀 수 있습니다. 위는 useState Hook을 사용하여 count state를 관리하는 React 구성 요소입니다. 개수 표시 버튼을 클릭하면 "게으름"이 표시됩니다.상태를 업데이트하기 위해 Increment` 함수를 호출합니다. 그러나 useState의 setter 함수(setCount)는 setTimeout 함수 때문에 3000밀리초 후에 호출된다. 결과적으로, 상태는 `3000밀리초`가 지난 후에야 업데이트된다.

버튼을 n번 클릭하여 상태를 증가시키면 상태가 한 번만 구현된다는 것을 알 수 있습니다. 셋카운트가 계속 구주(舊州)와 통화되기 때문이다. 구성 요소가 다시 렌더링될 때만 새 상태가 됩니다.

이 문제를 완화하기 위해 업데이트 프로그램 기능을 `useState` setter 함수에 전달하는 경우가 많다. 이 경우 이전 상태(prevState)를 매개 변수로 사용하여 다음 상태의 값을 계산합니다. 따라서 위의 문제는 아래 코드로 해결할 수 있습니다.

```js
setCount(prevCount => prevCount++)
```

그러나 새 상태 값을 기준으로 다른 상태를 업데이트하려는 경우 이 문제가 발생합니다.

이 `스텔스 상태` 문제를 해결하기 위한 보다 깔끔한 접근 방식은 `방사능 상태`를 사용하는 것이다. 따라서 진정한 사후 대응 상태를 제공하므로 다음과 같이 구성 요소를 다시 구현할 수 있습니다.

```js
import "./styles.css";
import React from "react";
import useRS from "radioactive-state";
export default function Example() {
  const count = useRS({ value: 0 });
  const lazyIncrement = () => {
    setTimeout(() => {
      count.value++;
    }, 3000);
  };
  return (
    <div className="app
          container
          d-flex
          flex-column
          justify-content-center
          align-items-center"
    >
      <div className="mb-5">
        <span className="count">{count.value}</span>
      </div>
      <article className="d-flex flex-column">
        <button
          className="mx-2 p-3 btn btn-success btn-sm"
          onClick={lazyIncrement}
        >
          increment count
        </button>
        <small className="m-2">
          <strong>Increment the count a number of times!</strong>
        </small>
      </article>
    </div>
  );
}
```

이제 정말로 반응성이 높은 새 상태를 갖게 되었기 때문에 새 상태의 올바른 값을 계산할 수 있습니다. 3000밀리초 후에 상태가 업데이트된다 하더라도 말이다. 여기서 코드를 가지고 놀 수 있습니다.

### 입력에 대한 반응성 바인딩

반응은 좋지만 폼 처리와 관련하여 많은 개발자들이 타사 라이브러리를 사용하는 것을 선호합니다. 반응 양식은 대개 제어된 구성 요소의 구성입니다. 이것들과 함께 일하는 것은 가치와 오류를 추적하는 것과 같은 많은 반복적이고 짜증나는 것들을 포함합니다.

다음과 같은 제어 구성 요소를 생성할 수 있습니다.

```js
const [email, setEmail] = useState("");
  return (
    <div className="App">
      <form>
        <label>Email:</label>
        <input
          value={email}
          placeholder="Enter Email"
          onChange={(e) => setEmail(e.target.value)}
          type="text"
        />
      </form>
    </div>
```

`e.target.value`를 사용하여 값을 추적해야 합니다. 만약 우리가 체크박스를 가지고 작업한다면 이것은 `target`이 될 것이다.체크했다. 체크박스, 레인지, 라디오 등과 같은 입력이 서로 다르면 더욱 어려워진다.

방사선 상태는 입력 값을 상태의 키에 바인딩하는 바인딩 API를 제공합니다. 이 기능은 다음과 같은 es6 스프레드 연산자를 사용합니다.

```undefined
<input {...state.$key}  />
```

우리는 단순히 키에 `$`를 접두사로 붙이고 위와 같이 액세스한다.

바인딩 API는 각 상태의 초기 값을 속성 상태로 사용하여 입력 유형을 결정합니다. 결과적으로 `상태`입니다.$key`는 다음을 반환합니다.

- `값` 및 `변경 시`를 포함하는 개체, 초기 값이 유형이면 `문자열` 또는 `숫자`입니다.

초기 값 유형이 `number`인 경우 `onChange` 함수는 `e.target.value`를 `string`에서 `number`로 변환한 후 키에 저장합니다.

- Checked와 On Change 소품이 들어 있는 개체입니다. `목표`를 사용합니다.초기 값 유형이 boolean이면 "state"인 경우 "on Change" 함수에서 내부적으로 체크했다.$key

구현 방식은 다음과 같습니다.

```php
const state = useRS({
  name: "iPhone 8",
  desc: "The mobile phone",
  promoPrice: 500,
  price: 1000,
  sold: true,
  color: "red"
});
const { $promoPrice, $price, $name, $sold, $color } = state;
```

바인딩은 다음과 같이 사용됩니다.

```bash
<input className="form-control" {...$name} type="text" />
```

저는 이러한 바인딩을 사용하여 반응성이 높은 상태의 제품 필터 구성 요소를 구축했습니다. 여기서 코드를 가지고 놀 수 있습니다.

### 추가 리렌더 없음 – 자동 변이 배치

여기서 주목해야 할 점은 `반응 상태`를 사용할 때 후크에게 객체를 전달해야 한다는 것이다.

#### 맞아요.

```cpp
const state = useRS({count: 0})
```

#### 틀렸어요!

```cpp
const state = useRS(0)
```

또한 아래의 기능을 고려하십시오.

```js
const customFunc = () => {
  state.name = "Lawrence";
  state.teams.frontend.react.push(["Lawrence", "Dave"]);
  state.commits++;
  state.teams.splice(10, 1);
  state.tasks = state.tasks.filter(x => x.completed);
};
```

위의 기능이 호출되면 여러 렌더 업데이트를 트리거할 것인지 여부가 결정됩니다. 하지만 그럴 리 없어요. 그리고 이것은 `반응 상태`에서 돌연변이가 단일 돌연변이로 결합되기 때문이다. 결과적으로, 국가가 아무리 많이 변형을 당하더라도, 그것은 단지 재반환에 대한 트리거가 될 것이다.

자세한 내용은 여기에서 확인할 수 있습니다.

### 반응형 소품

이것이 아마 방사능 상태의 가장 흥미로운 특징일 것이다. 반응에는 일방적인 데이터 흐름이 있습니다. 즉, 데이터가 상위 구성 요소에서 하위 구성 요소로 아래로 흐릅니다. 이 데이터(`prop`)는 일반적으로 불변하므로 변경하더라도 상위 구성요소에 영향을 미치지 않습니다.

상위 구성요소는 다음과 같이 하위 구성요소에 상태를 소품으로 전달할 수 있습니다.

```js
export default function App() {
  const [bulbSwitch, setBulbSwitch] = useState(false);

  return (
    <div className="App">
      <Bulb bulbSwitch={bulbSwitch} />
    </div>
  );
}
```

하위 구성 요소에서 `bulbSwitch` 상태를 음소거해도 상위 구성 요소에서 렌더 업데이트가 트리거되지 않습니다.

그러나 `적극적 상태`는 상황을 변화시킨다. 하위 구성 요소를 사용하면 상태를 변경하여 상위 구성 요소의 렌더링을 트리거할 수 있습니다. 또한 이 기능은 매우 강력한 기능으로 구성 요소를 메모할 필요가 없습니다. 여기서 자세히 알아보십시오.

### 방사능 상태가 빠르게 상승하고 있습니다.

물론, 우리가 `반응 상태`를 사용하는 것의 성능 함축에 대해 이야기하지 않는다면 우리의 담론은 완전하지 않다.
`적극적 상태`는 빠르다. 빨리 타오르네. 사용국 후크보다 25% 빠르며 이는 상당히 복잡한 앱용이다. 상태가 복잡해짐에 따라 사용 상태 후크를 지속적으로 능가한다.

> 이 숫자는 200개 객체의 배열이 렌더링되고 추가, 제거, 재주문 및 돌연변이와 같은 다양한 작업이 차례로 수행된 평균 100개의 성능 테스트에서 파생되었습니다. - 재활성화 상태 문서

useState Hook을 사용하면 상태를 업데이트할 때마다 새 상태가 생성됩니다. 그리고 상태를 업데이트하기 위해 setter function을 이 새로운 상태로 호출한다. 상태가 업데이트될 때마다 `방사성 상태`가 새 상태를 만들지 않습니다. 이것이 이용국가를 능가하는 주요 원인 중 하나다.

또한, 방사능 상태는 `JavaScript 프록시`를 사용하여 상태를 재귀적으로 프록시화함으로써 매우 반응성이 높은 상태를 생성한다. 여기서 더 자세히 알아보세요.

## 피해야 할 돌연변이 함정

방사능 상태는 놀랍지만, 그것을 사용하는 동안 피해야 할 함정들이 있다. 우리는 이 섹션에서 이러한 패턴을 피하기 위해 고려할 것입니다.

### 고가의 초기 상태 처리

초기 상태가 비용이 많이 들거나 장시간 실행되는 계산에서 얻은 경우, 아래와 같이 상태를 초기화하는 것은 비효율적일 수 있습니다.

```cpp
const state = useRS({
  x: getData(); // if getData is an expensive operation.
})
```

위의 패턴은 구성 요소가 렌더링될 때마다 `getData` 함수를 트리거합니다. 이것은 우리가 원하는 것이 아니다. 이건 안티패턴이에요. 올바른 접근 방식은 다음과 같습니다.

```cpp
const state = useRS({
  x: getData;
})
```

이제 이것은 국가를 초기화하기 위해 getData 기능을 한 번만 호출할 뿐, 결과적으로 훨씬 더 효율적이다.

### 돌연변이 기

아래 코드를 고려하십시오.

```coffeescript
const state = useRS({
  users: []
})

useEffect( () => {
  // do something ...
}, [state.users])


const addUser = (user) => state.users.push(user)
```

위는 이 문제를 설명하기 위한 작은 기여 사례입니다. addUser 기능을 호출하면 새 사용자가 상태의 사용자 배열에 추가됩니다.

그러나 use Effect 후크는 실행되지 않았다. 사용자 상태는 `AddUser` 함수를 호출하여 변이됩니다.

그 이유는 참조 유형 데이터(예: 배열 또는 개체)를 상태에서도 변환하면 참조가 그대로 유지되기 때문입니다. 이 기준 유형 날짜는 `use Effect` 후크에 대한 종속성으로 전달되었습니다. 그리고 (사용자 추가를 불러도) 변경되지 않았기 때문에 (사용 효과 후크는 실행되지 않았다.

이것은 틀림없이 이상한 벌레이다. 이것을 해결하기 위해 우리는 state.users를 통과시킨다.$는 아래와 같이 state.users가 아닌 effect에 대한 의존성입니다.

```coffeescript
const state = useRS( { users: [] })

useEffect( () => {
  // do something.
}, [state.users.$])
```

여기서 자세히 알아보십시오.

## 마지막 생각

리액티브 스테이트는 리액트 세계에 혁명적인 혁신을 가져온다. 그것은 매우 빠르고, 매우 반응적이며, 효율적이다. React 상태 관리의 일반적인 함정을 방지하기 위해 더 깨끗한 패턴을 제공합니다. 그리고 그것은 국가가 복잡해질수록 더 밝게 빛납니다. 이 게시물 이후로는 번거로움 없이 `반응 상태`를 사용할 수 있어야 한다고 생각하지만, `고트체스`만은 염두에 둬야 한다.