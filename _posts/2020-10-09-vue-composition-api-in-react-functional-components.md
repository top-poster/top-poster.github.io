---
layout: post
title: "React 기능 구성 요소에서 Vue Composition API 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/vue-composition-api-react-functional-components.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/vue-composition-api-react-functional-components.png?fit=730%2C487&ssl=1)

리액트 훅스의 인기는 v16.8에 처음 출시된 이후 치솟았다. 그리고 적절한 이유로 구성 요소를 단순한 기능으로 쓰고 구성하는 기능은 놀라운 성능과 유연성을 제공합니다. 뛰어난 TypeScript 지원과 함께 Hooks가 매력적인 것은 놀랄 일이 아닙니다.

그 후 아름다운 반응성 시스템과 향상된 정적 타이핑 지원 기능을 갖춘 Vue Composition API가 출시되었습니다. 프런트엔드 엔지니어가 되기에는 정말 좋은 시기이지만, 의사 결정 마비는 정말 힘든 일이 될 수 있다. Vue와 포트를 React Hooks에 쓰는 것을 포기합니까, 아니면 그 반대입니까?

잠깐! 만약 네가 선택할 필요가 없다면? React 구성 요소 내에서 Vue Composition API를 사용할 수 있다면 어떻겠습니까? 여러분이 그것을 할 수 있도록 하기 위한 `리액티브`가 온다.

## 리액티브란?

reactive는 React의 최고의 요소와 Vue.js의 최고의 요소라는 것을 하나의 프레임워크에서 설명한다. 간단히 말해서, 여러분은 반응 안에 Vue 조각들을 쓸 수 있습니다. 두 세계를 가장 좋게 생각하십시오.

> 참고: '재활성'은 현재도 여전히 실험 중이며 향후 몇 가지 획기적인 변화가 있을 수 있습니다. 아직 제작에 사용하지 말고, 편하게 사용해 보세요.

## 시작 중

이 자료에서는 Vue의 Composition API 및 React 기능 구성 요소 및 Hooks를 사용한 React 기능을 올바르게 이해하고 있다고 가정합니다. 어떤 방법을 써서라도 전문가가 될 필요는 없지만, 개념에 대한 근본적인 친숙함이 필요하다. 우리는 그들이 어떻게 움직이는지에 대해 자세히 알아보는 것이 아니라 그들이 어떻게 `복귀`의 도움으로 함께 일하는지에 대해 알아보는 것이다.

더 이상 하지 말고 recovery를 설치하자. 설치가 간단합니다. 아래 그림과 같이 Narn 또는 npm으로 패키지를 추가하십시오.

```coffeescript
npm i reactivue
 // or
yarn add reactivue
```

## 사용법

이 작업을 수행하기 위해 간단한 색 선택기를 만들어 보겠습니다. 아주 간단하게 하기 위해, 우리는 스타일링을 없애고 설정이 어떻게 작동하는지 이해하려고 합니다. 흥미로운 부분은 리액티브(reactive)가 Vue Composition API와 리액션(react) 기능을 통합하여 이들이 원활하게 연동되도록 하는 방법을 시각화하는 것이다.

우리의 색상 선택기는 단지 HTML 색상 입력에서 색상을 선택하고 선택된 색상을 기반으로 div의 배경색을 업데이트합니다. 할당으로 여러 스타일을 설정하도록 확장하거나 개념을 더 잘 이해하기 위해 복잡한 조건부 논리를 추가할 수 있습니다. 코드는 아래에 있고, 그 다음은 심층 분석입니다.

