---
layout: post
title: "대응 대 웹 구성 요소"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/reactvswebcomponents.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/reactvswebcomponents.png?fit=730%2C487&ssl=1)

이 기사에서는 웹 컴포넌트와 리액션에 대해 알아보겠습니다. 앞으로 나아가기 전에 여기서 주목해야 할 한 가지는 React와 웹 구성요소가 서로 다른 용도로 사용된다는 것입니다. 웹 구성요소를 통해 재사용 가능하고 강력하게 캡슐화된 사용자 정의 요소를 작성할 수 있습니다. 그러나 React는 UI 개발의 상태 관리 문제를 해결하는 선언형 자바스크립트 라이브러리이다. 오늘은 리액트 및 웹 구성요소에서 스타일링을 위해 제공되는 구성요소 유형, 라이브러리 및 접근성에 대해 설명하겠습니다.

## 구성 요소들

이미 설명한 대로 웹 구성 요소로 재사용 가능한 UI 요소를 작성할 수 있습니다. 하지만 React를 사용하면 동일한 작업을 수행할 수 있습니다. 예를 들어, 구성 요소를 생성하여 프로젝트의 다른 위치에 재사용할 수 있습니다. 여기 차이점이 있습니다. 반응 구성 요소는 반응 응용 프로그램에서만 재사용할 수 있습니다. 반면에 웹 구성 요소는 어디에서나 사용할 수 있습니다. HTML 사양에 포함되어 있고 네이티브이기 때문에 React, Angular, Vue 등에서 사용할 수 있습니다. 예를 들어, 웹 구성요소를 사용하여 작성된 사용자 정의 헤더 요소는 다양한 라이브러리와 프레임워크에서 사용될 수 있습니다.

이에 대해 설명드리면서, 두 사람이 함께 제공한 구성 요소의 유형에 대해 이야기해 보겠습니다.

## 웹 구성요소 유형

웹 구성 요소에는 세 가지 유형이 있습니다.

### 사용자 정의 요소

사용자 정의 요소를 사용하여 새로운 사용자 정의 HTML 태그를 생성할 수 있습니다. 브라우저의 JavaScript API 사용자 정의를 사용합니다.Elements.define() 메서드를 선택합니다. 사용자 지정 요소는 `HTMLEment`를 확장하는 JavaScript 클래스입니다. 또한 사용자 정의 요소는 HTML 파서가 인식할 수 있도록 이름에 하이픈이 필요합니다. 다음 예를 고려하십시오.

```xml
<!DOCTYPE html>
<html>
<head>
<title>Custom Element</title>
</head>
<body>
<div>
<h1>Custom Element</h1>
<test-element></test-element>
</div>

<script type="text/javascript">
class TestElement extends HTMLElement{
constructor() {
  super();
  this.setAttribute('name', 'ashton'); //setting an attribute

}
connectedCallback() {
    this.innerHTML = `<div><p>Hello ${this.getAttribute('name')}!</p><p>Nice to meet you</p></div>`;
  }
}
//register the custom element
customElements.define('test-element', TestElement);
</script>
</body>
</html>
```

#### 출력:

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/customelement.png?resize=514%2C236&ssl=1)

### 섀도 DOM

이렇게 하면 분리된 구성요소, 즉 문서를 작성할 수 있습니다.쿼리 선택기(`)는 섀도 DOM에 정의된 노드를 반환하지 않습니다. 더욱이, 그림자 DOM에 정의된 스타일은 CSS 범위와 같이 포함되어 있다. 섀도 DOM에서 div의 배경색을 파란색으로 변경하면 해당 div의 배경만 변경되고 그 밖의 div는 영향을 받지 않습니다.

```xml
<!DOCTYPE html>
<html>
<head>
<title>Custom Element</title>
</head>
<body>
<div>
<h1>Custom Element</h1>
<test-element></test-element>
</div>

<script type="text/javascript">
class TestElement extends HTMLElement{
constructor() {
  super();
  this.setAttribute('name', 'ashton'); //setting an attribute
  const shadowRoot = this.attachShadow({mode: 'open'}); //attaches a shadow DOM tree to <test-element> and returns a reference

}
connectedCallback() {
    this.shadowRoot.innerHTML = `<style>
     div {
     background:cyan;
     }
    </style>
    <div><p>Hello ${this.getAttribute("name")}!</p><p>Nice to meet you</p></div>`;
  }
}
//register the custom element
customElements.define('test-element', TestElement);
</script>
</body>
</html>
```

#### 출력:

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/shadowdombluebackground.png?resize=902%2C202&ssl=1)

### 템플릿

템플릿을 사용하면 `<template> 태그를 사용하여 로드 시 마크업 구조를 선언할 수 있습니다. 이 요소와 구성요소의 내용은 참조를 가져와 DOM에 수동으로 연결할 때까지 렌더링되지 않습니다. 동적 데이터를 추가할 수 있습니다. 따라서 여러 번에 걸쳐 일부 마크업 구조를 사용해야 하는 경우 템플릿을 사용할 수 있으며 동일한 코드를 반복하지 않도록 할 수 있습니다.

```xml
<!DOCTYPE html>
<html>
<head>
<title>Custom Element</title>
</head>
<body>
<div>
<h1>Custom Element</h1>
<template id="student_template">
<style>
    li {
      color:cyan;
      list-style: none;
    }
  </style>
<li>
    <span class="name"></span> &mdash;
    <span class="score"></span>
</li>
</template>
<test-element></test-element>
<ul id="students"></ul>
</div>

<script type="text/javascript">
class TestElement extends HTMLElement{
constructor() {
  super()
  const shadowRoot = this.attachShadow({ mode: "open" });

}
connectedCallback() {
let students = [
{name: "Emma", score: 90},
{name: "Ashton", score: 70},
{name: "Hannah", score: 85},
{name: "Steve", score: 90},
{name: "Amy", score: 75},
]

    students.forEach(student=>{
    let template = document.getElementById("student_template");
    let templateContent = template.content.cloneNode(true);
    this.shadowRoot.appendChild(templateContent); //append a clone of the template content to the shadow root

    this.shadowRoot.querySelector('.name').innerHTML = student.name //add the name
    this.shadowRoot.querySelector('.score').innerHTML = student.score //add the score


    document.getElementById("students").appendChild(this.shadowRoot) //append the list to the ul students
    })
  }
}
//register the custom element
customElements.define('test-element', TestElement);
</script>
</body>
</html>
```

#### 출력:

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/template.png?resize=542%2C212&ssl=1)

