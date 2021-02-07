---
layout: post
title: "Vue, Vue Router, Vuetify 및 노드-페치를 사용하여 자체 관리 애플리케이션 롤링"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/rollyourownmanagementapp.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/rollyourownmanagementapp.png?fit=730%2C487&ssl=1)

이 튜토리얼에서는 Vue, Vue Router, Vuetify 및 노드-페치와 협력하여 인벤토리 관리 애플리케이션의 프런트 엔드를 구축합니다. `Vue`는 웹상에서 사용자 인터페이스를 구축하기 위한 자바스크립트 프레임워크이며, `Vue Router`는 `Vue` 프레임워크의 공식 라우터이다. `Vuetify`는 아름답게 수작업으로 제작된 재료 구성요소를 갖춘 `Vue` UI 라이브러리이다. `node-fetch`는 API 끝점 또는 URL에 HTTP 요청을 할 수 있는 모듈입니다.

이 튜토리얼을 마치면 다음과 같은 응용 프로그램이 나타납니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/finisheddashboard.png?resize=1366%2C768&ssl=1)

## 전제조건

이 튜토리얼을 따르려면 다음이 필요합니다.

- 그래프QL과 Vue에 대한 기초적 이해
- 컴퓨터에 설치된 Docker, Node.js, NPM 및 Git
- Node.js 및 NPM 사용 방법에 대한 기본 이해

## 프로젝트 루트 디렉터리 생성

이 섹션에서는 프로젝트의 디렉터리 구조를 만듭니다. 그런 다음 애플리케이션 클라이언트의 Node.js 프로젝트를 초기화합니다.

터미널 창을 열고 GitHub에서 이 튜토리얼에 대해 미리 만든 백 엔드가 들어 있는 리포지토리를 복제합니다.

```php
git clone https://github.com/CSFM93/inventory-management-system
```

위의 명령을 실행하면 `inventory-management-system`이라는 디렉터리가 생성됩니다.

다음 디렉토리로 이동합니다.

```bash
cd inventory-management-system
```

폴더 구조는 다음과 같아야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/filelayout-1.png?resize=151%2C66&ssl=1)

`서버` 디렉토리에는 인벤토리 관리 응용 프로그램의 백엔드가 포함되어 있습니다. 백 엔드는 MongoDB 데이터베이스에 연결된 GraphQL 서버로 구성된다. 이 GraphQL 서버를 구축하기 위해 Node.js, Docker, MongoDB, graphql-yoga, mongoose 및 connect-history-api-fallback을 사용했습니다.

데이터베이스 내부에는 다음과 같은 컬렉션이 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/databasecollections.png?resize=960%2C583&ssl=1)

이제 백엔드에서 어떤 일이 벌어지고 있는지 알았으니 인벤토리 관리 애플리케이션의 프런트엔드를 구축해야 합니다.

여전히 `inventory-Management-system` 디렉토리에 `client`라는 하위 디렉토리를 생성합니다.

```undefined
mkdir client
```

`클라이언트` 디렉토리로 이동합니다.

```bash
cd client
```

`Vue CLI`를 사용하여 현재 디렉터리에 새 `Vue` 프로젝트를 생성합니다.

```undefined
vue create .
```

현재 디렉터리 유형 `y`에 새 프로젝트를 생성하라는 메시지가 표시되면:

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/generateprojectindirectory.png?resize=766%2C375&ssl=1)

사전 설정을 선택하라는 메시지가 나타나면 `수동으로 피쳐 선택`을 선택합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/pleasepickapreset.png?resize=754%2C371&ssl=1)

`Babel` 및 `라우터` 선택:

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/selectbabelandrouter.png?resize=769%2C375&ssl=1)

라우터 유형 `y`에 대해 기록 모드를 사용할지 묻는 질문:

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/routertypey.png?resize=759%2C376&ssl=1)

Vue 라우터 웹 사이트에 따르면, Vue 라우터의 기본 모드는 해시 모드입니다. 이 모드는 URL 해시를 사용하여 전체 URL을 시뮬레이션하여 URL이 변경되었을 때 페이지가 다시 로드되지 않도록 합니다. 해시를 제거하기 위해 기록을 활용하는 라우터의 기록 모드를 사용할 수 있습니다.

구성을 배치할 위치를 묻는 메시지가 나타나면 `전용 구성 파일`을 선택하고 이후 프로젝트 유형인 `y` 또는 `n`에 대한 구성을 저장할지 묻는 메시지가 표시됩니다.

Vue CLI가 프로젝트 생성을 완료한 후 누락된 종속성을 설치합니다.

먼저 다음 명령을 실행하여 `Vue CLI`를 사용하여 `Vuetify`를 설치합니다.

```undefined
vue add vuetify
```

사전 설정을 선택하라는 메시지가 나타나면 `기본값`을 선택합니다.

이 코드 블록에 `Vuetify`를 설치했습니다. 이 모듈을 사용하여 CSS에 대해 너무 걱정하지 않아도 인벤토리 관리 시스템 UI를 생성할 수 있습니다.

마지막으로 npm을 사용하여 "node-fetch"를 설치합니다.

```undefined
npm install node-fetch --save
```

이 코드 블록에서 `노드-페치`를 설치했습니다.

- `node-fetch`는 API 끝점 또는 임의의 URL에 HTTP 요청을 할 수 있는 모듈입니다. 이 모듈을 사용하여 서버에서 GraphQL API를 사용합니다.

이 섹션에서는 프로젝트 디렉토리를 생성하고 애플리케이션 클라이언트의 Node.js 프로젝트를 초기화했습니다. 다음 섹션에서는 이 응용 프로그램의 사용자 인터페이스 구축을 시작합니다.

## 응용 프로그램 레이아웃 및 경로 생성

이 단계에서는 `Vuetify`를 사용하여 레이아웃을 만들고 애플리케이션 클라이언트의 경로를 처리할 `vue-router`를 사용합니다.

`rc` 디렉토리로 이동합니다.

```bash
cd src
```

폴더 구조는 다음과 같아야 합니다.

`앱`을 엽니다.vue` 파일:

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/folderstructure.png?resize=201%2C385&ssl=1)

```css
nano App.vue
```

`<style>` 구성 요소를 제거하고 `<template> 구성 요소의 내용을 다음과 같이 교체합니다.

```xml
<template>
  <v-app id="app">
    <v-navigation-drawer v-model="drawer" app clipped>
      <v-list dense v-for="route in routes" :key="route.title">
        <v-list-item link @click="navigateTo(route.path)">
          <v-list-item-action>
            <v-icon>{route.icon}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{route.title}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar app clipped-left>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Application</v-toolbar-title>
    </v-app-bar>
    <v-main>
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <v-col>
            <router-view :key="$route.fullPath"></router-view>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
    <v-footer app>
      <span>&copy; { new Date().getFullYear() }</span>
    </v-footer>
  </v-app>
</template>
```

위의 코드 블록에서 < 구성 요소 내부에 다음과 같은 구성 요소인 <, < app-bar 및 v-footer를 추가했습니다.

- `v-navigation-drawer`는 응용 프로그램의 경로를 탐색할 수 있도록 `v-app-bar` 구성 요소와 함께 사용됩니다.
- `v-main` 구성 요소는 애플리케이션의 보기가 표시될 위치입니다.
- `v-footer` 구성 요소는 애플리케이션의 바닥글이 표시될 위치입니다.

