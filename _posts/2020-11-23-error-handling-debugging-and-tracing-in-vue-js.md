---
layout: post
title: "Vue.js에서 오류 처리, 디버깅 및 추적"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/error-handling-debugging-tracing-vue-js.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/error-handling-debugging-tracing-vue-js.png?fit=730%2C487&ssl=1)

소프트웨어 개발에서, 사물은 사용자에게 보이는 것만큼 순탄하지 않다. 개발자들이 항상 뒤에서 다루어야 할 오류와 버그가 있다. 짐작하시겠지만 디버깅 및 오류 추적은 시간이 많이 걸릴 수 있으며 사용하는 도구는 생산성을 크게 향상시킬 수 있습니다.

Vue.js를 사용하는 경우 다양한 유형의 오류가 발생할 수 있습니다. 일반적으로 이러한 오류를 처리하는 방법은 두 가지가 아닙니다. 이 튜토리얼에서는 몇 가지 모범 사례를 검토하고 Vue.js에서 오류를 처리하고, 디버거를 설정하고, 오류를 효율적으로 추적하는 방법을 시연합니다.

Vue CLI를 사용하여 Placeholder API의 항목 목록을 표시하는 작업관리 앱인 데모 프로젝트를 설정합니다.

이 프로젝트의 전체 코드는 GitHub에서 사용할 수 있습니다.

## 오류 처리란 무엇입니까?

오류 처리란 사용자 환경에 부정적인 영향을 주지 않고 응용프로그램 오류를 추적, 처리 및 해결하는 프로세스를 말합니다. 다음 세 가지 목표를 달성하는 것을 목표로 합니다.

- 처리되지 않은 예외가 있는 경우 프로덕션에서 응용 프로그램이 예기치 않게 중단되지 않도록 방지
- 오류가 기록되면 어떻게 되는지 확인
- 사용자 환경 개선(예: 버그가 발견되고 수정될 때 메시지 표시)

## 디버깅이란 무엇입니까?

디버깅은 소프트웨어에서 오류를 식별하고 제거하고 버그를 해결하는 프로세스입니다. 문제를 분석하고, 문제의 원인을 파악하고, 원인을 파악하며, 가능한 한 효율적으로 해결하기 위해 과정을 차트화해야 합니다.

## Vue.js에서 디버깅

Vue.js는 오류를 처리하고 응용 프로그램의 버그를 신속하게 해결하는 데 도움이 되는 디버거를 제공합니다. VueJs 디버거는 VSCode 및 Chrome 및 Firefox와 잘 통합되어 있습니다.

먼저 Chrome 또는 Firefox용 Vue Debugger 확장을 다운로드하여 설치합니다. 자세한 구성 방법은 Vue.js 문서에서 확인할 수 있습니다.

디버거를 설치했으면 VSCode Activity Bar로 이동하여 디버깅 아이콘을 클릭합니다. 기어/설정 아이콘이 표시됩니다. 이 아이콘을 클릭하고 원하는 브라우저를 선택합니다.

그런 다음 `launch.json` 파일을 열고 선택한 브라우저를 기준으로 해당 구성에 붙여넣습니다.

설정이 완료되면 응용 프로그램에서 여러 중단점을 설정하고 방금 설치한 디버거를 실행하는 것만큼 쉽게 오류를 추적할 수 있습니다.

## 추적이란 무엇인가?

추적은 오류의 근원을 검색하고 스택 추적에서 원인을 식별하는 프로세스입니다.

짐작하셨겠지만 Vue Debugger를 사용하면 추적도 훨씬 쉬워집니다.

### 중단점 설정

효과적인 추적을 달성하기 전에 중단점을 설정하는 방법을 배워야 합니다.

