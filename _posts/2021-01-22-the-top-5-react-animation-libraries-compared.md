---
layout: post
title: "상위 5 개 React 애니메이션 라이브러리 비교
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/reactanimationlibraries.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/reactanimationlibraries.png?fit=730%2C487&ssl=1)

React는 사용자 인터페이스를 구축하기위한 라이브러리입니다.
 React 프론트 엔드 개발자에게 웹 페이지에 애니메이션을 구현하는 것은 텍스트 또는 이미지 애니메이션에서 복잡한 3D 애니메이션에 이르기까지 일상 작업의 필수적인 부분입니다.
 애니메이션은 React 애플리케이션을 구축하는 동안 전반적인 사용자 경험을 개선하는 데 도움이 될 수 있습니다.
 

이 기사에서는 상위 5 개 React 애니메이션 라이브러리를 비교하고 각각의 인기도, 개발자 경험, 가독성, 문서화 및 번들 크기를 평가하여 다음 React 프로젝트에 적합한 라이브러리를 선택하는 데 도움을 줄 것입니다.
 

## 반응 봄
 

react-spring은 스프링 물리학을 기반으로 한 최신 애니메이션 라이브러리입니다.
 매우 유연하며 사용자 인터페이스에 필요한 대부분의 애니메이션을 다룹니다.
 react-spring은 사용 용이성, 보간 및 성능과 같은 React Motion의 일부 속성을 상속합니다.
 

react-spring이 작동하는지 확인하려면 아래 명령 중 하나를 사용하여 설치합니다.
 

```coffeescript
npm install react-spring
```

또는
 

```undefined
yarn add react-spring
```

다음으로 아래 코드를 추가하여`Hello React Spring` 텍스트를 만들고 애니메이션화합니다.
 

```js
import React, { useState } from 'react';
import ReactDOM from 'react-dom';
import { useSpring, animated } from 'react-spring';
import './styles.css';


function SpringDemo() {
  const [state, toggle] = useState(true)
  const { x } = useSpring({ from: { x: 0 }, x: state ? 1 : 0, config: { duration: 1000 } })
  return (
    <div onClick={() => toggle(!state)}>
      <animated.div
        style={{
          opacity: x.interpolate({ range: [0, 1], output: [0.3, 1] }),
          transform: x
            .interpolate({
              range: [0, 0.25, 0.35, 0.45, 0.55, 0.65, 0.75, 1],
              output: [1, 0.97, 0.9, 1.1, 0.9, 1.1, 1.03, 1]
            })
            .interpolate(x => `scale(${x})`)
        }}>
        Hello React Spring
      </animated.div>
    </div>
  )
}

export default SpringDemo
```

위의 코드에서 먼저`useSpring`을 임포트하고 react-spring에서 둘 다 애니메이션 처리했습니다.
 다음으로`useState` 후크를 사용하여 객체 x를 초기화했습니다.
 보간을 사용하여 여러 애니메이션 값을 범위 및 출력으로 가져와 x로 배율이 조정 된 하나의 결과를 형성했습니다.
 

보간은 여러 값을 가져와 하나의 결과를 형성 할 수있는 기능입니다.
 react-spring의 보간은 CSS 키 프레임 및 색상과 같은 일련의 상태에도 사용할 수 있습니다.
 대부분의 애니메이션은 애니메이션을 애니메이션 div 구성 요소로 래핑하여 수행됩니다.
 

react-spring은 React 애플리케이션 애니메이션을위한 강력한 플랫폼을 제공합니다.
 소품과 방법은 읽기 쉽고 이해하기 쉽습니다.
 

react-spring이 다른 React 애니메이션 라이브러리와 비교하여 어떻게 쌓이는 지 살펴 보겠습니다.
 

- 인기도 : GitHub에서 별 1 만 9 천개, NPM에서 매주 475,278 회 이상 다운로드
 Aragon, CodeSandbox, Next.js 등과 같은 회사 및 신생 기업에서 사용
 
- 문서 : 잘 작성되고 초보자에게 친숙한 문서 인 react -spring의 문서를 통해 문서에서 코드 스 니펫을 복사하고 CodeSandbox를 테스트하거나 미리 볼 수 있습니다.
 
- 번들 크기 (최소화) : react-spring 26.7kb!
 

## 프레이머 모션
 

Framer Motion은 애니메이션을 쉽게 만들 수있는 인기있는 React 애니메이션 라이브러리입니다.
 애니메이션이면의 복잡성을 추상화하고 개발자가 쉽게 애니메이션을 만들 수있는 단순화 된 API를 자랑합니다.
 더 좋은 점은 서버 측 렌더링, 제스처 및 CSS 변수를 지원한다는 것입니다.
 

