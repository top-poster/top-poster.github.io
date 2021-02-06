---
layout: post
title: "자신만의 스타일 구성 요소 라이브러리 구축"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/styled-components-tagged-template-literals.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/styled-components-tagged-template-literals.png?fit=730%2C487&ssl=1)

스타일링된 구성 요소가 펑 하고 등장하여 인라인 스타일의 React 구성 요소를 생성하는 방법에 대한 우리의 관점을 바꾸었다.

이 튜토리얼에서는 자신만의 스타일 구성 요소를 구축하는 방법에 대해 설명합니다. 이렇게 하면 후드 아래에서 스타일 구성 요소와 태그가 지정된 템플릿 리터럴이 어떻게 작동하는지 확인할 수 있습니다.

다음 내용을 다루겠습니다.

- 스타일 구성 요소란 무엇입니까?
- 태그가 지정된 템플릿 리터럴이란?
- 스타일 구성 요소의 작동 방식
- 자신만의 스타일 구성 요소를 구축하는 방법
- 스타일 구성 요소에 Ming 기능 추가

이 구현에 대한 전체 소스 코드는 GitHub에서 사용할 수 있다.

## 스타일 구성 요소란 무엇입니까?

스타일 구성 요소는 구성 요소와 스타일 간의 매핑을 없애도록 설계되었으므로 스타일을 정의할 때는 스타일을 첨부한 상태로 일반 반응 구성 요소를 구축하기만 하면 됩니다.

다음과 같이 빠른 인라인 스타일 구성 요소를 작성할 수 있습니다.

```cpp
js
const Button = styled.button`
    background-color: green;
```

그러면 배경색이 파란색으로 설정된 버튼 구성 요소(반응 구성 요소)가 생성됩니다. 버튼은 HTML 버튼을 렌더링하는 일반 React 구성 요소입니다. 뒷부분의 스타일링 코드가 HTML 버튼에 적용됩니다.

이렇게 사용할 수 있습니다.

```xml
js
<Button>Click Me</Button>
```

따라서 다음과 같은 사항을 작성하는 것과 같습니다.

```js
js
class Button extends Component {
    render() {
        return (
            <button style={
                background-color: blue
            }>{this.props.children}</button>
        )
    }
}
```

styled-components는 일반 HTML 태그의 배열을 특징으로 하며, 우리는 이 태그를 사용하여 해당 태그의 styleRact 구성 요소 버전을 만들 수 있다. 예를 들어:

- `스타일링`button`은 `button` 요소를 렌더링하는 react 구성 요소를 생성합니다.
- styled.div는 `div` 요소를 렌더링하는 Retact 구성 요소를 생성합니다.
- `스타일링`a는 앵커 `a` 요소를 렌더링하는 Retact 구성 요소를 생성합니다.

## 태그가 지정된 템플릿 리터럴이란?

styled-components는 JavaScript의 `[Tagged Template Liter]() 기능`을 사용하여 구성 요소를 스타일링합니다. 태그가 지정된 템플릿 리터럴을 사용하면 리터럴의 구문 분석을 보다 효과적으로 제어할 수 있습니다. 함수를 사용하여 템플릿 리터럴을 구문 분석할 수 있습니다.

태그가 지정된 템플릿 리터럴 구문은 다음과 같습니다.

```coffeescript
js
taggedFunction`string here
```

태그가 붙은함수`는 함수이며 백틱에는 문자열이 포함되어 있습니다. 태그가 붙은함수`는 다음과 같습니다.

```js
js
function taggedFunction(strings) {
    // ...
}
```

뒷부분의 문자열은 `태그`로 전달됩니다.배열의 문자열 매개 변수에서 함수 `함수` 함수 값은 템플릿 리터럴, 백틱 문자열에 포함될 수 있습니다.

```js
js
const val = 90
taggedFunction`string here ${val}
```

