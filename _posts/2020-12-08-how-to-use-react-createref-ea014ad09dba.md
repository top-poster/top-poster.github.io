---
layout: post
title: "React createRef를 사용하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2018/06/React-createref-DOM.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2018/06/React-createref-DOM.png?fit=730%2C487&ssl=1)

웹 응용 프로그램을 충분히 오래 개발했다면 jQuery, Mootools, Prototype.js 등과 같은 JavaScript DOM 라이브러리를 사용했을 가능성이 높습니다.

이러한 도서관의 등장은 인터랙티브 웹 애플리케이션이 어떻게 구축되는지에 큰 변화를 가져왔다.

DOM 추상화 API를 통해 웹 앱의 콘텐츠를 조작하는 것이 훨씬 쉬워졌습니다.

예를 들어, jQuery를 사용하여 다음과 같은 작업을 수행할 수 있습니다.

지난 몇 년 동안의 추세에 따라, 이제 Retact, Angular, Ember.js 및 Vue.js와 같은 JavaScript 프레임워크에 초점을 맞추고 있다는 사실도 알게 되었습니다.

이러한 프레임워크 사이의 한 가지 유사점은 구성요소 기반 아키텍처로 구축된다는 것이다. 그래서 요즘은 DOM 기반 상호 작용이 대부분 구성 요소 내에 캡슐화되어 있기 때문에 구성 요소에 대해 듣는 것보다 듣는 것이 더 일반적이다.

기본 제공 기능을 활용하여 이러한 최신 JavaScript 프레임워크로 많은 작업을 수행할 수 있지만 일부 기본 동작에 대해서는 실제 DOM과 상호 작용해야 할 경우가 있습니다. 대부분의 최신 프레임워크는 앱의 기본 DOM 표현에 액세스할 수 있는 API를 제공하며, React도 예외는 아닙니다.

이 튜토리얼에서는 반응 응용 프로그램에서 DOM과 상호 작용할 수 있는 방법을 고려합니다. 우리는 또한 어떻게 리액트 16.3에 소개된 리액트.createRef() 기능과 리액트 후속 버전에서 소개된 useRef훅을 사용할 수 있는지 살펴볼 것이다.

## 반응 변수와 DOM이란 무엇입니까?

React는 구성 요소에서 DOM에 액세스할 수 있는 ref라고 하는 기능을 제공합니다. 응용프로그램의 요소에 참조를 첨부하면 구성요소 내 어디에서나 구성요소의 DOM에 액세스할 수 있습니다.

또한 DOM 노드뿐만 아니라 React 요소에 직접 액세스하는 데 참조가 사용될 수 있습니다. reference와 관련하여 Return 설명서에는 다음과 같은 내용이 나와 있습니다.

> 참조는 렌더링 방법으로 작성된 DOM 노드 또는 반응 요소에 액세스하는 방법을 제공합니다.

일반적으로 상태 및 소품 메커니즘을 사용하여 필요한 상호작용을 달성할 수 없는 경우에만 참조의 사용을 고려해야 한다.

하지만 참고문헌을 사용하는 것이 적절한 경우가 한두 가지 있다. 그 중 하나는 타사 DOM 라이브러리와 통합하는 것입니다. 또한 텍스트 선택 처리 또는 미디어 재생 동작 관리와 같은 심층적인 상호 작용도 해당 요소에 대한 참조를 사용해야 합니다. 자세한 내용은 대응 참조 가이드를 참조하십시오.

## 반응에서 기준 생성

반응은 기준 생성의 세 가지 주요 방법을 제공합니다. 다음은 가장 오래된 방법부터 시작하는 다양한 방법 목록입니다.

- 문자열 참조(레거시 방법)
- 콜백 참조
- React.createRef (반응 16.3에서)
- ref 사용 후크 (반응 16.8에서)

### 반응의 문자열 참조

반응 응용 프로그램에서 참조를 만드는 기존 방법은 문자열 참조를 사용하는 것입니다. 이 방법은 가장 오래된 방법이며 이후 React 릴리스에서 제거되기 때문에 레거시 또는 더 이상 사용되지 않습니다.

