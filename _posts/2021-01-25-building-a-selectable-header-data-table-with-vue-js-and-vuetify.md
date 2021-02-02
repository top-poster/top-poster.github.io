---
layout: post
title: "Vue.js 및 Vuetify로 선택 가능한 헤더 데이터 테이블 빌드
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/vue-clouds.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/vue-clouds.png?fit=750%2C487&ssl=1)

데이터 테이블은 엔터프라이즈 급 애플리케이션을 구축 할 때 절대적으로 필요합니다.
 일반적으로 다양한 애플리케이션에 유용합니다.
 이 문서는 데이터가 표시되는 방식을 사용자에게 더 많이 제어 할 수 있도록 돕기위한 것입니다.
 이렇게하면 앱의 사용자 경험이 향상됩니다.
 

이를 위해 잘 알려진 Vue.js 용 머티리얼 디자인 프레임 워크 인 Vuetify를 사용하여 헤더를 수정할 수있는 데이터 테이블을 구축합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Selectable-column-data-table.gif?resize=730%2C360&ssl=1)

## 작동 원리
 

사용자는 테이블이 왼쪽에 표시 할 열을 선택할 수 있으며 오른쪽에서 변경 사항을 즉시 볼 수 있습니다.
 

그런 다음 저장 버튼을 클릭하여 브라우저에서 선택한 구성을 저장할 수 있습니다.
 결과적으로 사용자가 해당 페이지를 다시로드하거나 잠시 후 해당 페이지로 돌아 오면 사용자가 선택한 열은 동일하게 유지됩니다.
 

### 우리에게 필요한 것
 

데모에는 CodePen을 사용할 것이지만 Vue.js 프로젝트에서이를 구현하는 경우 이미 Vue.js와 Vue CLI가 설치되어있을 것입니다.
 다음과 같이 Vue CLI를 사용하여 Vuetify를 설치할 수 있습니다.
 

```undefined
vue add vuetify
```

그러면 Vuetify를 사용할 수 있도록 프로젝트에 필요한 변경 사항이 적용됩니다.
 Vuetify 문서에서 Vuetify 설치에 대한 자세한 내용을 읽을 수 있습니다.
 

이 CodePen 데모에서는 HTML 스크립트 태그를 사용하여 Vue 및 Vuetify를 직접 가져올 것입니다.
 

```xml
<html>

<head>
  <link href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/@mdi/font@4.x/css/materialdesignicons.min.css" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/vuetify@2.x/dist/vuetify.min.css" rel="stylesheet">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, minimal-ui">
</head>

<body>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.x/dist/vue.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/vuetify@2.x/dist/vuetify.js"></script>
</body>

</html>
```

### 데이터 테이블 생성
 

이제 설정이 완료되었으므로 코드 작성을 시작할 수 있습니다.
 

먼저 데이터 테이블을 채우는 데 사용할 데이터가 필요합니다.
 Vuetify 문서의 샘플 데이터를 사용하겠습니다.
 

펜의 JavaScript 부분에는 다음 코드가 있습니다.
 

```undefined
new Vue({
      el: '#app',
      vuetify: new Vuetify(),
      data () {
      return {
        headers: [
          {
            text: 'Dessert (100g serving)',
            align: 'start',
            sortable: false,
            value: 'name',
          },
          { text: 'Calories', value: 'calories' },
          { text: 'Fat (g)', value: 'fat' },
          { text: 'Carbs (g)', value: 'carbs' },
          { text: 'Protein (g)', value: 'protein' },
          { text: 'Iron (%)', value: 'iron' },
        ],
        desserts: [
          {
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: '1%',
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: '1%',
          },
          {
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            iron: '7%',
          },
          {
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
            iron: '8%',
          },
          {
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
            iron: '16%',
          },
          {
            name: 'Jelly bean',
            calories: 375,
            fat: 0.0,
            carbs: 94,
            protein: 0.0,
            iron: '0%',
          },
          {
            name: 'Lollipop',
            calories: 392,
            fat: 0.2,
            carbs: 98,
            protein: 0,
            iron: '2%',
          },
          {
            name: 'Honeycomb',
            calories: 408,
            fat: 3.2,
            carbs: 87,
            protein: 6.5,
            iron: '45%',
          },
          {
            name: 'Donut',
            calories: 452,
            fat: 25.0,
            carbs: 51,
            protein: 4.9,
            iron: '22%',
          },
          {
            name: 'KitKat',
            calories: 518,
            fat: 26.0,
            carbs: 65,
            protein: 7,
            iron: '6%',
          },
        ],
      }
    },
})
```