val은 템플릿 리터럴의 값입니다. JavaScript가 tagged에 문자열을 전달합니다.함수 ` 다음에 리터럴 값이 나옵니다.

```js
js
function taggedFunction(strings, val1) {
    // ...
}
```

문자열 매개 변수는 템플릿 리터럴의 문자열을 포함하는 배열입니다. val1 매개변수는 val 값을 보유합니다.

태그가 지정된 템플릿 리터럴에 두 개의 값이 있는 경우...

```js
js
const val = 90
const val2 = 900
taggedFunction`string here ${val} string2 ${val2}
```

…그럼 우리의 `자신`은함수 `는 다음과 같습니다.

```js
js
function taggedFunction(strings, val1, val2) {
    // ...
}
```

- ➡: 문자열이 포함됩니다.
- val1: ${val}는 90으로 고정됩니다.
- val2: ${val2}는 900을 보유합니다.

값의 매개 변수를 정의하는 대신 다음과 같이 단일 배열로 저장할 수 있습니다.

```js
js
function taggedFunction(strings, ...vals) {
    // ...
}
```

vals는 템플릿의 모든 값을 리터럴로 저장하는 배열입니다.

이것으로…

```js
js
const val = 90
const val2 = 900
taggedFunction`string here ${val} string2 ${val2}
```

…그 `스파이브`함수 `는 다음을 수신합니다.

➡:

```undefined
[ "string here ", " string2 ", "" ]
```

vals:

```undefined
[ 90, 900 ]
```

JavaScript는 값이 발생하는 지점에서 문자열을 중단합니다.

```undefined
string here ${val} string2 ${val2}
```

위의 항목은 `${val}` 및 `${val2} 지점에서 중단됩니다.

```undefined
string here ${val} string2 ${val2}
["string here ", "string2 ", ""]
```

이제 우리는 보간법을 사용하여 그것들을 값과 쉽게 결합할 수 있으며, 우리는 문자열 파라미터에서 CSS 코드를 받을 수 있다는 것을 안다.

```coffeescript
js
styled.button`
    background-color: blue;

```

그래서 `태그`는`스타일링` 뒤에 있는 함수 또는 함수입니다.버튼`은 다음을 수신합니다.

➡:

```undefined
[`
    background-color: blue;
`]
```

CSS 코드에 다음과 같은 값이 포함되어 있다면...

```js
js
const mainColor = "blue";
styled.button`
    background-color: ${mainColor};

```

태그가 지정된 함수는 다음을 수신합니다.

➡:

```undefined
[`
    background-color: `, `;`]