이제 `<script> 구성 요소의 내용을 다음 내용으로 교체합니다.

```xml
<script>
export default {
  name: "App",
  data: () => ({
    drawer: null,
    routes: [
      { path: "home", title: "Dashboard", icon: "mdi-view-dashboard" },
      { path: "users", title: "Users", icon: "mdi-cog" },
      { path: "categories", title: "Categories", icon: "mdi-cog" },
      { path: "products", title: "Products", icon: "mdi-cog" },
      { path: "inventory", title: "Inventory", icon: "mdi-cog" },
      { path: "orders", title: "Orders", icon: "mdi-cog" },
    ],
  }),
  created() {
    this.$vuetify.theme.light = true;
  },
}
</script>
```

위의 코드 블록에서 `App.vue`에 대한 `data` 필드와 `created()` 필드를 생성했습니다.

- 데이터 필드는 두 개의 하위 필드인 드로어와 경로를 포함합니다. 응용 프로그램 탐색 드로어를 제어할 때는 ➡을 사용하고 ➡에 있는 항목을 생성할 때 사용할 데이터를 포함합니다.
- 생성() 필드는 기본적으로 어둡게 설정되어 있으므로 응용 프로그램 테마를 빛으로 변경하는 데 사용됩니다.

아래의 코드를 `created()` 필드에 추가하십시오.

```xml
<script>
export default {
 . . .

  methods: {
    navigateTo(route) {
      if (this.$route.name !== route) {
        this.$router.push({ name: route }).catch((error) => {
          console.log(error)
        });
      }
    },
  },
}
</script>
```

위의 코드 블록에서 `methods` 필드를 추가했으며 이 메서드 내에 `navigateTo()라는 메서드를 추가했습니다. navigateTo() 메서드는 경로라는 문자열을 인수로 수신하고, 조건부 논리를 사용하여 현재 경로가 경로와 같지 않은지, 그리고 그것이 경로로 이동하는 경우 수신한 경로로 이동합니다. 이 메서드는 `<v-navigation-drawer>의 항목을 클릭할 때마다 호출됩니다.

당신의 앱.vue는 다음과 같은 모습을 보여야 한다.

```xml
<template>
  <v-app id="app">
    <v-navigation-drawer v-model="drawer" app clipped>
      <v-list dense v-for="route in routes" :key="route.title">
        <v-list-item link @click="navigateTo(route.path)">
          <v-list-item-action>
            <v-icon>{route.icon}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{route.title}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar app clipped-left>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Application</v-toolbar-title>
    </v-app-bar>
    <v-main>
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <v-col>
            <router-view :key="$route.fullPath"></router-view>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
    <v-footer app>
      <span>&copy; { new Date().getFullYear() }</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    drawer: null,
    routes: [
      { path: "home", title: "Dashboard", icon: "mdi-view-dashboard" },
      { path: "users", title: "Users", icon: "mdi-cog" },
      { path: "categories", title: "Categories", icon: "mdi-cog" },
      { path: "products", title: "Products", icon: "mdi-cog" },
      { path: "inventory", title: "Inventory", icon: "mdi-cog" },
      { path: "orders", title: "Orders", icon: "mdi-cog" },
    ],
  }),
  created() {
    this.$vuetify.theme.light = true;
  },
  methods: {
    navigateTo(route) {
      if (this.$route.name !== route) {
        this.$router.push({ name: route }).catch((error) => {
          console.log(error)
        });
      }
    },
  },
}
</script>
```

`라우터` 디렉토리로 이동합니다.

```bash
cd router
```

`index.js` 파일을 엽니다.

```css
nano index.js
```

`홈` 보기 구성 요소를 가져올 라인을 제거합니다.

```coffeescript
import Home from '../views/Home.vue'
```

`routs` 배열의 내용을 다음과 같이 바꿉니다.

```js
. . .

const routes = const routes = [
  {
    path: '/',
    name: 'home',
    component: () => import(/* webpackChunkName: "home" */ '../views/Home.vue')
  },
  {
    path: '/users',
    name: 'users',
    component: () => import(/* webpackChunkName: "table" */ '../views/Table.vue')
  },
  {
    path: '/categories',
    name: 'categories',
    component: () => import(/* webpackChunkName: "table" */ '../views/Table.vue')
  },
  {
    path: '/inventory',
    name: 'inventory',
    component: () => import(/* webpackChunkName: "table" */ '../views/Table.vue')
  },
  {
    path: '/products',
    name: 'products',
    component: () => import(/* webpackChunkName: "table" */ '../views/Table.vue')
  },
  {
    path: '/orders',
    name: 'orders',
    component: () => import(/* webpackChunkName: "table" */ '../views/Table.vue')
  }
]
```

위의 코드 블록에서 경로 배열, 홈, 사용자, 범주, 제품, 인벤토리, 주문 등에 다음 경로를 추가했습니다.

- 홈 경로에 카드 그리드가 표시되고 각 카드에 수집 이름과 문서 수가 표시되고 수집 데이터가 포함된 경로로 이동할 수 있습니다.
- 사용자, 제품, 재고, 주문 루트는 사용자, 제품, 재고, 주문 컬렉션의 문서를 각각 관리할 수 있게 한다.

모든 경로에서 보기 구성요소를 느리게 로드합니다. 모든 경로 내용은 홈 보기 구성요소에 표시되는 홈 경로 내용을 제외하고 테이블 보기 구성요소에 표시됩니다.

`index.js` 파일은 다음과 같아야 합니다.

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'home',
    component: () => import(/* webpackChunkName: "home" */ '../views/Home.vue')
  },
  {
    path: '/users',
    name: 'users',
    component: () => import(/* webpackChunkName: "table" */ '../views/Table.vue')
  },
  {
    path: '/categories',
    name: 'categories',
    component: () => import(/* webpackChunkName: "table" */ '../views/Table.vue')
  },
  {
    path: '/inventory',
    name: 'inventory',
    component: () => import(/* webpackChunkName: "table" */ '../views/Table.vue')
  },
  {
    path: '/products',
    name: 'products',
    component: () => import(/* webpackChunkName: "table" */ '../views/Table.vue')
  },
  {
    path: '/orders',
    name: 'orders',
    component: () => import(/* webpackChunkName: "table" */ '../views/Table.vue')
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router
```

이 섹션에서는 응용 프로그램 클라이언트의 레이아웃과 경로를 만들었습니다. 다음 섹션에서는 응용 프로그램 클라이언트에 대한 보기 만들기를 시작합니다.

## 홈 보기 만들기

이 섹션에서는 응용 프로그램 클라이언트에 대한 보기 만들기를 시작합니다. 신청서에는 홈과 테이블이라는 두 가지 뷰가 있어 홈에 상주하게 된다.vue와 테이블.vue 파일 이 섹션에서는 `홈` 보기를 만듭니다.

`views` 디렉토리로 이동합니다.

```bash
cd ../views
```

`Home`이라는 파일을 엽니다.vue:

```css
nano Home.vue
```

`<template>` 구성 요소 내부의 콘텐츠를 다음과 같이 교체합니다.

```xml
<template>
  <v-container fluid>
    <v-row dense>
      <v-col v-for="card in cards" :key="card.title" :cols="card.flex">
        <v-hover v-slot:default="{ hover }">
          <v-card :elevation="hover ? 16 : 2" :style="hover? 'cursor: pointer': '' ">
            <v-card-text @click="navigateTo(card.title.toLowerCase())">
              <h2 style="color:black" class="text-center">{card.title}</h2>
              <h2
                style="color:black; margin-top:20px; height: 50px"
                class="text-center"
              >{card.quantity}</h2>
            </v-card-text>
          </v-card>
        </v-hover>
      </v-col>
    </v-row>
  </v-container>
</template>
```

위의 코드 블록에서 `템플릿` 구성 요소 내부에 `v-container` 구성 요소를 추가했습니다. 이 구성 요소를 사용하여 5개의 카드로 그리드를 만듭니다. 각 카드에는 컬렉션 이름과 이 컬렉션에 있는 문서 수가 표시됩니다. 또한 클릭할 때마다 특정 경로를 탐색하기 위해 각 카드에 클릭 수신기를 추가했습니다.

`<script> 구성 요소 내의 내용을 다음과 같이 바꿉니다.

```xml
<script>
import actions from "../components/actions"