Framer Motion을 설치하려면 터미널에서 다음 두 명령 중 하나를 실행하십시오.
 

```undefined
yarn add framer-motion
```

또는
 

```coffeescript
npm install framer-motion
```

다음으로 다음 코드 블록을 추가하여 상자 구성 요소에 간단한 애니메이션을 추가합니다.
 

```js
import React from "react";
import { motion } from "framer-motion";
import "./styles.css";

export default function App() {
  const [isActive, setIsActive] = React.useState(false);
  return (
    <motion.div
      className="block"
      onClick={() => setIsActive(!isActive)}
      animate={{
        rotate: isActive ? 180 : 360
      }}
    >
      Hello Framer motion
    </motion.div>
  );
}
```

react-spring과 유사하게 Framer Motion은 애니메이션을위한 기본 제공 구성 요소를 제공합니다.
 `motion.div`는 애니메이션을 위해 개체를 래핑하는 데 사용됩니다.
 구성 요소 및 요소의 스타일을 더 빠르게 지정하고 코드 가독성을 향상시킵니다.
 이것의 단점은 애니메이션 요소가 커짐에 따라 부피가 커질 수 있다는 것입니다.
 전반적으로 Framer Motion은 매우 강력하고 사용자 정의가 가능하며 강력한 라이브러리입니다.
 

- 인기도 : GitHub의 별 8.4k, NPM의 주간 다운로드 292,291 회 이상
 
- 문서 : 이해하기 쉽고 초보자에게 친숙합니다.
 문서에서 주어진 구성 요소의 소스 코드를 찾을 수 있으며 CodeSandbox에서도 볼 수 있습니다.
 
- 번들 크기 (최소화) : 90.8kb로 축소 된 Framer Motion!
 

Framer Motion은 번들 크기가 훨씬 더 크며 인기와 별 측면에서 react-spring만큼 인기가 없지만 문서는 이해하기가 다소 쉬우 며 다음 React 프로젝트를 고려할 가치가 있습니다.
 

## 반응 전환 그룹
 

React Transition Group은 상용구 코드의 필요성을 최소로 줄여 개발 용 도구입니다.
 react-spring과 같은 다른 많은 React 애니메이션 라이브러리와 달리 React Transition Group은 애니메이션 정의를위한 간단한 구성 요소를 제공합니다. 라이브러리는 스타일 자체를 정의하지 않고 대신 유용한 방식으로 DOM을 조작하여 전환 및 애니메이션 구현을 훨씬 더 편안하게 만듭니다.
 즉, React Transition Group은 애니메이션 및 전환에 대한보다 직접적인 접근 방식을 제공합니다.
 

터미널에서 다음 명령 중 하나를 실행하여 React Transition Group을 설치하십시오.
 

```coffeescript
npm i react-transition-group
```

또는
 

```undefined
yarn add react-transition-group
```

다음으로, 이전과 같이이 코드 블록을 추가하여 React Transition Group을 사용하여 기본 애니메이션을 만듭니다.
 

```js
import React from "react";
import ReactDOM from "react-dom";
import { Transition } from "react-transition-group";
import "./styles.css";
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      in: false
    };
  }
  componentDidMount() {}
  onClick = () => {
    this.setState(state => ({ in: !state.in }));
  };
  render() {
    return (
      <div className="App">
        <button onClick={this.onClick}>BUTTON</button>
        <Transition in={this.state.in} timeout={5000} mountOnEnter appear>
          {state => <div className={`item item--${state}`} />}
        </Transition>
      </div>
    );
  }
}

export default app;
```

React Transition Group은 `Transition`과 같은 내장 구성 요소를 사용하여 애니메이션과 전환을 요소로 설정하여 애니메이션에서 요소를 분리합니다.
 

이제 성적표 :
 

- 인기도 : 7.9k GitHub 별 및 NPM에서 주간 다운로드 620 만 회 이상!
 
- 문서 : 문서는 초보자에게 친숙합니다.
 예제는 명확하고 Codesandbox 예제로 쉽게 이해할 수 있습니다.
 
- Bunde 크기 (최소화) : 반응 전환 그룹 14.2kb!
 
- Typescript 지원 : React 전환 그룹은 TypeScript에 대한 지원과 함께 제공되며 아래 명령을 사용하여 설치할 수 있습니다.
 

```css
npm i @types/react-transition-group
```