```

vals:

```undefined
[ "blue" ]
```

## 스타일 구성 요소의 작동 방식

우리는 `스타일링된` 개체에서 `스타일링된` 개체를 가져옵니다.

```coffeescript
js
import styled from "styled-components"
```

우리는 `tyled` 객체의 HTML 태그를 사용하여 인라인 방식의 구성 요소를 생성한다.

```css
js
styled.button
styled.div
```

따라서 우리는 `tyled` 객체가 HTML 태그를 속성으로 포함하고 그 값으로서 기능을 가지고 있다는 것을 알고 있으므로, `tyled`는 다음과 같이 보일 것이다.

```js
js
const styled = {
    button: function(strings, ...vals) {},
    div: function(strings, ...vals) {},
    ...
}
```

함수(strings, ... vals) {}은(는) 문자열 매개 변수에서 CSS 스타일링 코드를 수신하고 그 값을 vals 매개 변수에서 수신하는 태그 지정 함수입니다.

```cpp
js
const Button = styled.button
const Div = styled.div
```

위의 내용은 반응 구성 요소를 반환합니다. 버튼과 Div는 각각 버튼과 Div를 렌더링하는 리액트 구성 요소입니다.

## 자신만의 스타일 구성 요소를 구축하는 방법

이제 태그가 지정된 템플릿 리터럴과 스타일 구성 요소의 작동 방식을 이해했으므로 스타일 구성 요소 라이브러리를 직접 구축해 보겠습니다.

아래 단계를 수행하여 시스템에서 Node.js 프로젝트를 비계합니다.

```bash
mkdir styled-c
cd styled-c
npm init -y
touch index.js
```

우리의 모든 코드는 `index.js` 파일에 있을 것이다. 저희는 스타일 구성 요소 스타일을 흉내낼 거예요.

먼저 react에서 Component를 가져올 것입니다.

```js
js
// index.js
import React, { Component } from 'react';
```

그런 다음 HTML 태그 이름을 저장할 `스타일링` 개체와 배열을 만듭니다.

```cpp
js
const tags = [
    "button",
    "div"
]
const styled = {}
```

HTML 태그 이름을 속성으로 사용하여 스타일링된 개체를 동적으로 채우고 `genComponentStyle` 함수를 호출합니다.

```js
js
const tags = [
    "button",
    "div"
]
const styled = {}
tags.forEach(tag => {
    styled[tag] = genComponentStyle(tag)
})
```

`tag`는 태그 배열에 있는 HTML 태그 이름입니다.

위의 코드로 스타일링된 객체는 태그 배열의 HTML 태그를 속성으로 가집니다. 이 값은 함수여야 합니다. 즉, 템플릿 리터럴과 그 안에 있는 값을 수신하는 태그가 지정된 함수입니다. genComponentStyle 함수는 모든 태그에서 호출됩니다. genComponentStyle은 태그 이름에 대한 닫힘을 제공하며 React 구성 요소를 반환해야 합니다.

genComponentStyle 기능을 구현하려면:

```js
js
function genComponentStyle(tag) {
    return function(strings, ...vals) {
        return class extends Component {
            constructor(props) {
                super(props)
                this.style = {}
            }
            componentWillMount() {
                this.style = computeStyle(this.props, strings, vals)
            }
            componentWillUpdate(props) {
                this.style = computeStyle(props, strings, vals)
            }
            render() {
                return (
                    createElement(tag, { style: this.style, ...this.props }, [...this.props.children])
                )
            }
        }        
    }
}
```

genComponentStyle` 함수는 태그가 지정된 함수를 반환합니다. 이 함수는 `tyled` 개체의 HTML 태그 속성에 할당되고 템플릿 리터럴과 HTML 태그에 호출된 값을 수신합니다. 반응 구성 요소를 반환합니다.

이 함수는 백틱스에서 CSS 코드를 수신하기 때문에 문자열을 구문 분석하고 여기서 style 객체를 생성해야 한다.

이를 변환해야 합니다.

```coffeescript

    color: white;
    background-color: blue;
    padding: 20px;

```

이렇게 하려면:

```bash
js
{
    "color": "white",
    "background-color": "blue",
    "padding": "20px"
}
```

이것은 우리가 물체에 스타일을 배치하고 그것을 `스타일` 소품으로 전달함으로써 리액트 구성요소를 스타일링하기 때문이다.

```undefined
js
Click Me
```

computeStyle 기능이 바로 그것이다. 문자열과 vals 매개 변수에서 스타일을 계산해 this.style로 설정한다. 그런 다음 구성 요소는 `createElement` 함수를 사용하여 `tag`의 요소를 렌더링합니다.

```css
js
createElement(
    tag,
    { style: this.style, ...this.props }, [...this.props.children])
```

첫 번째 arg는 생성할 HTML 요소입니다. 두 번째 매개변수는 소품입니다. 보시다시피, 우리는 "this.style"을 값으로 하는 "style" 속성을 가지고 있습니다. 이렇게 하면 HTML 요소에 `style` prop가 추가되어 백틱 문자열에서 계산된 스타일로 요소를 효과적으로 스타일링합니다. 세 번째 매개 변수는 구성 요소의 태그 간에 렌더링될 하위 구성 요소를 설정합니다.