export default {
  name: "Home",
  data: () => ({
    cards: [],
  }),
  created() {
    this.initialize();
  },
}
</script>
```

위의 코드 블록에서 구성 요소 디렉토리의 `actions.js` 파일에서 `actions`라는 개체를 가져오고 있습니다. 그런 다음 `data`와 `created() 필드를 추가했습니다. 데이터 필드에 빈 배열을 포함하는 카드라는 하위 필드를 추가했습니다. 생성() 필드에서는 "initialize()"라는 메서드를 호출했습니다. 잠시 후 이 메서드를 생성합니다.

`수행`을 만들지 않았습니다.그러나 이 파일은 GraphQL API 사용을 담당하며 MongoDB 인스턴스의 데이터를 쿼리하고 변조할 수 있습니다.

`created()` 필드 아래에 다음 코드를 추가합니다.

```xml
<script>
. . .
export default {
  . . .

  methods: {
    async initialize() {
      let categories = await actions.getCategories();
      let users = await actions.getUsers();
      let products = await actions.getProducts();
      let inventoryItems = await actions.getInventoryItems();
      let orders = await actions.getOrders();
      this.cards = [
        { title: "Products", quantity: products.length, flex: 4 },
        { title: "Categories", quantity: categories.length, flex: 4 },
        { title: "Inventory", quantity: inventoryItems.length, flex: 4 },
        { title: "Users", quantity: users.length, flex: 4 },
        { title: "orders", quantity: orders.length, flex: 4 },
      ]
    },
  },
}
</script>
```

위의 코드 블록에서 `methods` 필드를 추가했으며 이 필드 내에서 `initialize()라는 메서드를 생성했습니다. 초기화() 내에서 `수행` 객체가 제공하는 방법을 사용하여 모든 컬렉션에서 데이터를 검색한 후 이 데이터를 가져와 `카드` 배열에 추가했습니다.

초기화() 방법 아래에 다음 코드를 추가합니다.

```xml
<script>
. . .

export default {
  . . .

  methods: {
  . . .

    navigateTo(route) {
      if (this.$route.name !== route) {
        this.$router.push({ name: route }).catch((error) => {
          console.log(error);
        })
      }
    },
  },
}
</script>
```

위의 코드 블록에서 methods 필드 내에 navigateTo()라는 메서드를 추가했습니다. navigateTo() 메서드는 경로라는 문자열을 인수로 수신하고, 조건부 논리를 사용하여 현재 경로가 경로와 같지 않은지, 그리고 그것이 경로로 이동하는 경우 수신한 경로로 이동합니다. 이 방법은 이 보기에서 카드를 누를 때마다 호출됩니다.

당신의 집.vue` 파일은 다음과 같은 모양이어야 합니다.

```xml
<template>
  <v-container fluid>
    <v-row dense>
      <v-col v-for="card in cards" :key="card.title" :cols="card.flex">
        <v-hover v-slot:default="{ hover }">
          <v-card :elevation="hover ? 16 : 2" :style="hover? 'cursor: pointer': '' ">
            <v-card-text @click="navigateTo(card.title.toLowerCase())">
              <h2 style="color:black" class="text-center">{card.title}</h2>
              <h2
                style="color:black; margin-top:20px; height: 50px"
                class="text-center"
              >{card.quantity}</h2>
            </v-card-text>
          </v-card>
        </v-hover>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import actions from "../components/actions"

export default {
  name: "Home",
  data: () => ({
    cards: [],
  }),
  created() {
    this.initialize();
  },
  methods: {
    async initialize() {
      let categories = await actions.getCategories();
      let users = await actions.getUsers();
      let products = await actions.getProducts();
      let inventoryItems = await actions.getInventoryItems();
      let orders = await actions.getOrders();
      this.cards = [
        { title: "Products", quantity: products.length, flex: 4 },
        { title: "Categories", quantity: categories.length, flex: 4 },
        { title: "Inventory", quantity: inventoryItems.length, flex: 4 },
        { title: "Users", quantity: users.length, flex: 4 },
        { title: "orders", quantity: orders.length, flex: 4 },
      ]
    },
    navigateTo(route) {
      if (this.$route.name !== route) {
        this.$router.push({ name: route }).catch((error) => {
          console.log(error);
        })
      }
    },
  },
}
</script>
```

이 섹션에서는 응용 프로그램 클라이언트에 대한 `홈` 보기를 만들었습니다. 다음 섹션에서 `표` 보기를 작성합니다.

## 테이블 보기 만들기

이 섹션에서는 사용자, 제품, 범주, 인벤토리 및 주문 경로에 대한 데이터를 표시하는 테이블 뷰를 만듭니다. Vuetify에서 제공하는 v-data-table 구성 요소를 사용하여 데이터를 표시하려고 합니다.

여전히 `views` 디렉토리에 `Table`이라는 파일을 작성합니다.vue:

```css
nano Table.vue
```

다음 코드를 `표` 안에 추가합니다.vue` 파일:

```xml
<template>
  <v-data-table :headers="headers" :items="rows" sort-by="name" class="elevation-2">
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>{$route.name.toUpperCase()}</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">New Item</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{ formTitle }</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row v-for="(value, key) in editedItem " v-bind:key="key">
                  <v-col
                    cols="12" sm="6" md="4"
                    v-if=" key !== 'id' && key !=='category' && key !='dateAdded' && key !=='product' && key !=='user' ">
                    <v-text-field
                      :type =" key === 'quantity' || key === 'price' ? 'Number' : 'text' "
                      v-model="editedItem[key]"
                      :label="key"
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-row v-if="editedIndex === -1 && $route.name === 'inventory' ">
                  <v-col cols="12" sm="6" md="4">
                    <v-select
                      v-model="editedItem.product"
                      :items="options.products"
                      label="products"
                      item-text="name"
                      item-value="id"
                    ></v-select>
                  </v-col>
                </v-row>
                <v-row v-else-if="$route.name === 'products'">
                  <v-col cols="12" sm="6" md="4">
                    <v-select
                      v-model="editedItem.category"
                      :items="options.categories"
                      label="categories"
                      item-text="name"
                      item-value="id"
                    ></v-select>
                  </v-col>
                </v-row>
                <v-row v-else-if="editedIndex === -1 && $route.name === 'orders'">
                  <v-col cols="12" sm="6" md="4">
                    <v-select
                      v-model="editedItem.product"
                      :items="options.inventoryItems"
                      label="products"
                      item-text="product.name"
                      item-value="product.id"
                    ></v-select>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-select
                      v-model="editedItem.user"
                      :items="options.users"
                      label="users"
                      item-text="username"
                      item-value="id"
                    ></v-select>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="save">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)">mdi-pencil</v-icon>
      <v-icon small @click="deleteItem(item)">mdi-delete</v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>
```

위의 코드 블록에서 `템플릿` 구성 요소를 생성했으며, 그 안에 `v-data-table` 구성 요소를 추가했습니다. `v-data-table` 구성 요소 내에 컬렉션의 데이터를 관리하는 데 필요한 구성 요소를 추가했습니다. 위 코드의 대부분은 Vuetify에서 제공되었으며, 당신은 오직 하나만이 아닌 여러 컬렉션의 데이터를 처리할 수 있도록 만들 필요가 있었다.

`<template>` 아래에 다음 코드를 추가합니다.

```xml
<script>
import actions from "../components/actions";

export default {
  name: "Table",
  data: () => ({
    dialog: false,
    headers: [],
    options: {},
    rows: [],
    editedIndex: -1,
    editedItem: {},
    defaultItem: {},
  }),
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
  },
}
</script>
```

위의 코드 블록에서 먼저 `<script> 구성 요소를 생성한 후 그 안에 `actions.js`에서 `actions`라는 개체를 가져왔습니다. 또한 이 구성 요소 내에서 MongoDB 인스턴스의 컬렉션에서 데이터를 포함하는 `<v-data-table>`을 생성하는 데 필요한 정보를 포함하는 개체를 생성했습니다. 이 개체 내에 이름, 데이터, 계산됨 필드를 추가했습니다.