문자열 참조는 단순히 원하는 요소에 ref 소자를 추가하여 ref의 문자열 이름을 값으로 전달함으로써 생성됩니다. 다음은 간단한 예입니다.

```js
class MyComponent extends React.Component {

  constructor(props) {
    super(props);
    this.toggleInputCase = this.toggleInputCase.bind(this);
    this.state = { uppercase: false };
  }
  
  toggleInputCase() {
    const isUpper = this.state.uppercase;
    
    // Accessing the ref using this.refs.inputField
    const value = this.refs.inputField.value;
    
    this.refs.inputField.value =
      isUpper
        ? value.toLowerCase()
        : value.toUpperCase();
        
    this.setState({ uppercase: !isUpper });
  }

  render() {
    return (
      <div>
        {/* Creating a string ref named: inputField */}
        <input type="text" ref="inputField" />
        
        <button type="button" onClick={this.toggleInputCase}>
          Toggle Case
        </button>
      </div>
    );
  }
  
}
```

여기서는 `<input>` 요소를 렌더링하는 간단한 Retact 구성 요소와 대문자 및 소문자 간에 입력의 대소문자를 전환할 수 있는 `<button> 요소를 생성했다.

우리는 `상한 사례` 속성이 `거짓`으로 설정된 구성 요소의 상태를 초기화했다. 이 속성을 통해 입력의 현재 사례를 결정할 수 있습니다.

우리가 <input> 요소에 대해 만든 문자열 참조에 중점을 두고 있다. 우리는 또한 `inputField`라는 이름의 `<input>` 요소에 대한 참조를 만들었다.

나중에 `<버튼`의 클릭 이벤트 핸들러에서 우리는 `this.refs.inputField`를 통해 ref에 접속했다. 그리고 입력의 값을 변경하기 위해 `<input> 요소의 DOM을 조작했다.

다음은 교호작용의 형태에 대한 예제 데모입니다.

이것은 ref를 사용하는 방법에 대한 매우 간단한 예이지만, 우리는 Retact 구성 요소에서 문자열 ref를 어떻게 사용할 수 있는지 볼 수 있었다. 앞에서 설명한 대로 Return 응용 프로그램에서 문자열 참조는 사용하지 않도록 설정해야 합니다.

다음은 문자열 참조와 관련하여 대응 설명서에 나와 있는 내용입니다.

> 현재 참조에 액세스하는 데 'this.refs.textInput'을 사용하는 경우 콜백 패턴 또는 'createRef' API를 대신 사용하는 것이 좋습니다.

### 반응에서 콜백 참조 사용

콜백 참조는 참조의 이름을 문자열로 전달하는 대신 참조 작성을 위해 콜백 함수를 사용합니다. React 16.3 이전 버전을 사용하는 경우 이 방법을 사용하는 것이 좋습니다.

콜백 함수는 Retact 구성 요소 인스턴스 또는 HTML DOM 요소를 인수로 수신하여 다른 곳에 저장하고 액세스할 수 있습니다. 콜백 참조를 사용하면 이전 코드 조각은 다음과 같습니다.

```js
class MyComponent extends React.Component {

  constructor(props) {
    super(props);
    this.toggleInputCase = this.toggleInputCase.bind(this);
    this.state = { uppercase: false };
  }
  
  toggleInputCase() {
    const isUpper = this.state.uppercase;
    
    // Accessing the ref using this.inputField
    const value = this.inputField.value;
    
    this.inputField.value =
      isUpper
        ? value.toLowerCase()
        : value.toUpperCase();
        
    this.setState({ uppercase: !isUpper });
  }

  render() {
    return (
      <div>
        {/* Creating a callback ref and storing it in this.inputField */}
        <input type="text" ref={elem => this.inputField = elem} />
        
        <button type="button" onClick={this.toggleInputCase}>
          Toggle Case
        </button>
      </div>
    );
  }
  
}
```

