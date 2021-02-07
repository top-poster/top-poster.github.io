---
layout: post
title: "Vue 3과 함께 Vue 4 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/vuex-4-with-vue-3.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/vuex-4-with-vue-3.png?fit=730%2C487&ssl=1)

Vue 앱에서 선호하는 상태 관리 솔루션인 Vuex 4는 Vue 3과 호환되는 버전입니다. Vue 4를 Vue 3과 함께 사용하는 방법을 살펴보겠습니다.

## 설치

Vue 3과 함께 Vuex를 설치할 수 있는 몇 가지 방법 중 하나는 스크립트 태그를 사용하는 것입니다.

사용하기 위해 다음과 같이 쓸 수 있습니다.

```xml
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <button @click="increment">increment</button>
      <p>{count}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          increment(state) {
            state.count++;
          }
        }
      });

      const app = Vue.createApp({
        methods: {
          increment() {
            this.$store.commit("increment");
          }
        },
        computed: {
          count() {
            return this.$store.state.count;
          }
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

위에 Vue 4 및 Vue에 대한 스크립트를 추가한 다음 Vuex 글로벌 개체를 코드에서 사용할 수 있습니다.

상점을 만들기 위해 우리는 Vuex라고 부른다.기본 저장소를 만들기 위해 추가할 상태와 돌연변이가 있는 객체가 있는 생성자를 저장합니다. 상태는 Vuex 저장소에 데이터를 저장하는 속성입니다. Vue 3 앱의 어디에서나 데이터에 액세스할 수 있습니다. 돌연변이는 Vuex 저장소의 상태를 수정할 수 있는 기능입니다.

스토어를 만들고 나면 `Vue.createApp` 방식으로 전달하여 스토어를 앱에 추가합니다. 앱.use(store); 메서드 호출을 통해 Vue 3 앱의 스토어를 사용할 수 있습니다.

이제 매장이 추가되었으니, 우리는 이것을 사용할 수완전히 `이것`을 사용할 수 있다."달러는 주를 얻고 우리 상점을 조작하기 위해 재산을 저장한다. 이거.$store.commit` 방법을 사용하면 돌연변이를 스토어에 커밋할 수 있습니다.

"카운트" 상태를 업데이트하기 위해 점포 내 `증가` 변이를 자행한다. 따라서 증분 버튼을 클릭하면 count Vuex 상태가 count 계산 속성과 함께 업데이트됩니다.

## 게터즈

앱에 상점 상태를 더 쉽게 추가하기 위해, 우리는 게터를 사용할 수 있습니다. Getter는 다른 값에서 작동되거나 결합된 상태 또는 상태를 반환하는 함수입니다.

예를 들어 다음과 같이 기록하여 게터를 추가할 수 있습니다.