```undefined
import * as React from "react";
import { useState, ChangeEvent } from "react";
import { defineComponent, ref, computed, onUnmounted, onMounted } from "reactivue";

interface Props {
  color: string;
  height: number;
  width: number;
}

const MyColor = defineComponent(
  // setup function in Vue
  ({ color, width, height }: Props) => {
    const background = ref(color);
    const boxWidth = ref(width);
    const boxHeight = ref(height);
    const boxStyle: any = computed(() => {
      return {
        width: `${boxWidth.value}px`,
        height: `${boxHeight.value}px`,
        backgroundColor: background.value
      };
    });

    onMounted(() => console.log("Hello World"));
    onUnmounted(() => console.log("Goodbye World"));

    return { background, boxWidth, boxStyle, boxHeight };
  },
  // functional component in React
  ({ background, boxWidth, boxStyle }) => {
    // you are now in react territory. You can use all hooks and methods.
    const [newStyle, setNewStyle] = useState(boxStyle);
    const onChangeColor = ({
      target: { value }
    }: ChangeEvent<HTMLInputElement>) => {
      setNewStyle({
        ...boxStyle,
        backgroundColor: value
      });
    };
    return (
      <div>
        <input
          type="color"
          onChange={(e) => onChangeColor(e)}
          style={ marginBottom: "20px", width: "300px" }
        />

        <div style={newStyle}></div>
      </div>
    );
  }
);

// use it as you normally would
render(<MyColor color="red" width={300} height={200} >, el)
```

작동 예는 다음과 같습니다.

이것은 꽤 이해하기 쉽다. React와 설정에 필요한 방법을 모두 reactive에서 가져왔습니다.

여기서는 `@vue/reactivity`에서 반응성 API를 가져오는 이른바 팩토리 구성 요소를 생성할 수 있습니다. 그런 의미에서 아래 선은 동일합니다.

```undefined
// both line are equivalent.
import { ref, reactive, computed } from 'reactivue'
import { ref, reactive, computed } from '@vue/reactivity
```

DefineComponent 기능은 Vue의 라이프사이클 후크를 표시합니다. 우리는 `Props` 객체를 만들어 리액트 컴포넌트에 전달한다. Composition API를 사용하여 ref로 내부 상태를 설정할 수 있을 뿐만 아니라 React 구성 요소 내부에 스타일을 업데이트하는 계산된 속성을 생성할 수 있다.

Composition API의 작동 방식에 대해서는 자세히 설명하지 않겠습니다. 우리가 알아야 할 것은 `배경`, `박스 폭`, `박스 높이`, `박스 스타일`이 모두 반응성이어서 리액트 부품으로 전달되어 어떤 리액트 부품에서처럼 사용하고 변형할 수 있다는 것이다.

우리의 경우, 리액트의 useState Hook을 사용하여 newStyle 상태를 설정하고 있습니다. 우리는 배경 소품을 수락하고 Retact 구성 요소 내부에서 새 상태로 업데이트합니다.

여기에서는 이미 Vue에서 상태를 전달하여 React에서 상태를 변경하는 방법을 볼 수 있습니다. 그런 다음 Vue의 반응성(숫자에서 px까지)과 새로운 스타일을 설정하는 React를 통해 다시 계산된다.

이것은 처음에는 혼란스러워 보일 수 있지만, 놀랍게도 간단하다. 코드를 사용하여 다른 스타일의 변경 내용을 추적하거나 다른 계산된 속성을 추가해 보십시오.

### 훅스

후크로도 사용할 수 있습니다.

Define Component 공장은 사실상 setup을 사용하기 위한 구문론적인 설탕이다. 아래 코드는 구성 요소 팩토리를 사용하는 위의 코드와 유사합니다.

```js
import * as React from "react";
import { useState, ChangeEvent } from "react";
import { useSetup, ref, computed } from "reactivue";

function MyColor(Props: Props) {
  const state = useSetup(
    ({ color, width, height }: Props) => {
      // shortened for brevity; same as above example
      const boxStyle: any = computed(() => {
        return {
          width: `${boxWidth.value}px`,
          height: `${boxHeight.value}px`,
          backgroundColor: background.value
        };
      });
      return { background, boxWidth, boxStyle, boxHeight };
    },
    Props // pass React props to it
  );

  // state is a plain object just like React state
  const { boxStyle } = state;

  const [newStyle, setNewStyle] = useState(boxStyle);
  const onChangeColor = ()=> { //shortened for brevity}


  return (<div>  /**/ </div);
}
```

