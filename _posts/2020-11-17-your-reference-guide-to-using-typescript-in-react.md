---
layout: post
title: "반응에서 TypeScript 사용에 대한 참조 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/react-typescript-guide.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/react-typescript-guide.png?fit=730%2C487&ssl=1)

자바스크립트의 문제 중 하나는 동적 유형 특성으로, 런타임까지 데이터와 변수 유형을 알 수 없다는 것을 의미한다. 이것은 많은 부작용을 일으킬 수 있다. 예를 들어 변수가 무엇이든 될 수 있기 때문에 코드베이스 내에서 혼동을 일으킬 수 있습니다.

이 문제를 해결하기 위해 마이크로소프트는 TypeScript를 출시했다. TypeScript의 수석 설계자인 Anders Hejlsberg는 "정적 타이핑, 클래스 [ 및] 모듈과 같은 대규모 애플리케이션 개발에 부족한 것으로 JavaScript를 강화할 수 있다면 어떻겠습니까?"라고 말했습니다. 바로 TypeScript에 대한 내용입니다."

TypeScript는 즉시 가장 널리 사용되는 정적 형식의 자바스크립트 버전이 되었다. 자바스크립트 개발자들이 데이터와 변수를 정적으로 입력할 수 있도록 하였다. 곧, Retact.js에 도입되어, Retactdevs가 TypeScript로 그들의 Retact 앱을 쓸 수 있게 되었다.

하지만 여기에는 비용이 따릅니다. TypeScript 유형 및 구문은 특히 React와 함께 사용할 때 따라가기 어려울 수 있습니다.

리액트에는 소품, 클래스 구성요소, 기능 구성요소, 기능 매개변수, 구성요소 수명 주기 후크 및 구성원 특성과 같은 많은 기능이 있습니다. TypeScript 내에서 이러한 정보를 입력하는 것은 쉽지 않기 때문에, 이 글은 초기 및 고급 Reactev에서 모두 빠른 참조 및 학습 가이드 역할을 하는 것을 목표로 합니다.

이를 통해 React에서 모범 사례 및 일반 TS 유형을 신속하게 검색할 수 있습니다. 준비됐어? 시작하자.

## TypeScript 유형

TypeScript에는 확장자가 *.d.ts인 파일을 저장하는 입력 폴더가 있습니다. 이러한 파일에는 값이 어떤 모양을 취할 것인지 유추하는 인터페이스가 포함되어 있습니다. 이것이 바로 TypeScript가 JavaScript에 데이터 입력을 가져올 수 있도록 하는 것입니다.

인터페이스는 값의 모양을 설명합니다.

```css
type AppState {
    propOne: number;
    propTwo: string
}
```

앱스테이트(AppState)는 데이터 유형의 가치가 어떻게 보일지 설명한다. 첫째, 우리는 그것이 속성 `propOne`과 `propTwo`를 보유하는 개체일 것이라고 추론하는데, 전자는 숫자형이고 후자는 문자열형이다. 부울 유형을 `propOne`에 할당하면 TypeScript가 TypeError를 발생시킵니다.

반응 프로젝트에 TypeScript를 포함하면 모든 반응 요소에는 취할 모양을 정의하는 인터페이스가 있습니다. 먼저 기능 구성 요소부터 살펴보겠습니다.

## 함수 성분

함수 구성 요소는 반응에서 JSX 요소를 반환하는 일반 함수이며 보기를 만드는 데 사용됩니다. 초기에는 상태 비저장 구성 요소이지만, React 훅이 출시되면 상태 저장 및 스마트화가 가능합니다.

반응 함수 구성 요소를 정의하면 `반응`이 수행됩니다.FunctionComponent의 형상:

```js
function App: React.FunctionComponent<> {
    return (
        <>
            // ...
        </>
    )
}
```

우리는 또한 리액트라는 속어를 사용할 수 있다.FC: "

```js
function App: React.FC<> {
    return (
        <>
            // ...
        </>
    )
}
```

React.Funct Component 또는 React.FC`는 함수 구성 요소의 반환 형식을 명시적으로 설명합니다.

소품 정의를 화살표 `<<]에 입력할 수 있다.