## 반응 구성 요소

반응 구성 요소에는 두 가지 주요 유형이 있습니다.

### 클래스 구성 요소

클래스 구성 요소는 단순히 `Ract`를 확장하는 JS 클래스입니다.구성 요소의 클래스입니다. 소품을 사용하고 구성 요소의 상태를 관리하며 렌더링 기능을 통해 JSX 코드를 반환합니다. 소품에는 상위 구성 요소에서 전달된 데이터가 포함됩니다. 구성 요소의 데이터는 상태 개체에 저장할 수 있습니다. 상태가 변경될 때마다 구성 요소가 다시 렌더링됩니다. 또한 구성 요소를 생성하는 시점부터 파괴될 때까지 서로 다른 지점에서 실행되는 라이프사이클 메소드도 포함되어 있습니다. 예를 들어, 생성자()는 구성 요소의 인스턴스가 시작될 때 호출되는 첫 번째 방법이며, 따라서 여기서 상태가 초기화된다.

예를 들어 보겠습니다.

```js
import React from 'react';
import './style.css';

class MyComponent extends React.Component {

  constructor(props) {
    super(props);

    this.state = {
        value:"Touch me"
    };
  }
  buttonHandler=()=>{
    if (this.state.value=="Touch me"){
      this.setState({value:'Touched!'})
    }
    else {
      this.setState({value:'Touch me'})
    }
  }
  render() {
    return(
      <div>
          <h1>My Component!</h1>
          <button onClick={this.buttonHandler} className="button">{this.state.value}</button>
      </div>
    );
  }
}
export default MyComponent;
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/mycomponentbutton.png?resize=544%2C292&ssl=1)

버튼을 눌렀을 때:

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/mycomponentwithtouchedbutton.png?resize=560%2C276&ssl=1)

### 기능 구성 요소

간단히 말해, 기능 컴포넌트는 JSX 코드를 반환하는 자바스크립트 함수이다. 이전에는 기능 구성요소가 국가 관리를 지원하지 않았기 때문에 순전히 표현 구성요소였다. 그러나 React Hooks를 사용하면 이제 상태 및 수명 주기 방법도 사용할 수 있습니다.

기능 구성 요소를 사용하여 위의 예를 들어 보겠습니다.

```js
import React, {useState} from 'react';
import './style.css';

