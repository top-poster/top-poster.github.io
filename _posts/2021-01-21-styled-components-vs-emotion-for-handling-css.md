---
layout: post
title: "CSS 처리를위한 스타일 구성 요소 대 Emotion-JS
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/sc-emotion.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/sc-emotion.png?fit=730%2C487&ssl=1)

## 소개
 

지난 몇 년 동안 JavaScript가 많은 조정과 개선을 거치면서 JavaScript 코드 내에서 CSS를 처리하기위한 몇 가지 새로운 라이브러리를 보았습니다.
 styled–components 및 Emotion은 React와 같은 인기있는 JavaScript 프레임 워크에서 구성 요소를 스타일링하는 동안 개발자가 주로 사용하는 두 가지 주요 CSS-in-JS 라이브러리입니다.
 

이 기사에서는 차이점, 단점 및 이점을 포함하여 스타일이 지정된 구성 요소와 감정을 비교합니다.
 

## 개요
 

### 스타일 구성 요소
 

스타일 구성 요소 라이브러리를 사용하면 개발자가 새로운 스타일의 반응 구성 요소를 만들고 기존 반응 구성 요소의 스타일을 변경할 수 있습니다.
 `Props`는 스타일 구성 요소의 주요 기능 요소이며 특히 개체 스타일 변경에 사용됩니다.
 

제 생각에 스타일 구성 요소의 가장 큰 장점은`className` 소품을 허용하는 한이 특정 라이브러리를 사용하여 모든 구성 요소를 스타일링 할 수 있다는 것입니다.
 `withComponent`함수 도우미를 사용하여 특정 구성 요소에 연결된 렌더링 된 태그를 변경할 수도 있습니다.
 나중에 다른 구성 요소에서 특정 구성 요소의 특정 스타일을 재사용하려는 경우 필요할 수 있습니다.
 이러한 경우 작업을 수행하려면 렌더링 된 태그를 변경해야합니다.
 

### 감정
 

감정의 용도는 스타일 구성 요소의 용도와 매우 다릅니다.
 이 라이브러리의 주요 기능은 JavaScript를 사용하여 다양한 CSS 스타일을 작성하기 위해 스타일 구성을 예측할 수 있다는 것입니다.
 이 라이브러리는 문자열 및 객체 스타일을 모두 지원합니다.
 

일반적으로 Emotion을 사용하는 두 가지 주요 방법이 있습니다 : 프레임 워크에 구애받지 않고 React를 사용하는 것입니다.
 아래에서 설명 하겠지만 각각의 장점과 단점이 있습니다.
 

#### 프레임 워크에 구애받지 않음
 

다른 유형의 스타일링 방법과 달리 프레임 워크에 구애받지 않는 접근 방식은 외부 플러그인이나 추가 설정이 필요하지 않습니다.
 Emotion은 프레임 워크에 관계없이 자동 공급 업체 접두사, 중첩 선택기 및 기타 기능을 처리하는 데 능숙한 안정적인 지원 시스템과 함께 제공됩니다.
 

또한 프레임 워크에 구애받지 않는 접근 방식을 사용하면 가장 간단한 CSS 함수를 사용하여 다른 클래스 이름을 쉽게 생성 할 수 있습니다.
 

#### 반응 방법
 

Emotion과 함께 React 메서드를 사용하는 것은 독특하고 색다른 인라인 디자인 작업을 생성하는 가장 좋은 방법이며 구성 가능한 빌드 환경에 적용 할 때 가장 잘 작동합니다.
 이 방법을 사용하면 향상된 스타일 옵션을 보장하는 전용 CSS prop 지원은 물론 구성없이 서버 측 렌더링을 수행 할 수 있습니다.
 

## 스타일 구성 요소를 감정과 비교
 

JavaScript 용 CSS를 작성할 때 스타일 구성 요소와 Emotion은 모두 매우 효율적이고 잘 수행되며 특정 목적을 위해 각 라이브러리를 사용하는 전용 개발자 기반이 있습니다.
 

다음 섹션에서는 각 라이브러리를 사용하는 다양한 이유를 살펴보고 어떤 것이 적합한 지 결정하는 데 도움이됩니다.
 

### 스타일이 지정된 구성 요소를 선택하는 이유는 무엇입니까?
 

스타일 구성 요소는 React 구성 요소의 조정 된 버전으로, 스타일 구성 요소에 소품을 전달하기가 매우 쉽습니다.
 이 기능은 스타일 구성 요소를 사용하여 CSS를 작성하는 과정을 훨씬 쉽게 만듭니다.
 스타일 구성 요소를 통해 기존 일반 구성 요소의 스타일을 변경하고 테마를 수정할 수도 있습니다.
 

테마 작업을 수행 할 때 스타일 구성 요소는 다양한 모양, 테마 및 사용자 지정 느낌을 지원하는 훌륭한 옵션입니다.
 스타일 구성 요소는 또한 모든 유형의 스타일 구성 요소에 적용 할 수있는 전역 스타일을 허용합니다.
 모든 구성 요소에 적용 할 수있는 전역 스타일을 만들면 서로 다른 스타일 구성 요소를 동시에 처리 할 때 시간과 노력을 절약 할 수 있습니다.
 