- `name` 필드는 구성 요소의 이름을 설정하는 곳입니다.
- `data` 필드는 `v-data-table`을 생성하고 관리하는 데 필요한 정보를 보관하는 필드입니다. data 필드 내부에 dialog(대화), 헤더(헤더), 옵션(옵션), 행(rows), 편집(edited) 하위 필드를 추가했습니다.인덱스", `편집됨항목", `기본값`항목:
`dialog` 필드는 `<v-dialog>` 구성 요소를 표시하는 데 사용할 필드입니다.
헤더 필드는 <v-data-table>의 헤더를 설정하는 필드입니다.
`옵션` 필드는 `<v-select> 구성 요소에 필요한 키-값 쌍 개체의 배열을 저장하는 필드입니다.
행 필드는 MongoDB 인스턴스의 컬렉션에서 검색된 문서를 저장하는 필드입니다.
편집된인덱스 필드는 "새 항목 추가" 작업인지 "항목 편집" 작업인지 여부를 제어하는 필드입니다.
편집된Item` 필드는 항목을 편집할 때 사용할 모델 개체를 저장하는 필드입니다.
기본값Item` 필드는 새 항목을 추가할 때 사용할 모델 개체를 저장하는 필드입니다.
계산 필드는 `템플릿` 구성 요소에 필요한 복잡한 논리를 저장하는 필드이며, 이 필드에는 `v-dialog` 구성 요소의 제목에 값을 설정하는 데 사용할 `폼 제목`이라는 메서드가 추가되었습니다.
- `dialog` 필드는 `<v-dialog>` 구성 요소를 표시하는 데 사용할 필드입니다.
- 헤더 필드는 <v-data-table>의 헤더를 설정하는 필드입니다.
- `옵션` 필드는 `<v-select> 구성 요소에 필요한 키-값 쌍 개체의 배열을 저장하는 필드입니다.
- 행 필드는 MongoDB 인스턴스의 컬렉션에서 검색된 문서를 저장하는 필드입니다.
- 편집된인덱스 필드는 "새 항목 추가" 작업인지 "항목 편집" 작업인지 여부를 제어하는 필드입니다.
- 편집된Item` 필드는 항목을 편집할 때 사용할 모델 개체를 저장하는 필드입니다.
- 기본값Item` 필드는 새 항목을 추가할 때 사용할 모델 개체를 저장하는 필드입니다.
- 계산 필드는 `템플릿` 구성 요소에 필요한 복잡한 논리를 저장하는 필드이며, 이 필드에는 `v-dialog` 구성 요소의 제목에 값을 설정하는 데 사용할 `폼 제목`이라는 메서드가 추가되었습니다.

`computed` 필드 아래에 다음 코드를 추가합니다.

```xml
<script>
. . .

export default {
  . . .

  created() {
    this.initialize()
  },
  methods: {
    async initialize() {
      switch (this.$route.name) {
        case "users":
          break
        case "categories":
          break
        case "products":
          break
        case "inventory":
          break
        case "orders":
          break
      }
    }
  }
}
</script>
```

위의 코드 블록에서 계산 필드 아래에 생성() 및 메서드 필드를 추가했습니다.

- 생성() 필드는 이 보기가 생성될 때 실행되는 메서드로, 이 메서드 내에서 "initialize()"라는 메서드를 호출했습니다.
- 메서드 필드는 현재 보기에 필요한 모든 메서드를 저장하는 속성입니다. 이 속성 내에 `initialize()`라는 메서드를 추가했습니다.

초기화() 방법은 GraphQL 서버에서 정보를 검색하여 `데이터` 속성의 필드에 저장하는 방법입니다. 이 방법은 사용자, 범주, 제품, 재고, 주문 등 다음과 같은 경우의 스위치 문구로 구성된다. 즉, 경로를 방문하면 사례 중 하나가 트리거되고 이 경로에 대한 <v-data-table>을 생성하는 데 필요한 정보가 검색됩니다.

`case "users"의 내용을 다음 코드로 바꿉니다.

```undefined
. . .

async initialize() {
  switch (this.$route.name) {
    case "users":
      this.headers = [
        { text: "ID", value: "id" },
        { text: "Name", value: "name" },
        { text: "username", value: "username" },
        { text: "Email", value: "email" },
        { text: "Phone number", value: "phoneNumber" },
        { text: "Actions", value: "actions", sortable: false },
      ]

      this.editedItem = { name: "", username: "", email: "", phoneNumber: ""}
      this.defaultItem = { name: "", username: "", email: "", phoneNumber: ""}

      const users = await actions.getUsers()
      this.rows = users
      break

    . . .
}
```

위에 추가한 코드는 `사용자` 경로를 방문할 때 트리거됩니다. 이 코드 블록은 편집된 헤더에 필요한 데이터를 추가하는 역할을 합니다.항목", `기본값`데이터 속성의 항목 및 행 필드입니다. 행 객체에 할당된 데이터는 동작 객체가 제공하는 getUsers() 메서드를 호출하여 검색되었습니다.

위의 코드 블록에서 했던 것과 동일한 작업을 나머지 `사례`에도 수행할 것입니다.

나머지 사례의 내용을 교체한 후 `초기화()`는 다음과 같이 표시됩니다.

```undefined
. . .  

async initialize() {
  switch (this.$route.name) {
    case "users":
      this.headers = [
        { text: "ID", value: "id" },
        { text: "Name", value: "name" },
        { text: "username", value: "username" },
        { text: "Email", value: "email" },
        { text: "Phone number", value: "phoneNumber" },
        { text: "Actions", value: "actions", sortable: false },
      ];
      this.editedItem = { name: "", username: "", email: "", phoneNumber: ""}
      this.defaultItem = { name: "", username: "", email: "", phoneNumber: ""}
      const users = await actions.getUsers()
      this.rows = users
      break
    case "categories":
      this.headers = [
        { text: "ID", value: "id" },
        { text: "Name", value: "name" },
        { text: "Actions", value: "actions", sortable: false },
      ]
      this.editedItem = { name: "" }
      this.defaultItem = { name: "" }
      const categories = await actions.getCategories()
      this.rows = categories
      break
    case "products":
      this.headers = [
        { text: "ID", value: "id" },
        { text: "Name", value: "name" },
        { text: "Category", value: "category" },
        { text: "Price", value: "price" },
        { text: "Date added", value: "dateAdded" },
        { text: "Actions", value: "actions", sortable: false },
      ]
      this.editedItem = { name: "", price: 0, category: ""}
      this.defaultItem = { name: "", price: 0, category: ""}
      this.options.categories = await actions.getCategories()
      const products = await actions.getProducts()
      for (let i = 0; i < products.length; i++) {
        products[i].category = products[i].category.name;
        products[i].dateAdded = new Date(products[i].dateAdded)
      }
      this.rows = products;
      break
    case "inventory":
      this.headers = [
        { text: "ID", value: "id" },
        { text: "Product", value: "product" },
        { text: "Quantity", value: "quantity" },
        { text: "Actions", value: "actions", sortable: false },
      ]
      this.editedItem = { quantity: 0 }
      this.defaultItem = { product: "", quantity: 0 }
      this.options.products = await actions.getProducts()
      const inventoryItems = await actions.getInventoryItems()
      for (let i = 0; i < inventoryItems.length; i++) {
        inventoryItems[i].product = inventoryItems[i].product.name
      }
      this.rows = inventoryItems
      break
    case "orders":
      this.headers = [
        { text: "ID", value: "id" },
        { text: "Product", value: "product" },
        { text: "Quantity", value: "quantity" },
        { text: "User", value: "user" },
        { text: "Total", value: "total" },
        { text: "Date added", value: "dateAdded" },
        { text: "Actions", value: "actions", sortable: false },
      ]
      this.editedItem = { quantity: 0 }
      this.defaultItem = { product: "", user: "", quantity: 0 }
      this.options.inventoryItems = await actions.getInventoryItems();
      this.options.users = await actions.getUsers()
      const orders = await actions.getOrders()
      for (let i = 0; i < orders.length; i++) {
        orders[i].product = orders[i].product.name
        orders[i].user = orders[i].user.username
        orders[i].dateAdded = new Date(orders[i].dateAdded)
      }
      this.rows = orders
      break;
  }
},
```

초기화() 방법 아래에 다음 코드를 추가합니다.

```js
. . .

editItem(item) {
  this.editedIndex = this.rows.indexOf(item);
  this.editedItem = Object.assign({}, item);
  this.dialog = true;
},

close() {
  this.dialog = false;
  this.$nextTick(() => {
    this.editedItem = Object.assign({}, this.defaultItem);
    this.editedIndex = -1
  });
},
}  

