---
layout: post
title: "Vue 슬롯에 대한 심층 분석"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/vue-city.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/vue-city.png?fit=730%2C487&ssl=1)

기존 상위 구성 요소에서 하위 구성 요소 방식으로가 아니라 구성 요소를 재사용할 수 있도록 하는 방법으로 여러 구성 요소 간에 데이터를 전달하고자 할 수 있습니다.

구성 요소는 동일한 종류의 코드를 다시 렌더링하기 위해 서로 다른 위치에 덤프할 수 있는 웹 템플릿을 만드는 데 사용됩니다. 구성 요소를 재사용할 수 있지만 귀속된 속성에 따라 다른 위치에서 구성 요소를 다르게 렌더링할 수 있다면 얼마나 멋질까요? 꽤 멋지다.

Vue 슬롯을 사용하면 구성 요소의 일부 또는 전체를 다른 사용 사례에 따라 다르게 렌더링할 수 있는 재사용 가능한 템플릿으로 전환할 수 있습니다. 슬롯에 넣기만 하면 됩니다.

이 기사에서는 Vue 슬롯의 개념을 이해하고 사용하는 방법을 보여드리겠습니다.

## 슬롯의 기본 사용

슬롯의 정의에서 전체 개념을 이해합니다. 즉, 구성요소를 재사용할 수 있도록 만드는 것입니다. 이제 Vue 슬롯을 보여주는 몇 가지 예를 살펴보고 이를 활용하는 방법에 대해 알아보겠습니다.

`앱`에 버튼 몇 개를 만들어 보겠습니다.버튼을 vue하고 `Buttons`에서 다시 사용할 수 있도록 합니다.vue:

```xml
/**App.vue**/
<template>
  <div id="app">
  <button-helper>
    <button type="button" class="Red">Red</button>
    <button type="button" class="Green">Green</button>
    <button type="button" class="Blue">Blue</button>
  </button-helper>
  </div>
</template>
<script>
import buttonHelper from './components/Buttons.vue'
export default {
  name: 'App',
  components: {
    'button-helper':buttonHelper
  }
    }
</script>

<style>
.Red{
  width:120px;
  height: 50px;
  margin: 50px;
  padding: 10px;
  background-color:red;
  text-align: center;
  cursor:pointer;
}
.Green{
  width:120px;
  height: 50px;
  margin: 50px;
  padding: 10px;
  background-color:Green;
  text-align: center;
  cursor:pointer;
}
.Blue{
  width:120px;
  height: 50px;
  margin: 50px;
  padding: 10px;
  background-color:blue;
  text-align: center;
  cursor:pointer;
}
</style>
```

```xml
//Buttons.vue

<template>
<div>
  <slot></slot>
  </div>
</template>
<script>
export default {
  name: 'Buttons',
    }
</script>
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/firstImage.png?resize=730%2C202&ssl=1)

위의 예에서는 `<App>` 템플릿에 버튼을 몇 개 생성했습니다. 우리는 또한 그들을 `Buttons.vue`로 불렀다. 앱에서 `슬롯`을 제거하면.vue, `<h1> 버튼 목록</h1>`만 표시됩니다.

하위 구성 요소 태그에 속성을 내장하는 것만으로는 한 구성 요소(`App.vue`)에서 다른 구성 요소(`Buttons.vue`)로 구성 요소의 속성을 전달할 수 없기 때문입니다.

단추가 `Buttons.vue`에서 렌더링되는 방식을 제어하려면 어떻게 해야 합니까? 녹색 단추가 먼저 렌더링되고 빨간색 단추가 마지막으로 렌더링되기를 원할 것입니다.

다음은 구성 요소를 다르게 렌더링하여 재사용할 수 있는 방법을 보여 주는 예입니다.

```xml
//App.vue

<template>
  <div id="app">
  <button-helper>
    <h1 slot="title">List of Buttons</h1>
    <button slot="red" type="button" class="Red">Red</button>
    <button slot="green" type="button" class="Green">Green</button>
    <button slot="blue" type="button" class="Blue">Blue</button>
  </button-helper>
  </div>
</template>
<script>
import buttonHelper from './components/Buttons.vue'
export default {
  name: 'App',
  components: {
    'button-helper':buttonHelper
  }
    }
</script>

<style>
.Red{
  width:120px;
  height: 50px;
  margin: 50px;
  padding: 10px;
  background-color:red;
  text-align: center;
  cursor:pointer;
}
.Green{
  width:120px;
  height: 50px;
  margin: 50px;
  padding: 10px;
  background-color:Green;
  text-align: center;
  cursor:pointer;
}
.Blue{
  width:120px;
  height: 50px;
  margin: 50px;
  padding: 10px;
  background-color:blue;
  text-align: center;
  cursor:pointer;
}
</style>
```

```xml
//Buttons.vue
<template>
<div>
  <h1>This is the list of buttons from App.vue</h1>