const MyComponent = (props) =>{
    const [value, setValue] = useState("Touch me"); //creating a state using the useState hook

    const buttonHandler=()=>{
    if (value=="Touch me"){
      setValue("Touched!") //set the value to Touched! if it is Touch me, changing state
    }
    else {
      setValue("Touch me") //changing state
    }
    }

    return (
      <div>
          <h1>My Component!</h1>
          <button onClick={buttonHandler} className="button">{value}</button>
      </div>
    )


}
export default MyComponent;
```

버튼을 눌렀을 때:

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/functionalcomponentbutton.png?resize=532%2C262&ssl=1)

## 스타일링

구성 요소를 구축하는 데 있어서 UI는 큰 부분을 차지합니다. 사용자는 항상 아름답고, 매력적이며, 사용하기 쉬운 구성 요소를 만들어야 합니다. 하지만 처음부터 직접 스타일링하는 것은 다소 지루하고 시간이 많이 걸리는 과정일 수 있습니다. 게다가 React에서는 웹 컴포넌트처럼 스코프 스타일이 없기 때문에 더 어려워진다. 그래서, 여러분은 항상 여러분을 위해 일을 할 수 있는 외부 모듈을 찾습니다.

리액트는 생태계가 넓다. 따라서 웹 구성요소에 비해 스타일링 라이브러리와 프레임워크가 많다.

### 반응하다

#### 재료-UI

React에서 가장 인기 있는 UI 프레임워크 중 하나입니다. 그것은 구글에 의해 만들어지고 유지된다. 몇 가지 UI 구성 요소, 스타일, 테마, 레이아웃 및 아이콘 등을 제공합니다. 그것은 매우 다재다능하며, 그것을 계속 개선하기 위해 지속적인 노력과 노력이 요구되고 있다. 최신 안정 버전은 2020년 12월 현재 v4.11.1이다.

#### 대응 부트스트랩

리액트 부트스트랩은 부트스트랩 자바스크립트를 대체하는 또 다른 인기 있는 리액트 UI 프레임워크이다. 그것은 다양한 구성 요소, 메잉 서포트, 레이아웃 등을 제공합니다. 또한 대규모 성장 팀이 있으며 최신 버전은 부트스트랩 4.5를 지원하는 v1.4.0이다.

#### 시맨틱 UI 반응

그것은 시맨틱 UI를 위한 리액션 통합이다. 단추, 컨테이너, 로더 및 입력 등과 같은 여러 가지 멋진 사용자 지정 요소를 제공합니다. 그것은 시맨틱 UI와 같은 스타일링 시스템과 테마를 가지고 있다. 그래서, 만약 여러분이 그것을 안다면, 여러분은 새로운 것을 배울 필요가 없습니다. 또한 기본 마크업에 액세스할 수 있는 하위 구성 요소를 갖추고 있으므로 요소 사용자 지정에 있어 유연성을 확보할 수 있습니다.

### 웹 구성 요소

#### 재료 웹 구성요소

이것은 Material-UI 프레임워크의 웹 구성요소 버전입니다. 하지만, 그것은 여전히 진행 중인 작업입니다. 최신 버전은 v0.20.0이며 1.0 릴리스까지 큰 변화가 예상된다.

#### 웹 구성 요소의 부트스트랩

웹 구성 요소에서 부트스트랩을 사용할 수 있는 모듈이 있습니다. 그러나 일부는 여전히 불안정하거나 개발 중에 있으며 다른 일부는 React-Bootstrap만큼 많은 기능을 제공하지 않습니다. 예를 들어, 부트스트랩-웹 구성 요소는 웹 구성 요소를 위한 부트스트랩의 재작성이지만 여전히 개발 모드에 있습니다. 아이볼릿 부트스트랩은 부트스트랩에서 영감을 받은 웹 구성요소 집합을 제공하지만 광범위한 구성요소 및 기능을 가지고 있지 않다.

#### 엘릭스

캐러셀, 버튼, 메뉴 등 공통 UI 패턴을 위한 바로 사용할 수 있는 웹 컴포넌트의 오픈 컬렉션입니다. 또한 기존 Elix 구성 요소를 기반으로 사용자 지정하거나 새로운 요소를 만들 수 있는 유연성을 제공합니다.

## 접근성(a11y)

웹 사이트의 접근성에 대해 이야기할 때, 우리는 모든 사람이 쉽게 사용할 수 있다는 것을 의미합니다. 당사의 웹 사이트가 Reactor 웹 구성 요소로 만들어진 경우 이러한 구성 요소에 액세스할 수 있어야 합니다. 그래서, 의문이 생기죠, 그렇죠?

반응을 통해 완전히 액세스할 수 있는 웹 사이트를 만들 수 있습니다. 표준 HTML 기술을 사용하여 수행할 수 있는 경우가 많습니다. 예를 들어, JSX의 `aria-*` 속성을 일반 HTML에서 사용하는 것과 동일한 방식으로 사용할 수 있습니다. 즉, camel Case가 필요하지 않습니다. 리액트에서는 모든 요소를 감싸는 추가 디브를 추가하지만 HTML 의미론을 깨지 않을까요? 그래, 하지만 우리는 <반응>을 사용하면 이 문제를 쉽게 해결할 수 있어.조각>` 태그.

접근성의 또 다른 측면은 애플리케이션을 키보드에서만 사용할 수 있어야 한다는 것이다. 이제 React는 런타임 동안 HTML DOM을 지속적으로 변경하며, 이로 인해 키보드 포커스가 손실될 수 있습니다. 그러나 구성 요소의 다양한 라이프사이클 방법에 포커스를 프로그래밍 방식으로 설정할 수 있다. 또한, "react-document-title"을 사용하여 화면(구성 요소)이 변경될 때마다 제목을 변경할 수 있습니다.

사용자 지정 요소가 모든 기본 HTML 요소를 확장할 수 있기 때문에 웹 구성 요소도 액세스할 수 있으며, 따라서 접근성 기능을 포함한 모든 특성을 상속합니다. 또한 섀도 DOM 내에서 생성된 요소에 액세스할 수 없다는 오해가 있을 수 있습니다. 그러나, 이것은 사실이 아니며, 화면 리더는 내용에 쉽게 접근할 수 있다.

이 글은 여기까지입니다. 그 차이점들은 아래 표에 요약되어 있습니다.