. . .
```

위의 코드 블록에서 methods 속성인 editItem() 및 close()에 다음 메서드를 추가했습니다.

- editItem() 필드는 `행` 배열의 개체 중 하나인 `항목`이라는 개체가 `edited`를 변경하는 인수로 수신하는 메서드입니다.행 배열의 항목 인덱스에 대한 인덱스는 항목을 편집된 항목에 할당합니다.Item` 개체를 선택한 다음 `dialog` 값을 `true`로 설정하여 `v-dialog` 구성 요소를 엽니다.
- 닫기() 필드는 `dialog` 값을 `false`로 설정하여 `v-dialog` 구성 요소를 닫고 `기본값`을 할당하는 방법입니다.`편집된 항목`에 대한 항목항목` 후 `편집됨` 변경인덱스 값에서 `-1`로

아래의 코드를 `닫기()` 방법에 추가하십시오.

```undefined
. . .

async deleteItem(item) {
  const index = this.rows.indexOf(item)
  const message = "Are you sure you want to delete this item?"

  switch (this.$route.name) {
    case "users":
      confirm(message) && (await actions.deleteUser(item.id)) &&
      this.rows.splice(index,1)
      break
    case "categories":
      confirm(message) && (await actions.deleteCategory(item.id)) &&
      this.rows.splice(index, 1)
      break
    case "products":
      confirm(message) && (await actions.deleteProduct(item.id)) &&
      this.rows.splice(index, 1)
      break
    case "inventory":
      confirm(message) && (await actions.deleteInventoryItem(item.id)) &&
      this.rows.splice(index, 1)
      break
    case "orders":
      confirm(message) && (await actions.deleteOrder(item.id)) &&
      this.rows.splice(index, 1)
      break
  }
}

. . .
```

위의 코드 블록에서 `methods` 속성에 `deleteItem()이라는 메서드를 추가했습니다. deleteItem()은 행 배열의 객체 중 하나인 항목이라는 객체를 인수로 수신하여 행 배열에서 항목의 인덱스를 찾은 다음 index라는 변수에 저장합니다. 인덱스를 찾은 후 메시지라는 변수를 만들고 그 안에 이 항목을 삭제할지 묻는 문자열을 저장했습니다. 마지막으로 사용자, 범주, 제품, 인벤토리 또는 주문 경로에서 항목을 삭제할 때마다 실행되는 스위치 문을 만듭니다.

deleteItem() 메서드 아래에 다음 코드를 추가합니다.

```undefined
. . .  

async save() {
      if (this.editedIndex > -1) {
        switch (this.$route.name) {
          case "users":
            break
          case "categories":
            break
          case "products":
            break
          case "inventory":
            break
          case "orders":
            break
        }
      } else {
        switch (this.$route.name) {
          case "users":
            break
          case "categories":
            break
          case "products":
            break
          case "inventory":
            break
          case "orders":
            break
        }
      }
      this.close()
    }
```

위의 코드 블록에서 `methods` 속성에 `save()라는 메서드를 추가했습니다. 조건부 로직을 사용하여 새 항목과 편집된 항목 저장 작업을 처리했습니다. 편집한 경우색인이 `-1`보다 크므로 편집한 항목을 저장하려고 하지만 그렇지 않은 경우 새 항목을 저장하려고 한다는 의미입니다. 각 조건 내에서 사용자, 범주, 제품, 인벤토리 또는 주문 경로에 있을 때 데이터를 저장할 수 있도록 스위치 문을 만들었습니다. 조건 중 하나를 실행한 후 `<v-dialog> 구성 요소를 닫기 위해 `close() 메서드를 호출했습니다.

각 `case "users"에 다음 코드를 추가합니다.

```coffeescript
async save() {  
  if (this.editedIndex > -1) {
    switch (this.$route.name) {
      case "users":
          await actions.updateUser(this.editedItem).then((user) => {
          Object.assign(this.rows[this.editedIndex], user);
        })
        break

      . . .
    }
  } else {
    switch (this.$route.name) {
      case "users":
          await actions.addUser(this.editedItem).then((user) => {
          this.rows.push(user)
        })
        break

      . . .
    }
  }
  this.close()
}
```

위의 코드 블록에서 두 `case "users"에 대한 논리를 추가했습니다. 첫 번째 `사용자`의 경우: `수행` 객체가 제공하는 `업데이트 사용자() 방법을 사용하여 `사용자` 컬렉션에 있는 기존 문서의 값을 업데이트한 다음 응답으로 전송된 객체를 가져와서 `행` 배열의 객체에 할당합니다.인덱스. 두 번째 `사용자`: `수행` 개체에서 제공하는 `사용자 추가()` 방법을 사용하여 `사용자` 컬렉션에 새 문서를 추가한 다음 응답에 있는 개체를 가져와 `행` 배열에 추가합니다.

위의 코드 블록에서 했던 것과 동일한 작업을 나머지 `사례`에도 수행할 것입니다.

나머지 사례의 내용을 교체한 후 `저장()` 방법은 다음과 같습니다.

```js
async save() {
  if (this.editedIndex > -1) {
    switch (this.$route.name) {
      case "users":
        await actions.updateUser(this.editedItem).then((user) => {
          Object.assign(this.rows[this.editedIndex], user);
        })
        break
      case "categories":
        await actions.updateCategory(this.editedItem).then((category) => {
          Object.assign(this.rows[this.editedIndex], category);
        })
        break
      case "products":
        this.editedItem.price = parseFloat(this.editedItem.price);
        for (let i = 0; i < this.options.categories.length; i++) {
          if (this.options.categories[i].name === this.editedItem.category) {
            this.editedItem.category = this.options.categories[i].id
          }
        }
        await actions.updateProduct(this.editedItem).then((product) => {
          product.category = product.category.name;
          Object.assign(this.rows[this.editedIndex], product)
        })
        break
      case "inventory":
        this.editedItem.quantity = parseInt(this.editedItem.quantity)
        await actions
          .updateInventoryItem(this.editedItem)
          .then((inventoryItem) => {
            inventoryItem.product = inventoryItem.product.name
            Object.assign(this.rows[this.editedIndex], inventoryItem)
          })
        break
      case "orders":
        this.editedItem.quantity = parseInt(this.editedItem.quantity);
        console.log("edited", this.editedItem);
        await actions.updateOrder(this.editedItem).then((order) => {
          order.product = order.product.name
          order.user = order.user.username
          Object.assign(this.rows[this.editedIndex], order)
        })
        break
    }
  } else {
    switch (this.$route.name) {
      case "users":
        await actions.addUser(this.editedItem).then((user) => {
          this.rows.push(user)
        })
        break
      case "categories":
        await actions.addCategory(this.editedItem).then((category) => {
          console.log("category", category)
          this.rows.push(category)
        })
        break
      case "products":
        this.editedItem.price = parseFloat(this.editedItem.price);
        await actions.addProduct(this.editedItem).then((product) => {
          product.category = product.category.name
          product.dateAdded = new Date(product.dateAdded)
          this.rows.push(product)
        })
        break
      case "inventory":
        this.editedItem.quantity = parseInt(this.editedItem.quantity)
        await actions
          .addInventoryItem(this.editedItem)
          .then((inventoryItem) => {
            inventoryItem.product = inventoryItem.product.name
            this.rows.push(inventoryItem)
          })
        break
      case "orders":
        this.editedItem.quantity = parseInt(this.editedItem.quantity);
        console.log("model", this.editedItem);
        let order = await actions
          .addOrder(this.editedItem)
          .then((order) => {
            order.product = order.product.name
            order.user = order.user.username
            order.dateAdded = new Date(order.dateAdded)
            this.rows.push(order)
          })
        break
    }
  }
  this.close()
}
```

