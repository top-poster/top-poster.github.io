---
layout: post
title: "Vue.js의 대응성에 대한 가이드"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/vue-reactivity.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/vue-reactivity.png?fit=730%2C487&ssl=1)

이 기사에서는 Vue.js의 반응성에 대해 살펴보고 어떻게 작동하는지 이야기하겠습니다.

## 도입

Relative(반응도)는 응용 프로그램 상태의 변경 사항이 DOM에 자동으로 렌더링되는 상황을 설명합니다.

Vue는 최신 반응형 사용자 인터페이스 및 단일 애플리케이션을 구축하기 위한 점진적인 프레임워크입니다. Vue는 데이터 모델에 바인딩된 템플릿 선언을 통해 HTML 마크업을 증강 렌더링할 수 있도록 합니다.

Vue.js는 데이터 모델과 구성요소 구성의 선언적 렌더링에 초점을 맞춘 적응형 반응형 아키텍처를 가지고 있다.

Vue는 Vue의 Relativeity를 통해 애플리케이션의 모든 계층에서 통신하고 뷰 베이스 데이터 수정을 업데이트할 수 있습니다.

### Vue 3에서 프록시를 사용하여 반응성을 처리하는 방법

리액티브(reactivity)는 데이터 흐름과 변화의 전파를 기반으로 하는 프로그래밍 패러다임이다. 이를 통해 데이터 변경이나 이벤트에 대해 선언적인 방식으로 조정할 수 있습니다.

Vue에서는 반응성 시스템이 초기 버전부터 항상 존재해왔다. Vue의 가장 강력한 기능 중 하나는 반응성 시스템입니다.

## Vue 2의 반응 상태

Vue 3에서 반응성의 작동 방식을 살펴보기 전에 Vue 2 애플리케이션에서 반응성 데이터를 생성하는 방법을 간략히 살펴보겠습니다. Vue가 데이터의 변경 내용을 추적하려면 데이터 기능에서 반환된 개체 외부에 속성을 선언해야 합니다.

```xml
<template>
  <h1>My name is { name }</h1>
</template>
<script>
  export default {
    data() {
      return {
        name: "amycruz"
      };
    }
  };
</script>
```

Vue 2에서 Vue는 모든 속성을 살펴보고 `Object.defineProperty()`를 사용하여 각 데이터에 대한 Getter와 세터를 생성하고 데이터 변경 사항을 추적하고 있는지 확인합니다.

MDN 웹 문서에 따르면 object.defineProperty()는 개체에 직접 새 속성을 정의합니다. 또한 개체의 기존 속성을 수정하고 개체를 반환할 수도 있습니다. object.defineProperty를 사용하면 개체의 Getter와 Setter를 쉽게 설정할 수 있습니다.

```js
const data = {
    count: 10
};

const newData = {

}

Object.defineProperty(newData, 'count', {
  get() { return data.count; },
  set(newValue) { data.count = newValue; },
});

console.log(newData.count); // 10

newData.count = 20;

console.log(newData.count); // 20
```

여기에 데이터(data)와 newData(newData)라는 두 가지 개체를 추가했습니다. 데이터는 우리의 원래 객체를 담고 있고 new Data는 객체의 역할을 할 것이다. `object.defineProperty()를 사용하여 속성을 설정합니다.

속성이 액세스 또는 수정되는 시기를 추적하려면 다음을 수행합니다.

```js
const data = {
    count: 10
};

const newData = {

}

function track(){
  console.log("Prop accessed")
}
function trigger(){
  console.log("Prop modified")
}
Object.defineProperty(newData, 'count', {
  get() {track();return data.count; },
  set(newValue) { data.count = newValue;trigger(); },
});

console.log(newData.count); 
// Prop accessed 
// 10

newData.count = 20;
// Prop modified

console.log(newData.count); 
// Prop accessed
// 20
```

트리거 및 트랙 기능을 사용하면 변경이 발생하는 시기를 쉽게 알 수 있으며 종속성 추적 및 변경 알림 작업을 수행할 수 있습니다.

특히 Vue 2에서는 리액티브의 한계를 피하기 위해 리액티브가 Vue 3인 방법을 확실히 이해하는 것이 중요합니다. 또한 구성 API와 같은 새로운 기능을 사용하려면 리액티브를 이해해야 합니다.

기본적으로 JavaScript는 자연적으로 반응하지 않습니다.

```js
// index.js 
let cost = 10
let qty =  5
let total = cost * qty
console.log(`The total amount is ${total}`) // The total is 50
cost = 20
console.log(`The total amount is ${total}`) // The total is 50. total doesnt get updated
```

코드 예제를 보면 비용을 20으로 변경한 후에도 총 값이 업데이트되지 않은 것을 알 수 있습니다.

코드에 반응성을 약간 더하기 위해 계산을 함수로 바꿀 수 있습니다.

```js
let cost = 10
let qty =  5
let total = 0

