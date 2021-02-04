---
layout: post
title: "Jest 및 Enzyme으로 React Hooks 테스트
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/02/react-hooks-testing-enzyme-jest.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/02/react-hooks-testing-enzyme-jest.png?fit=730%2C487&ssl=1)

React의 16.8.0 버전 릴리스는 React Hooks 기능의 안정적인 릴리스를 의미했습니다.
 React Hooks는 2018 년에 도입되었으며 React 생태계에서 호평을 받았습니다.
 기본적으로 클래스 구성 요소없이 상태와 같은 기능을 사용하여 구성 요소를 만드는 방법입니다.
 

## React Hooks는 무엇입니까?
 

Hooks 기능은 React 개발자가 수년 동안 직면 한 많은 문제를 해결하기 때문에 환영할만한 변화입니다.
 이러한 문제 중 하나는 React가 `클래스`구성 요소간에 재사용 가능한 상태 논리를 지원하지 않는 경우입니다.
 이것은 때로는 거대한 구성 요소, 생성자 및 수명주기 메서드의 중복 논리로 이어질 수 있습니다.
 

필연적으로 이로 인해 렌더 소품 및 고차 구성 요소와 같은 복잡한 패턴을 사용해야하며 이는 복잡한 코드베이스로 이어질 수 있습니다.
 

후크는 상태, 수명주기 메서드, refs e.t.c에 액세스하여 재사용 가능한 구성 요소를 작성할 수 있도록하여 이러한 모든 문제를 해결하는 것을 목표로합니다.
 

## 다양한 유형의 React Hooks
 

다음은 React 앱에서 일반적으로 사용되는 몇 가지 주요 후크입니다.
 

- useState : 상태가 포함 된 순수 함수를 작성할 수 있습니다.
 
- useEffect : 부작용을 수행 할 수 있습니다.
 부작용으로는 API 호출, DOM 업데이트, 이벤트 리스너 구독 등이 있습니다.
 
- useContext : 컨텍스트가 포함 된 순수 함수를 작성할 수 있습니다.
 
- useRef : 가변`ref` 객체를 반환하는 순수 함수를 작성할 수 있습니다.
 

특정 엣지 케이스에 대해 React 앱에서 사용할 수있는 다른 후크는 다음과 같습니다.
 

- useReducer :`useState`의 대안.
 `(state, action) => newState` 유형의 감속기를 허용하고`dispatch` 메소드와 쌍을 이루는 현재 상태를 반환합니다.
 여러 하위 값을 포함하는 복잡한 상태 로직이 있거나 다음 상태가 이전 상태에 의존하는 경우 일반적으로`useState`보다 선호됩니다.
 
- useMemo : useMemo는 메모 된 값을 반환하는 데 사용됩니다.
 
- useCallback :이 후크는 메모 된 콜백을 반환하는 데 사용됩니다.
 
- useImperativeMethods : useImperativeMethods는`ref`를 사용할 때 부모 구성 요소에 노출되는 인스턴스 값을 사용자 지정합니다.
 
- useMutationEffects : useMutationEffect는 DOM 변이를 수행 할 수 있다는 점에서 useEffect 후크와 유사합니다.
 
- useLayoutEffect : useLayoutEffect 후크는 DOM에서 레이아웃을 읽고 동 기적으로 다시 렌더링하는 데 사용됩니다.
 

## React Hooks를 사용하여 React 앱을 빌드하는 방법
 

React Hooks에 대한 테스트를 작성하는 방법을 알아보기 전에 Hooks를 사용하여 React 앱을 빌드하는 방법을 살펴 보겠습니다.
 2018 년 F1 레이스와 매년 우승자를 보여주는 앱을 만들 예정입니다.
 

전체 앱은 CodeSandbox에서보고 상호 작용할 수 있습니다.
 

위 앱에서는`useState` 및`useEffect` 후크를 사용하고 있습니다.
 `index.js` 파일로 이동하면`App` 함수에서`useState`가 사용되는 인스턴스를 볼 수 있습니다.
 

```undefined
// Set the list of races to an empty array
let [races, setRaces] = useState([]);
// Set the winner for a particular year
let [winner, setWinner] = useState("");
```

`useState`는 현재 상태 값과이를 업데이트 할 수있는 함수 인 값 쌍을 반환합니다.
 객체가되어야하는 클래스의 상태와는 반대로 모든 유형의 값 (문자열, 배열 e.t.c)으로 초기화 할 수 있습니다.
 

여기에서 사용되는 다른 후크는`useEffect` 후크입니다.
 `useEffect` 후크는 함수 구성 요소에서 부작용을 수행하는 기능을 추가합니다.
 기본적으로`componentDidMount`,`componentDidUpdate` 및`componentWillUnmount` 수명주기에서 일반적으로 수행하는 작업을 수행 할 수 있습니다.
 