네 `테이블`.vue` 파일은 다음과 같아야 합니다.

```xml
<template>  
  <v-data-table :headers="headers" :items="rows" sort-by="name" class="elevation-2">
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>{$route.name.toUpperCase()}</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">New Item</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{ formTitle }</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row v-for="(value, key) in editedItem " v-bind:key="key">
                  <v-col
                    cols="12" sm="6" md="4"
                    v-if=" key !== 'id' && key !=='category' && key !='dateAdded' && key !=='product' && key !=='user' ">
                    <v-text-field
                      :type =" key === 'quantity' || key === 'price' ? 'Number' : 'text' "
                      v-model="editedItem[key]"
                      :label="key"
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-row v-if="editedIndex === -1 && $route.name === 'inventory' ">
                  <v-col cols="12" sm="6" md="4">
                    <v-select
                      v-model="editedItem.product"
                      :items="options.products"
                      label="products"
                      item-text="name"
                      item-value="id"
                    ></v-select>
                  </v-col>
                </v-row>

                <v-row v-else-if="$route.name === 'products'">
                  <v-col cols="12" sm="6" md="4">
                    <v-select
                      v-model="editedItem.category"
                      :items="options.categories"
                      label="categories"
                      item-text="name"
                      item-value="id"
                    ></v-select>
                  </v-col>
                </v-row>

                <v-row v-else-if="editedIndex === -1 && $route.name === 'orders'">
                  <v-col cols="12" sm="6" md="4">
                    <v-select
                      v-model="editedItem.product"
                      :items="options.inventoryItems"
                      label="products"
                      item-text="product.name"
                      item-value="product.id"
                    ></v-select>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-select
                      v-model="editedItem.user"
                      :items="options.users"
                      label="users"
                      item-text="username"
                      item-value="id"
                    ></v-select>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="save">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)">mdi-pencil</v-icon>
      <v-icon small @click="deleteItem(item)">mdi-delete</v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>


<script>
import actions from "../components/actions";

export default {
  name: "Table",
  data: () => ({
    dialog: false,
    headers: [],
    options: [],
    rows: [],
    editedIndex: -1,
    editedItem: {},
    defaultItem: {},
  }),
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
  },
  created() {
    this.initialize()
  },
  methods: {
    async initialize() {
      switch (this.$route.name) {
        case "users":
          this.headers = [
            { text: "ID", value: "id" },
            { text: "Name", value: "name" },
            { text: "username", value: "username" },
            { text: "Email", value: "email" },
            { text: "Phone number", value: "phoneNumber" },
            { text: "Actions", value: "actions", sortable: false },
          ];

          this.editedItem = { name: "", username: "", email: "", phoneNumber: ""}
          this.defaultItem = { name: "", username: "", email: "", phoneNumber: ""}

          const users = await actions.getUsers()
          this.rows = users
          break

        case "categories":
          this.headers = [
            { text: "ID", value: "id" },
            { text: "Name", value: "name" },
            { text: "Actions", value: "actions", sortable: false },
          ]

          this.editedItem = { name: "" }
          this.defaultItem = { name: "" }

          const categories = await actions.getCategories()
          this.rows = categories

          break
        case "products":
          this.headers = [
            { text: "ID", value: "id" },
            { text: "Name", value: "name" },
            { text: "Category", value: "category" },
            { text: "Price", value: "price" },
            { text: "Date added", value: "dateAdded" },
            { text: "Actions", value: "actions", sortable: false },
          ]

          this.editedItem = { name: "", price: 0, category: ""}
          this.defaultItem = { name: "", price: 0, category: ""}

          this.options.categories = await actions.getCategories()
          const products = await actions.getProducts()
          for (let i = 0; i < products.length; i++) {
            products[i].category = products[i].category.name;
            products[i].dateAdded = new Date(products[i].dateAdded)
          }
          this.rows = products;

          break
        case "inventory":
          this.headers = [
            { text: "ID", value: "id" },
            { text: "Product", value: "product" },
            { text: "Quantity", value: "quantity" },
            { text: "Actions", value: "actions", sortable: false },
          ]

          this.editedItem = { quantity: 0 }
          this.defaultItem = { product: "", quantity: 0 }

          this.options.products = await actions.getProducts()
          const inventoryItems = await actions.getInventoryItems()

          for (let i = 0; i < inventoryItems.length; i++) {
            inventoryItems[i].product = inventoryItems[i].product.name
          }
          this.rows = inventoryItems
          break

        case "orders":
          this.headers = [
            { text: "ID", value: "id" },
            { text: "Product", value: "product" },
            { text: "Quantity", value: "quantity" },
            { text: "User", value: "user" },
            { text: "Total", value: "total" },
            { text: "Date added", value: "dateAdded" },
            { text: "Actions", value: "actions", sortable: false },
          ]

          this.editedItem = { quantity: 0 }
          this.defaultItem = { product: "", user: "", quantity: 0 }

          this.options.inventoryItems = await actions.getInventoryItems();
          this.options.users = await actions.getUsers()
          const orders = await actions.getOrders()

          for (let i = 0; i < orders.length; i++) {
            orders[i].product = orders[i].product.name
            orders[i].user = orders[i].user.username
            orders[i].dateAdded = new Date(orders[i].dateAdded)
          }
          this.rows = orders

          break;
      }
    },

    editItem(item) {
      console.log(item)
      this.editedIndex = this.rows.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1
      });
    },

    async deleteItem(item) {
      const index = this.rows.indexOf(item)
      const message = "Are you sure you want to delete this item?"

      switch (this.$route.name) {
        case "users":
          confirm(message) && (await actions.deleteUser(item.id)) &&
          this.rows.splice(index, 1)
          break
        case "categories":
          confirm(message) && (await actions.deleteCategory(item.id)) &&
          this.rows.splice(index, 1)
          break
        case "products":
          confirm(message) && (await actions.deleteProduct(item.id)) &&
          this.rows.splice(index, 1)
          break
        case "inventory":
          confirm(message) && (await actions.deleteInventoryItem(item.id)) &&
          this.rows.splice(index, 1)
          break
        case "orders":
          confirm(message) && (await actions.deleteOrder(item.id)) &&
          this.rows.splice(index, 1)
          break
      }
    },

    async save() {
      if (this.editedIndex > -1) {
        switch (this.$route.name) {
          case "users":
            await actions.updateUser(this.editedItem).then((user) => {
              Object.assign(this.rows[this.editedIndex], user);
            })
            break

          case "categories":
            await actions.updateCategory(this.editedItem).then((category) => {
              Object.assign(this.rows[this.editedIndex], category);
            })
            break

          case "products":
            this.editedItem.price = parseFloat(this.editedItem.price);
            for (let i = 0; i < this.options.categories.length; i++) {
              if (this.options.categories[i].name === this.editedItem.category) {
                this.editedItem.category = this.options.categories[i].id
              }
            }
            await actions.updateProduct(this.editedItem).then((product) => {
              product.category = product.category.name;
              Object.assign(this.rows[this.editedIndex], product)
            })
            break

          case "inventory":
            this.editedItem.quantity = parseInt(this.editedItem.quantity)
            await actions
              .updateInventoryItem(this.editedItem)
              .then((inventoryItem) => {
                inventoryItem.product = inventoryItem.product.name
                Object.assign(this.rows[this.editedIndex], inventoryItem)
              })
            break

          case "orders":
            this.editedItem.quantity = parseInt(this.editedItem.quantity);
            console.log("edited", this.editedItem);

            await actions.updateOrder(this.editedItem).then((order) => {
              order.product = order.product.name
              order.user = order.user.username
              Object.assign(this.rows[this.editedIndex], order)
            })
            break
        }
      } else {
        switch (this.$route.name) {
          case "users":
            await actions.addUser(this.editedItem).then((user) => {
              this.rows.push(user)
            })
            break

          case "categories":
            await actions.addCategory(this.editedItem).then((category) => {
              console.log("category", category)
              this.rows.push(category)
            })
            break

          case "products":
            this.editedItem.price = parseFloat(this.editedItem.price);
            await actions.addProduct(this.editedItem).then((product) => {
              product.category = product.category.name
              product.dateAdded = new Date(product.dateAdded)
              this.rows.push(product)
            })
            break

          case "inventory":
            this.editedItem.quantity = parseInt(this.editedItem.quantity)
            await actions
              .addInventoryItem(this.editedItem)
              .then((inventoryItem) => {
                inventoryItem.product = inventoryItem.product.name
                this.rows.push(inventoryItem)
              })
            break

          case "orders":
            this.editedItem.quantity = parseInt(this.editedItem.quantity);
            console.log("model", this.editedItem);
            let order = await actions
              .addOrder(this.editedItem)
              .then((order) => {
                order.product = order.product.name
                order.user = order.user.username
                order.dateAdded = new Date(order.dateAdded)
                this.rows.push(order)
              })
            break
        }
      }
      this.close()
    },
  },
}
</script>
```

이 섹션에서는 `표` 보기를 만들었습니다. 다음 섹션에서는 `actions.js`라는 파일을 만들고 그 안에는 이 보기와 `Home` 보기에서 사용한 `actions` 개체와 메서드를 만듭니다.

## actions.js 파일 생성

이 섹션에서는 `actions.js`라는 파일을 만듭니다. 이 파일에는 GraphQL 서버와 상호 작용하는 데 필요한 방법이 포함되어 있습니다. 이러한 방법은 `actions`라는 개체에 상주하게 됩니다.

`구성 요소` 디렉토리로 이동합니다.

```bash
cd ../components
```

`actions.js` 파일 생성:

```css
nano actions.js
```

다음 코드를 `actions.js` 파일에 추가하십시오.

```js
import fetch from "node-fetch"  
const actions = {}

