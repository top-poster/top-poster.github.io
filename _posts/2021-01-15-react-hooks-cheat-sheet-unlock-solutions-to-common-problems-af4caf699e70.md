---
layout: post
title: "React Hooks 치트 시트 : 모범 사례와 예제
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/04/react-hooks-cheat-sheet-best-practices.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/04/react-hooks-cheat-sheet-best-practices.png?fit=730%2C487&ssl=1)

편집자 주 :이 React Hooks 튜토리얼은 더 많은 React Hooks 모범 사례와 예제를 포함하도록 2021 년 1 월에 마지막으로 업데이트되었습니다.
 

React Hooks에는 매우 간단한 API가 있지만 방대한 커뮤니티와 다양한 사용 사례를 고려할 때 React Hooks 모범 사례와 일반적인 문제를 해결하는 방법에 대한 질문이 제기 될 수 있습니다.
 

이 튜토리얼에서는 몇 가지 React Hooks 모범 사례를 간략히 설명하고 간단한 시나리오에서 고급 시나리오까지 예제를 통해 몇 가지 사용 사례를 강조합니다.
 일반적인 React Hooks 질문을 해결하는 방법을 보여주기 위해 여기에있는 예제와의 실시간 상호 작용을 위해 함께 제공되는 웹 앱을 만들었습니다.
 

## React Hooks 치트 시트 : 모범 사례 및 예제
 

이 React Hooks 치트 시트에는 많은 코드 조각이 포함되어 있으며 Hooks가 유창하다고 가정합니다.
 Hooks를 완전히 처음 사용하는 경우 React Hooks API 참조 가이드로 시작하는 것이 좋습니다.
 

이 React Hooks 치트 시트에는 다음 후크와 관련된 모범 사례가 포함되어 있습니다.
 

- `useState`
 
- `useEffect`
 
- `useContext`
 
- `useLayoutEffect`
 
- `useReducer`
 
- `useCallback`
 
- `useMemo`
 
- `useRef`
 

## `useState`
 

`useState`를 사용하면 함수 구성 요소 내에서 로컬 상태를 사용할 수 있습니다.
 이 함수에 초기 상태를 전달하면 현재 상태 값 (초기 상태 일 필요는 없음)이 포함 된 변수와이 값을 업데이트하는 다른 함수가 반환됩니다.
 

이 React`useState` 비디오 자습서를 확인하십시오.
 

### 상태 변수 선언
 

상태 변수를 선언하는 것은 `useState (initialStateValue)`와 같이 초기 상태 값으로 `useState`를 호출하는 것만 큼 간단합니다.
 

```js
const DeclareStateVar = () => {
  const [count] = useState(100)
  return <div> State variable is {count}</div>
}
```

### 상태 변수 업데이트
 

상태 변수 업데이트는`useState` 호출에 의해 반환 된 업데이트 프로그램 함수를 호출하는 것만 큼 간단합니다.`const [stateValue, updaterFn] = useState (initialStateValue);`.
 

위의 스크린 캐스트를 담당하는 코드는 다음과 같습니다.
 

```js
const UpdateStateVar = () => {
  const [age, setAge] = useState(19)
  const handleClick = () => setAge(age + 1)

  return (
    <div>
      Today I am {age} Years of Age
      <div>
        <button onClick={handleClick}>Get older! </button>
      </div>
    </div>
  )
}
```

### React`useState` Hook이 즉시 업데이트되지 않는 이유는 무엇입니까?
 

`useState` /`setState`가 즉시 업데이트되지 않는 경우 대답은 간단합니다. 단지 대기열 일뿐입니다.
 

React`useState` 및`setState`는 상태 객체를 직접 변경하지 않습니다.
 성능을 최적화하기 위해 대기열을 생성하므로 변경 사항이 즉시 업데이트되지 않습니다.
 

### React Hooks 및 여러 상태 변수
 

다음과 같이 기능 구성 요소 내에서 여러 상태 변수를 사용하고 업데이트 할 수 있습니다.
 