구성 요소에는 `component WillMount`와 `component WillUpdate`의 두 가지 라이프사이클 후크가 있습니다.
Component WillMount는 구성 요소의 초기 마운트에서 호출되며, 스타일을 계산하여 this.style에 할당합니다. 이렇게 하면 요소가 DOM에 마운트되기 전에 인라인 스타일이 계산됩니다.

인라인 스타일은 `componentWillUpdate`에서도 계산됩니다. 이렇게 하면 구성 요소를 다시 렌더링할 때마다 요소의 인라인 스타일이 새로 고쳐지고 변경 시 요소가 스타일을 업데이트합니다.

`computeStyle` 구현은 다음과 같습니다.

```js
js
function computeStyle(props, strings, vals) {
    strings = evalInterpolation(props, strings, vals)
    const style = {}
    strings.split(";").forEach((str)=> {
        let [prop, val] = str.trim().split(":")
        if(prop !== undefined && val !== undefined) {
            prop = prop.trim()
            val = val.trim()
            style[prop] = val
        }
    });
    return style
}
```

컴퓨팅 스타일은 props 매개 변수에서 구성 요소의 소품, 문자열 매개 변수에서 리터럴된 템플릿, vals 소품에서 값을 받아들인다. 함수에 전달된 백틱에서 스타일을 계산합니다. evalIntercolation` 함수는 템플릿 리터럴의 값을 평가하고 평가된 문자열을 반환합니다.

ComputeStyle은 문자열이 발생하는 모든 위치에서 문자열을 분할합니다. 이렇게 하면 CSS 선택기가 `;로 분할되므로 문자열에 있는 각 CSS 선택기를 가져올 수 있습니다. 그리고 나서, 각각의 셀렉터를 얻기 위해 그 위를 고리 모양으로 감습니다. 셀렉터를 `:`에서 분할하여 셀렉터 속성과 속성 값을 가져옵니다.

우리는 부동산과 그 가치를 각각 prop와 val에 할당한다. 그런 다음 객체 `스타일`로 조립합니다. 완료되면 개체에서 CSS 선택기의 속성 및 값을 포함하는 개체 `style`이 반환됩니다.

`평가간섭`의 구현은 다음과 같다.

```js
js
function evalInterpolation(props, strings, vals) {
    let resultStr = ""
    for (var i = 0; i < strings.length; i++) {
        var str = strings[i];
        var val
        if(vals) {
            val = vals[i]
            if(val !== undefined) {
                if(typeof val === "function") {
                    val = val(props)
                }
                str += val
            }
        }
        resultStr += str
    }
    return resultStr
}
```

이 함수는 문자열 배열을 순환하고 문자열 배열을 동일한 값 인덱스와 결합하여 문자열로 값을 보간합니다. 값이 함수인 경우 소품으로 호출되고 결과는 현재 문자열과 결합됩니다.

이렇게 하면 템플릿 리터럴의 함수를 사용할 수 있습니다.

```js
js
const Button = styled.button`
    background-color: ${(props) => props.theme.bgColor};
    padding: ${props => props.small ? '2px 4px' : '6px 14px'};