<div>
  <slot name="title"></slot>
  <slot name="green"></slot>
</div>
<div>
  <slot name="title"></slot>
  <slot name="blue"></slot>
</div>
<div>
  <slot name="title"></slot>
  <slot name="red"></slot>
  </div>
<div>
  <slot name="title"></slot>
  <slot name="blue"></slot>
  <slot name="red"></slot>
  <slot name="green"></slot>
</div>
</div>
</template>
<script>
export default {
  name: 'Buttons',
    }
</script>
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/secondImage.png?resize=730%2C639&ssl=1)

위에, 우리는 우리의 슬롯에 이름을 붙였습니다. 둘 이상의 속성을 슬롯으로 전달할 때 이 기능은 매우 중요합니다. 슬롯에 이름을 할당해야 합니다. 예를 들어, 호출할 때 모든 슬롯이 함께 표시되지 않도록 할 수 있습니다.

이렇게 하면 하위 구성 요소의 이름을 불러 원하는 슬롯을 호출할 수 있습니다.

자식 구성 요소에 전달하려는 상위 구성 요소의 속성에 "disp="name"을 추가하여 슬롯 이름을 지정합니다. 우리는 우리가 우리의 슬롯이 호출되기를 원하는 위치에 `="이름"을 더함으로써 우리의 자식 구성 요소에서 명명된 슬롯을 부른다. 이 메서드를 이름 속성을 가진 명명 슬롯이라고 합니다.

또한 "<template>"의 "v-slot" 지시어를 사용하여 템플릿의 이름을 지정할 수 있습니다. 이렇게 하면 슬롯 이름이 v-slot의 주장으로 전달됩니다.

```xml
//Buttons.vue
<template>
  <div>
    <h1>List of Buttons</h1>
    <slot name="green"></slot>
    <slot name="red"></slot>
    <slot name="blue"></slot>
  </div>
</template>
<script>
export default {
  name: 'Buttons',
    }
</script>
```

```xml
//App.vue
<template>
  <div id="app">
    <Buttons>
      <template v-slot:green>
        <button class="Green">I am Green</button>
      </template>
    </Buttons>
      <Buttons>
      <template v-slot:blue>
        <button class="Blue">I am Blue</button>
      </template>
    </Buttons>
      <Buttons>
      <template v-slot:red> 
        <button class="Red">I am Red</button>
      </template>
    </Buttons>
    </div>
</template>
<script>
import Buttons from './components/Buttons'
export default {
  name: 'App',
  components: {
    Buttons,
  }
    }
</script>
<style>
.Red{
  width:120px;
  height: 50px;
  margin: 50px;
  padding: 10px;
  background-color:red;
  text-align: center;
  cursor:pointer;
}
.Green{
  width:120px;
  height: 50px;
  margin: 50px;
  padding: 10px;
  background-color:Green;
  text-align: center;
  cursor:pointer;
}
.Blue{
  width:120px;
  height: 50px;
  margin: 50px;
  padding: 10px;
  background-color:blue;
  text-align: center;
  cursor:pointer;
}
</style>
```

### 데이터 바인딩

이제 바인딩 식을 통해 슬롯을 전달하는 몇 가지 고급 방법을 살펴보겠습니다. 이러한 방식으로, 우리는 특히 DOM을 조작할 때와 같이 사용자의 입력으로부터 사용 사례를 기반으로 슬롯을 렌더링할 수 있습니다.

사용자의 입력에 따라 하위 구성 요소(`Buttons.vue`)에서 상위 구성 요소(`App.vue`)로 데이터를 바인딩하는 간단한 프로그램을 만들 예정입니다.

```xml
// Button.vue

<template>
  <div class="hello">
    <h1>{ msg }</h1>
    <slot name="propsTest" :count="count" :addOne="addOne" />
  </div>
</template>
<script>
export default {
  name: "Buttons",
  props: {
    msg: String
  },
  data() {
    return {
      count: 0
    };
  },
  methods: {
    addOne() {
      this.count++;
    }
  }
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
```