actions.sendOperation = async (operation) => {
  const fetchData = await fetch("http://localhost:4000/graphql", {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
          query: operation,
      }),
  })

  const { data, error } = await fetchData.json();
  return data
}
```

위의 코드 블록에서 `node-fetch` 모듈을 가져와 `fetch`라는 변수를 할당하고 `actions`라는 개체를 생성했으며 이 개체에 `sendOperation()이라는 메서드를 추가했습니다. sendOperation() 메서드는 쿼리 또는 돌연변이 연산 `operation`을 포함하는 문자열을 인수로 수신합니다. 노드-페치 모듈을 사용해 GraphQL 서버로 연산(operation)을 전송하고, 응답 데이터와 오류를 각각 데이터, 오류라는 변수에 저장했다가 데이터를 반환한다. 이 방법은 모든 쿼리 및 돌연변이를 GraphQL 서버로 보내는 데 사용됩니다.

`actions.js` 파일 아래에 다음 코드를 추가합니다.

```js
actions.getUsers = async () => {
  let query = `query{  
  getUsers{
      id
      name
      username
      email
      phoneNumber
  }
  }    
`
  const data = await actions.sendOperation(query)
  return data.getUsers
}
```

위의 코드 블록에서 `actions` 개체에 `getUsers()라는 메서드를 추가했습니다. 이 방법에서는 사용자 컬렉션의 모든 문서를 검색하기 위해 필요한 `쿼리`를 만듭니다. 쿼리를 만든 후 "send Operation() 메서드를 호출하고 이 "query"를 인수로서 전달합니다. 마지막으로 검색된 데이터를 `data`라는 변수에 저장한 다음 이 `data` 개체 내에 있는 `getUsers` 속성의 값을 반환합니다.

다음 코드를 `actions.js` 파일 아래에 추가합니다.

```bash
actions.addUser = async (item) => {
  let mutation = `mutation{
    addUser(input:{
      name:"${item.name}"
      username:"${item.username}"
      phoneNumber:"${item.phoneNumber}"
      email:"${item.email}"
    }){
      id
      name
      username
      email
      phoneNumber
    }
  }`
  const data = await actions.sendOperation(mutation)
  return data.addUser
}
```

위의 코드 블록에서 `actions` 개체에 `addUser()라는 메서드를 추가했습니다. 이 방법에서는 사용자 컬렉션에 새 문서를 추가하는 데 필요한 `음영`을 만듭니다. mutation을 생성한 후 send operation() 메서드를 호출하고 이 mutation을 인수로 전달합니다. 마지막으로 검색된 데이터를 `data`라는 변수에 저장한 다음 이 `data` 개체 내에 있는 `addUser` 속성의 값을 반환합니다.

다음 코드를 `actions.js` 파일 아래에 추가합니다.

```bash
actions.updateUser = async (item) => {  
  let mutation = `mutation {
      updateUser(id: "${item.id}", input: {
        name:"${item.name}",
        username:"${item.username}",
        email:"${item.email}",
        phoneNumber:"${item.phoneNumber}"
      }){
        id
        name
        username
        email
        phoneNumber
      }
    }`
  const data = await actions.sendOperation(mutation)
  return data.updateUser
}
```

위의 코드 블록에서 `actions` 개체에 `updateUser()라는 메서드를 추가했습니다. 이 방법에서는 `사용자` 컬렉션의 기존 문서를 업데이트하는 데 필요한 `음영`을 만듭니다. mutation을 생성한 후 send operation() 메서드를 호출하고 이 mutation을 인수로 전달합니다. 마지막으로 검색된 데이터를 `data`라는 변수에 저장한 다음 이 `data` 개체 내에 있는 `updateUser` 속성의 값을 반환합니다.

다음 코드를 `actions.js` 파일 아래에 추가합니다.

```js
actions.deleteUser = async (id) => {  
  let mutation = `mutation{
      deleteUser(id:"${id}")
    }`
  const data = await actions.sendOperation(mutation)
  return data.deleteUser
}
```

위의 코드 블록에서 `actions` 개체에 `deleteUser()라는 메서드를 추가했습니다. 이 방법에서는 `사용자` 컬렉션에서 문서를 삭제하는 데 필요한 `음영`을 만듭니다. mutation을 생성한 후에는 send operation(연산 전송) 메서드를 호출하고 이 mutation을 인수로서 전달합니다. 마지막으로 검색된 데이터를 `data`라는 변수에 저장한 다음 이 `data` 개체 내에 있는 `delete Use` 속성의 값을 반환합니다.

이전 코드 블록의 사용자 컬렉션에 대해 수행한 것과 동일한 작업을 나머지 컬렉션에도 수행한 다음 액션 개체를 내보냅니다.

위에서 설명한 대로 수행한 후 `actions.js` 파일은 다음과 같아야 합니다.

```js
import fetch from "node-fetch"  
const actions = {}

actions.sendOperation = async (operation) => {
  const fetchData = await fetch("http://localhost:4000/graphql", {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      query: operation,
    }),
  })

  const { data, error } = await fetchData.json();
  return data
}

//////////////////////////      USERS      /////////////////////////////

actions.getUsers = async () => {
  let query = `query{
  getUsers{
      id
      name
      username
      email
      phoneNumber
  }
  }    
`
  const data = await actions.sendOperation(query)
  return data.getUsers

}

actions.addUser = async (item) => {
  let mutation = `mutation{
    addUser(input:{
      name:"${item.name}"
      username:"${item.username}"
      phoneNumber:"${item.phoneNumber}"
      email:"${item.email}"
    }){
      id
      name
      username
      email
      phoneNumber
    }
  }`
  const data = await actions.sendOperation(mutation)
  return data.addUser
}

actions.updateUser = async (item) => {
  let mutation = `mutation {
      updateUser(id: "${item.id}", input: {
        name:"${item.name}",
        username:"${item.username}",
        email:"${item.email}",
        phoneNumber:"${item.phoneNumber}"
      }){
        id
        name
        username
        email
        phoneNumber
      }
    }`
  const data = await actions.sendOperation(mutation)
  return data.updateUser
}

actions.deleteUser = async (id) => {
  let mutation = `mutation{
      deleteUser(id:"${id}")
    }`
  const data = await actions.sendOperation(mutation)
  return data.deleteUser
}

/////////////////////        CATEGORIES       ///////////////////////////   

actions.getCategories = async () => {
  let query = `query{
  getCategories{
      id
      name
  }
  }    