주요 차이점은 Vue에서 React의 일반 상태 개체로 상태에 액세스한다는 것입니다. 간단히 상태로부터 필요한 것을 파괴하고 반응 기능 구성 요소 내에서 사용할 수 있습니다.

### 후크 공장

React Hooks에서 좋아하는 또 다른 중요한 개념은 구성 로직을 구성 및 재사용하는 것입니다. 그렇지 않으면 사용자 지정 Hooks로 알려져 있습니다. `reactive`에서 후크공장으로 이것을 달성할 수 있다.

주어진 숫자 배열의 평균을 계산하고 배열의 숫자를 입력 값으로 증가시키기 위한 사용자 지정 후크를 작성한다고 가정합시다. use Calculation(계산 사용) 후크를 작성하겠습니다.

```coffeescript
import { createSetup, ref, computed, onUnmounted, onMounted } from "reactivue";

export interface Props {
  numbers: Array<number>;
}

// create a custom hook that can be reused
export const useCalculation = createSetup((props: Props) => {
  const numbers = ref(props.numbers);
  const average = computed(
    () => numbers.value.reduce((a, b) => a + b) / numbers.value.length
  );
  const increment = (by: number) => {
    return numbers.value.map((num) => {
      return num + by;
    });
  };

  onMounted(() => console.log("Hello World"));
  onUnmounted(() => console.log("Goodbye World"));

  return { numbers, average, increment };
});

export default useCalculation;
```

그런 다음 다음과 같이 이 논리를 재사용할 수 있습니다.

```js
import * as React from "react";
import { ChangeEvent, useState } from "react";

import { Props, useCalculation } from "./useCalculation";

export const App = (props: Props) => {
  const [myNumbers, setMyNumbers] = useState(props.numbers);
  const { increment, average } = useCalculation({
    numbers: myNumbers
  });

  const onChange = ({
    target: { value }
  }: ChangeEvent<HTMLInputElement>) => {
    setMyNumbers(increment(parseInt(value,10)));
  };
  return (
    <div>
      <input type="number" onChange={(e) => onChange(e)} />
      <ul>
        {" "}
        {myNumbers.map((num, index) => {
          return <li key={index}>{num}</li>;
        })}
      </ul>
      <br />
      <div>The Average is of numbers = {average} </div>
    </div>
  );
};
```

작동 데모는 다음과 같습니다.

## 추가 API

### 라이프 사이클

reactive는 Vue의 기본 라이프사이클 후크를 구현한 다음 React의 라이프사이클 후크와 바인딩합니다. React 등가물이 없는 일부 라이프사이클의 경우 호출해야 할 때 가까운 곳에 호출됩니다(예: OnMounted는 "onCreated" 직후에 호출됩니다).

그것들은 뷰에서처럼 대부분의 시간을 사용할 수 있습니다.

- `DefineComponent() – React 기능 구성 요소를 반환하는 설정 함수 및 렌더 함수를 수락합니다.
- useSetup() – Composition API의 설정을 해결합니다.
- `create Setup() – 재사용 가능한 사용자 지정 후크로 로직을 포장하는 공장

## 결론

여기서 우리는 두 개의 강력한 도구에서 하나의 개념을 혼합하고 일치시킬 수 있는 흥미로운 가능성을 탐구했다. 비록 이것이 여전히 매우 실험적이긴 하지만, Vue와 Vue의 반응성 시스템의 단순성과 React의 가장 좋은 부분 등 우리가 누릴 수 있는 큰 이점을 볼 수 있습니다.

Vue에서 반응성 데이터와 계산된 속성을 설정한 다음 소품 또는 상태 개체를 통해 반응에서 어떻게 작업할 수 있는지 살펴보았다. 우리는 이를 쉽게 파악할 수 있는 예와 작은 애플리케이션을 통해 가능성을 파악했다. 예를 들어 멋진 프로젝트를 구축하면 한 걸음 더 나아갈 수 있습니다. Vue 😃의 관점에서 볼 때 가장 좋은 반응이 될 것입니다.