```

함수는 항상 `구성 요소` 소품을 인수로 받아들여야 합니다.

이것으로 우리의 코드는 완성되었다.

```js
js
// index.js
import React, { createElement, Component } from 'react';
const tags = [
    "button",
    "div"
]
function evalInterpolation(props, strings, vals) {
    let resultStr = ""
    for (var i = 0; i < strings.length; i++) { var str = strings[i]; var val if(vals) { val = vals[i] if(val !== undefined) { if(typeof val === "function") { val = val(props) } str += val } } resultStr += str } return resultStr } function computeStyle(props, strings, vals) { strings = evalInterpolation(props, strings, vals) const style = {} strings.split(";").forEach((str)=> {
        let [prop, val] = str.trim().split(":")
        if(prop !== undefined && val !== undefined) {
            prop = prop.trim()
            val = val.trim()
            style[prop] = val
        }
    });
    return style
}
function genComponentStyle(tag) {
    return function(strings, ...vals) {
        return class extends Component {
            constructor(props) {
                super(props)
                this.style = {}
            }
            componentWillMount() {
                this.style = computeStyle(this.props, strings, vals)
            }
            componentWillUpdate(props) {
                this.style = computeStyle(props, strings, vals)
            }
            render() {
                return (
                    createElement(tag, { style: this.style, ...this.props }, [ ...this.props.children ])
                )
            }
        }        
    }
}
const styled = {}
tags.forEach(tag => {
    styled[tag] = genComponentStyle(tag)
})
export default styled
```

스타일링된 버튼 구성 요소를 생성하려면:

```js
js
// test.js
import styled from "./"
const Button = styled.button`
    padding: 6px 12px;
    background: palevioletred;
    color: white;
    border: 1px solid transparent;
    border-radius: 4px;
    font-size: 16px;
    margin: 2px;
`
<button>Button</button>
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/styled-components-button-component.png?resize=720%2C144&ssl=1)

반응 앱에서 스타일링된 버튼 구성 요소를 사용하려면:

```js
js
// App.js
import React from 'react';
import "./App.css"
import styled from "./"
const Div = styled.div`
    border: 2px solid palevioletred;
    border-radius: 3px;
    padding: 20px;
`
const Button = styled.button`
    padding: 6px 12px;
    background: palevioletred;
    color: white;
    border: 1px solid transparent;
    border-radius: 4px;
    font-size: 16px;
    margin: 2px;
`
class App extends React.Component {
    render() {
        return (
          <div>
            <button>Button1</button> 
            <button>Button2</button> 
            <button>Button3</button>
          </div>
) } } export default App
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/styled-button-component-react-app.png?resize=720%2C213&ssl=1)

축하합니다! 자신만의 스타일 구성 요소를 구축했습니다.

당사의 스타일 구성 요소는 버튼 및 div 태그만 지원합니다. 다른 HTML 요소를 추가하는 방법은 다음과 같습니다.

```cpp
js
const tags = [
    "button",
    "div",
    "a",
    "input",
    "select"
]
```

## 스타일 구성 요소에 Ming 기능 추가

스타일 구성 요소는 테마 구성 요소에 사용되는 테마 공급자 구성 요소를 내보냅니다.

스타일 구성 요소에 테마 기능을 추가하려면 테마 제공 r의 테마 소프로피스에 테마를 포함하는 개체를 전달하십시오. 그런 다음 테마로 지정하려는 스타일 구성 요소가 `테마 공급자` 태그 사이에 배치됩니다. `props`를 참조합니다.스타일 구성 요소 CSS의 테마 속성.

테마 공급자 구성 요소를 추가하려면 컨텍스트 만들기를 사용하여 컨텍스트를 만들고 공급자 구성 요소를 사용하여 테마 소품의 테마를 스타일 구성 요소 트리 아래로 전달합니다.

```php
js
import React, { createElement, Component, useContext } from 'react';
const ThemeContext = React.createContext()
...
function ThemeProvider(props) {
    const outerTheme = props.theme
    const innerTheme = useContext(ThemeContext)
    const theme = { ... outerTheme, ... innerTheme }
    return (
        
            
                {props.children}
            
        
    )
}
...
export {
    ThemeProvider
}
```

우리는 useContext 후크를 수입했다. 그런 다음 "React.createContext"()를 사용하여 컨텍스트(`테마 컨텍스트`)를 만들었습니다.

우리의 `테마 프로바이더`는 기능 구성요소이다. 소품에서 테마 객체를 받아들이기 때문에 소품 객체의 테마를 참조해 `외부`에 저장한다.테마 var. 그런 다음, 우리는 우리의 `테마 컨텍스트`에 있는 내부 테마를 `컨텍스트 사용` 후크를 사용하여 소비한다. 우리 `콘텍스트`에는 초기 테마가 없지만, `테마콘텍스트`에 내부 테마를 추가하기로 결정한다면 코드가 깨지지 않도록 사용하였습니다.

다음으로는 `내부`를 병합합니다.테마 및 `외부`하나의 테마에 테마. 그런 다음 `테마 제공자` 구성 요소의 하위 구성 요소를 렌더링합니다. 이 아동 소품들은 `테마콘텍스트` 사이에 끼워져 있다.`테마 컨텍스트`의 공급자` 구성 요소 우리는 주제를 테마 컨텍스트에 전달합니다.가치 제안을 통한 공급자입니다. 이렇게 하면 테마를 하위 구성 요소에서 사용할 수 있습니다.