`
  const data = await actions.sendOperation(query)
  return data.getCategories
}

actions.addCategory = async (item) => {
  let mutation = `mutation{
        addCategory(input:{
          name:"${item.name}"
        }){
          id
          name
        }
      }`
  const data = await actions.sendOperation(mutation)
  return data.addCategory
}

actions.updateCategory = async (item) => {
  let mutation = `mutation {
        updateCategory(id: "${item.id}", input: { name: "${item.name}" }) {
          id
          name
        }
      }`
  const data = await actions.sendOperation(mutation)
  return data.updateCategory
}

actions.deleteCategory = async (id) => {
  let mutation = `mutation{
        deleteCategory(id:"${id}")
      }`
  const data = await actions.sendOperation(mutation)
  return data.deleteCategory
}

///////////////////////////     PRODUCTS      ///////////////////////////

actions.getProducts = async () => {
  let query = `query{
        getProducts{
          id
          name
          price
          category{
            name
          }
          dateAdded
        }
      }
`
  const data = await actions.sendOperation(query)
  return data.getProducts
}

actions.addProduct = async (item) => {
  let mutation = `mutation{
        addProduct(input:{
          name:"${item.name}",
          price:${item.price},
          category:"${item.category}"
        }){
          id
          name
          price
          category{
            name
          }
          dateAdded
        }
      }`
  const data = await actions.sendOperation(mutation)
  return data.addProduct
}

actions.updateProduct = async (item) => {
  let mutation = `mutation {
        updateProduct(id:"${item.id}",input:{
          name:"${item.name}",
          price:${item.price},
          category:"${item.category}"
        }){
          id
          name
          price
          category{
            name
          }
          dateAdded
        }
      }`
  const data = await actions.sendOperation(mutation)
  return data.updateProduct
}

actions.deleteProduct = async (id) => {
  let mutation = `mutation{
        deleteProduct(id:"${id}")
      }
      `
  const data = await actions.sendOperation(mutation)
  return data.deleteProduct
}


///////////////////         INVENTORY           //////////////////////

actions.getInventoryItems = async () => {
  let query = `query{
        getInventoryItems{
          id
          quantity
          product{
            id
            name
          }
        }
      }
`
  const data = await actions.sendOperation(query)
  return data.getInventoryItems
}

actions.addInventoryItem = async (item) => {
  let mutation = `mutation {
        addInventoryItem(
          input: { quantity: ${item.quantity}, product: "${item.product}" }
        ) {
          id
          quantity
          product {
            name
          }
        }
      }`
  const data = await actions.sendOperation(mutation)
  return data.addInventoryItem
}

actions.updateInventoryItem = async (item) => {
  let mutation = `mutation{
        updateInventoryItem(id:"${item.id}",input:{quantity:${item.quantity}){
         id
         quantity
         product{
           name
         }
       }
       }`
  const data = await actions.sendOperation(mutation)
  return data.updateInventoryItem
}

actions.deleteInventoryItem = async (id) => {
  let mutation = `mutation {
        deleteInventoryItem(id:"${id}")
      }
      `
  const data = await actions.sendOperation(mutation)
  return data.deleteInventoryItem
}

///////////////               ORDERS            ////////////////////////

actions.getOrders = async () => {
  let query = `query{
      getOrders{
        id
        quantity
        product{
          name
        }
        user{
          username
        }
        total
        dateAdded
      }
    }
`
  const data = await actions.sendOperation(query)
  return data.getOrders
}

actions.addOrder = async (item) => {
  let mutation = `mutation {
      addOrder(
        input: { quantity: ${item.quantity}, product: "${item.product}", user: "${item.user}" }
      ) {
        id
        quantity
        product{
          name
        }
        user{
          username
        }
        total
        dateAdded
      }
    }`
  const data = await actions.sendOperation(mutation)
  return data.addOrder
}

actions.updateOrder = async (item) => {
  let mutation = `mutation{
      updateOrder(id:"${item.id}",input:{quantity:${item.quantity}){
        id
        quantity
        product{
          name
        }
        user{
          username
        }
        total
        dateAdded
     }
     }`
  const data = await actions.sendOperation(mutation)
  return data.updateOrder
}

actions.deleteOrder = async (id) => {
  let mutation = `mutation {
      deleteOrder(id:"${id}")
    }
    `
  const data = await actions.sendOperation(mutation)
  return data.deleteOrder
}

export default actions
```

이 섹션에서는 GraphQL 서버와 상호 작용하여 애플리케이션 클라이언트 구축을 완료해야 하는 파일을 만들었습니다. 다음 섹션에서 응용 프로그램을 실행합니다.

## 응용 프로그램 실행

이 섹션에서는 응용 프로그램을 실행하지만 실행하기 전에 응용 프로그램 클라이언트에 대한 정적 파일을 만듭니다.

다음 명령을 실행합니다.

```coffeescript
npm run build
```

이 명령을 실행하면 응용 프로그램 클라이언트 정적 파일이 `dist`라는 디렉터리에 저장됩니다.

응용 프로그램 서버의 루트 디렉터리로 이동하십시오.

```bash
cd ../../../server
```

서버 디렉토리의 폴더 구조는 다음과 같아야 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/serverdirectory.png?resize=207%2C243&ssl=1)

응용 프로그램 서버를 실행하기 전에 다음 명령을 실행하여 새 MongoDB 인스턴스를 만듭니다.

```undefined
docker-compose up -d
```

위의 명령은 `docker-yml` 파일의 정보를 사용하여 다음 URI `mongodb://localhost:27017/mongodb`에서 액세스할 수 있는 새 MongoDB 인스턴스를 만듭니다.

다음 명령을 실행하여 노드 모듈을 설치합니다.

```coffeescript
npm install
```

GraphQL 서버를 시작하고 애플리케이션 클라이언트를 서비스하려면 다음 명령을 실행합니다.

```coffeescript
npm start
```

브라우저에서 `http://localhost:4000/`로 이동하면 다음과 유사한 항목이 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/dashboard.png?resize=748%2C400&ssl=1)

`users` 경로로 이동하여 새 사용자를 생성합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/users.png?resize=1366%2C768&ssl=1)

`카테고리` 경로로 이동하여 새 카테고리를 생성합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/categories.png?resize=1366%2C768&ssl=1)

`products` 경로로 이동하여 새 제품을 생성합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/prodcutspage.png?resize=1366%2C768&ssl=1)

`인벤토리` 경로로 이동하여 새 인벤토리 항목을 생성합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/inventory.png?resize=1366%2C768&ssl=1)

`주문` 경로로 이동하여 새 주문을 만듭니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/orders.png?resize=1366%2C768&ssl=1)

이 섹션에서는 응용 프로그램 클라이언트를 배포하고 GraphQL 서버를 사용하여 서비스를 시작했습니다.

## 결론

이 튜토리얼에서는 인벤토리 관리 응용 프로그램의 프런트 엔드 응용 프로그램을 생성했습니다. 먼저 응용 프로그램 서버가 포함된 리포지토리를 복제했습니다. 리포지토리를 복제한 후 `Vue`와 `Vuetify`를 사용하여 클라이언트의 사용자 인터페이스를 생성한 다음 `node-fetch`를 사용하여 클라이언트를 GraphQL API에 연결했습니다. 마지막으로 GraphQL 서버를 사용하여 클라이언트 응용 프로그램을 서비스하기 시작했습니다.