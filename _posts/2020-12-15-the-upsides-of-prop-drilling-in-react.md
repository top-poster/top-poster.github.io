---
layout: post
title: "반응 시 소자 드릴링의 업사이드"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/react-drilling.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/react-drilling.png?fit=730%2C487&ssl=1)

## 소품 시추 소개

React를 사용할 때는 항상 다른 구성 요소와 데이터를 공유할 필요가 있었습니다. 이는 가장 기본적인 방법으로 프로펠링 시추 작업을 수행할 수 있습니다. 프로펠링은 구성 요소 간의 단방향 데이터 공유를 가능하게 합니다. 데이터가 소품 형태로 전달되거나 공유되었습니다.

아래 다이어그램을 살펴보겠습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/props-drilling.png?resize=730%2C411&ssl=1)

C 컴포넌트에서 A 컴포넌트의 데이터에 액세스하려면, B 컴포넌트의 소품으로 전달되어야 하며, 마지막으로 C 컴포넌트의 소품으로 전달되어야 합니다. 이를 나사산이라고 합니다.

위의 설명에서 여러분은 프로펠링의 기본과 우리가 왜 그것이 필요한지 이해할 수 있습니다.

이제 위에서 배운 내용을 사용하여 간단한 애플리케이션을 만들어 보겠습니다. 이 튜토리얼의 모든 코드는 여기에서 찾을 수 있습니다.

먼저, React 애플리케이션의 src 폴더에 두 개의 파일을 생성하고 각각 app.js와 name.js의 이름을 지정해 봅시다.

그런 다음 아래 코드를 name.js 파일에 복사합니다.

```js
import React from 'react';
const Name = () =>{
  return(
  <div>
     {/* names should be here*/}
  </div>
  )
}
export default Name;
```

이제 app.js 파일에 아래 코드를 추가해 보겠습니다.

```js
import React,{useState} from 'react';
import Name from './Names'
const App = () =>{
  const [data , setData] = useState([
    {
     name:'Ijeoma Belinda',
     age: 13,
    },
    {
     name:'Ozioko Chioma',
     age: 17,
    }
  ])
  return(
    <Name data = {data} />
  )
}
export default App;
```

우리가 `name.js`에 표시해야 할 데이터는 `app.js` 파일에 상태로 저장됩니다. 소품으로 전달되려면 16행 코드를 다음과 같이 교체해야 합니다.

```xml
<Name data = {data} />
```

이제 다음과 같이 name.js 파일에 사용할 수 있습니다.

```js
import React from 'react';
const Name = (props) =>{
  return(
  <div>
      {props.data.map( unitData => <h1>{unitData.name}</h1>)}
  </div>
  )
}
export default Name;
```

app.js 및 name.js에 대한 최종 코드는 다음과 같습니다.

```js
import React,{useState} from 'react';
import Name from './Names'
const App = () =&gt;{
  const [data , setData] = useState([
    {
     name:'Ijeoma Belinda',
     age: 13,
    },
    {
     name:'Ozioko Chioma',
     age: 17,
    }
  ])
  return(
    &lt;Name data = {data} /&gt;
  )
}
export default App;
```

아래 name.js 파일:

```js
import React from 'react';
const Name = (props) =>{
  return(
  <div>
      {props.data.map( unitData => <h1>{unitData.name}</h1>)}
  </div>
  )
}
export default Name;
```

`props`는 함수 구성 요소에 매개 변수로 추가됩니다.

모든 작업이 올바르게 수행된 경우, 이제 브라우저에 저장된 데이터가 표시되어야 합니다.

### 소품 시추의 이점

소규모 애플리케이션으로 작업할 때, 소량 드릴링은 구성 요소 간의 빠르고 쉬운 데이터 전송 방법 역할을 할 수 있습니다. 다른 일반적인 데이터 전송 방법과 달리, 소자 드릴링은 비교적 쉽게 학습하고 구현할 수 있다.

이 외에도, 소품으로 전달된 데이터는 새로운 변경사항을 반영하기 위해 상태 변경 시 쉽게 업데이트할 수 있습니다.