```bash
type AppProps = {
    message: string;
    age: number;
};
```

AppProps는 앱에 전달된 소품이 취할 인터페이스로, 소품을 받을 경우 아래에 앱 구성 요소를 쓸 수 있다.

```js
type AppProps {
    message: string;
    age: number;
}

function App: React.FC<AppProps>(props: AppProps) {
    return (
        <>
            // ...
        </>
    )
}
```

`?`를 사용하여 다음 입력에서 선택적 값을 설정할 수 있습니다.

```bash
type AppProps {
    message: string;
    age?: number;
}
```

이제, 연령 속성은 선택 사항이 됩니다. 앱 구성 요소는 렌더링할 수 있으며, 소품 개체의 연령 속성은 생략합니다.

화살표 `<<<] 안에 형식 선언을 생략할 수 있다.

```cpp
function App<{message: string; age: number;}>({message: string; age: number;}: AppProps) {
    // ...
}
```

기능 구성 요소의 내부 기능을 다음과 같이 입력할 수 있습니다.

```php
function App<{message: string; age: number;}>({message: string; age: number;}: AppProps) {
    // ...

    function clickHandler (val: number) {
        // ...
    }

    return (
        <>
            <button onClick={() => clickHandler(45)}            
        </>
    )
}
```

## 클래스 구성 요소

클래스 구성 요소는 반응에 보기를 만드는 데 사용됩니다. 그들은 근본적으로 블록을 쌓고 있다. 반응 앱에는 이러한 앱이 많이 포함될 수 있으며 UI의 작은 단위가 표시되는 방법을 정의합니다.

클래스 구성 요소에는 구성 요소 라이프사이클의 모든 상태에서 사용자 지정 코드를 실행하기 위해 연결할 수 있는 라이프사이클 후크가 있습니다.

그들은 리액트를 사용한다.구성 요소 >:

```java
class App extends React.Component<> {
    // ...    
}
```

## 소품 및 상태

클래스 구성 요소에 소품 및 상태 유형 매개 변수를 제공할 수 있습니다.

```cpp
type AppProps = {
    message: string;
    age: number;
};

type AppState = {
    id: number;
};

class App extends React.Component<AppProps, AppState> {
    state: AppState = {
        id: 0
    };

    // ...    
}
```

appProps는 appState 앞에 있는 < 화살표 사이에 삽입됩니다.

## 클래스 메소드

클래스 메서드에 대한 인수가 있는 경우 인수를 입력할 수 있습니다.

```js
class App extends React.Component<AppProps, AppState> {
    state: AppState = {
        id: 0
    };

    clickHandler = (val: number) => {
        // ...
    }

    render() {
        return (
            <>
                <button onClick={() => this.clickHandler(90)}>Click</button>
            </>
        )
    }
}
```

defaultProps는 React 구성 요소로 전달된 소품 인수의 기본값을 정의하는 데 사용됩니다. defaultProps는 런타임 중에 사용 가능한 것으로 추정되는 prop가 누락될 때 오류를 방지하는 데 유용합니다. defaultValues 내의 값은 소품 값이 됩니다.

형식 추론의 일부 에지 사례는 여전히 TypeScript에서 문제가 되기 때문에 함수 구성 요소에 `defaultProps`를 포함할 때 `Ract.Fc`가 삭제된다.

즉, 소품은 보통 때와 같이 사용됩니다.

```js
type AppProps = { message: string; age: number } & typeof defaultProps;
const defaultProps = {
    message: "",
    age: 0
};

function App(props: AppProps) {
    // ...
};

App.defaultProps = defaultProps;
```

## 양식 및 이벤트

양식은 일반적으로 내부 상태에서 정보를 수집하는 데 사용됩니다. 주로 로그인 및 등록 페이지에서 사용되므로 양식을 통해 제출된 정보를 수집하여 서버로 전송하여 처리할 수 있습니다.

Ract.FormEvent는 일반적으로 요소의 이벤트를 입력하는 데 사용됩니다.

```js
class App extends React.Component<> {
    clickHandler = (e: React.FormEvent<HTMLButtonElement>) => {
        // ...
    }

    changeHandler = (e: React.FormEvent<HTMLInputElement>) => {
        // ...
    }