이것은 Vuetify가 즉시 제공하는 데이터 테이블 구성 요소에 제공 할 데이터입니다.
 

펜의 HTML 부분에서 데이터 테이블 구성 요소 인`V-data-table`을 사용합니다.
 

```xml
<v-row>
  <v-col md="8" sm="12">
    <v-data-table :headers="headers" :items="desserts" :items-per-page="5" class="elevation-1"></v-data-table>
  </v-col>
</v-row>
```

headers props에 바인딩 된 headers 배열은 위의 data 속성에 정의되어 있습니다.
 여기에는`text` 및`value` 키가있는 객체가 포함됩니다.
 이것은`v-data-table`에 필요한 헤더 객체의 형식입니다.
 텍스트는 표시 될 것이며 값은 각 헤더의 고유 한 값입니다.
 이것은 또한 각 헤더의 식별자 역할을합니다.
 

디저트 배열은 테이블의 내용을 전달하며 항목 소품에 바인딩되어 데이터 테이블에 내용으로 표시 할 내용을 알려줍니다.
 `페이지 당 항목`과 같은 다른 항목은 한 페이지에서 보려는 레코드 수를 결정합니다.
 `class` 소품은 CSS 클래스를 추가합니다.
 

이를 통해 데이터 테이블은 다음과 같아야합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/list-of-foods.png?resize=730%2C377&ssl=1)

### 헤더를 선택 가능하게 만들기
 

HTML에서는`v-autocomplete`라는 또 다른 Vuetify 구성 요소를 사용합니다.
 이것은 기본적으로 검색 기능이있는 선택 드롭 다운입니다.
 

```undefined
<v-autocomplete :items="headers" v-model="headersSelected" return-object label="Select headers" multiple chips></v-autocomplete>
```

헤더 배열 (이전에 정의 된)을이 컴포넌트의 items prop에 전달하고 있음을 주목하십시오.
 이는 드롭 다운이 테이블의 헤더로 채워짐을 의미합니다.
 이 구성 요소의`v-model`은`headersSelected`라는 새 변수에 바인딩됩니다.
 즉, 모든 헤더가 드롭 다운에 있으므로 선택한 모든 헤더가`headersSelected` 변수로 푸시됩니다.
 이것을 빈 배열로 정의합니다.
 

JavaScript 부분에서 데이터 속성에 다음을 추가합니다.
 

```css
...
headersSelected: [],
...
```

이제 HTML의 데이터 테이블 구성 요소로 돌아가`headers` prop을`headersSelected` 변수에 바인딩 할 수 있습니다.
 이렇게하면 데이터 테이블의 헤더가 동적이 될 수 있습니다.
 

```js
...
<v-data-table :headers="headersSelected" :items="desserts" :items-per-page="5" class="elevation-1"></v-data-table>
```

이제`headersSelected`는 기본적으로 빈 배열이므로 사용자가 첫 번째로드에서 데이터를 볼 수 있도록 페이지로드시 데이터로 채워야합니다.
 이를 위해 다음과 같은 방법을 사용합니다.
 

```js
...
methods: {
        populateHeaders(){
          let headers = JSON.parse(localStorage.getItem('headers'))
          if(!headers){
            this.headersSelected = this.headers
          }else{
            this.headersSelected = headers
          }
        },
}
...
```

위의 메소드는 `headers`속성이 있는지 `localStorage`를 확인합니다.
 사용할 수없는 경우 기본 헤더 ( `this.headers`)를`headersSelected` 변수에 할당합니다.
 

그런 다음`created ()`라이프 사이클 후크에서 메서드를 호출 할 수 있습니다.
 

```coffeescript
created(){
        this.populateHeaders()
      },
```

이제 테이블 헤더를 선택할 수 있습니다.
 다음으로 `localStorage`에 선택 사항을 저장하는 방법을 살펴 보겠습니다.
 하지만 먼저 지금까지 완전한 HTML 및 JavaScript 코드를 살펴 보겠습니다.
 