이 {TemeProvider}을(를) 통해 가져올 `테마 제공자`를 내보냅니다.

이제 모든 스타일 구성 요소에 대해 반환되는 구성 요소를 수정하여 테마 컨텍스트를 사용할 수 있도록 합니다.

```js
js
...
function genComponentStyle(tag) {
    return function(strings, ...vals) {
        return class extends Component {
            static contextType = ThemeContext
            constructor(props, context) {
                super(props, context)
                this.style = {}
            }
            componentWillMount() {
                if(this.context)
                    this.props = { ...this.props, theme: this.context}
                this.style = computeStyle(this.props, strings, vals)
            }
            componentWillUpdate(props) {
                if(this.context)
                    props = { ...props, theme: this.context}
                this.style = computeStyle(props, strings, vals)
            }
            render() {
                let props = this.props
                if(this.context) {
                    props = { ...this.props, theme: this.context }
                    this.style = computeStyle(props, strings, vals)
                }
                return (
                    createElement(tag, { style: this.style, ...props }, [...props.children])
                )
            }
        }        
    }
}
...
```

먼저 정적 `ContextType` 변수를 `ThemeContext`로 설정한다. 이렇게 하면 구성 요소에서 테마 공급자(TemeProvider)에 전달된 테마 개체를 사용할 수 있습니다. 테마는 this.context로 전달됩니다.

그래서 우리는 `component WillMount`와 `component WillUpdate`의 코드를 수정하여 `this.context`를 확인하기 위해 렌더링하고 그 안의 테마 객체를 `props`와 병합했다. 이렇게 하면 스타일 구성 요소에 전달된 소품 개체에서 테마 속성을 사용할 수 있습니다.

이상입니다. 스타일 구성 요소 버전에 Ming 기능을 추가했습니다.

다음은 스타일 구성 요소에 Ming 기능을 추가하는 전체 코드입니다.

```js
import React, { createElement, Component, useContext } from 'react';
const ThemeContext = React.createContext()
const tags = [
    "button",
    "div"
]
function evalInterpolation(props, strings, vals) {
    let resultStr = ""
    for (var i = 0; i < strings.length; i++) {
        var str = strings[i];
        var val
        if(vals) {
            val = vals[i]
            if(val !== undefined) {
                if(typeof val === "function") {
                    val = val(props)
                }
                str += val
            }
        }
        resultStr += str
    }
    return resultStr
}
function computeStyle(props, strings, vals) {
    strings = evalInterpolation(props, strings, vals)
    const style = {}
    strings.split(";").forEach((str)=> {
        let [prop, val] = str.trim().split(":")
        if(prop !== undefined && val !== undefined) {
            prop = prop.trim()
            val = val.trim()
            style[prop] = val
        }
    });
    return style
}
function genComponentStyle(tag) {
    return function(strings, ...vals) {
        return class extends Component {
            static contextType = ThemeContext
            constructor(props, context) {
                super(props, context)
                this.style = {}
            }
            componentWillMount() {
                if(this.context)
                    this.props = { ...this.props, theme: this.context}
                this.style = computeStyle(this.props, strings, vals)
            }
            componentWillUpdate(props) {
                if(this.context)
                    props = { ...props, theme: this.context}
                this.style = computeStyle(props, strings, vals)
            }
            render() {
                let props = this.props
                if(this.context) {
                    props = { ...this.props, theme: this.context }
                    this.style = computeStyle(props, strings, vals)
                }
                return (
                    createElement(tag, { style: this.style, ...props }, [...props.children])
                )
            }
        }        
    }
}
function ThemeProvider(props) {
    const outerTheme = props.theme
    const innerTheme = useContext(ThemeContext)
    const theme = { ... outerTheme, ... innerTheme}
    return (
        <React.Fragment>
            <ThemeContext.Provider value={theme}>
                {props.children}
            </ThemeContext.Provider>
        </React.Fragment>
    )
}
export {
    ThemeProvider
}
const styled = {}
tags.forEach(tag => {
    styled[tag] = genComponentStyle(tag)
})
export default styled
```