위에 나열된 기능 외에도 스타일 구성 요소는 한 HTML 구성 요소에서 다른 HTML 구성 요소로 쉽게 전환됩니다.
 이러한 유연성 덕분에이 라이브러리는 CSS 작성 측면에서 다른 방법보다 더 나은 옵션이됩니다.
 예를 들어 특정 HTML 요소가 스타일 구성 요소에 의해 렌더링되면 개발자는 해당 HTML 요소에 추가 속성을 추가 할 수 있습니다.
 

마지막으로 스타일 구성 요소는 거의 모든 CSS 프레임 워크와 호환되어 모든 테마 및 스타일 요구 사항을 지원합니다.
 

### CSS 작성에 Emotion을 사용하는 이유는 무엇입니까?
 

일반적으로 Emotion은 다른 라이브러리보다 개발자 친화적입니다.
 Emotion의 가장 큰 장점은 CSS 작성을 위해 쉽게 처리 할 수있는 개체 스타일입니다.
 

예를 들어, 개발자가 동일한 이름 지정 스타일을 피하면서 서로 다른 구성 요소에 대해 고유 한 이름을 만들어야하는 스타일 구성 요소의 경우를 생각해보십시오.
 고유 한 스타일을 따르지 않거나 의미있는 설명없이 구성 요소의 이름을 지정하면 모든 노력이 몇 초 만에 낭비 될 수 있습니다.
 반면에 이름 지정 작업은 CSS props의 적용에 의존하기 때문에 Emotion에서 매우 간단합니다.
 

마지막으로, 많은 개발자들은 작은 크기, 고성능 및 전반적인 유연성 때문에 다른 CSS-in-JS 대안보다 Emotion을 선호합니다.
 

## 스타일 구성 요소 및 감정을 사용한 다양한 스타일 옵션
 

### 스타일 구성 요소를 사용한 스타일 지정
 

스타일링 구성 요소에 대한 몇 가지 샘플 코드부터 살펴 보겠습니다.
 

```js
import styled from '@emotion/styled'

const Button = styled.button`
  color: turquoise;
`

render(<Button>This my button component.</Button>)
```

props를 구현하여 컴포넌트의 스타일을 변경하거나 새 스타일을 만들 수 있습니다.
 

```js
import styled from '@emotion/styled'

const Button = styled.button`
  color: ${props =>
    props.primary ? 'hotpink' : 'turquoise'};
`

const Container = styled.div(props => ({
  display: 'flex',
  flexDirection: props.column && 'column'
}))

render(
  <Container column>
    <Button>This is a regular button.</Button>
    <Button primary>This is a primary button.</Button>
  </Container>
)
```

아래 예에서`className` 소품을 사용하여 스타일을 만들 수 있습니다.
 

```js
import styled from '@emotion/styled'
const Basic = ({ className }) => (
  <div className={className}>Some text</div>
)

const Fancy = styled(Basic)`
  color: hotpink;
`

render(<Fancy />)
```

마지막으로 스타일 구성 요소에서 렌더링 태그를 변경할 수 있습니다.
 다음 코드 블록에서 두 번째 구성 요소가 Section과 동일한 스타일을 갖지만 옆으로 렌더링되는 방식을 확인합니다.
 

```js
import styled from '@emotion/styled'

const Section = styled.section`
  background: #333;
  color: #fff;
`
const Aside = Section.withComponent('aside')
render(
  <div>
    <Section>This is a section</Section>
    <Aside>This is an aside</Aside>
  </div>
)
```

### 프레임 워크에 구애받지 않는 스타일링에 Emotion 사용
 

Emotion은 스타일링 구성 요소에 대해 간소화되었으므로 프레임 워크에 구애받지 않는 방법은 매우 간단합니다.
 

```js
import { css, cx } from '@emotion/css'

const color = 'white'

render(
  <div
    className={css`
      padding: 32px;
      background-color: hotpink;
      font-size: 24px;
      border-radius: 4px;
      &:hover {
        color: ${color};
      }
    `}
  >
    Hover to change color.
  </div>
)
```

### Emotion을 사용하여 React의 구성 요소 스타일 지정
 

접근 방식은 React에서 비슷하게 간단합니다.
 

```undefined
// this comment tells babel to convert jsx to calls to a function called jsx instead of React.createElement
/** @jsx jsx */
import { css, jsx } from '@emotion/react'

const color = 'white'

render(
  <div
    css={css`
      padding: 32px;
      background-color: hotpink;
      font-size: 24px;
      border-radius: 4px;
      &:hover {
        color: ${color};
      }
    `}
  >
    Hover to change color.
  </div>
)
```

## 결론
 

간단하고 효율적이며 복잡하지 않은 스타일링을 위해 Emotion은 훌륭한 CSS-to-JS 라이브러리입니다.
 반면에 더 독특하고 복잡한 스타일 옵션의 경우 스타일 구성 요소가 더 나은 방법 일 수 있습니다.
 CSS 작성의 경우와 마찬가지로 의사 결정 과정의 대부분은 프로젝트 설정 및 개인 취향에 달려 있습니다.
 