VSCode에서 라인 번호를 두 번 클릭하여 중단점을 추가합니다. 그런 다음 `npm run serve`를 실행하여 프로젝트를 수행합니다. 편집기의 디버그 보기로 이동하여 원하는 브라우저 구성을 선택합니다. 5번을 누르거나 녹색 재생 버튼을 클릭합니다. 이 때 코드가 실행될 때 중단점이 적중해야 합니다.

디버그 보기를 보면 응용 프로그램을 디버깅하는 데 유용한 데이터를 찾을 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/vscode-debugging-data.png?resize=720%2C405&ssl=1)

## Vue.js의 오류 처리

Vue.js 응용 프로그램의 오류 처리 작업은 복잡할 수 있습니다. 가장 좋은 방법은 그것을 단계로 나누는 것이다.

제가 개발하는 모든 애플리케이션에서, 저는 항상 일반적인 오류 서비스 클래스를 만들고 오류 유형에 따라 발생하는 오류를 처리하고 처리하는 기능을 포함하여 오류 처리 계획을 수립합니다.

### 오류 유형

Vue.js 앱에는 다음과 같은 네 가지 주요 유형의 오류가 발생할 수 있습니다.

- 잘못된 구문을 사용할 경우 구문 오류가 발생합니다.
- 실행 중 잘못된 작업으로 인해 런타임 오류가 발생함
- 논리적 오류는 프로그램 논리의 실수로 인해 발생하기 때문에 식별하기 어렵다.
- API를 사용할 때 HTTP 오류가 자주 발생합니다.

이제 Vue Debugger를 사용하여 수행할 수 있는 오류 처리 및 디버깅 작업의 몇 가지 구체적인 예를 확대해보겠습니다.

## Vue.js에서 오류 서비스 생성

오류 서비스 클래스는 모든 오류를 처리하고 오류를 처리하는 방법을 결정합니다.

```cpp
import Swal from "sweetalert2";
import "sweetalert2/dist/sweetalert2.min.css";

export default class ErrorService {
  constructor() {
    // this.initHandler();
  }

  static onError(error) {
    const response = error.response;
    if (response && response.status >= 400 && response.status < 405) {
      // You can handle this differently
      ErrorService.sentryLogEngine(error);
      return false;
    }
    // Send Error to Log Engine e.g LogRocket
    ErrorService.logRocketLogEngine(error);
  }

  static onWarn(error) {
    // Send Error to Log Engine e.g LogRocket
    this.logRocketLogEngine(error);
  }

  static onInfo(error) {
    // You can handle this differently
    this.sentryLogEngine(error);
  }

  static onDebug(error) {
    const response = error.response;
    if (response && response.status >= 400 && response.status < 405) {
      // You can handle this differently
      this.sentryLogEngine(error);
      return false;
    }
    // Send Error to Log Engine e.g LogRocket
    this.logRocketLogEngine(error);
  }

  static initHandler() {
    const scope = this;
    window.onerror = (message, url, lineNo, columnNo, error) => {
      console.log(error, "test");
      if (error) {
        scope.onError(error);
        console.log(message, url, lineNo, columnNo, error);
      }
    };
  }

  static displayErrorAlert(message) {
    Swal.fire({
      title: "Error!",
      text: message,
      icon: "error",
    });
  }

  static logRocketLogEngine(error) {
    // Implement LogRocket Engine here
    console.log(error, "LogRocket");
  }

  static sentryLogEngine(error) {
    // Implement Sentry Engine here
    console.log(error, "Sentry");
  }
}
```

## 가능한 모든 Vue 오류 검색

Vue.js는 가능한 모든 Vue 오류를 포착하는 오류 처리기를 제공합니다. main.js 파일 내에서 이 작업을 수행할 수 있습니다.

```coffeescript
import Vue from "vue";
import App from "./App.vue";
import { ErrorService } from "./Services/ErrorService";
import store from "./store";

Vue.config.productionTip = false;

// Handle all Vue errors
Vue.config.errorHandler = (error) => ErrorService.onError(error);

new Vue({
  store,
  render: (h) => h(App),
}).$mount("#app");
```

