---
layout: post
title: "Vuex 및 Axios로 API 사용 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/API-1.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/API-1.png?fit=730%2C487&ssl=1)

API(application programming interface)는 애플리케이션에 액세스하기 위한 프로그래밍 표준의 집합이다. 즉, API를 사용하면 백엔드 및 프런트엔드 애플리케이션이 사용자 모르게 서로 통신할 수 있습니다.

API "사용"은 요청을 받고 API를 통해 응답을 보내는 것을 의미합니다. 이 문서에서는 Vuex 및 Axios를 사용하는 서버의 API를 사용하는 방법에 대해 설명합니다.

## Axios와 Vuex 소개

Angular 2, JQuery, 그리고 버전 2.0까지 Vue.js와 같은 일부 프레임워크는 HTTP API가 내장되어 있다. Vue.js 2.0에서 개발자들은 내장 HTTP 클라이언트 모듈인 Vue-resource가 더 이상 필수적이지 않으며 타사 라이브러리로 대체될 수 있다고 결정하였다. 현재 가장 추천하는 도서관은 악시오스입니다.

Axios는 기본적으로 약속을 사용하는 유연한 HTTP 클라이언트 라이브러리이며 클라이언트와 서버에서 모두 실행되므로 서버측 렌더링 시 데이터를 가져오는 데 적합합니다. Vue.js와 함께 사용하면 편리합니다.

깊이 내포된 구성요소에 대해서는 전달 소품이 어렵거나 거의 불가능할 수 있습니다. 부모/자녀 인스턴스 직접 참조를 사용하거나 이벤트를 통해 상태의 여러 복사본을 변경 및 동기화하려고 시도할 수 있습니다. 이러한 옵션은 유지 관리할 수 없는 코드와 버그로 이어질 수 있으므로 권장하지 않습니다.

Vuex는 Vue.js 애플리케이션을 위한 상태 관리 패턴 및 라이브러리이다. 응용 프로그램의 모든 구성 요소를 중앙 집중식으로 저장하는 역할을 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/actions-mutations-.gif?resize=730%2C411&ssl=1)

## Vuex 사용 시기

대규모 단일 페이지 애플리케이션(SPA)을 구축하지 않은 경우 Vuex를 사용하는 것이 너무 복잡하고 불필요하다고 느낄 수 있습니다. 그러나 중규모에서 대규모 SPA를 구축하는 경우, 상태를 더 잘 처리하고 구성 요소 간에 상태를 공유하는 방법에 대해 생각하게 되는 상황에 직면했을 가능성이 높습니다. 그렇다면, Vuex는 여러분에게 적합한 도구입니다!

## Vuex를 사용해야 하는 이유

일반적으로 Vuex를 사용합니다.

- 구성 요소가 상태를 공유하고 업데이트해야 하는 경우
- Vuex는 데이터/상태의 단일 진리 소스를 제공하기 때문입니다.
- 상태 관리 기능이 있는 경우 하나의 구성 요소에서 여러 구성 요소를 통해 이벤트를 전달할 필요가 없기 때문입니다.
- 글로벌 상태는 반응성이므로, 즉 상태를 변경하면 글로벌 상태를 사용하는 다른 모든 구성 요소에서 업데이트할 수 있습니다.

## 상태 관리 및 Vuex 상태 관리 패턴 이해

상태 관리는 애플리케이션 상태 또는 더 중요한 것은 주어진 시간에 사용자에게 제시되어야 하는 데이터의 관리이다.

Vue.js와 같은 SPA에서는 지정된 시간에 여러 구성 요소가 데이터를 서버로 다시 보내기 전에 데이터와 상호 작용하고 변경할 수 있습니다. 따라서 개발자는 Vuex 상태 관리를 통해 이러한 변경 사항(상태)을 관리할 수 있는 방법이 필요합니다.

Vuex는 공유 상태를 구성 요소에서 추출하여 글로벌 싱글톤으로 관리하고 구성 요소가 트리 내 위치에 관계없이 상태 또는 트리거 작업에 액세스할 수 있도록 하는 상태 관리 패턴입니다.

## 가장 간단한 Vuex 저장소 구조를 설정하는 방법

Vue 앱을 만들려면 Vue CLI가 필요합니다. Vue CLI가 설치되어 있지 않으면 터미널에서 다음을 실행합니다.

```coffeescript
npm install -g @vue/cli
```

Vue CLI가 시스템에 이미 설치되어 있는지 확인하려면 아래 코드를 실행하십시오. Vue가 이미 설치되어 있는 경우 Vue 버전이 나타납니다.

```css
vue --version
2.9.6
```

이제 터미널에서 다음 코드를 실행하여 Vue 프로젝트를 만듭니다.

```undefined
/** if you want to create the vue project in the folder you
are currently on, use this code**/
vue create .

/**if you want to create a new folder for your vue app,
you should use this code**/
vue create myApp

/**if you are using Vue CLI version lesser than 3.0 use this code.**/
vue-init webpack myApp
```

그런 다음 아래 이미지와 같은 질문에 답하여 Vue 앱을 만드십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/tk_AdsCSz.png?resize=730%2C352&ssl=1)

