---
layout: post
title: "반응에서 스타일 구성 요소 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/styled-components-react.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/styled-components-react.png?fit=730%2C487&ssl=1)

현대의 컴포넌트 기반 프런트 엔드 프레임워크의 등장으로, 특정 컴포넌트에 범위가 매겨진 CSS를 쓸 필요성이 커졌다. 그리고 프런트 엔드 영역의 대부분의 문제와 마찬가지로, 이를 해결하기 위한 다양한 솔루션이 생겨났습니다.

가장 일반적인 솔루션은 자바스크립트에서 컴포넌트와 앱 스타일(CSS)을 쓰는 패턴인 CSS-in-JS이다. 오늘날 많은 CSS-in-JS 라이브러리가 존재하지만, 아마도 가장 대중적이고 지속적인 것, 그리고 이 게시물의 초점은 스타일 구성 요소일 것이다.

스타일링 컴포넌트(Styled-components)는 현대의 프런트 엔드 개발자를 위해 개발자 경험을 획기적으로 향상시키는 동시에 거의 완벽한 사용자 경험을 제공하는 CSS-in-JS 라이브러리이다. 개발자들에게 컴포넌트 범위 CSS를 쓸 수 있는 능력을 주는 것 외에도, 스타일링 컴포넌트는 다음을 포함한 많은 다른 이점을 제공한다.

- 자동 벤더 접두사
- 각 스타일 구성 요소의 고유 클래스 이름
- 스타일 유지 관리가 용이함 - 개발자가 다른 구성요소에 영향을 주지 않고 스타일을 삭제하거나 수정할 수 있음

이제 스타일 구성 요소 라이브러리에 대해 제대로 소개되었으므로 스타일 API를 사용하는 기본 사용 사례를 살펴보겠습니다.

## '스타일링' API: 모든 것이 시작되는 곳

`스타일링` API를 사용하면 일반 HTML 요소나 다른 `스타일링 컴포넌트`를 기본으로 하여 이름 그대로 스타일을 가진 컴포넌트인 `스타일링 컴포넌트`를 만들 수 있다.

첫 번째 방법을 살펴봅시다. 스타일링된 div 요소와 스타일링된 < 요소를 React 구성 요소로 만들려면 다음과 같이 쓰면 됩니다.

```js
import React from "react";
import styled from "styled-components";
const StyledDiv = styled.div`
  padding: 1.5rem;
  background-color: skyblue;
`
const StyledH1 = styled.h1`
  color: blue;