## Vuex의 'ErrorService'

Vuex에서 `ErrorService` 클래스를 사용하면 Axios의 HTTP 오류를 처리하고 처리할 수 있습니다. 또한 오류를 Vuex 상태에 저장하여 사용자에게 올바르게 표시할 수 있습니다.

```js
import Vue from "vue";
import Vuex from "vuex";
import { ErrorService } from "./Services/ErrorService";
import axios from "axios";

Vue.use(Vuex);

const store = new Vuex.Store({
  state: {
    todos: [],
    errors: [],
    users: [],
  },

  actions: {
    async getTodos({ commit }) {
      try {
        const response = await axios.get(
          `https://jsonplaceholder.typicode.com/todos`
        );
        const { data } = response;
        commit("STORE_TODOS", data);
      } catch (error) {
        // Handling HTTPs Errors
        commit("STORE_ERRORS", error);
      }
    },

    async getUsers({ commit }) {
      try {
        const response = await axios.get(
          `https://jsonplaceholder.typicode.com/users`
        );
        const { data } = response;
        commit("STORE_USERS", data);
      } catch (error) {
        // Handling HTTPs Errors
        commit("STORE_ERRORS", error);
      }
    },
  },

  mutations: {
    STORE_TODOS: (state, data) => {
      state.todos = data;
    },

    STORE_ERRORS: (state, error) => {
      // Call Error Service here
      ErrorService.onError(error);
      ErrorService.initHandler();

      // Store error to state(optional)
      if (error.response) {
        state.errors = error.response;
      }
    },

    STORE_USERS: (state, data) => {
      state.users = data;
    },
  },

  getters: {
    getTodo: (state) => (id) => {
      return state.todos.find((todo) => todo.id == id);
    },
    getUser: (state) => (id) => {
      return state.users.find((user) => user.id == id);
    },
  },

  // strict: true
});

export default store;
```

## 구성 요소에 오류 표시

Vue의 반응성을 활용할 수 있는 Vuex 상태에 오류가 저장되므로 다음과 같이 오류 구성 요소에 오류를 표시할 수 있습니다.

```xml
<script>
import { mapGetters } from "vuex";
import ErrorService from "../Services/ErrorService";

export default {
  name: "HelloWorld",
  props: {
    todo: Object,
  },
  computed: {
    ...mapGetters(["getUser"]),
  },

  methods: {
    getUserName(id) {
      const user = this.getUser(id);
      if (user) return user.username;
    },

    // Handling Errors in component
    methodThrowsException() {
      try {
        // Do unexpected job
      } catch (error) {
        ErrorService.onError(error);
      }
    },
  },
};
</script>
```

## Vue.js에서 플러그인 오류 표시

이러한 오류는 여러 가지 방법으로 사용자에게 표시할 수 있습니다. vue-sweet alert2 플러그인을 사용하여 오류를 표시하겠습니다.

```xml
<script>
import { mapGetters } from "vuex";
import ErrorService from "../Services/ErrorService";

export default {
  name: "HelloWorld",
  props: {
    todo: Object,
  },
  computed: {
    ...mapGetters(["getUser"]),
  },

  methods: {
    getUserName(id) {
      const user = this.getUser(id);
      if (user) return user.username;
    },

    // Display Error with SweetAlert (when Name is Click)
    displayAlert() {
      ErrorService.displayErrorAlert("Testing message");
    },
  },
};
</script>
```

## 결론

애플리케이션의 오류를 효율적으로 처리하고 버그를 해결할 수 있는 기능이 매우 중요합니다. 생산성을 최대화하려면 Vue.js 앱을 디버깅하기 위한 올바른 도구와 방법을 선택하면 큰 차이가 있을 수 있습니다.

이러한 워크스루로 VueJS의 오류 관리, 디버깅 및 추적을 완벽하게 이해할 수 있기를 바랍니다.