//declare a computation
const computeTotal = () => total = cost * qty
computeTotal(); 
console.log(total) // 50
cost = 20
console.log(total) // still outputs 50
computeTotal() //run the computation again
console.log(total) // outputs 100
```

보시는 것처럼 계산 기능을 실행하여 총계의 계산된 값을 업데이트할 수 있습니다.

위의 코드 예에서는 업데이트된 합계를 얻기 위해 비용 값이 변경된 후 계산을 수동으로 실행했다. 이 프로세스는 신뢰할 수 없으므로 계산을 수동으로 실행해야 합니다. 이는 성능에 부정적인 영향을 미칠 수 있기 때문에 문제가 될 수 있습니다.

## 3화에서의 반응성

Vue 3에서는 Vue 2의 많은 단점을 해결하기 위해 반응형 API가 다시 작성되었습니다. 반응형 시스템은 JavaScript의 프록시를 활용하기 위해 다시 작성되었습니다. 프록시는 값을 가져오고 설정하는 것과 같은 작업을 가로채는 개체 또는 함수를 감싸는 래퍼 역할을 합니다.

Vue 3의 새로운 사후 대응 시스템을 통해 프록시 개체를 사용하여 데이터의 변화를 관찰할 수 있는 더 나은 지원이 제공됩니다.

프록시 개체를 사용하면 다른 개체에 대한 프록시를 만들 수 있으며, 이 프록시는 해당 개체의 기본 작업을 가로채고 재정의할 수 있습니다.

프록시를 사용하면 get, set 등의 작업을 인터셉트하여 데이터에 액세스하거나 데이터를 변경할 때 즉시 확인할 수 있습니다.

프록시를 생성하려면 다음 두 가지 매개 변수를 전달해야 합니다.

`target`: 데이터 개체
`➡`: 가로채려는 작업을 정의하는 개체입니다.

아래의 기본 예를 살펴보겠습니다.

```cpp
const data = {
  meal: "rice"
}

const handler = {
  get(target, prop, receiver){
    console.log("Data Get: ", target, prop);
    return target[prop];
  },
  set(target, key, value, receiver) {
    console.log("Data Set: ", target, key, value);
    return target[key] = value;
  }
}

const proxy = new Proxy(data, handler);

console.log(proxy) // { meal: "rice" }
console.log(proxy.meal) 
// Data Get: { meal: "rice" } meal
// rice
proxy.meal = "yam"
// Data Set: { meal: "rice" } meal yam
// yam
```

위의 예에서는 데이터 개체를 수락한 프록시 개체를 만들었습니다. 또한 객체에서 get과 set 작업을 가로채는 데 사용하는 핸들러도 있습니다. 우리가 `식사` 소품 접근을 시도하면 `데이터겟: {식사: 쌀밥}식`이 콘솔에 기록됩니다. 우리가 프록시 객체에서 `식사` prop 값을 업데이트하려고 할 때도 같은 일이 일어난다.

개체 게터 및 세터의 정상 동작을 유지하기 위해서는 `반사` 개체를 사용하여 일반 개체 동작을 반영해야 합니다.

아래 예제를 살펴보겠습니다.

```undefined
const data = {
  meal: "rice"
}

const handler = {
  get(target, prop, receiver){
    console.log("Data Get: ", target, prop);
    return Reflect.get(target, prop, receiver);
  },
  set(target, key, value, receiver) {
    console.log("Data Set: ", target, key, value);
    return Reflect.set(target, key, value, receiver);
  }
}

const proxy = new Proxy(data, handler);

console.log(proxy) // { meal: "rice" }
console.log(proxy.meal) 
// Data Get: { meal: "rice" } meal
// rice
proxy.meal = "yam"
// Data Set: { meal: "rice" } meal yam
// yam
```

리플렉트(Reflect)를 사용하면 이전처럼 수동으로 액세스하거나 변경할 필요가 없습니다.

데이터에 액세스하거나 변경되는 시기를 확인하려면 다음 세 가지 기능을 추가할 수 있습니다.

- `track`: 다른 사람이 데이터에 액세스했을 때 알려 줍니다.
- watch: 누가 물건의 받침대를 설정했을 때 알려줍니다.
- `트리거`: 객체의 데이터가 변경되면 알려줍니다.

아래 예제를 확인하십시오.

```js
const data = {
  meal: "rice"
}
const track =(target, prop, receiver)=>{
  console.log("Data Get: ", target, prop);
}
const trigger =(target, key, value, receiver)=>{
  console.log("Data Change: ", target, key, prop, value);
}
const watch =(target, key, value, receiver)=>{
  console.log("Data Set: ", target, prop, value);
}
const handler = {
  get(target, prop, receiver){
    track(target, prop, receiver)
    return Reflect.get(target, prop, receiver);
  },
  set(target, key, value, receiver) {
     watch(target, key, value, receiver)
    if(target[key]!=value){
      trigger(target, key, value, receiver)
    }
    return Reflect.set(target, key, value, receiver);
  }
}