여기서 우리는 크게 두 가지를 변경했습니다. 먼저 콜백 함수를 거부하고 다음과 같이 `this.inputField`에 저장하는 것을 정의했습니다.

```undefined
<input type="text" ref={elem => this.inputField = elem} />
```

그런 다음 이벤트 핸들러에서 "this.refs.inputField" 대신 "this.inputField"를 사용하여 ref에 액세스합니다.

예에서와 같이 인라인 콜백 참조를 사용할 때는 구성 요소에 대한 모든 업데이트에 대해 콜백 함수를 두 번 호출하고, 먼저 `null`을 사용한 다음, 다시 DOM 요소를 사용하여 호출해야 합니다.

그러나 콜백 함수를 구성 요소 클래스의 바인딩된 메서드로 만들어 이 동작을 방지할 수 있습니다.

참조 작성에 콜백 기능을 사용하면 참조가 작성, 설정 및 설정되지 않은 방법을 보다 효과적으로 제어할 수 있습니다.

### React.createRef 사용

리액트 API에는 리액트 16.3부터 콜백 함수를 사용한 것과 거의 동일한 방식으로 ref를 생성할 수 있는 createRef() 방법이 포함되어 있었다. React.createRef()를 호출하여 ref를 생성하고 결과 ref를 요소에 할당합니다.

여기서 `Ract.createRef()를 사용하면 다음과 같은 이전 예를 볼 수 있습니다.

```js
class MyComponent extends React.Component {

  constructor(props) {
    super(props);
    this.inputField = React.createRef();
    this.toggleInputCase = this.toggleInputCase.bind(this);
    this.state = { uppercase: false };
  }
  
  toggleInputCase() {
    const isUpper = this.state.uppercase;
    
    // Accessing the ref using this.inputField.current
    const value = this.inputField.current.value;
    
    this.inputField.current.value =
      isUpper
        ? value.toLowerCase()
        : value.toUpperCase();
        
    this.setState({ uppercase: !isUpper });
  }

  render() {
    return (
      <div>
        {/* Referencing the ref from this.inputField */}
        <input type="text" ref={this.inputField} />
        
        <button type="button" onClick={this.toggleInputCase}>
          Toggle Case
        </button>
      </div>
    );
  }
  
}
```

여기 몇 가지 변화가 있습니다. 먼저, 생성자()에서 다음과 같이 React.createRef()를 거부하는 ref를 만들어 this.inputField에 저장하였다.

```coffeescript
this.inputField = React.createRef();
```

다음으로, 이벤트 핸들러에서 "this.inputField.current" 대신 "this.inputField.current"를 사용하여 refict에 액세스합니다. 이는 `React.createRef()로 작성된 참조에 주목할 필요가 있다. 노드에 대한 참조는 ref의 `current` 속성에서 액세스할 수 있게 됩니다.