```xml
<template>
  <div id="app">
    <Buttons msg="Simple Slot Binding With Counter Program">
      <template v-slot:propsTest="{ count, addOne }">
        <p>{ count }</p>
        <button @click="addOne" class="Red">Add One</button>
      </template>
    </Buttons>
  </div>
</template>
<script>
import Buttons from "./components/Buttons";
export default {
  name: "app",
  components: {
    Buttons
  }
};
</script>
<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.Red{
  width:120px;
  height: 50px;
  margin: 50px;
  padding: 10px;
  background-color:red;
  text-align: center;
  cursor:pointer;
}
</style>
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/thirdImage.png?resize=730%2C264&ssl=1)

위의 예에서는 `Button`에서 카운터 속성을 전달했습니다.`앱`으로 부음vue와 vue-contract를 사용하여 버튼을 추가했다.

### 폴백 컨텐츠

그래서 우리는 Button.vue의 Fallback 콘텐츠를 우리의 소유지에 할당하기를 원할지도 모른다.이러한 방식으로 `App`에 속성이 할당되지 않은 경우vue 또는 `Button`에서 데이터에 액세스하는 기타 구성 요소.vue " 구성 요소는 예비 컨텐츠로 돌아갈 수 있습니다.

```xml
//Buttons.vue
<template>
<button type="submit" class="Red">
  <slot>Submit</slot>
</button>
</template>
<script>
export default {
  name: 'Buttons',
    }
</script>
<style>
.Red{
  width:120px;
  height: 50px;
  margin: 50px;
  padding: 10px;
  background-color:red;
  text-align: center;
  cursor:pointer;
}
</style>
```

```xml
//App.vue
<template>
  <div id="app">
    <Buttons>
      send
    </Buttons>
  </div>
</template>

<script>
import Buttons from './components/Buttons'
export default {
  name: 'App',
  components: {
    Buttons,
  }
    }
</script>
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/fourthImage.png?resize=730%2C497&ssl=1)

앱 5번째 줄에 있는 송신버튼을 제거하면.vue 버튼의 텍스트가 제출에서 송신으로 바뀌는 것을 볼 수 있습니다.

## 범위 지정 슬롯

Vue에서 속성을 전달하려면 상위 구성 요소의 데이터를 정의하고 데이터에 값을 할당합니다. 그런 다음 속성 값을 하위 구성 요소에 전달하여 데이터가 하위 구성 요소의 속성이 되도록 합니다.

그러나 범위 지정 슬롯을 사용하면 자식 구성 요소의 속성을 범위 지정 슬롯에 전달한 다음 상위 구성 요소에서 액세스할 수 있습니다. 이것은 뷰에서 지나가는 전통 재산의 역행이다. 이 개념은 상위 구성 요소가 하위 구성 요소의 일부 데이터에 액세스할 수 있도록 허용하는 것입니다.

이 개념을 제대로 이해하기 위해 간단한 표현을 만들고 `슬롯스코프`를 사용하여 자식 구성 요소에서 부모 구성 요소로 속성을 전달한다.

```xml
//App.vue
<template>
  <div>
    <Buttons>
      <template slot-scope="firstSlotScope">
        <p>{firstSlotScope.text}</p>
        <!-- Renders <p>I'll get rendered inside the first slot.</p> -->
      </template>
      <template slot="not-first" slot-scope="secondSlotScope">
        <p>{secondSlotScope.text}</p>
        <!-- Renders <p>I'll get rendered inside the second slot.</p> -->
      </template>
    </Buttons>
  </div>
</template>
<script>
import Buttons from './components/Buttons';
export default {
  name: 'App',
  components: {
    Buttons,
  }
}
</script>
```

```xml
//Buttons.vue
<template>
  <div>
    <h1>Scoped Slots Example</h1>
    <slot :text="firstSlotText"></slot>
    <slot name="not-first" :text="secondSlotText"></slot>
  </div>
</template>
<script>
export default {
  name: 'Buttons',
  data() {
    return {
      firstSlotText: "I'll get rendered inside the first slot, I'll be first to be rendered.",
      secondSlotText: "I'll get rendered inside the second slot, I'll be second to be rendered."
    }
  }
}
</script>
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/scoped-slots-example.png?resize=730%2C352&ssl=1)

Vue 슬롯을 사용하여 데이터를 바인딩하고 해당 데이터에 값을 할당하는 대신 상위 구성 요소에서 하위 구성 요소의 데이터에 소품으로 액세스할 수 있습니다.

## 결론

슬롯 속성을 사용하여 구성 요소의 속성을 재사용하는 다양한 방법을 확인했습니다. 이 자료에서는 슬롯 이름을 동적으로 지정하거나 슬롯 속성을 사용하여 지정하는 방법도 설명합니다. 또한 슬롯이 있는 상위 구성요소로 특정 데이터를 전달하는 방법과 범위 슬롯을 사용하는 방법도 확인했습니다.

이 Github repo를 사용하여 슬롯을 사용하여 더 많은 연습을 할 수 있습니다. 저장소를 설정하는 모든 지침은 ReadMe.md 파일에 첨부되어 있습니다. 이 프로젝트의 전체 코드는 여기에서 찾을 수 있습니다.

Vue의 공식 사이트에서 Vue 슬롯에 대해 자세히 볼 수 있습니다. 이 기사에 대한 제안이나 질문이 있으면 언제든지 연락하세요.