```xml
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <button @click="increment">increment</button>
      <p>{doubleCount}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          increment(state) {
            state.count++;
          }
        },
        getters: {
          doubleCount: (state) => {
            return state.count * 2;
          }
        }
      });

      const app = Vue.createApp({
        methods: {
          increment() {
            this.$store.commit("increment");
          }
        },
        computed: {
          ...Vuex.mapGetters(["doubleCount"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

"우리는 "카운트" 상태에 2를 곱한 "더블카운트" 게터를 추가했다. 그런 다음 구성 요소에서 Getter 이름을 사용하여 계산된 속성에 매핑하는 Vuex.mapGetters 메서드를 호출한다. 또한 템플릿에 포함되어 있어 가치를 확인할 수 있습니다.

만약 우리가 게터로서 방법을 원한다면, 우리는 게터에 함수를 반환할 수 있습니다. 예를 들어 다음과 같이 쓸 수 있습니다.

```xml
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <div>
        <p>{getTodoById(1).text}</p>
      </div>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          todos: [
            { id: 1, text: "drink", done: true },
            { id: 2, text: "sleep", done: false }
          ]
        },
        getters: {
          getTodoById: (state) => (id) => {
            return state.todos.find((todo) => todo.id === id);
          }
        }
      });

      const app = Vue.createApp({
        computed: {
          ...Vuex.mapGetters(["getTodoById"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

우리는 `투도스` 뷰익스 스토어 주(州)를 가지고 있으며, 우리는 그것으로부터 `id`의 속성 값으로 입력을 받기를 원한다. 그러기 위해서는 `get`이 필요하다.TodosById` getter 메서드로, 함수를 반환합니다. 이후 함수는 `상태`에서 항목을 반환합니다.find를 호출하여 id 값을 구하는 방식으로 배열을 수행합니다.

구성 요소에서 우리는 계산된 속성에 메소드를 매핑하는 것과 동일한 방법으로 "Vuex.mapGetters"라고 부른다.그러면 반환되는 함수를 호출하여 작업관리 항목을 `id` 값으로 가져올 수 있습니다. 따라서 `drink`는 `id:1`이므로 브라우저 화면에 `drink`가 표시되어야 한다.

## 돌연변이

우리는 이미 이전의 예에서 돌연변이를 보았습니다. 그것은 단지 상태를 수정하기 위해 사용할 수 있는 방법입니다.

돌연변이 방법은 상태 값을 수정하는 데 사용할 수 있는 페이로드를 취할 수 있습니다. 예를 들어 다음과 같이 쓸 수 있습니다.

```xml
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <button @click="increment">increment</button>
      <p>{count}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          increment(state, n) {
            state.count += n;
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          increment() {
            this.$store.commit("increment", 5);
          }
        },
        computed: {
          count() {
            return this.$store.state.count;
          }
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

우리의 `증가` 돌연변이 방법은 `카운트` 상태의 값을 증가시키는 데 사용되는 `n` 매개 변수를 가지고 있다. 그리고 나서, 우리는 이것을 `이것`이라고 부른다.$store.commit 메서드에 값을 전달하기 위한 두 번째 인수가 포함된 $store.commit 메서드입니다. n은 이제 5가 되어야 하므로 카운트 뷰엑스 주는 5가 늘어나게 된다.

## 객체 스타일 돌연변이 커밋

우리는 `이것`에 대해 어떤 목적을 전달할 수 있다.$store.commit` 메서드. 예를 들어 다음과 같이 쓸 수 있습니다.

```xml
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <button @click="increment">increment</button>
      <p>{count}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          increment(state, { amount }) {
            state.count += amount;
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          increment() {
            this.$store.commit({
              type: "increment",
              amount: 5
            });
          }
        },
        computed: {
          count() {
            return this.$store.state.count;
          }
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

우리는 이것을 `라고 불렀다.$store.commit` 개체와 `type` 및 `금액` 속성입니다.

type 속성은 호출할 돌연변이 방법의 이름을 찾는 데 사용됩니다. 그래서 우리는 `증가` 돌연변이 방법을 `유형`의 값이라고 부르겠습니다. 다른 속성들은 우리가 `증분` 방법의 두 번째 주장으로 전달하는 객체와 함께 전달될 것이다.

따라서 우리는 `증가`의 두 번째 매개 변수에서 `금액` 속성을 얻어 Vuex 스토어의 `카운트` 상태를 업데이트하는 데 사용합니다.

## 행동들

돌연변이는 몇 가지 한계가 있습니다. 돌연변이 방법은 Vuex를 사용하여 실행 순서를 추적할 수 있도록 동기화되어야 합니다. 그러나 Vuex는 상태를 수정하기 위해 돌연변이를 실행할 수 있는 실행 방법을 가지고 있다.

동작은 비동기 코드를 포함하여 모든 종류의 코드를 실행할 수 있습니다. 예를 들어 다음과 같이 쓸 수 있습니다.

```xml
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <p>{answer}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          answer: ""
        },
        mutations: {
          setAnswer(state, answer) {
            state.answer = answer;
          }
        },
        actions: {
          async getAnswer(context) {
            const res = await fetch("https://yesno.wtf/api");
            const answer = await res.json();
            context.commit("setAnswer", answer);
          }
        }
      });
      const app = Vue.createApp({
        mounted() {
          this.$store.dispatch("getAnswer");
        },
        computed: {
          answer() {
            return this.$store.state.answer;
          }
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

이렇게 하면 작업 속성의 방법으로 추가된 `setAnswer` 동작이 추가됩니다. 컨텍스트 매개 변수에는 돌연변이를 커밋할 수 있는 커밋 방법이 있습니다. 인수는 돌연변이의 이름입니다.

getAnswer 동작 방식은 비동기식으로, setAnswer 돌연변이를 커밋하기 위한 context.commit 메소드를 호출한다. 두 번째 인수의 정답은 setAnswer 방식으로 `answer` 매개 변수의 값으로 전달되며, `state.answer` 속성의 값으로 설정됩니다.

그러면 구성 요소에서 이것을 이용해서 답을 얻을 수 있다.$store.state.answer 속성. 마운트된 후크에서는 이것을 이렇게 부릅니다.$store.dispatch(getAnswer; "getAnswer")는 getAnswer 작업을 전송합니다. 따라서 템플릿에 답이 있는 개체가 표시되어야 합니다.

## 결론.

Vuex 4는 이전 버전의 Vuex와 크게 다르지 않습니다. v4 업데이트는 주로 Vue 3과의 호환성을 목적으로 합니다.

Getter, 돌연변이, 동작과 같이 Vuex 저장 상태를 가져오고 설정하는 데 사용되는 부분이 동일합니다.