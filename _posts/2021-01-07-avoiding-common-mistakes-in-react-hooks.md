---
layout: post
title: "반응 후크의 일반적인 실수 방지"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/react-hooks.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/react-hooks.png?fit=730%2C487&ssl=1)

React Hooks 구성 요소는 몇 년 전부터 존재해왔지만 이러한 기능을 사용할 때 여전히 자주 발생하는 몇 가지 실수가 있습니다. 이 기사에서는 React Hooks로 전환할 때 발생하는 몇 가지 일반적인 오류와 이를 피하는 방법에 대해 설명합니다.

## 부작용으로 인한 코드 중복

React Hooks 구성 요소를 통해 이전 제품과 동일한 기능을 얻을 수 있지만, 이러한 기능을 수행하는 프로세스는 크게 다릅니다. 클래스 구성 요소를 사용하면 다양한 구성 요소 수명 주기 동안 부작용이 발생합니다. 그에 비해 React Hooks는 구성 요소 상태가 변경됨에 따라 부작용을 실행합니다. 이 경우 중복이 발생할 수 있습니다.

예를 들어, 상태 `카운트`가 업데이트될 때마다 페이지 제목을 업데이트하는 `카운터` 구성 요소를 예로 들 수 있습니다.

```js
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
  }
  componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

componentDidMount와 componentDidUpdate의 코드가 동일합니다. 이는 구성 요소를 DOM에 추가할 때와 이후의 모든 상태 업데이트에서 모두 코드를 실행해야 하기 때문입니다. 이러한 복제는 구성요소 라이프사이클의 다른 단계에서 동일한 부작용을 실행해야 하기 때문에 발생합니다. 코드를 메소드로 추출하더라도 메소드를 두 번 호출해야 하므로 중복을 피하는 데 도움이 되지 않는다.

React Hooks는 상태 변경의 결과로 부작용을 실행하므로 다음과 같은 중복 문제를 방지할 수 있습니다.

```js
import React, { useState, useEffect } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

기본적으로 `use Effect`는 모든 렌더 후와 후속 업데이트에서 실행되기 때문에 복제 문제를 해결한다.

또한 `useState`를 사용하여 특정 상태 변수의 변경 내용을 나타내는 데 반응(말투 의도 없음)할 수 있습니다.

```js
function Counter() {
  const [count, setCount] = useState(0)

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count]);

  ...
}
```

이 경우, "효과"는 주의 "카운트"가 변경될 때마다 실행됩니다. 이렇게 하면 상태 논리와 부작용의 밀접한 관계가 있어 읽고 이해하기 쉽다.

## '사용 효과'로 작업 처리

use Effect Hook이 유용할 수 있는 한, "너무 유용하게" 되어 코드를 지나치게 복잡하게 만들 수 있는 경우가 있다. 아래 `TodoList` 구성 요소의 경우 이를 확인할 수 있습니다.

```js
function TodoList({ onSuccess }) {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [todos, setTodos] = useState(null);

  const fetchTodos = () => {
    setLoading(true);
    callApi()
      .then((res) => setTodos(res))
      .catch((err) => setError(err))
      .finally(() => setLoading(false));
  };

  useEffect(() => {
    fetchTodos();
  }, []);

  useEffect(() => {
    if (!loading && !error && todos) {
      onSuccess();
    }
  }, [loading, error, todos, onSuccess]);

  return <div>{todos.map(todo => <Todo item={todo} /> )}</div>;
}
```

use Effect 후크는 두 가지가 있는데 하나는 초기 렌더링 시 실행되며, 두 번째는 로드와 에러가 거짓이지만 작업량이 채워졌을 때 실행된다(즉, API 호출이 성공했을 때 opp로 전달된 메서드를 onSuccess). 이것은 코드를 복잡하게 만드는 과잉 공학 사례입니다.

API 호출이 성공했을 때만 onSuccessful을 실행하려고 하므로 .then 핸들러와 구성 요소에서 호출할 수 있습니다.

```js
function TodoList({ onSuccess }) {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [todos, setTodos] = useState(null);

  const fetchTodos = () => {
    setLoading(true);
    callApi()
      .then((res) => {
         setTodos(res);
         onSuccess();
      })
      .catch((err) => setError(err))
      .finally(() => setLoading(false));
  };

  useEffect(() => {
    fetchTodos();
  }, []);

  return <div>{todos.map(todo => <Todo item={todo} /> )}</div>;
}
```

코드 복잡성을 제한하려면 `사용 효과`로 작업을 처리하지 마십시오.

## 리렌더가 필요하지 않을 때 'useState' 사용

Retact 구성 요소의 주요 요소 중 하나는 데이터 흐름을 구동하는 상태와 그에 따른 사용자 인터페이스 업데이트입니다. 모든 상태 변경 트리거는 구성 요소뿐만 아니라 하위 구성 요소도 다시 렌더링합니다. 성능 문제를 방지하려면 반드시 필요한 경우에만 useState를 사용하십시오.