```js
// On initial render of component, fetch data from API.
useEffect(() => {
  fetch(`https://ergast.com/api/f1/2018/results/1.json`)
    .then(response => response.json())
    .then(data => {
      setRaces(data.MRData.RaceTable.Races);
    });
  fetch(`https://ergast.com/api/f1/2018/driverStandings.json`)
    .then(response => response.json())
    .then(data => {
      let raceWinner = data.MRData.StandingsTable.StandingsLists[0].DriverStandings[0].Driver.familyName + " " + data.MRData.StandingsTable.StandingsLists[0].DriverStandings[0].Driver.givenName;
      setWinner(raceWinner);
    });
}, []);
```

앱에서는`useEffect` 후크를 사용하여 API 호출을 수행하고 F1 레이스 데이터를 가져온 다음`setRaces` 및`setWinner` 함수를 사용하여 각각의 값을 상태로 설정합니다.
 

이는 앱을 빌드하기 위해 후크를 조합하여 사용하는 방법의 예일뿐입니다.
 `useEffect`후크를 사용하여 일부 소스에서 데이터를 가져오고 `useState`를 사용하여 데이터를 상태로 설정합니다.
 

## Jest 또는 Enzyme으로 React Hooks 테스트
 

### Jest와 Enzyme은 무엇입니까?
 

Jest와 Enzyme은 React 앱 테스트에 사용되는 도구입니다.
 Jest는 JavaScript 앱을 테스트하는 데 사용되는 JavaScript 테스트 프레임 워크이고 Enzyme은 React 구성 요소의 출력을 더 쉽게 주장, 조작 및 탐색 할 수있게 해주는 React 용 JavaScript 테스트 유틸리티입니다.
 

아마도 React를위한 테스트 도구 일 것이므로 React Hooks를 테스트하는 데 사용할 수 있는지 살펴 보겠습니다.
 이를 위해 CodeSandbox에 테스트 스위트에 사용할 앱을 만들었습니다.
 CodeSandbox에서 앱을 포크하여 따라 할 수 있습니다.
 

`__tests__` 폴더로 이동하여 테스트 스위트가 포함 된`hooktest.js` 파일을 확인합니다.
 

```coffeescript
import React from "react";
import ReactDOM from "react-dom";
import App from "../index";
it("renders without crashing", () => {
  const div = document.createElement("div");
  ReactDOM.render(<App />, div);
  ReactDOM.unmountComponentAtNode(div);
});
```

먼저 앱이 충돌없이 렌더링되는지 확인하기위한 테스트를 작성합니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/02/testing-react-hooks-jest-enzyme.png?resize=730%2C280&ssl=1)

다음으로 Enzyme 테스트 라이브러리를 사용하여 React Hooks를 테스트 해 보겠습니다.
 Enzyme을 사용하려면 CodeSandbox 앱에 다음 종속성을 설치해야합니다.
 

- 효소
 
- 효소 어댑터 반응 -16
 

`__tests__` 폴더로 이동하여 테스트 스위트가 포함 된`hooktest.js` 파일을 확인합니다.
 

`hooktest.js` 파일에 추가 테스트 블록이 추가됩니다.
 Enzyme에서 가져온`shallow` 방법을 사용하여 테스트하고 있습니다.
 얕은 방법 또는 렌더링은 구성 요소를 하나의 단위로 테스트하는 데 사용됩니다.
 DOM이 필요하지 않은 컴포넌트 트리의 시뮬레이션 된 렌더링입니다.
 

Enzyme을 사용하여 테스트하려고하면 아래와 같은 오류가 발생합니다.
 

후크는 함수 구성 요소의 본문 내에서만 호출 할 수 있습니다.
 (https://fb.me/react-invalid-hook-call)
 

위의 오류는이 문제에서 볼 수 있듯이 아직 Enzyme에서 후크가 지원되지 않음을 의미합니다.
 

결과적으로 우리는 Enzyme을 사용하여 React Hooks에 대한 구성 요소 테스트를 수행 할 수 없습니다.
 그래서 무엇을 사용할 수 있습니까?
 

### 반응 테스트 라이브러리 소개
 

react-testing-library는 React 컴포넌트를 테스트하기위한 매우 가벼운 솔루션입니다.
 `react-dom`및 `react-dom / test-utils`를 확장하여 가벼운 유틸리티 기능을 제공합니다.
 반응 구성 요소가 사용되는 방식과 매우 유사한 테스트를 작성하도록 권장합니다.
 

react-testing-library를 사용하여 Hook에 대한 테스트를 작성하는 예를 살펴 보겠습니다.
 

위 앱에서는 `useState`, `useEffect`, `useRef`의 세 가지 유형의 후크가 사용 중이며 모두에 대한 테스트를 작성합니다.
 

개수를 늘리고 줄이는 `useState`예제 외에도 두 가지 예제를 더 추가했습니다.
 

`useRef` Hook 구현의 경우 기본적으로`useRef`를 사용하여`ref` 인스턴스를 만들고이를 입력 필드로 설정하고 있으며 이는 이제 ref를 통해 입력 값에 액세스 할 수 있음을 의미합니다.
 

`useEffect` Hook 구현은 기본적으로`name` 상태 값을`localStorage`로 설정합니다.
 

계속해서 위의 모든 구현에 대한 테스트를 작성해 보겠습니다.
 다음에 대한 테스트를 작성합니다.
 

- 초기 `수`상태는 0입니다.
 
- `증가`및 `감소`버튼이 작동합니다.
 
- 입력 필드를 통해 이름을 제출하면 `name`상태의 값이 변경됩니다.
 
- `name` 상태는 localStorage에 저장됩니다.
 

`__tests__` 폴더로 이동하여 테스트 스위트와 아래 코드 가져 오기 행이 포함 된`hooktest.js` 파일을 확인하십시오.
 

```js
// hooktest.js
import { render, fireEvent, getByTestId} from "react-testing-library";
```

- `render` — 구성 요소를 렌더링하는 데 도움이됩니다.
 `document.body`에 추가되는 컨테이너로 렌더링됩니다.
 
- `getByTestId` —`data-testid`로 DOM 요소를 가져옵니다.
 
- `fireEvent`— DOM 이벤트를 "발동"하는 데 사용됩니다.
 `문서`에 이벤트 핸들러를 연결하고 이벤트 위임을 통해 일부 DOM 이벤트를 처리합니다.
 버튼 클릭
 
- rerender — 페이지 새로 고침을 시뮬레이션하는 데 사용됩니다.
 

다음으로`hooktest.js` 파일에 아래 테스트 스위트를 추가합니다.
 

```php
// hooktest.js