const proxy = new Proxy(data, handler);

console.log(proxy) // { meal: "rice" }
console.log(proxy.meal) 
// Data Get: { meal: "rice" } meal
// rice
proxy.meal = "yam"
// Data Set:  { meal: 'rice' } meal yam { meal: 'rice' }
// Data Change:  { meal: 'rice' } meal yam { meal: 'rice' }
// yam
proxy.meal = "beans"
// Data Set:  { meal: 'yam' } meal beans { meal: 'yam' }
// Data Change:  { meal: 'yam' } meal beans { meal: 'yam' }
// beans
proxy.meal = "beans"
// Data Set:  { meal: 'beans' } meal beans { meal: 'beans' }
// beans
```

여기서는 추가한 사용자 지정 기능에 따라 데이터가 변경될 때 수행할 작업을 결정할 수 있습니다.

Vue는 JavaScript 또는 Vue에서 반응성 상태를 생성하는 데 사용되는 반응성 메서드와 함께 제공됩니다. 기본적으로, 반응형 방법은 프록시를 생성하고 제공된 데이터 개체를 감싸서 궁극적으로 프록시 개체로 바꾸는 기능일 뿐입니다.

Vue는 기본적으로 데이터를 프록시 개체로 변환하여 속성이 액세스 또는 수정될 때 종속성 추적 및 변경 알림을 수행할 수 있습니다.

현재 우리가 알고 있는 모든 것을 결합하여, 우리는 우리만의 반응적인 방법을 만들 수 있어야 합니다.

```js
const reactive = (data) =>{

    const track = (target, prop, receiver) =>{
      console.log("Data Get: ", target, prop);
    }
    const trigger = (target, key, value, receiver) =>{
      console.log("Data Change: ", target, key, value, receiver);
    }
    const watch =(target, key, value, receiver)=>{
      console.log("Data Set: ", target, key, value, receiver);
    }
    const handler = {
      get(target, prop, receiver){
        track(target, prop, receiver)
        return Reflect.get(target, prop, receiver);
      },
      set(target, key, value, receiver) {
         watch(target, key, value, receiver)
        if(target[key]!=value){
          trigger(target, key, value, receiver)
        }
        return Reflect.set(target, key, value, receiver);
      }
    }
    
    const proxy = new Proxy(data, handler);
    
    return proxy
}

const store = reactive({
  count: 0
})

store.count;
// Data Get: { count: 0 } count
// 0
store.count++;
// Data Set: { count: 0 } count 1 { count: 0 }
// Data Change: { count: 0 } count 1 { count: 0 }
// 1
```

우리는 방금 우리의 데이터 객체를 프록시로 바꾸는 `reactive` 방법을 만들었다. 또한 이를 통해 모든 제품 변경 시 변경 알림을 수행할 수 있습니다.

이러한 프록시 개체는 사용자가 볼 수 없지만, 후드 아래에서 Vue가 속성에 액세스하거나 수정할 때 종속성 추적 및 변경 알림을 수행할 수 있도록 합니다. Vue 팀은 Vue 3에서 반응성 패키지를 별도의 패키지로 릴리스했습니다.

Vue는 반응성으로 만들어진 모든 개체를 내부적으로 추적하므로 항상 동일한 개체에 대해 동일한 프록시를 반환합니다.

이를 통해 모든 구성 요소 인스턴스는 구성 요소의 렌더 중에 "터치된" 속성을 종속성으로 기록하는 해당 감시기 인스턴스를 가지고 있다고 말할 수 있다. 나중에 종속성 설정기가 트리거되면 감시자에게 이를 통지하여 구성 요소가 다시 렌더링됩니다.

첫 번째 마운트 또는 렌더에서 구성 요소는 종속성(또는 개체의 데이터) 목록을 추적했습니다. 렌더링 중에 액세스한 속성입니다. 이렇게 하면 구성 요소가 이러한 각 속성의 가입자가 됩니다. 따라서 프록시가 설정된 작업을 가로채면 속성이 모든 구독 구성 요소에 다시 렌더링하도록 알립니다.

## 결론

축하합니다! 이제 Vue에서 반응성이 무엇이고 어떻게 작동하는지 잘 이해하게 되었습니다. 우리는 또한 Vue 2에서 반응 상태에 대해 배웠고 이 지식을 사용하여 Vue 3에서 반응성의 값을 보다 효과적으로 파악하고 이해했다.