이를 이해하기 위해 상태 `카운트`를 업데이트하는 버튼과 상태 `카운트`를 사용하여 API 호출을 하는 버튼 두 개를 사용하는 간단한 구성 요소를 살펴보겠습니다.

```js
function ClickButton(props) {
  const [count, setCount] = useState(0);

  const onClickCount = () => {
    setCount((c) => c + 1);
  };

  const onClickRequest = () => {
    apiCall(count);
  };

  return (
    <div>
      <button onClick={onClickCount}>Counter</button>
      <button onClick={onClickRequest}>Submit</button>
    </div>
  );
}
```

이 예에서는 상태가 `렌더` 방식으로 사용되지 않음을 확인할 수 있습니다. 즉, 상태가 변경될 때마다 불필요한 렌더링을 발생시킬 수 있습니다. 이 경우 useRef 후크는 렌더링 간의 값을 유지하고 리렌더를 유발하지 않기 때문에 더 적절할 것이다.

구성 요소는 다음과 같이 작성해야 합니다.

```js
function ClickButton(props) {
  const count = useRef(0);

  const onClickCount = () => {
    count.current++;
  };

  const onClickRequest = () => {
    apiCall(count.current);
  };

  return (
    <div>
      <button onClick={onClickCount}>Counter</button>
      <button onClick={onClickRequest}>Submit</button>
    </div>
  );
}
```

불필요한 재렌딩을 방지하려면 상태가 사용자 인터페이스를 업데이트하지 않을 때 `useState`를 사용하지 마십시오.

## 'onClick'을 사용하여 탐색 트리거

이 문제는 웹 개발의 일반적인 불량 관행이며, React Hooks에만 국한되지 않습니다. 다른 페이지로 연결되는 버튼이 있다고 가정해 보겠습니다.

```js
function ClickButton(props) {
  const history = useHistory();

  const onClick = () => {
    history.push('/next-page');
  };

  return <button onClick={onClick}>Go to next page</button>;
}
```

버튼에 onClick(온클릭) 수신기를 붙이고 History(히스토리)를 사용하여 다음 페이지로 이동하는 것은 단순한 클라이언트 측 탐색이기 때문에 유혹적일 수 있다.그렇지 않은가? 틀렸다.

이 시나리오의 첫 번째 문제는 이 버튼이 링크로 탐지되지 않아 화면 판독기에 대한 탐지가 거의 불가능하다는 것이다. 더욱이, 버튼 위를 맴도는 것은 많은 사용자가 링크와 연결되었다는 UX 힌트임에도 불구하고 화면의 하단 모서리에 후속 링크가 표시되지 않는다.

사용자가 혼란을 겪지 않도록 하려면 항상 다음과 같이 `<Link/>`를 사용하여 탐색을 트리거해야 합니다.

```js
function ClickButton(props) {
  return (
    <Link to="/next-page">
      <span>Go to next page</span>
    </Link>
  );
}
```

## 반응 후크에 대한 테스트 다시 쓰기

클래스 구성 요소가 후크가 있는 함수 구성 요소로 변환될 때 다시 쓰기 테스트가 문제가 됩니다. 테스트를 다시 작성해야 하는지 여부는 테스트가 구성 요소의 구현별 세부사항에 따라 달라지는지에 따라 결정됩니다. 샘플 테스트는 다음과 같습니다.

```coffeescript
test('updateCount updates the count state', () => {
  // using enzyme
  const wrapper = mount(<Counter initialCount="0" />)
  expect(wrapper.state('count')).toBe(0)
  wrapper.instance().updateCount(1)
  expect(wrapper.state('count')).toBe(1)
})
```

구성 요소가 후크로 작성되면 클래스 구성 요소(즉, `.state`)에 특정한 속성에 따라 달라지기 때문에 테스트가 중단됩니다.

사용자가 구성요소의 구현 세부사항과 관련이 없기 때문에 사용자의 관점에서 시험하는 것이 더 나은 접근법일 것이다. 이 경우 테스트를 다시 작성하는 방법은 다음과 같습니다.

```coffeescript
import { render, screen } from '@testing-library/react';

test('updateCount updates the count state', () => {
  // using React Testing Library
  render(<Counter initialCount="0" />)
  expect(screen.getByText("Initial count: 0")).toBeInTheDocument()

  userEvent.click(screen.getByText("Update count"))
  expect(screen.getByText("Initial count: 1")).toBeInTheDocument()
})
```

이 방법을 사용하면 구현 세부 정보가 변경되어 테스트가 중단되지 않습니다.

## 결론

이 기사에서는 React Hooks로 전환할 때 발생하는 몇 가지 일반적인 실수를 살펴봤습니다. 또한 이러한 실수나 모범 사례를 피하는 방법도 알아냈습니다. React Hooks에 대해 자세히 알아보십시오.