다양한 스타일의 구성 요소:

```js
import React from 'react';
import styled, { ThemeProvider } from "./styled.js"
const Div = styled.div`
    border-radius: 3px;
    border: 2px solid ${props => props.theme.bgColor};
    padding: 20px;
`
const Button = styled.button`
    padding: 6px 12px;
    background: ${(props) => props.theme.bgColor};
    color: white;
    border: 1px solid transparent;
    border-radius: 4px;
    font-size: 16px;
    margin: 2px;
`
export default class ThemeStyled extends React.Component {
    constructor() {
        super()
        this.state = {
            theme: {
                bgColor: "violet"
            }
        }
    }
    setTheme(bgColor) {
        this.setState({...this.state.theme, theme: { bgColor })
    }
    render() {
        return (
            <ThemeProvider theme={this.state.theme}>
                <Div>
                    <Button onClick={()=> this.setTheme("red")}>Set Theme(Red)</Button>
                    <Button onClick={()=> this.setTheme("green")}>Set Theme(Green)</Button>
                    <Button onClick={()=> this.setTheme("violet")}>Set Theme Default</Button>
                </Div>
            </ThemeProvider>
        )
    }
}
```

우리는 bgColor 속성이 바이올렛으로 설정된 테마 상태를 유지합니다. "Div"와 "Button" 스타일의 부품이 있습니다. 우리는 테마 객체에 있는 bgColor에 의해 설정된 Div 구성요소의 테두리 색상이 있습니다. 또한 버튼 구성 요소의 배경색은 테마.bg 컬러로 설정되어 있습니다.
"Div"와 "Button"을 각각 "S"et Theme(빨간색)" "S"et Theme(녹색)" "S"et Theme(기본)"로 표현합니다.

이러한 버튼을 클릭하면 상태 개체의 `bgColor` 속성이 변경됩니다. 설정 테마(빨간색)는 div 테두리 색과 버튼 배경색을 빨간색으로 바꾸는 bg 색상을 빨간색으로 바꾼다. 마찬가지로 테마 설정(녹색)과 테마 설정(기본값) 버튼을 누르면 테마 색상이 각각 녹색과 바이올렛(기본 색상)으로 변경됩니다.

## 신뢰할 수 있는 접근 스타일 구성 요소

보시다시피, 실제로 스타일링된 구성 요소의 작동 방식을 이해하는 것은 매우 쉽습니다. 사용자 고유의 스타일 구성 요소도 만들 수 있습니다.

스타일 구성 요소를 둘러싼 대부분의 혼란은 태그가 지정된 템플릿 리터럴 기능에서 비롯된다. 그러나 이제 태그가 지정된 템플릿 리터럴의 작동 방식도 이해하게 되었습니다.

후드 아래에서 너트와 볼트가 어떻게 작동하는지 보다 자세히 알 수 있다면, 완전히 자신만만하고 비교적 쉽게 스타일링된 구성 요소를 사용하여 접근할 수 있어야 합니다.