    render() {
        return (
            <div>
                <button onClick={this.clickHandler}>Click</button>
                <input type="text" onChange={this.changeHandler} />
            </div>
        )
    }
}
```

click handler와 change handler의 주장 e에는 모두 반작용이 있다.양식 이벤트 유형입니다.

우리는 `반응`으로 처리기의 인수를 입력하는 것을 생략할 수 있다.`이벤트` 형식을 지정하고 처리기의 반환 값을 대신 입력합니다. 이것은 리액트를 사용하여 수행됩니다.이벤트 핸들러 변경:

```js
class App extends React.Component<> {
    clickHandler: React.FormEvent<HTMLButtonElement> = (e) => {
        // ...
    }

    changeHandler: React.FormEvent<HTMLInputElement> = (e) => {
        // ...
    }

    render() {
        return (
            <div>
                <button onClick={this.clickHandler}>Click</button>
                <input type="text" onChange={this.changeHandler} />
            </div>
        )
    }
}
```

리액트.변경 이벤트<T>`는 이벤트 핸들러 인수를 입력하는 데 사용할 수 있는 이벤트 유형입니다.

```js
class App extends React.Component<> {
    clickHandler = (e: React.ChangeEvent<HTMLButtonElement>) => {
        // ...
    }

    changeHandler = (e: React.ChangeEvent<HTMLInputElement>) => {
        // ...
    }

    render() {
        return (
            <div>
                <button onClick={this.clickHandler}>Click</button>
                <input type="text" onChange={this.changeHandler} />
            </div>
        )
    }
}
```

T는 이벤트가 등록된 요소의 유형입니다.

### 단추

반응을 설명합니다.단추 요소의 이벤트 유형을 변경합니다. 버튼은 HTMLButtonElement 클래스의 인스턴스입니다. 여기서 T는 HTML 버튼 엘리먼트, 리액트 등이 된다.변경 이벤트는 `응답하라`가 됩니다.이벤트 변경"

### 입력

`반응`에 대해 설명합니다.모든 입력 요소에 대해 이벤트 변경`: "텍스트", "암호", "색상", "버튼", "미디어" 등입니다. 모두 HTMLInputElement 클래스의 인스턴스입니다. 모든 유형의 경우=["text", "password" 등]

T는 HTMLTputElement가 된다. 대응 변경 이벤트는 `대응`이 됩니다.이벤트 변경"

### 텍스트 영역

반응을 나타냅니다.텍스트 영역 요소에 대한 ChangeEvent 및 텍스트 영역 요소는 HTMLTextAreaElement의 인스턴스입니다.

T는 HTMLTextAreaElement가 된다.
대응 변경 이벤트는 `대응`이 됩니다.이벤트 변경"

### 선택

Select 요소는 선택 옵션이 있는 드롭다운 목록을 만드는 데 사용됩니다. 선택 요소는 HTMLSelectElement의 인스턴스입니다.

T는 HTMLSelectElement가 된다. 대응 변경 이벤트는 `대응`이 됩니다.이벤트 변경"

### 형태

양식 요소는 DOM에서 정보를 수집합니다. 폼 요소는 HTML 양식 요소의 인스턴스입니다. T는 HTMLFormElement가 될 것이다. 대응 변경 이벤트는 `대응`이 됩니다.이벤트 변경"

### 비디오, 오디오

비디오는 브라우저에서 비디오를 재생하는 데 사용됩니다. 비디오 요소는 HTML VideoElement의 인스턴스입니다. 반응을 설정하는 방법에 대해 설명합니다.양식 요소의 이벤트를 변경합니다.

T는 HTML VideoElement가 된다.
대응 변경 이벤트는 `대응`이 됩니다.이벤트 변경"

오디오는 브라우저에서 오디오 파일(mp3, aac 등)을 재생하는 데 사용됩니다. 오디오 요소는 HTML AudioElement의 인스턴스입니다.

T는 HTML 오디오 Element가 된다.
대응 변경 이벤트는 `대응`이 됩니다.이벤트 변경"

### 리액트.합성 이벤트

이 유형은 이벤트 유형과 관련이 없는 경우에 사용됩니다.

```js
class App extends React.Component<> {
    submitHandler = (e: React.SyntheticEvent) => {
        // ...
    }