it("App loads with initial state of 0", () => {
  const { container } = render(<App />);
  const countValue = getByTestId(container, "countvalue");
  expect(countValue.textContent).toBe("0");
});
```

테스트는 먼저 getByTestId 도우미를 사용하여 요소를 가져 와서 초기 카운트 상태가 0으로 설정되었는지 확인합니다.
 그런 다음`expect ()`및`toBe ()`함수를 사용하여 콘텐츠가 0인지 확인합니다.
 

다음으로 증가 및 감소 버튼이 작동하는지 확인하는 테스트를 작성합니다.
 

```php
// hooktest.js

it("Increment and decrement buttons work", () => {
  const { container } = render(<App />);
  const countValue = getByTestId(container, "countvalue");
  const increment = getByTestId(container, "incrementButton");
  const decrement = getByTestId(container, "decrementButton");
  expect(countValue.textContent).toBe("0");
  fireEvent.click(increment);
  expect(countValue.textContent).toBe("1");
  fireEvent.click(decrement);
  expect(countValue.textContent).toBe("0");
});
```

위 테스트에서 `on Button`을 클릭하면 상태가 1로 설정되고 `off Button`을 클릭하면 상태가 1로 설정되는지 확인합니다.
 

다음 단계에서는 입력 필드를 통해 이름을 제출하면 실제로`name` 상태 값이 변경되고`localStorage`에 성공적으로 저장되었는지 확인하는 테스트를 작성합니다.
 

```undefined
// hooktest.js

it("Submitting a name via the input field changes the name state value", () => {
  const { container, rerender } = render(<App />);
  const nameValue = getByTestId(container, "namevalue");
  const inputName = getByTestId(container, "inputName");
  const submitButton = getByTestId(container, "submitRefButton");
  const newName = "Ben";
  fireEvent.change(inputName, { target: { value: newName } });
  fireEvent.click(submitButton);
  expect(nameValue.textContent).toEqual(newName);
  rerender(<App />);
  expect(window.localStorage.getItem("name")).toBe(newName);
});
```

위의 테스트 어설 션에서`fireEvent.change` 메소드는`input` 필드에 값을 입력하는 데 사용되며 그 후 제출 버튼을 클릭합니다.
 

그런 다음 테스트는 버튼을 클릭 한 후의 ref 값이 `newName`과 같은지 확인합니다.
 마지막으로`rerender` 메서드를 사용하여 앱 다시로드를 시뮬레이션하고 이전에 설정된 이름이 localStorage에 저장되었는지 확인합니다.
 

### 결론
 

이 기사에서는 react-testing-library를 사용하여 React Hooks 및 React 컴포넌트에 대한 테스트를 작성하는 방법을 살펴 보았습니다.
 또한 React Hooks를 사용하는 방법에 대한 짧은 입문서를 살펴 보았습니다.
 더 알고 싶으세요?
 React hooks의 일반적인 실수를 피하는 방법에 대한이 게시물을 확인하세요.
 

질문이나 의견이 있으면 아래에서 공유 할 수 있습니다.
 