```

이를 통해 이제 일반 구성 요소로 사용할 수 있는 두 가지 반응 구성 요소가 있습니다.

```js
function App() {
  return (
      <StyledDiv>
        <StyledH1>A styled H1 element</StyledH1>
      </StyledDiv>
  );
```

브라우저의 결과는 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/styled-h1-element-example.jpeg?resize=497%2C197&ssl=1)

그만큼 구성 요소를 쉽게 스타일링할 수 있습니다!

> 참고: 'tyled' API가 백틱(")을 사용한다는 것을 알아챘을 수 있습니다. ES6의 이 기능은 태그가 지정된 템플릿 리터럴이라고 합니다. 함수를 호출하는 것과 같습니다.

앞서 말씀드린 것처럼 우리는 다른 `스타일 구성 요소`를 베이스로 삼아 새로운 `스타일 구성 요소`를 만들 수도 있습니다. 따라서 `StyledDiv`의 모든 스타일 속성을 가지면서도 테두리가 둥근 `StyledComponent`를 하나 더 만들고 싶다면 다음과 같은 작업을 수행할 수 있습니다.

```js
const StyledRoundedDiv = styled(StyledDiv)`
  border-radius: 30px;

```

그래서 이제 <Styled RoundedDiv>는 모든 스타일의 <StyledDiv>를 계승하지만 <경계 반지름>의 속성을 조정하기 위한 고유의 스타일을 가지고 있다. 이 기능을 사용하면 모든 구성 요소가 하나의 기본 구성 요소에서 스타일을 상속하는 다른 구성 요소를 생성할 수 있습니다.

또한 `Styled` API를 통해 자바스크립트를 사용하여 객체(JSX에서 일반적으로 사용됨)를 스타일링하여 StyledComponent`를 생성할 수 있다. 설명서에 명시된 바와 같이, "이것은 기존 스타일 객체를 가지고 있고 스타일 구성 요소로 점차 이동하기를 원할 때 특히 유용합니다."

스타일 객체를 사용하여 `TyledDiv` 구성 요소를 다시 만든다면 코드는 다음과 같습니다.

```cpp
const StyledDiv = styled.div({
  padding: "1.5rem",
  backgroundColor: "skyblue"
})
```

## 리액트 소품을 이용한 다이내믹한 스타일링

스타일 구성 요소의 또 다른 강력한 기능은 소품을 사용하여 구성 요소의 스타일을 변경할 수 있는 기능입니다. 이 개념을 더 잘 설명하기 위해 재료-UI 반응 라이브러리를 살펴보도록 하겠습니다. 우리는 다양한 소품들을 가지고 있는 <버튼> 구성품을 가지고 있지만, 우리는 단지 다양한 소품들과 색채 소품에만 초점을 맞출 것이다.

`변종` 소품으로 우리는 버튼의 윤곽이 드러날지, 아니면 `색` 소품에 지정된 색상으로만 채울지 결정할 수 있다. 색상 소품으로 어떤 값을 `변형` 소품으로 전달하느냐에 따라 윤곽선 색상이나 버튼 색상을 지정할 수 있다.

우리가 스타일 구성 요소를 사용하여 이러한 구성 요소를 구현한다면, 우리의 코드는 아마도 다음과 같을 것이다.

```js
const Button = styled.button`
  border-radius: 4px;
  padding: 6px 16px;
  min-width: 64px;
  text-transform: uppercase;
  font-weight: 500;
  font-size: 0.875rem;
  letter-spacing: 0.02857em;
  line-height: 1.75;
  font-family: "Roboto", "Helvetica", "Arial", sans-serif;
  background-color: ${(props) => backgroundColor(props)};
  ${(props) => colorAndBorder(props)}
`;
const colorToValue = {
  primary: "#1976d2",
  secondary: "rgb(220, 0, 78)",
};
const colorAndBorder = (props) => {
  var finalColor = "white";
  if (props.variant == "outlined") {
    if (colorToValue[props.color]) {
      finalColor = colorToValue[props.color];
    } else {
      finalColor = "black";
    }
  } else if (props.variant == "contained") {
    if (props.color) {
      finalColor = "white";
    } else {
      finalColor = "black";
    }
  }
  return css`
    color: ${finalColor};
    border: ${(prop) =>
      props.variant == "outlined" ? "1px solid " + finalColor : "none"};
  `;
};
const backgroundColor = (props) => {
  if (props.variant == "outlined") {
    return "white";
  } else if (props.variant == "contained") {
    if (props.color) {
      let color = colorToValue[props.color];
      if (color) {
        return color;
      } else {
        return "#e0e0e0";
      }
    } else {
      return "#e0e0e0";
    }
  }
};
```

위의 코드 블록에서 보면 버튼 구성 요소의 기본 스타일과 소품에 따라 배경색, 색상, 경계선 속성에 대한 동적 스타일이 있음을 알 수 있다.

styled와 css API는 taged template 리터럴을 사용하기 때문에 자바스크립트(string 보간법)를 CSS에 주입해 동적으로 구성 요소를 스타일링할 수 있는데, 이 두 가지 사용자 지정 함수인 background Color와 color And Border를 사용했다.

background color는 styled component의 소품을 가져와 버튼의 적절한 배경색을 문자열로 되돌린다. 이제 `Button` 구성요소 스타일의 외부 기능이 해당 문자열을 사용하여 단추를 스타일링할 수 있습니다.

그러나 `색상`과 `경계` 속성의 경우 외부 함수에서 사용할 문자열 대신 `버튼` 스타일 문자열에서 직접 사용할 수 있는 값을 반환하는 함수를 사용했다. 여기에 css 도우미 기능이 들어온다.

`css` 도우미 함수는 특히 `css` 함수에 전달된 템플릿 리터럴이 문자열 보간을 포함하는 경우 템플릿 리터럴 스타일에 직접 사용할 수 있는 값을 생성합니다.

이제 다음과 같은 `Button` 구성 요소를 사용할 수 있습니다.

```js
function App() {
  return (
    <>
      <Button variant="contained" color="primary">
        Primary Contained
      </Button>

      <Button variant="outlined" color="secondary">
       Secondary Outlined
      </Button>

      <Button variant="contained">
       Default contained
      </Button>
    </>
  );
}
```

### 'attors' 생성자를 사용한 추가 스타일링 방법

스타일링 컴포넌트는 또한 `attrs` 방법을 사용하여 추가 소품이나 HTML 속성을 컴포넌트에 추가할 수 있게 한다. `attrs`를 사용하여 동적 소품뿐만 아니라 `<input> 요소의 유형과 같은 정적 소품 또는 속성을 정의할 수 있다. 다음은 사용 중인 `attrs` 방법의 예입니다.

```js
const Link = styled.a.attrs((props) => ({
  href: props.$to,
}))`
  text-decoration: none;
  color: #000;
  border: 1px solid #000;
  padding: 1rem 1.5rem;
  border-radius: 5px;
`;

function App() {
  return (
    <>
      <Link $to="https://blog.logrocket.com">Test link</Link>
    </>
  );
}
```

위 코드에서 우리는 HTML 요소가 앵커 요소인 `<Link>` 구성 요소에 `href` 속성을 첨부했다. href 요소에 전달된 값은 당사의 < 구성 요소의 과도 소자 $to에서 수신되었습니다.

과도 소품은 DOM에 나타나지 않는 소품을 사용할 수 있는 스타일 구성 요소이다. 따라서 당사의 `링크` 구성 요소를 검사하면 다음과 같이 간단히 렌더링되었음을 알 수 있습니다.

```undefined
<a href="https://blog.logrocket.com" class="sc-bdfBwQ geNjDe">Test link</a>
```

## MingReact 앱에 스타일 구성 요소 적용

테마 공급자(TemeProvider) 구성 요소를 통해 Ming을 지원하는 스타일 구성 요소는 테마 공급자(TemeProvider) 구성 요소를 사용하여 Ming을 지원한다. 우리는 테마 소품을 <테마 제공자>에게 전달할 수 있으며 이 소품은 <테마 제공자>의 모든 아이들에게 소품으로 전달될 것이다.

우리는 이제 테마 소품의 가치를 바탕으로 아동 구성품을 역동적으로 스타일링할 수 있게 되었다. 다음은 작동 방식을 보여주는 간단한 예입니다.

```js
const lightTheme = {
  main: "#fff",
};
const darkTheme = {
  main: "#000",
};
const invertColor = (props) => {
  if (props.theme.main == "#000"){
    return "#fff"
  } else {
    return "#000"
  }
}
const Div = styled.div`
  padding: 4rem;
  background-color: ${props => props.theme.main}
`
const Button = styled.button`
  padding: 1.5rem 2rem;
  color: ${props => invertColor(props)};
  border: 1px solid ${props => invertColor(props)};
  background-color: ${props => props.theme.main};
  transition: all 0.5s;
  &:hover{
    color: ${props => props.theme.main};
    background-color: ${props => invertColor(props)};
    border: 1px solid ${props => props.theme.main};
  }
`
/*Default props for the Button component in case it has no theme attached to its props
*/
Button.defaultProps = {
  theme: darkTheme
}
function App() {
  return (
    <>
    <ThemeProvider theme = {lightTheme}>
      <Div>
        <Button>Light Theme button</Button>
      </Div>
    </ThemeProvider>
    <ThemeProvider theme = {darkTheme}>
      <Div>
        <Button>Dark Theme button</Button>
      </Div>
      <Button>Dark Theme button</Button>
    </ThemeProvider>

    </>
  );
}
```

브라우저에서 위의 코드 블록의 결과는 다음과 같아야 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/code-block-light-dark-themes.jpeg?resize=331%2C476&ssl=1)