    render() {
        return (
            <form onSubmit={this.submitHandler}>
                ...
            </form>
        )
    }
}
```

### 훅스

후크는 Reactv16.8+에서 지원됩니다. 그것들은 우리가 기능적인 요소에서 상태를 사용할 수 있게 해준다.

### 국가를 사용하다

이것은 기능적 구성요소의 상태를 설정하는 데 사용되며 구성요소의 수명에 걸쳐 상태를 유지합니다.

```cpp
const [state, setState] = useState(0)
```

우리는 `상태`를 숫자로 유추하고 `상태 설정`을 숫자를 취하는 함수로 추론할 수 있다.

```coffeescript
const [state, setState] = useState<number, (v: boolean) => : void >(0)
```

상태가 개체를 사용하는 경우:

```java
type StateObject = {
    loading: boolean
}

const [state, setState] = useState<StateObject, (boolean) => : void >({loading: true})
```

useState 초기값이 null인 경우가 많다. 조합 유형을 사용하여 다음과 같이 명시적으로 유형을 선언할 수 있습니다.

```coffeescript
const [state, setState] = useState<StateObject | null, (boolean) => : void >(null)
```

이 `StateObject | null`은 TypeScript에 상태가 `StateObject` 유형 또는 `null` 유형일 수 있음을 알려줍니다.

### useReducer

useReduce는 우리가 상태를 유지하고 동시에 우리의 기능적인 요소로부터 상점에 조치를 보낼 수 있게 해준다. 초기 상태 및 감쇠기 기능을 사용합니다.

```js
const intialState = {
    loggedIn: false
}

function reducer(state, action) {
    // ...    
}

const [state, dispatch ] = useReducer(initialState, reducer)
```

상태 유형을 정의한 다음 반환 유형에 따라 환원 함수를 입력할 수 있습니다. 디스패치는 감소기 함수의 작업 인수 유형을 기준으로 입력할 수 있습니다.

```java
type LogginState = {
    loggedIn: boolean;
};

type ActionType = {
    type: string;
    payload: boolean;
};

const intialState: LogginState = {
    loggedIn: false
}

function reducer(state: LogginState, action: ActionType): LogginState {
    // ...    
}

const [state, dispatch ] = useReducer<LogginState, (v: ActionType) => : void>(initialState, reducer)
```

### ref를 사용하다

useRef를 사용하면 React 노드의 ref에 액세스하여 구성 요소의 수명 동안 ref를 유지할 수 있다.

null 유형은 useRef에 대부분 전달되므로 다음과 같이 표시됩니다.

```undefined
const ref= useRef<HTMLElement | null>(null)
```

참조하려는 요소의 유형을 보다 구체적으로 설명할 수 있습니다.

```undefined
// this types useRef to button elements
const buttonRef= useRef<HTMLButtonElement | null>(null)

// this types useRef to input elements
const inputRef= useRef<HTMLInputElement | null>(null)
```

리액트에서도 useRef를 사용할 수 있다.구성 요소 ":

```undefined
const inputRef= useRef<React.Component | null>(null)
```

### 컨텍스트를 사용합니다.

useContext는 우리가 그들의 일생 동안 우리의 기능적 요소에서 맥락을 만들고 유지할 수 있게 해준다. 컨텍스트는 React Context API를 사용하여 생성됩니다.

컨텍스트 사용 및 컨텍스트 만들기 기능을 사용하여 컨텍스트를 만드는 방법은 다음과 같습니다.

```cpp
const authContext = createContext({isAuth: true})
const context = useContext(authContext)
```

`createContext`의 기본값 유형은 인수 유형의 기본을 형성합니다.

```java
type AuthType = {
    isAuth: boolean;
};

const authContext = createContext<AuthType>({isAuth: true})
```

또한 `useContext` 인수 유형은 `AuthType`이 됩니다.

```cpp
const context = useContext<AuthType>(authContext)
```

## 결론

이 가이드가 유익하고 도움이 되었기를 바랍니다. 또한 이 Github repo에서 ReactTypeScript 유형 추론에 대한 자세한 내용을 읽을 수 있습니다.