그런 다음 다음 코드를 사용하여 프로젝트에 필요한 종속성을 설치합니다.

```undefined
/** to install vuex store for the project**/
npm install vuex

/**to install axios for handling http requests**/
npm install axios
```

## 단순 Vuex 저장소 구조 설정

먼저 `src` 폴더에 폴더를 생성하고 이름을 `store`로 지정합니다. `store` 폴더에서 파일을 생성하고 이름을 `index.js`로 지정합니다.

```css
├── index.html
└── src
    ├── components
    ├── App.vue
    └──store
       └── index.js        # where we assemble modules and export the store
```

그런 다음 `store/index.js` 파일에 다음 코드를 입력합니다.

```js
// import dependency to handle HTTP request to our back end
import axios from 'axios'

//to handle state
const state = {}

//to handle state
const getters = {}

//to handle actions
const actions = {}

//to handle mutations
const mutations = {}

//export store module
export default {
    state,
    getters,
    actions,
    mutations
}
/** we have just created a boiler plate for our vuex store module**/
```

`main.js` 파일에 다음 코드를 추가하여 스토어를 등록합니다.

```js
//add this line to your main.js file
import store from './store'

new Vue({
//add this line to your main.js file
store,
render: h => h(App)
})
//other lines have already been added to your main.js file by default
```

## Vuex 작용 및 돌연변이 설명

Vuex에서 동작은 상태를 변형시키는 대신 돌연변이를 일으킨다. 이것은 돌연변이가 상태를 변화시키거나 변화시킬 수 있지만, 행동은 변화를 영구화한다는 것을 의미한다. 액션에는 임의 비동기 작업이 포함될 수 있습니다. 즉, 작업이 순서대로 실행될 필요가 없으며 코드가 작업이 실행될 때까지 기다릴 필요가 없습니다. 프로그램이 계속 실행될 수 있습니다.

동작은 `store.dispatch` 방식으로 트리거되며, `이(가) 포함된 구성 요소로 디스패치될 수 있습니다.$store.httpsxxxx`)$ 구성 요소 메서드를 `store.dispatch`에 매핑하는 `mapActions` 도우미를 사용할 수도 있습니다.

행동이 돌연변이를 일으키기 때문에 Vuex 상태는 변화가 필요하지만, 그것은 돌연변이를 범해야만 바뀔 수 있다. Vuex 돌연변이는 각각 문자열 유형과 핸들러 함수를 가지고 있다. 핸들러 함수는 우리가 상태를 변경하거나 변경하는 곳이며, 상태를 첫 번째 인수로 수신한다.

돌연변이 처리기 함수는 동기화되어야 하며 `이`를 사용하여 구성 요소에 돌연변이를 적용할 수 있습니다.$store.commit(`xxx`)을 사용하거나 구성 요소 메서드를 `store.commit`에 매핑하는 `mapMutations` 도우미를 사용합니다.

돌연변이 및 작용이 무엇이며 어떻게 사용되는지를 이해하기 위해 JSON 자리 표시자(테스트 및 프로토타이핑을 위한 가짜 온라인 REST API)의 API를 소비하는 간단한 돌연변이를 등록합니다.

```js
// import dependency to handle HTTP request to our back end
import axios from 'axios'
import Vuex from 'vuex'
import Vue from 'vue'

//load Vuex
Vue.use(Vuex);

//to handle state
const state = {
posts: []
}

//to handle state
const getters = {}

//to handle actions
const actions = {
getPosts({ commit }) {
axios.get('https://jsonplaceholder.typicode.com/posts')
.then(response => {
commit('SET_POSTS', response.data)
})
}
}

//to handle mutations
const mutations = {
SET_POSTS(state, posts) {
state.posts = posts
}
}

//export store module
export default new Vuex.Store({
state,
getters,
actions,
mutations
})
```

이제 `myStore`라는 이름의 새 구성 요소를 생성하고 다음 코드를 여기에 붙여 넣으십시오.

터미널에서 `npm run dev`를 실행하여 애플리케이션을 서비스한 다음 `localhost:8080/myStore`로 이동하여 Vuex 애플리케이션을 확인하십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/veux-store.png?resize=730%2C548&ssl=1)

## Vuex getters가 설명했습니다.

구성 요소 간에 상태를 전달하는 과정에서 저장소 상태를 기준으로 파생된 상태를 열거해야 할 수 있습니다. Vuex에서는 Getter가 실제로 값을 저장하지 않습니다. 대신 Vuex 상태를 간접적으로 검색하고 설정합니다.

Vuex getters는 첫 번째 인수로 상태를 받고 두 번째 인수로 다른 게터들을 받을 수 있다. MapGetters는 저장소 게터를 로컬 계산 속성에 매핑하는 도우미입니다.

게터란 무엇이고 어떻게 사용하는지를 이해하기 위해, 우리는 우리의 상태를 핫 코딩하여 우리의 스토어 상태에 기초하여 파생된 상태를 계산할 것이다.