```xml
<html>

<head>
  <link href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/@mdi/font@4.x/css/materialdesignicons.min.css" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/vuetify@2.x/dist/vuetify.min.css" rel="stylesheet">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, minimal-ui">
</head>

<body>
  <div id="app" data-app>
    <template>
      <v-row align="center">
        <v-col class="d-flex" cols="12" sm="12" md="4">
<!--Select dropdown -->
          <v-autocomplete :items="headers" v-model="headersSelected" return-object label="Select headers" multiple chips></v-autocomplete>
        </v-col>

        <v-col md="8" sm="12">
<!--Data table-->
          <v-data-table :headers="headersSelected" :items="desserts" :items-per-page="5" class="elevation-1"></v-data-table>
        </v-col>
      </v-row>

    </template>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.x/dist/vue.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/vuetify@2.x/dist/vuetify.js"></script>
</body>

</html>
```

자바 스크립트 :
 

```undefined
new Vue({
      el: '#app',
      vuetify: new Vuetify(),
      data () {
      return {
        headers: [
          {
            text: 'Dessert (100g serving)',
            align: 'start',
            sortable: false,
            value: 'name',
          },
          { text: 'Calories', value: 'calories' },
          { text: 'Fat (g)', value: 'fat' },
          { text: 'Carbs (g)', value: 'carbs' },
          { text: 'Protein (g)', value: 'protein' },
          { text: 'Iron (%)', value: 'iron' },
        ],
        desserts: [
          {
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: '1%',
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: '1%',
          },
          {
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            iron: '7%',
          },
          {
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
            iron: '8%',
          },
          {
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
            iron: '16%',
          },
          {
            name: 'Jelly bean',
            calories: 375,
            fat: 0.0,
            carbs: 94,
            protein: 0.0,
            iron: '0%',
          },
          {
            name: 'Lollipop',
            calories: 392,
            fat: 0.2,
            carbs: 98,
            protein: 0,
            iron: '2%',
          },
          {
            name: 'Honeycomb',
            calories: 408,
            fat: 3.2,
            carbs: 87,
            protein: 6.5,
            iron: '45%',
          },
          {
            name: 'Donut',
            calories: 452,
            fat: 25.0,
            carbs: 51,
            protein: 4.9,
            iron: '22%',
          },
          {
            name: 'KitKat',
            calories: 518,
            fat: 26.0,
            carbs: 65,
            protein: 7,
            iron: '6%',
          },
        ],
        headersSelected: [],
      }
    },
      methods: {
        populateHeaders(){
          let headers = JSON.parse(localStorage.getItem('headers'))
          if(!headers){
            this.headersSelected = this.headers
          }else{
          this.headersSelected = headers
          }
        },
      },
      created(){
        this.populateHeaders()
      },
    })
Vue.use(Vuetify);
```

### 선택 저장
 

저장하려면 버튼과 메소드가 필요합니다.
 Vuetify, HTML의`v-btn` 버튼 구성 요소를 사용합니다.
 

```undefined
<v-btn @click="save" class="mt-3">Save</v-btn>
```

`@ click`은 해당 구성 요소의 이벤트 리스너입니다.
 이 버튼을 클릭하면 해당 버튼에 의해 생성되는 클릭 이벤트를 수신하고 save 메서드를 호출합니다.
 

이제 저장 방법을 정의하겠습니다.
 

```js
methods: {
        ...
        save(){
          localStorage.setItem('headers', JSON.stringify(this.headersSelected))
        alert('Table format saved')
        }
}
```

`this.headersSelected`에 포함 된 선택된 헤더를 키 헤더가있는 JSON 문자열로`localStorage`에 저장합니다.
 

이렇게하면 저장 버튼을 클릭하면 해당 지점에서 선택한 헤더가 `localStorage`에 저장됩니다.
 사용자가 다른 선택을하고 저장을 클릭하면 새 선택이 이전 선택을 덮어 씁니다.
 

## 결론
 

사용자 경험은 응용 프로그램을 구축 할 때 중요한 고려 사항입니다.
 데이터 테이블은 일반적으로 사용자에게 한눈에 많은 양의 정보를 제공하는 데 사용됩니다.
 사용자에게보고 싶은 데이터 열을 결정할 수있는 기능을 부여하는 것은 사용자 경험을 개선하는 좋은 방법입니다.
 

그게 다야!
 

전체 코드 및 데모 :
 