![image](https://i1.wp.com/storage.googleapis.com/blog-images-backup/1*1MFDgE1LQuAc1_wyBgyVNQ.gif?ssl=1)

위의 스크린 캐스트를 담당하는 코드는 다음과 같습니다.
 

```js
const MultipleStateVars = () => {
  const [age, setAge] = useState(19)
  const [siblingsNum, setSiblingsNum] = 
    useState(10)

  const handleAge = () => setAge(age + 1)
  const handleSiblingsNum = () => 
      setSiblingsNum(siblingsNum + 1)
 

  return (
    <div>
      <p>Today I am {age} Years of Age</p>
      <p>I have {siblingsNum} siblings</p>

      <div>
        <button onClick={handleAge}>
          Get older! 
        </button>
        <button onClick={handleSiblingsNum}>
            More siblings! 
        </button>
      </div>
    </div>
  )
}
```

### 개체 상태 변수 사용
 

문자열 및 숫자와는 달리 객체를 `useState`에 전달되는 초기 값으로 사용할 수도 있습니다.
 

개체가 병합되는 것이 아니라 대체되기 때문에 전체 개체를`useState `업데이터 함수에 전달해야합니다.
 

```undefined
// 🐢 setState (object merge) vs useState (object replace)
// assume initial state is {name: "Ohans"}

setState({ age: 'unknown' })
// new state object will be
// {name: "Ohans", age: "unknown"}

useStateUpdater({ age: 'unknown' })
// new state object will be
// {age: "unknown"} - initial object is replaced
```

위 스크린 캐스트의 코드는 다음과 같습니다.
 

```js
const StateObject = () => {
  const [state, setState] = useState({ age: 19, siblingsNum: 4 })
  const handleClick = val =>
    setState({
      ...state,
      [val]: state[val] + 1
    })
  const { age, siblingsNum } = state

  return (
    <div>
      <p>Today I am {age} Years of Age</p>
      <p>I have {siblingsNum} siblings</p>

      <div>
        <button onClick={handleClick.bind(null, 'age')}>Get older!</button>
        <button onClick={handleClick.bind(null, 'siblingsNum')}>
          More siblings!
        </button>
      </div>
    </div>
  )
}
```

### 함수에서 상태 초기화
 

초기 상태 값을 전달하는 것과는 반대로 아래와 같이 함수에서 상태를 초기화 할 수도 있습니다.
 

```js
const StateFromFn = () => {
  const [token] = useState(() => {
    let token = window.localStorage.getItem("my-token");
    return token || "default#-token#"
  })

  return <div>Token is {token}</div>
}
```

### 기능적`setState`
 

`useState`호출에서 반환 된 업데이트 프로그램 함수는 good ol ``setState `와 유사한 함수를 사용할 수도 있습니다.
 

```js
const [value, updateValue] = useState(0)
// both forms of invoking "updateValue" below are valid 👇
updateValue(1);
updateValue(previousValue => previousValue + 1);
```

이는 상태 업데이트가 이전 상태 값에 의존 할 때 이상적입니다.
 

위 스크린 캐스트의 코드는 다음과 같습니다.
 

```js
const CounterFnSetState = () => {
  const [count, setCount] = useState(0);
  return (
    <>
      <p>Count value is: {count}</p>
      <button onClick={() => setCount(0)}>Reset</button>
      <button 
        onClick={() => setCount(prevCount => prevCount + 1)}>
        Plus (+)
      </button>
      <button 
        onClick={() => setCount(prevCount => prevCount - 1)}>
       Minus (-)
      </button>
    </>
  );
}
```

여기에 직접 편집 가능한 `useState`치트 시트가 있습니다.
 

## `useEffect`
 

`useEffect`를 사용하면 React Hooks 시대에 이해해야 할 중요한 개념 인 기능적 구성 요소 내에서 부작용을 호출 할 수 있습니다.
 

### 기본적인 부작용
 

위의 스크린 캐스트를 담당하는 코드는 다음과 같습니다.
 

```js
const BasicEffect = () => {
  const [age, setAge] = useState(0)
  const handleClick = () => setAge(age + 1)

  useEffect(() => {
    document.title = 'You are ' + age + ' years old!'
  })

  return <div>
    <p> Look at the title of the current tab in your browser </p>
    <button onClick={handleClick}>Update Title!! </button>
  </div>
}
```

### 정리 효과
 

얼마 후 효과를 정리하는 것은 매우 일반적입니다.
 이것은`useEffect`에 전달 된 효과 함수 내에서 함수를 반환함으로써 가능합니다.
 다음은`addEventListener`를 사용한 예입니다.
 

```js
const EffectCleanup = () => {
  useEffect(() => {
    const clicked = () => console.log('window clicked')
    window.addEventListener('click', clicked)

    // return a clean-up function
    return () => {
      window.removeEventListener('click', clicked)
    }
  }, [])

  return <div>
    When you click the window you'll 
    find a message logged to the console
  </div>
}
```

### 여러 효과
 

아래와 같이 기능 구성 요소 내에서 여러`useEffect` 호출이 발생할 수 있습니다.
 

```js
const MultipleEffects = () => {
  // 🍟
  useEffect(() => {
    const clicked = () => console.log('window clicked')
    window.addEventListener('click', clicked)

    return () => {
      window.removeEventListener('click', clicked)
    }
  }, [])

  // 🍟 another useEffect hook 
  useEffect(() => {
    console.log("another useEffect call");
  })

  return <div>
    Check your console logs
  </div>
}
```

`useEffect`호출은 건너 뛸 수 있습니다. 즉, 모든 렌더링에서 호출되지는 않습니다.
 두 번째 배열 인수를 효과 함수에 전달하면됩니다.
 

### 효과 건너 뛰기 (배열 종속성)
 

```js
const ArrayDepMount = () => {
  const [randomNumber, setRandomNumber] = useState(0)
  const [effectLogs, setEffectLogs] = useState([])

  useEffect(
    () => {
      setEffectLogs(prevEffectLogs => [...prevEffectLogs, 'effect fn has been invoked'])
    },
    []
  )

  return (
    <div>
      <h1>{randomNumber}</h1>
      <button
        onClick={() => {
          setRandomNumber(Math.random())
        }
      >
        Generate random number!
      </button>
      <div>
        {effectLogs.map((effect, index) => (
          <div key={index}>{'🍔'.repeat(index) + effect}</div>
        ))}
      </div>
    </div>
  )
}
```

위의 예에서`useEffect`는 하나의 값인`[randomNumber]`로 전달됩니다.
 

따라서 효과 함수는 마운트시 그리고 새로운 난수가 생성 될 때마다 호출됩니다.
 

다음은 난수 생성 버튼을 클릭하고 새 난수를 생성 할 때 다시 실행되는 효과 함수입니다.
 

![image](https://i0.wp.com/storage.googleapis.com/blog-images-backup/1*mSqiFgHeY6k84us2RBnLkg.gif?ssl=1)

### 효과 건너 뛰기 (빈 배열 종속성)
 

이 예에서`useEffect`는 빈 배열`[]`로 전달됩니다.
 따라서 효과 함수는 마운트시에만 호출됩니다.
 

```js
const ArrayDepMount = () => {
  const [randomNumber, setRandomNumber] = useState(0)
  const [effectLogs, setEffectLogs] = useState([])

  useEffect(
    () => {
      setEffectLogs(prevEffectLogs => [...prevEffectLogs, 'effect fn has been invoked'])
    },
    []
  )

  return (
    <div>
      <h1>{randomNumber}</h1>
      <button
        onClick={() => {
          setRandomNumber(Math.random())
        }
      >
        Generate random number!
      </button>
      <div>
        {effectLogs.map((effect, index) => (
          <div key={index}>{'🍔'.repeat(index) + effect}</div>
        ))}
      </div>
    </div>
  )
}
```

클릭되는 버튼과 호출되지 않은 효과 기능은 다음과 같습니다.
 

![image](https://i0.wp.com/storage.googleapis.com/blog-images-backup/1*VVxa13t8u8oobG_1GIM1Qw.gif?ssl=1)

### 효과 건너 뛰기 (배열 종속성 없음)
 

배열 종속성이 없으면 효과 함수는 모든 단일 렌더링 후에 실행됩니다.
 

```coffeescript
useEffect(() => {
console.log(“This will be logged after every render!”)
})
```

더 자세히 알아보고 싶다면 편집 가능한 라이브 `useEffect`치트 시트가 있습니다.
 

## `useContext`
 

`useContext`는 Context 소비자에 의존해야하는 스트레스를 덜어줍니다.
 React Context는`MyContext.Consumer`와 그것이 노출하는 render props API와 비교할 때 더 간단한 API를 가지고 있습니다.
 

컨텍스트는 여러 구성 요소간에 공유 된 데이터를 처리하는 React의 방법입니다.
 

다음 예제는`useContext` 또는`Context.Consumer`를 통해 컨텍스트 객체 값을 사용하는 것의 차이점을 강조합니다.
 

```js
// example Context object
const ThemeContext = React.createContext("dark");

// usage with context Consumer
function Button() {
  return <ThemeContext.Consumer>
        {theme => <button className={theme}> Amazing button </button>}
  </ThemeContext.Consumer>
}


// usage with useContext hook 
import {useContext} from 'react';

function ButtonHooks() {
 const theme = useContext(ThemeContext)
 return <button className={theme}>Amazing button</button>
}
```

다음은`useContext`를 사용한 실제 예입니다.
 

![image](https://i1.wp.com/storage.googleapis.com/blog-images-backup/1*sJEVsJmB2vHc8vqqP4nAJA.png?ssl=1)

위의 예를 담당하는 코드는 다음과 같습니다.
 

```php
const ThemeContext = React.createContext('light');

const Display = () => {
 const theme = useContext(ThemeContext);
 return <div
        style={
        background: theme === 'dark' ? 'black' : 'papayawhip',
        color: theme === 'dark' ? 'white' : 'palevioletred',
        width: '100%',
        minHeight: '200px'
        }
    >
        {'The theme here is ' + theme}
    </div>
}
```

여기에 수정 가능한 실시간 React Context 치트 시트가 있습니다.
 

## `useLayoutEffect`
 

`useLayoutEffect`는`useEffect`와 매우 동일한 서명을 갖습니다.
 아래에서`useLayoutEffect`와`useEffect`의 차이점에 대해 설명합니다.
 

```coffeescript
useLayoutEffect(() => {
//do something
}, [arrayDependency])
```

### `useEffect`와 유사한 사용법
 

다음은`useLayoutEffect`로 빌드 된`useEffect`의 동일한 예입니다.
 

![image](https://i2.wp.com/storage.googleapis.com/blog-images-backup/1*a7MsYcXko93rq_9KtjiXpg.gif?ssl=1)

다음은 코드입니다.
 

```js
const ArrayDep = () => {
    const [randomNumber, setRandomNumber] = useState(0)
    const [effectLogs, setEffectLogs] = useState([])
  
    useLayoutEffect(
      () => {
        setEffectLogs(prevEffectLogs => [...prevEffectLogs, 'effect fn has been invoked'])
      },
      [randomNumber]
    )
  
    return (
      <div>
        <h1>{randomNumber}</h1>
        <button
          onClick={() => {
            setRandomNumber(Math.random())
          }
        >
          Generate random number!
        </button>
        <div>
          {effectLogs.map((effect, index) => (
            <div key={index}>{'🍔'.repeat(index) + effect}</div>
          ))}
        </div>
      </div>
    )
  }
```

### `useLayoutEffect` 대`useEffect`
 

`useEffect`와`useLayoutEffect`의 차이점은 무엇입니까?
 `useEffect`에 전달 된 함수는 레이아웃 및 페인트 후, 즉 렌더링이 화면에 커밋 된 후 실행됩니다.
 브라우저가 화면을 업데이트하지 못하도록 차단해서는 안되는 대부분의 부작용에는 괜찮습니다.
 

하지만`useEffect`가 제공하는 동작을 원하지 않는 경우도 있습니다.
 예를 들어 부작용으로 DOM을 시각적으로 변경해야하는 경우`useEffect`가 최선의 선택이 아닙니다.
 

사용자가 변경 사항의 깜박임을 보지 않도록`useLayoutEffect`를 사용할 수 있습니다.
 `useLayoutEffect`에 전달 된 함수는 브라우저가 화면을 업데이트하기 전에 실행됩니다.
 

`useEffect`와`useLayoutEffect`의 차이점에 대한 자세한 내용은 제 후속 기사를 읽을 수 있습니다.
 

다음은 편집 가능한 라이브`useLayoutEffect` 치트 시트입니다.
 

## `useReducer`
 

`useReducer`는`useState`의 대안으로 사용될 수 있습니다.
 이전 상태 값이나 많은 상태 하위 값에 대한 종속성이있는 복잡한 상태 논리에 이상적입니다.
 

사용 사례에 따라`useReducer`를 상당히 테스트 할 수 있습니다.
 

### 기본 사용법
 

`useState`를 호출하는 것과는 반대로, 아래와 같이`reducer` 및`initialState`를 사용하여`useReducer`를 호출합니다.
 `useReducer` 호출은 state 속성과`dispatch` 함수를 반환합니다.
 

위의 스크린 캐스트를 담당하는 코드는 다음과 같습니다.
 

```js
const initialState = { width: 15 };

const reducer = (state, action) => {
  switch (action) {
    case 'plus':
      return { width: state.width + 15 }
    case 'minus':
      return { width: Math.max(state.width - 15, 2) }
    default:
      throw new Error("what's going on?" )
  }
}

const Bar = () => {
  const [state, dispatch] = useReducer(reducer, initialState)
  return <>
    <div style={ background: 'teal', height: '30px', width: state.width }></div>
    <div style={marginTop: '3rem'}>
        <button onClick={() => dispatch('plus')}>Increase bar size</button>
        <button onClick={() => dispatch('minus')}>Decrease bar size</button>
    </div>
    </>
}

ReactDOM.render(<Bar />)
```

### 느리게 상태 초기화
 

`useReducer`는 세 번째 함수 매개 변수를받습니다.
 이 함수에서 상태를 초기화 할 수 있으며이 함수에서 반환 된 모든 항목이 상태 객체로 반환됩니다.
 이 함수는 두 번째 매개 변수 인`initialState`와 함께 호출됩니다.
 

위 예의 코드는 다음과 같습니다.
 

```js
const initializeState = () => ({
  width: 100
})

// ✅ note how the value returned from the fn above overrides initialState below: 
const initialState = { width: 15 }
const reducer = (state, action) => {
  switch (action) {
    case 'plus':
      return { width: state.width + 15 }
    case 'minus':
      return { width: Math.max(state.width - 15, 2) }
    default:
      throw new Error("what's going on?" )
  }
}

const Bar = () => {
  const [state, dispatch] = useReducer(reducer, initialState, initializeState)
  return <>
    <div style={ background: 'teal', height: '30px', width: state.width }></div>
    <div style={marginTop: '3rem'}>
        <button onClick={() => dispatch('plus')}>Increase bar size</button>
        <button onClick={() => dispatch('minus')}>Decrease bar size</button>
    </div>
    </>
}

ReactDOM.render(Bar)
```

### `this.setState`의 동작을 모방합니다.
 

`useReducer`는 Redux만큼 엄격하지 않은 감속기를 사용합니다.
 예를 들어 감속기에 전달 된 두 번째 매개 변수 `action`에는 `type`속성이 필요하지 않습니다.
 

이렇게하면 두 번째 매개 변수의 이름을 바꾸고 다음을 수행하는 것과 같은 흥미로운 조작이 가능합니다.
 

```js
const initialState = { width: 15 }; 

const reducer = (state, newState) => ({
  ...state,
  width: newState.width
})

const Bar = () => {
  const [state, setState] = useReducer(reducer, initialState)
  return <>
    <div style={ background: 'teal', height: '30px', width: state.width }></div>
    <div style={marginTop: '3rem'}>
        <button onClick={() => setState({width: 100})}>Increase bar size</button>
        <button onClick={() => setState({width: 3})}>Decrease bar size</button>
    </div>
    </>
}

ReactDOM.render(Bar)
```

다음은 수정 가능한`useReducer` 치트 시트입니다.
 

## `useCallback`
 

`useCallback`은 메모 된 콜백을 반환합니다.
 컴포넌트를`React.Memo ()`로 래핑하면 코드를 재사용하려는 의도를 알립니다.
 이것은 매개 변수로 전달 된 함수로 자동 확장되지 않습니다.
 

React는`useCallback`으로 래핑 될 때 함수에 대한 참조를 저장합니다.
 이 참조를 새 구성 요소에 대한 속성으로 전달하여 렌더링 시간을 줄이십시오.
 

### `useCallback` 예제
 

다음 예제는 다음 설명 및 코드 조각의 기초를 형성합니다.
 

![image](https://i0.wp.com/storage.googleapis.com/blog-images-backup/1*Iy316AxOQNNXEcMHKeGw7w.gif?ssl=1)

다음은 코드입니다.
 

```coffeescript
const App = () => {
    const [age, setAge] = useState(99)
    const handleClick = () => setAge(age + 1)
    const someValue = "someValue"
    const doSomething = () => {
      return someValue
    }
  
    return (
      <div>
        <Age age={age} handleClick={handleClick}/>
        <Instructions doSomething={doSomething} />
      </div>
    )
}

const Age = ({ age, handleClick }) => {
  return (
    <div>
      <div style={ border: '2px', background: "papayawhip", padding: "1rem" }>
        Today I am {age} Years of Age
      </div>
      <pre> - click the button below 👇 </pre>
      <button onClick={handleClick}>Get older! </button>
    </div>
  )
}

const Instructions = React.memo((props) => {
  return (
    <div style={ background: 'black', color: 'yellow', padding: "1rem" }>
      <p>Follow the instructions above as closely as possible</p>
    </div>
  )
})

ReactDOM.render (
  <App />
)
```

위의 예에서 상위 구성 요소`<Age />`는 Get older 버튼을 클릭 할 때마다 업데이트 (및 다시 렌더링)됩니다.
 

결과적으로`<Instructions />`하위 구성 요소도 다시 렌더링됩니다.`doSomething` prop에 새 참조가있는 새 콜백이 전달되기 때문입니다.
 

`Instructions` 하위 구성 요소가`React.memo`를 사용하여 성능을 최적화하더라도 여전히 다시 렌더링됩니다.
 

`<Instructions />`가 불필요하게 다시 렌더링되는 것을 방지하려면 어떻게해야합니까?
 

### 참조 된 함수가있는`useCallback`
 

```coffeescript
const App = () => {
  const [age, setAge] = useState(99)
  const handleClick = () => setAge(age + 1)
  const someValue = "someValue"
  const doSomething = useCallback(() => {
    return someValue
  }, [someValue])

  return (
    <div>
      <Age age={age} handleClick={handleClick} />
      <Instructions doSomething={doSomething} />
    </div>
  )
}

const Age = ({ age, handleClick }) => {
  return (
    <div>
      <div style={ border: '2px', background: "papayawhip", padding: "1rem" }>
        Today I am {age} Years of Age
      </div>
      <pre> - click the button below 👇 </pre>
      <button onClick={handleClick}>Get older! </button>
    </div>
  )
}

const Instructions = React.memo((props) => {
  return (
    <div style={ background: 'black', color: 'yellow', padding: "1rem" }>
      <p>Follow the instructions above as closely as possible</p>
    </div>
  )
})

ReactDOM.render(<App />)
```

### 인라인 함수가있는`useCallback`
 

`useCallback`은 인라인 함수에서도 작동합니다.
 다음은 인라인`useCallback` 호출을 사용한 동일한 솔루션입니다.
 

```coffeescript
const App = () => {
  const [age, setAge] = useState(99)
  const handleClick = () => setAge(age + 1)
  const someValue = "someValue"

  return (
    <div>
      <Age age={age} handleClick={handleClick} />
      <Instructions doSomething={useCallback(() => {
        return someValue
      }, [someValue])} />
    </div>
  )
}

const Age = ({ age, handleClick }) => {
  return (
    <div>
      <div style={ border: '2px', background: "papayawhip", padding: "1rem" }>
        Today I am {age} Years of Age
      </div>
      <pre> - click the button below 👇 </pre>
      <button onClick={handleClick}>Get older! </button>
    </div>
  )
}

const Instructions = memo((props) => {
  return (
    <div style={ background: 'black', color: 'yellow', padding: "1rem" }>
      <p>Follow the instructions above as closely as possible</p>
    </div>
  )
})

render(<App />)
```

다음은 편집 가능한 라이브`useCallback` 치트 시트입니다.
 

## `useMemo`
 

`useMemo` 함수는 메모 된 값을 반환합니다.
 `useMemo`는 전체 함수 대신 반환 값을 내부화한다는 점에서`useCallback`과 다릅니다.
 동일한 함수에 핸들을 전달하는 대신 React는 매개 변수가 변경 될 때까지 함수를 건너 뛰고 이전 결과를 반환합니다.
 

이렇게하면 필요할 때까지 잠재적으로 비용이 많이 드는 작업을 반복적으로 수행하지 않아도됩니다.
 함수에 정의 된 변경 변수는`useMemo`의 동작에 영향을주지 않으므로이 메서드는주의해서 사용하십시오.
 예를 들어 타임 스탬프 추가를 수행하는 경우이 메서드는 시간이 변경되는 것을 신경 쓰지 않고 함수 매개 변수가 달라진다는 점만 고려합니다.
 

### `useMemo` 예제
 

다음 예제는 다음 설명 및 코드 조각의 기초를 형성합니다.
 

![image](https://i2.wp.com/storage.googleapis.com/blog-images-backup/1*jlGFv-2D2Yu6VoSGx5Fu3w.png?ssl=1)

위의 스크린 샷을 담당하는 코드는 다음과 같습니다.
 

```coffeescript
const App = () => {
    const [age, setAge] = useState(99)
    const handleClick = () => setAge(age + 1)
    const someValue = { value: "someValue" }
    const doSomething = () => {
      return someValue
    }
  
    return (
      <div>
        <Age age={age} handleClick={handleClick}/>
        <Instructions doSomething={doSomething} />
      </div>
    )
}

const Age = ({ age, handleClick }) => {
  return (
    <div>
      <div style={ border: '2px', background: "papayawhip", padding: "1rem" }>
        Today I am {age} Years of Age
      </div>
      <pre> - click the button below 👇 </pre>
      <button onClick={handleClick}>Get older! </button>
    </div>
  )
}

const Instructions = React.memo((props) => {
  return (
    <div style={ background: 'black', color: 'yellow', padding: "1rem" }>
      <p>Follow the instructions above as closely as possible</p>
    </div>
  )
})

ReactDOM.render (
  <App />
)
```

위의 예는`useCallback`의 예와 유사합니다.
 여기서 유일한 차이점은 `someValue`는 문자열이 아니라 객체라는 것입니다.
 이로 인해`Instructions` 구성 요소는`React.memo` 사용에도 불구하고 여전히 다시 렌더링됩니다.
 

왜?
 객체는 참조로 비교되며`<App />`이 다시 렌더링 될 때마다`someValue`에 대한 참조가 변경됩니다.
 

해결책이 있습니까?
 

### 기본 사용법
 

`someValue`객체는 `useMemo`를 사용하여 메모 할 수 있습니다.
 이것은 불필요한 다시 렌더링을 방지합니다.
 

```coffeescript
const App = () => {
    const [age, setAge] = useState(99)
    const handleClick = () => setAge(age + 1)
    const someValue = useMemo(() => ({ value: "someValue" }))
    const doSomething = () => {
      return someValue
    }
  
    return (
      <div>
        <Age age={age} handleClick={handleClick}/>
        <Instructions doSomething={doSomething} />
      </div>
    )
}

const Age = ({ age, handleClick }) => {
  return (
    <div>
      <div style={ border: '2px', background: "papayawhip", padding: "1rem" }>
        Today I am {age} Years of Age
      </div>
      <pre> - click the button below 👇 </pre>
      <button onClick={handleClick}>Get older! </button>
    </div>
  )
}

const Instructions = React.memo((props) => {
  return (
    <div style={ background: 'black', color: 'yellow', padding: "1rem" }>
      <p>Follow the instructions above as closely as possible</p>
    </div>
  )
})

ReactDOM.render (<App />)
```

다음은 편집 가능한 라이브`useMemo` 데모입니다.
 

## `useRef`
 

`useRef`는 "ref"객체를 반환합니다.
 반환 된 객체의`.current` 속성에서 값에 액세스합니다.
 예를 들어`.current` 속성은 초기 값 —`useRef (initialValue)`로 초기화 될 수 있습니다.
 개체는 구성 요소의 전체 수명 동안 유지됩니다.
 

이 포괄적 인`useRefs` 가이드에서 자세히 알아 보거나`useRefs` 비디오 자습서를 확인하십시오.
 

### DOM 액세스
 

아래 샘플 애플리케이션을 고려하십시오.
 

위의 스크린 캐스트를 담당하는 코드는 다음과 같습니다.
 

```js
const AccessDOM = () => {
  const textAreaEl = useRef(null);
  const handleBtnClick = () => {
    textAreaEl.current.value =
    "The is the story of your life. You are an human being, and you're on a website about React Hooks";
    textAreaEl.current.focus();
  };
  return (
    <section style={ textAlign: "center" }>
      <div>
        <button onClick={handleBtnClick}>Focus and Populate Text Field</button>
      </div>
      <label
        htmlFor="story"
        style={
          display: "block",
          background: "olive",
          margin: "1em",
          padding: "1em"
        }
      >
        The input box below will be focused and populated with some text
        (imperatively) upon clicking the button above.
      </label>
      <textarea ref={textAreaEl} id="story" rows="5" cols="33" />
    </section>
  );
};
```

### 인스턴스 유사 변수 (일반 컨테이너)
 

DOM 참조를 보유하는 것 외에 "ref"객체는 모든 값을 보유 할 수 있습니다.
 ref 객체가 문자열 값을 보유하는 아래 유사한 애플리케이션을 고려하십시오.
 

![image](https://i2.wp.com/storage.googleapis.com/blog-images-backup/1*jLxqYWFdw0LDl8_axo5hMw.gif?ssl=1)

코드는 다음과 같습니다.
 

```js
const HoldStringVal = () => {
    const textAreaEl = useRef(null);
    const stringVal = useRef("This is a string saved via the ref object --- ")
    const handleBtnClick = () => {
      textAreaEl.current.value =
      stringVal.current + "The is the story of your life. You are an human being, and you're on a website about React Hooks";
      textAreaEl.current.focus();
    };
    return (
      <section style={ textAlign: "center" }>
        <div>
          <button onClick={handleBtnClick}>Focus and Populate Text Field</button>
        </div>
        <label
          htmlFor="story"
          style={
            display: "block",
            background: "olive",
            margin: "1em",
            padding: "1em"
          }
        >
          Prepare to see text from the ref object here. Click button above.
        </label>
        <textarea ref={textAreaEl} id="story" rows="5" cols="33" />
      </section>
    );
  };
```

정리를 위해`setInterval`의 반환 값을 저장하는 것과 동일하게 수행 할 수 있습니다.
 

```js
function TimerWithRefID() {
  const setIntervalRef = useRef();

  useEffect(() => {
    const intervalID = setInterval(() => {
      // something to be done every 100ms
    }, 100);

    // this is where the interval ID is saved in the ref object 
    setIntervalRef.current = intervalID;
    return () => {
      clearInterval(setIntervalRef.current);
    };
  });
}
```

### 다른 예
 

실제에 가까운 예제를 작업하면 Hook에 대한 지식을 생생하게 표현할 수 있습니다.
 React Suspense로 데이터 가져 오기가 출시 될 때까지 Hooks를 통해 데이터를 가져 오는 것이 더 많은 Hooks 연습을위한 좋은 연습임이 입증되었습니다.
 

다음은로드 표시기를 사용하여 데이터를 가져 오는 예입니다.
 

![image](https://i0.wp.com/storage.googleapis.com/blog-images-backup/1*sr9I9TkSj8GCgSI411rGPA.gif?ssl=1)

코드는 다음과 같습니다.
 

```js
const fetchData = () => {
  const stringifyData = data => JSON.stringify(data, null, 2)
  const initialData = stringifyData({ data: null })
  const loadingData = stringifyData({ data: 'loading...' })
  const [data, setData] = useState(initialData)

  const [gender, setGender] = useState('female')
  const [loading, setLoading] = useState(false)

  useEffect(
    () => {
      const fetchData = () => {
        setLoading(true)
        const uri = 'https://randomuser.me/api/?gender=' + gender
        fetch(uri)
          .then(res => res.json())
          .then(({ results }) => {
            setLoading(false)
            const { name, gender, dob } = results[0]
            const dataVal = stringifyData({
              ...name,
              gender,
              age: dob.age
            })
            setData(dataVal)
          })
      }

      fetchData()
    },
    [gender]
  )

  return (
    <>
      <button
        onClick={() => setGender('male')}
        style={ outline: gender === 'male' ? '1px solid' : 0 }
      >
        Fetch Male User
      </button>
      <button
        onClick={() => setGender('female')}
        style={ outline: gender === 'female' ? '1px solid' : 0 }
      >
        Fetch Female User
      </button>

      <section>
        {loading ? <pre>{loadingData}</pre> : <pre>{data}</pre>}
      </section>
    </>
  )
}
```

여기에 편집 가능한 라이브 `useRef`치트 시트가 있습니다.
 

## 결론
 

후크는 기능적 구성 요소에 많은 힘을줍니다.
 이 치트 시트가 React Hooks를 일상적으로 사용하는 데 유용하다는 것이 증명되기를 바랍니다.
 

건배!
 