마지막으로 다음과 같이 `<input> 구성 요소로 참조를 전달한다.

```undefined
<input type="text" ref={this.inputField} />
```

우리는 반응 애플리케이션에서 참조 생성의 다양한 방법을 탐구했다. 다음 섹션에서는 React.createRef의 보다 흥미로운 특징에 대해 자세히 살펴보겠습니다.

### React 'use Ref' 후크 사용

React v16에서 릴리스되면서, Hooks API는 React 애플리케이션에서 코드를 추상화하고 재사용하는 실질적인 수단이 되었다. 그러한 후크 중 하나는 기능적 구성요소에 ref를 만들어 사용할 수 있게 하는 useRef이다.

`useRef` 후크를 사용하더라도 기본적으로 함수 인스턴스를 만들 수 없기 때문에 함수 구성 요소에서 ref 속성을 사용할 수 없습니다. 우리는 이 글의 뒷부분에서 이 문제를 어떻게 해결할지 반포워딩으로 토론할 것입니다.

useRef 후크를 사용하려면 ref.current가 useRef 후크를 참조하고 호출해야 하는 객체를 전달합니다. 이 후크 호출은 앞에서 설명한 `createRef` 방법을 사용하는 것처럼 사용할 수 있는 ref 개체를 반환해야 합니다.

useRef 후크를 사용할 경우 이전의 예는 다음과 같습니다.

```js
const MyComponent = () => {
    const [uppercase, setUppercase] = React.useState(false)
    const inputField = React.useRef(null)
    const toggleInputCase = () => {
        // Accessing the ref using inputField.current
        const value = inputField.current.value;
        inputField.current.value = uppercase ? value.toLowerCase() : value.toUpperCase();
        setUppercase(previousValue => !previousValue)
    }

    return(
       <div>
           {/* Referencing the ref from this.inputField */}
           <input type="text" ref={inputField} />
           <button type="button" onClick={toggleInputCase}>
                Toggle Case  
           </button>
       </div>
```

보시다시피 코드는 React.createRef 구현의 코드와 상당히 유사합니다. 우리는 useRef 후크를 거부하고 useRef HTML 요소의 ref 속성을 참조하는 패스(pass)를 생성한다.

`<button> 요소의 이벤트 핸들러에 대해서도 프로세스는 이전과 동일합니다. 상태 변수의 현재 값인 대문자에 따라 참조할 수 있는 HTML 요소의 값 속성을 업데이트한다(`ref.current`를 사용하여 액세스할 수 있습니다.

## 반응에서 구성 요소 참조 생성

이전 섹션에서는 React.createRef API를 사용하여 ref를 생성하는 방법을 살펴보았습니다. 실제 참조는 ref의 `current` 속성에 저장됩니다.

지금까지의 예에서는 응용 프로그램의 DOM 노드에 대한 참조만 생성했습니다. 그러나 React 구성 요소에 대한 참조도 만들 수 있으므로 이러한 구성 요소의 인스턴스 방법에 액세스할 수 있습니다.

클래스 구성 요소는 마운트될 때 클래스의 인스턴스를 만들기 때문에 클래스 구성 요소에만 참조를 만들 수 있습니다. 기능 구성 요소에는 참고를 사용할 수 없습니다.

반응 구성 요소에 대한 참조를 사용하여 시연할 수 있는 매우 간단한 예를 살펴보겠습니다. 이 예에서는 다음 두 가지 구성 요소를 생성합니다.

- 단순히 <입력> 요소를 감싸고 두 가지 방법을 제공하는 FormInput 구성 요소. 하나는 입력에 어떤 값이 포함되어 있는지 알기 위한 것이고 다른 하나는 입력 텍스트를 선택하기 위한 것입니다.
- MyComponent는 FormInput 컴포넌트와 버튼을 클릭하면 입력의 텍스트를 선택할 수 있다.

구성 요소의 코드 조각은 다음과 같습니다.

```js
class FormInput extends React.Component {

  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }
  
  hasText() {
    return this.textInput.current.value.length > 0;
  }
  
  selectInputText() {
    this.textInput.current.select();
  }
  
  render() {
    return (
      <div>
        {/* Creating a string ref named: textInput */}
        &lt;input type="text" ref={this.textInput} />
      </div>
    );
  }
  
}
```

이전과 마찬가지로 React.createRef()를 refict하는 ref를 만들고 렌더 함수의 >input> 요소에 ref를 추가하였습니다. 두 가지 방법을 만들었습니다.

- `는 입력 요소의 값이 비어 있지 않음을 나타내는 부울을 반환하는 텍스트()를 가지고 있습니다. 따라서 입력이 비어 있으면 false를 반환하고, 그렇지 않으면 true를 반환합니다.
- 입력 요소에서 전체 텍스트를 선택하는 입력 텍스트()를 선택합니다.

우리가 만든 ref의 `current` 속성, 즉 `this.textInput.current`에 액세스함으로써 우리의 방법에서 입력 요소에 대한 참조를 얻는다는 점에 유의한다.

이제 `마이 컴포넌트`를 만들어 보겠습니다. 다음은 코드 조각입니다.

```js
const MyComponent = (props) => {

  const formInput = React.createRef();
  
  const inputSelection = () => {
    const input = formInput.current;
    
    if (input.hasText()) {
      input.selectInputText();
    }
  };
  
  return (
    <div>
      <button type="button" onClick={inputSelection}>
        Select Input
      </button>
      
      <FormInput ref={formInput} />
    </div>
  );
  
};
```

이 코드 조각에서는 상태 비저장 구성 요소이므로 클래스 구성 요소 대신 함수 구성 요소를 사용합니다. 또한 이 구성 요소를 사용하여 기능 구성 요소 내부에서 ref를 어떻게 사용할 수 있는지 시연하고 싶습니다.

여기서는 React.createRef()를 reficting하여 "formInput" 상수에 저장합니다. 기능 구성 요소는 클래스가 아니므로 인스턴스를 생성하지 않으므로 "this"를 사용하지 않습니다.

우리가 만든 ref를 앞에서 만든 < 구성 요소에 추가했다는 `렌더()` 방법에 주목한다. DOM 노드에 참조를 추가했던 이전 예와 달리 여기서는 구성 요소에 참조를 추가하고 있습니다.

참조는 기능 구성요소가 아닌 클래스 구성요소에 대해서만 작성할 수 있습니다. FormInput은 클래스 컴포넌트이므로 참조를 만들 수 있습니다. 그러나 이 예에서와 같이 `formInput`을 사용하여 기능 구성요소 내부에 ref를 사용할 수 있다.

마지막으로, `input Selection() 함수`에서는 이전과 같이 reference의 `current` 속성을 사용하여 구성 요소에 대한 참조에 액세스한다.

참조가 FormInput 구성 요소의 인스턴스를 가리키기 때문에 FormInput 구성 요소의 hasText() 및 SelectInputText() 메서드에 액세스할 수 있습니다. 이렇게 하면 기능 구성 요소에 대한 참조를 만들 수 없는 이유가 확인됩니다.

다음은 교호작용의 형태에 대한 예제 데모입니다.

![image](https://i1.wp.com/storage.googleapis.com/blog-images-backup/1*4ShHAxNZEK5Ehzz4gUTg-w.gif?resize=960%2C540&ssl=1)

## 제어되지 않는 구성 요소의 참조

React는 양식 데이터 변경 시 구성요소 업데이트 처리를 담당하기 때문에 기본적으로 React의 모든 구성요소가 제어됩니다.

React에서 제어되지 않는 구성 요소를 다룰 때 참조는 매우 유용합니다. 그 이유는 양식 데이터가 변경될 때 이벤트 핸들러를 사용하여 상태를 업데이트하는 대신 DOM에서 양식 값을 가져오는 참조에 의존하기 때문입니다. 제어되지 않는 구성 요소에 대해 자세히 알아볼 수 있습니다.

간단한 제어 구성 요소를 생성한 다음 제어되지 않는 구성 요소를 생성하여 참조를 사용하여 DOM에서 폼 값을 가져오는 방법을 시연해 보겠습니다.

```js
class ControlledFormInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = { value: "Glad" };
  }
    
  handleChange(evt) {
    this.setState({ value: evt.target.value });
  }
  
  render() {
    return (
      <div>
        <h1>Hello {this.state.value}!</h1>
        <input type="text" value={this.state.value} onChange={this.handleChange} placeholder="Enter your name" />
      </div>
    )
  }
}
```

위의 코드 조각은 `<input> 요소를 포함하는 제어된 구성 요소를 보여줍니다. `입력` 요소의 가치는 국가의 `가치` 속성에서 비롯된다는 점에 유의하십시오. 우리는 국가의 `값`을 `글래드`(아무튼 내 이름)로 초기화했다.

또한 handleChange() 이벤트 핸들러를 사용하여 상태의 `value` 속성을 `evt.target.value`에 액세스하여 얻은 입력 요소의 값으로 업데이트합니다.

다음은 교호작용의 형태에 대한 예제 데모입니다.