```js
// import dependency to handle HTTP request to our back end
import Vuex from 'vuex'
import Vue from 'vue'

//load Vuex
Vue.use(Vuex);

//to handle state
const state = {
products: [
{
id: 1,
title: 'Shirt',
price: '$40'
},
{
id: 2,
title: 'Trouser',
price: '$10'
},
]
}

//to handle state
const getters = {
allProducts: (state) => state.products
}

//to handle actions
const actions = {}

//to handle mutations
const mutations = {}

//export store module
export default new Vuex.Store({
state,
getters,
actions,
mutations
})
```

다음으로, "myStore" 폴더에 다음 코드를 붙여넣습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/vuex-store-getter.png?resize=730%2C418&ssl=1)

## 동작, 돌연변이, 상태 및 getter를 사용하여 Vuex 및 Axios와 API 사용

Vuex 작업, 상태, 돌연변이 및 getter를 사용하여 Axios를 사용하여 Vuex 저장소를 생성하고 API를 사용하는 방법을 올바르게 이해하기 위해 가짜 JSON 백엔드에서 사용자 정보를 가져오는 간단한 Vuex 애플리케이션을 생성합니다.

```js
// import dependency to handle HTTP request to our back end
import axios from 'axios'
import Vuex from 'vuex'
import Vue from 'vue'

//load Vuex
Vue.use(Vuex);

//to handle state
const state = {
users: []
}

//to handle state
const getters = {
allUsers: (state) => state.users
}

//to handle actions
const actions = {
getUsers({ commit }) {
axios.get('https://jsonplaceholder.typicode.com/users')
.then(response => {
commit('SET_USERS', response.data)
})
}
}

//to handle mutations
const mutations = {
SET_USERS(state, users) {
state.users = users
}
}

//export store module
export default new Vuex.Store({
state,
getters,
actions,
mutations
})
```

`myStore` 폴더에 다음 코드를 붙여넣습니다.

```xml
<template>
  <div class="hello">
    <h1>{ msg }</h1>
    <h1>Made By Getters</h1>
  <div v-for='gettersuser in gettersusers' :key='gettersuser.id'>
    {gettersuser.id} {gettersuser.name} {gettersuser.address}
    </div>
    <h1>Made By Actions</h1>
  <div v-for='user in users' :key='user.id'>
    {user.id} {user.name} {user.address}
    </div>
  </div>
</template>

<script>
export default {
  name: 'myStore',
  data () {
    return {
      msg: 'Welcome to my Vuex Store'
    }
  },
  computed: {
    gettersusers() {
    return this.$store.getters.allUsers
    },
    users() {
    return this.$store.state.users
    }
  },
  mounted() {
    this.$store.dispatch("getUsers");
  }
}
</script>
```

이제 터미널에서 `npm run dev`를 실행하여 애플리케이션을 서비스하고 다시 `localhost:8080/myStore`로 이동하여 첫 번째 Vuex 애플리케이션이 생성되었는지 확인하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/vuex-store-getters-and-actions.png?resize=730%2C465&ssl=1)

## 다양한 Vuex 저장소 구조

아래는 가장 간단한 Vuex 저장소 구조로, 작업, 게터, 상태 및 돌연변이를 호출하여 `index.js 파일`로 내보냅니다.

```css
├── index.html
└── src
    ├── components
    ├── App.vue
    └──store
       └── index.js         # where we assemble modules and export the store
```

애플리케이션이 커짐에 따라 당사의 작업, 돌연변이, 게터 및 상태 모듈을 서로 다른 파일로 분리해야 할 수도 있습니다.

```css
├── index.html
└── src
    ├── components
    ├── App.vue
    └──store
       ├── index.js        # where we assemble modules and export the
       ├── actions.js      # root actions
       ├── mutations.js    # root mutation
       ├── getters.js      # root getters
       └── state
```

단일 상태 트리를 사용하기 때문에 응용 프로그램의 모든 상태는 하나의 큰 개체 안에 포함됩니다. 이는 우리의 행동, 돌연변이, 상태가 커질수록 우리의 응용 프로그램이 커져 가게가 훨씬 더 커질 수 있기 때문에 우리가 그것을 따라갈 수 없을지도 모르기 때문입니다.

훌륭한 해결책은 우리 가게를 모듈로 묶는 것이다. 각 모듈은 자체 상태, 돌연변이, 동작 및 게터를 포함할 수 있으며 중첩된 모듈도 포함할 수 있습니다.

```css
├── index.html
├── main.js
├── api
│   └── ... # abstractions for making API requests
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # where we assemble modules and export the store
    ├── actions.js        # root actions
    ├── mutations.js      # root mutations
    └── modules
        ├── cart.js       # cart module
        └── products.js
```

## 결론

본 튜토리얼에서는 Vue 애플리케이션에 Vuex가 필요한 이유와 상태 관리에 대해 알아보았습니다. 또한 Vuex 스토어가 무엇으로 구성되어 있는지, Vuex에서 어떤 동작, 돌연변이, 게터 및 상태가 있는지 살펴보았습니다.

또한 Vuex를 시연하기 위한 간단한 Vue 애플리케이션뿐만 아니라 다양한 Vuex 저장소 구조를 학습하여 애플리케이션에 가장 적합한 구조를 결정할 수 있습니다.