이 점을 더 이해하기 위해 app.js 파일로 돌아가서 다음과 같은 새 사용자의 데이터로 상태를 업데이트해 보겠습니다.

```coffeescript
import React,{useState , useEffect} from 'react';
import Name from './Names'
const App = () =>{
  const [data , setData] = useState([
    {
     name:'Ijeoma Belinda',
     age: 13 },
      {
     name:'Ozioko Chioma',
     age: 17
    }
  ])
  useEffect(() => {
   setData([
   ...data,
    {
      name:'Eze ifechi',
      age: 17,
     }
  ])
  },[])
  return(
    <Name data = {data} />
  )
}
export default App;
```

추가 코드나 논리를 추가하지 않고도 브라우저의 렌더링된 페이지가 새로운 변경사항을 반영하도록 즉시 업데이트되었음을 알 수 있습니다.

### 프로펠러 드릴링의 단점

프로펠러 드릴링에는 단점이 있지만, 어떤 경우에는 그럴 가치가 없습니다. 코드베이스가 증가하면 프로펠링으로 인해 코드가 지나치게 복잡해질 수 있으며, 추가 기능이 추가될수록 더 악화될 수 있습니다.

이 외에도 소품은 데이터가 하위 구성요소로 전달되기 위해 꼭 필요하지 않은 구성요소로 전달될 수 있어 코드베이스가 불필요하게 증가할 수 있습니다.

이를 입증하기 위해 singleName.js로 알려진 새 파일을 추가한 다음 아래와 같이 name.js 파일로 렌더링하여 코드를 조정하십시오.

```js
import React from 'react';
import SingleName from './singleName'
const Name = (props) =>{
  return(
  <div>
      {props.data.map( unitData => <SingleName unitData = {unitData} />)}
  </div>
  )
}
export default Name;
```

다음으로 다음 코드를 새 파일(`singleName.js`)에 추가하겠습니다.

```js
import React from 'react';
const SingleName = (props) =>{
  return(
  <div>
      <h1>{props.data.name}</h1>
  </div>
  )
}
export default SingleName;
```

하위 구성 요소(이 경우 singleName.js)로 이동하는 데 데이터가 필요하지 않더라도 name.js 파일에 전달됩니다. 특히 프로젝트가 비교적 작은 경우에는 위의 내용이 일부에게는 문제가 되지 않을 수 있습니다.

코드베이스가 증가함에 따라, 특히 소품이 스레드의 중간쯤에 이름을 바꾼 경우에는 소품 이름을 추적하기가 어려워질 수 있다.

이런 상황에 부딪히면 해결이 어려워질 수 있습니다.

## 이러한 문제를 해결하는 방법

가장 먼저 해야 할 일은 부품을 덜 사용하는 것입니다. 코드베이스가 너무 커지는 것을 방지하기 위해 적절한 드릴링이 필요한 불필요한 구성 요소를 추가하지 마십시오.

따라서 코드를 한 구성 요소에만 국한해서는 안 됩니다. 그러나 추가 구성 요소를 추가하지 않아도 되는 상황이 발생할 수 있습니다. 이 경우 코드를 지나치게 복잡하게 만들고 싶지 않다는 점을 명심하십시오.

기본 소품 이름을 항상 사용하여 쉽게 추적할 수 있습니다.

응용 프로그램에서 사용할 수 있는 프로펠링에는 다른 대안이 있습니다. 애플리케이션의 거의 모든 부분에서 데이터가 필요한 경우 컨텍스트 API를 사용할 수 있습니다. 컨텍스트 API는 애플리케이션 여러 부분에 필요한 모든 데이터를 제공할 수 있으며, 이를 통해 프롭 드릴링과 함께 발생하는 모든 문제를 해결할 수 있습니다.

## 결론

비록 단점이 있더라도, 프로펠러 드릴링은 여전히 구성 요소들 간의 데이터 전송의 실행 가능한 방법이며, 상대적으로 작은 애플리케이션에 사용되어야 한다. 그러나 더 큰 응용 프로그램을 사용하는 경우에는 이 방법이 주요 데이터 전송 방법으로 권장되지 않습니다.