코드에서 볼 수 있듯이, 빛이라는 두 개의 물체가 있다.테마와 `어두움`테마는 각각 명암 테마의 원색을 나타낸다.

`반전색`이라는 기능도 있는데, 단순히 소품을 들여와 소품에 부착된 테마의 반대 원색을 되돌려주는 기능이다. 테마 객체와 반전 컬러 기능을 사용하여 가볍고 어두운 테마를 만들고 그에 따라 구성 요소를 스타일링할 수 있었습니다.

앰퍼샌드를 사용했다는 사실도 알아 두십시오(`

## 애니메이션 기능

키프레임 도우미 기능을 통해 CSS 애니메이션을 지원하는 것이 당연하다. 이 함수는 템플릿을 리터럴로 사용하여 애니메이션의 CSS를 정의하고 이제 `css` 도우미 함수로 보간하거나 `tyled` API와 함께 사용할 수 있는 값을 반환합니다.

다음은 코드에서 `키프레임` 기능이 어떻게 표시되는지 보여주는 문서의 간단한 예입니다.

```js
import styled, { keyframes } from 'styled-components'

const fadeIn = keyframes`
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
`

const FadeInButton = styled.button`
  animation: 1s ${fadeIn} ease-out;
` 
```

## 결론

드디어 제가 생각하는 스타일 구성 요소 라이브러리에서 가장 중요하고 자주 사용하는 부분을 다루었습니다. 라이브러리에 대한 자세한 내용은 여기 있는 포괄적인 설명서를 참조하십시오.