React Transition Group은 좋은 애니메이션 라이브러리이며 번들 크기가 매우 작습니다.
 가장 인기있는 애니메이션 라이브러리 중 하나이며 다음 React 프로젝트에서 고려해야합니다.
 

## 리 액트 모션
 

React-Motion은 사실적인 애니메이션을 만들고 구현하는 더 쉬운 접근 방식을 자랑하는 애니메이션 라이브러리입니다.
 React-Motion은 React 요소에 대한 거의 자연스러운 애니메이션을 만들기 위해 물리학을 사용합니다.
 

React-Motion을 설치하고 터미널에서 아래 명령을 실행하여 기본 애니메이션을 만듭니다.
 

```undefined
yarn add react-motion
```

또는
 

```coffeescript
npm i react-motion
```

기본 애니메이션 구성 요소를 만들려면 다음 코드 줄을 추가하십시오.
 

```js
import React from 'react'
import { StaggeredMotion, spring } from 'react-motion'
const AnimatedGridContents = props => {
  return (
    <StaggeredMotion
      defaultStyles={props.items.map(() => ({ opacity: 1, translateY: -30 }))}
      styles={prevInterpolatedStyles =>
        prevInterpolatedStyles.map((_, i) => {
          return i === 0
            ? { opacity: spring(1), translateY: spring(0) }
            : {
              opacity: prevInterpolatedStyles[i - 1].opacity,
              translateY: spring(prevInterpolatedStyles[i - 1].translateY)
            }
        })
      }
    >
      {interpolatingStyles => (
        <div className="grid">
          {interpolatingStyles.map((style, i) => (
            <div
              className="card"
              key={props.items[i]}
              style={{
                opacity: style.opacity,
                transform: `translateY(${style.translateY}px)`
              }}
            >
              {props.items[i]}
            </div>
          ))}
        </div>
      )}
    </StaggeredMotion>
  )
}
```

위의 코드 블록에서 StaggeredMotion을 사용하여 카드 구성 요소에 전환을 추가했습니다.
 React-Motion을 사용하면 React에서 애니메이션 구성 요소를 단순화하는 API를 활용할 수 있습니다.
 

이제 React-Motion 인기도와 코드 품질을 분석해 보겠습니다.
 

- 인기도 : GitHub에서 별 19.2k, NPM에서 603,199 다운로드 이상
 
- 문서 : 문서는 대화 형입니다.
 문서에서 주어진 구성 요소의 소스 코드를 복사 할 수 있습니다.
 
- 번들 크기 (최소화) : react-motion 19.8kb
 

전반적으로 React-Motion은 React 애플리케이션을위한 사운드 애니메이션 라이브러리이며, 특히 거의 생생한 애니메이션 동작이 있습니다.
 

## 반응 이동
 

React Move는 아름다운 데이터 기반 애니메이션을 React로 만들기위한 라이브러리입니다.
 이것은 TypeScript를 즉시 지원하고 사용자 정의 트위닝 기능을 지원하도록 설계되었습니다. Tweening은 동화의 약자이며 이미지 사이의 프레임 인 키 프레임을 생성하는 프로세스입니다.
 React Move는 전환에 대한 수명주기 이벤트도 제공하며 React Move의 애니메이션에 사용자 정의 트윈을 전달할 수도 있습니다.
 

React Move가 다른 구성 요소 라이브러리와 어떻게 겹쳐 지는지 살펴 보겠습니다.
 

- 인기도 : GitHub의 별 6.2k, NPM의 주간 다운로드 117,029 회 이상!
 React Move는 개발자 커뮤니티에서 인기를 얻고 있습니다.
 
- 문서 : 문서는 훌륭합니다.
 React Move는 CodeSandbox에서 코드를 테스트 할 수있는 기회와 코드를 제공합니다.
 
- 번들 크기 (최소화) : react-move 12.7kb!
 

React Move는 모든 종류의 React 애니메이션 및 전환에 사용할 수 있습니다.
 개발자가 React 애플리케이션에서 애니메이션을 사용자 정의 할 수있는 더 많은 권한을 제공하는 사용자 정의 트위닝으로 더욱 빛납니다.
 

## 결론
 

프로젝트에 관계없이 작업하고 있습니다.
 수많은 애니메이션 라이브러리를 통해 사용자 친화적 인 애니메이션과 전환을 쉽고 빠르게 만들 수 있습니다.
 이러한 라이브러리 중 상당수는 사용자 정의가 가능하며 내장 기능과 변경 사항이 포함되어 있습니다.
 이 비교를 통해 다음 React 애플리케이션에 적합한 라이브러리를 선택할 수 있기를 바랍니다.
 