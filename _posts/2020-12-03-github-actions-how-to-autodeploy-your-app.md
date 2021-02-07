---
layout: post
title: "GitHub 작업: 앱을 자동 배포하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/github-actions-vuejs-netlify-autodeploy.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/github-actions-vuejs-netlify-autodeploy.png?fit=730%2C487&ssl=1)

변경 내용을 게시하고 오류를 수정하는 앱에서 작업한 적이 있는 경우 꺼내기 요청을 프로덕션 분기에 병합하기 전에 미리 보기 빌드 및 테스트가 실행될 때까지 기다리는 것이 얼마나 고통스러운지 알 수 있습니다.

이 튜토리얼에서는 GitHub Actions를 사용하여 프로세스를 자동화하는 방법에 대해 설명합니다. 샘플 Vue.js 앱을 만들고 몇 가지 테스트를 작성한 다음 원격 저장소로 푸시합니다. 테스트 및 기타 검사를 실행하는 작업을 트리거한 다음 모든 검사가 통과하면 자동으로 프로덕션 분기로 병합됩니다. 이렇게 하면 프로덕션 Netliify로 빌드가 트리거됩니다.

다음 사항에 대해 자세히 설명합니다.

- GitHub Actions란 무엇인가?
- Netliify란 무엇인가?
- Vue.js 앱을 만드는 중
- Vue.js에서 단위 테스트 설정
- GitHub 작업으로 Netliify에 배포
- Github Actions를 사용한 배포 준비
- GitHub Actions 사용 방법
- GitHub에 리포지토리 밀어넣기

## GitHub Actions란 무엇인가?

GitHub Actions는 프로젝트의 특정 프로세스를 자동화하기 위해 리포지토리에 작성하는 지시사항입니다. GitHub Actions를 사용하면 GitHub에서 직접 코드를 빌드, 테스트 및 배포할 수 있습니다.

2019년 11월 출시된 GitHub Actions는 스스로 "GitHub에 대한 원인과 결과를 위한 API"라고 청구한다. 이를 통해 푸시, 새 릴리스, 이슈 생성 등과 같은 지정된 이벤트를 기반으로 워크플로우를 자동화하고 이러한 워크플로우를 리포지토리에 배치하여 소프트웨어 개발 관행을 공유, 재사용 및 포크할 수 있습니다.

## Netliify란 무엇인가?

Netliify는 최신 웹 프로젝트를 자동화하기 위한 정적 배포 플랫폼입니다. 지속적인 배포, 서버 없는 양식 처리, AWS Lambda 지원 등이 있습니다.

Netlifify를 사용하면 세 가지 빠른 단계로 웹 응용 프로그램을 쉽게 전송할 수 있습니다.

- 리포지토리 연결
- 빌드 설정 구성
- 사이트 배포

## Vue.js 앱을 만드는 중

먼저 Vue CLI를 사용하여 앱의 비계를 만듭니다.

Vue.js의 작동 방식이나 Vue 앱을 테스트하는 방법에 너무 집중하지 않겠습니다. 자세한 내용은 Vue Testing Library를 통해 Vue 구성 요소 테스트에 대한 튜토리얼을 참조하십시오.

다음 명령을 사용하여 Vue CLI를 설치합니다.

```coffeescript
npm install -g @vue/cli # for NPM
yarn add global @vue/cli # for yarn
```

vue create auto-deploy를 실행하고 아래 옵션을 선택하여 Vue CLI를 사용하여 새 프로젝트를 생성합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/options-to-create-new-project.png?resize=730%2C227&ssl=1)

코드 편집기에서 프로젝트 폴더를 열고 `/src/components/HelloWorld.vue` 구성 요소를 삭제합니다. 이 데모에서는 `/src/App.vue` 구성 요소를 사용하여 간단한 실행 가능한 앱을 만들고 테스트합니다.

앱에서.vue` 파일, 파일의 내용을 아래 코드 조각으로 바꿉니다.

```xml
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <h1>Todo app</h1>
    <div id="add-todos">
      <input v-model="task" type="text" class="task-input" />
      <button class="add-task-button" @click="addTask">Add Task</button>
    </div>
    <div id="todos">
      <ul>
        <li v-for="(todo, index) in todos" :key="index" class="todo-item">
          <span>{ todo }</span>
          <button class="delete-task-button" @click="deleteTask(todo)">Delete</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      task: "",
      todos: []
    };
  },
  methods: {
    addTask() {
      if (this.task !== "") {
        this.todos.push(this.task);
        this.task = "";
      }
    },
    deleteTask(todoItem) {
      this.todos = this.todos.filter(item => item !== todoItem);
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding: 20px;
  max-width: 500px;
  margin: auto;
}
.task-input {
  margin-right: 10px;
}
.todo-item {
  display: flex;
  justify-content: space-between;
  padding: 10px 20px;
  border: 1px solid #b9b9b9;
  margin-top: 4px;
}
</style>
```

위의 코드 조각은 구성 요소의 초기 컨텐츠에 있는 기본 작업관리 앱에 대한 것입니다. 브라우저에서 `localhost:8080`을 방문하면 앱이 다음과 같이 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/todo-app-view.png?resize=730%2C424&ssl=1)

이제 작업관리 항목을 추가하고 삭제할 수 있습니다.

## Vue.js에서 단위 테스트 설정

프로젝트를 만들 때 유닛 테스트 옵션을 선택했기 때문에 Vue CLI는 자동으로 구성을 설정하고 `vue-test-utils`라는 패키지를 설치했습니다. 이 상자는 Vue의 API와 유사한 유틸리티 기능을 제공합니다. 우리는 이러한 효용 함수를 사용하여 구성 요소와 상호 작용할 수 있다.

`패키지`를 보시면요.루트 폴더에 있는 json 파일의 경우 다음 명령 아래에 `sign` 키가 있어야 합니다.

```bash
"test:unit": "vue-cli-service test:unit"
```

이것은 유닛 테스트를 실행하기 위한 명령입니다. 이 시점에서 명령을 실행하려고 하면 터미널에서 사전 작성된 테스트가 실행됩니다. 이는 모든 것이 올바르게 작동하고 테스트 구성이 올바른지 확인하기 위한 것입니다.

이제 `/tests/` 폴더를 삭제하고 프로젝트의 루트 디렉토리인 `__tests___`에 새 폴더를 만듭니다. Jest는 `/tests/unit`와 `__tests___`의 두 폴더에서 테스트 사양을 확인합니다. 이건 모두 선호도의 문제입니다. 이 튜토리얼에서는 `__tests__` 폴더를 사용합니다.

새로 만든 폴더에 `App.spec.js` 파일을 생성하고 아래 코드를 추가하십시오.

```js
import { mount } from "@vue/test-utils";
import App from "@/App.vue";
describe("App.vue", () => {
  it("should add a task when button is clicked", async () => {
    const wrapper = mount(App);
    const task = "Do laundry";
    wrapper.find("input.task-input").setValue(task);
    await wrapper.find("button.add-task-button").trigger("click");
    expect(wrapper.vm.$data.todos.indexOf(task)).toBeGreaterThan(-1);
  });
  it("should render the tasks", () => {
    const todos = ["pick up groceries", "buy guitar pick"];
    const wrapper = mount(App, {
      data() {
        return {
          todos
        };
      }
    });
    expect(wrapper.find("div#todos>ul").text()).toContain(todos[0]);
  });
  it("should delete a task when the delete button is clicked", async () => {
    const todos = ["Order pizza for dinner"];
    const wrapper = mount(App, {
      data() {
        return {
          todos
        };
      }
    });
    await wrapper.find("button.delete-task-button").trigger("click");
    expect(wrapper.vm.$data.todos.indexOf(todos[0])).toEqual(-1);
  });
});
```

위의 코드는 구성 요소가 정확히 우리가 원하는 대로 작동하도록 보장하는 테스트 제품군입니다. 우리는 새로운 작업을 추가하고 작업을 삭제하는 것이 효과가 있으며 작업이 실제로 페이지에 렌더링되었는지 검증했습니다.

이제 테스트를 포함하여 앱을 설정하고 실행 중이므로 GitHub Actions에서 배포를 설정할 수 있도록 NetApp에 새 앱을 배포해 보겠습니다.

GitHub에 앱 저장소를 새로 만들고 코드를 눌러야 합니다. 이 가이드의 나머지 부분은 깃허브를 사용하겠습니다.

## GitHub 작업으로 Netliify에 배포

공식 Netliify 웹 사이트의 책임자가 계정을 만든 다음 로그인합니다.

대시보드에서 Git에서 새 사이트 단추를 클릭하고 GitHub 계정을 Netliify에 연결합니다.

새 앱의 리포지토리를 선택하고 아래 이미지에서 동일한 구성을 사용하십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/configurations-for-new-app.png?resize=730%2C307&ssl=1)

사이트 배포 단추를 클릭하고 앱이 준비될 때까지 기다립니다.

Netlifify에서 자동으로 생성된 링크를 사용하여 앱을 열 수 있습니다.

## Github Actions를 사용한 배포 준비

Netlifify는 즉시 자동 배포 기능을 제공하지만 이를 사용하지 않습니다. 테스트를 실행한 후 GitHub Actions가 배포를 대신 처리하기를 원합니다. 예를 들어 기능이 아직 개발 중이거나 롤아웃할 준비가 되지 않은 경우 등 변경 사항을 즉시 배포하지 않는 것이 좋습니다.

Netlifify는 빌드가 성공할 때마다 배포하기 때문에 푸시할 때마다 Netlifify가 앱을 자동으로 빌드하고 배포하는 것을 방지하고자 합니다. 우리는 또한 우리의 테스트를 고려하고자 합니다. 그래서 우리는 GitHub Actions를 사용할 것입니다.

### Netlifify에서 빌드 사용 안 함

첫 번째 단계는 Netliify에서 빌드를 중지하는 것입니다. 그러려면 사이트 설정 > 빌드로 이동합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/screenshot-showing-stop-builds-checkbox.png?resize=730%2C231&ssl=1)

### GitHub에서 Netlifify 비밀 구성

이제 Netlifify에 대한 빌드를 사용하지 않도록 설정했으므로 Netlifify에서 제공하는 비밀 키를 설정해야 합니다. 이 키를 사용하면 Netliify 외부에 사이트를 배포할 수 있습니다.

우리가 필요한 두 가지 주요 키는 개인 액세스 토큰과 새로 만든 사이트의 API ID입니다.

사이트의 API ID는 사용자 사이트의 설정 > 일반 > 사이트 세부 정보 아래에 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/api-id-view.png?resize=730%2C294&ssl=1)

GitHub에 있는 앱의 리포지토리에 복사하여 붙여넣기할 예정이기 때문에 참고하시기 바랍니다. 걱정 마, 안전해. 두고 봐.

다음으로 개인 액세스 토큰을 생성하겠습니다. 아래 단계를 따르십시오.

계정 설정으로 이동합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/account-settings-view.png?resize=730%2C497&ssl=1)

응용 프로그램 > 개인 액세스 토큰에서 새 액세스 토큰을 만들고 이를 설명하는 이름을 지정합니다. 페이지를 나가기 전에 토큰을 복사해야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/how-to-create-new-access-token.png?resize=730%2C288&ssl=1)

토큰은 한 번만 표시됩니다. 복사하는 것을 잊어버린 경우 새 복사 항목을 만들어야 합니다.

앱의 리포지토리를 열고 설정 > 암호로 이동한 다음 두 가지 암호를 만듭니다. 첫 번째 `NETLIFY_AUTH_TOKEN`은 방금 복사한 개인 액세스 토큰에 대한 것입니다. 두 번째 `NETLIFY_SITE_`ID`는 사이트 정보 아래에 있는 사이트의 API ID에 대한 것입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/view-netlify-auth-token-site-id.png?resize=730%2C387&ssl=1)

당신의 비밀은 위의 스크린샷처럼 보여야 합니다. 또한 이러한 비밀에 대한 가치는 절대 노출되지 않으며 접근도 제한적이라는 것을 알게 될 것입니다.

이제 Vue.js 앱을 빌드, 테스트 및 배포하도록 GitHub Actions를 설정해야 합니다.

## GitHub Actions 사용 방법

GitHubactions 사용 방법을 보여드리기 위해 두 개의 워크플로를 생성하겠습니다. 첫 번째는 마스터 지점에 대한 풀 요청으로 실행되며 마스터할 코드를 빌드, 테스트 및 병합합니다. 두 번째는 마스터 지점에 대한 커밋으로 실행되어 이번에 우리 앱을 만들고 테스트하고 배포할 것이다.

로컬 컴퓨터에서 마스터 분기로 직접 커밋을 푸시할 수 있는 상황(핫 픽스 등)이 있을 수 있기 때문에 워크플로우를 분리했습니다. 우리는 우리가 무엇을 하든 간에 어떤 것도 깨지지 않도록 확실히 하고 싶다.

### 자동 배포 워크플로우 생성

코드 편집기에서 프로젝트의 루트 폴더에 `.ghub/workflows/`와 같은 디렉터리 구조를 만들고 `/workflows` 폴더에 `autodeploy.yml`이라는 파일을 추가합니다.

아래 조각을 복사하여 `autodeploy.yml` 파일에 붙여넣으십시오.

```coffeescript
name: Auto Deploy
on:
  push:
    branches: [master] # run on pushes to master
jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2 # setup the repository in the runner
      - name: Setup Node.js # setup Node.js in the runner
        uses: actions/setup-node@v1
        with:
          node-version: '12'
      - uses: actions/cache@v2
        with:
          path: ~/.npm
          key: ${ runner.os }-node-${ hashFiles('**/package-lock.json') }
          restore-keys: ${ runner.os }-node-
      - run: npm ci # install dependencies
      - run: npm run build --if-present # build the project
      - run: npm run test:unit # run the tests
      # deploy site to netlify using secrets created on repository
      - uses: netlify/actions/cli@master 
        env:
          NETLIFY_AUTH_TOKEN: ${ secrets.NETLIFY_AUTH_TOKEN }
          NETLIFY_SITE_ID: ${ secrets.NETLIFY_SITE_ID }
        with:
          args: deploy --dir=dist --prod
          secrets: '["NETLIFY_AUTH_TOKEN", "NETLIFY_SITE_ID"]'
```

위의 워크플로 파일에서 다음 일련의 순차적 단계를 포함하는 작업 `빌드 앤드 테스트`를 생성했습니다.

- Runner에서 리포지토리 설정
- Runner에서 Node.js v12 설정
- 캐시된 종속성이 유효한지 확인(실행 시간 단축)
- 새 종속성 설치
- 앱을 만들어 보십시오.
- 테스트를 실행해 보십시오.
- 이전 두 단계가 성공한 경우 생성한 비밀 토큰을 사용하여 Netliify에 앱을 배포하고 저장소에 저장하십시오.

변경 내용을 GitHub에 커밋하고 푸시한 다음 리포지토리의 작업 탭으로 이동합니다. 워크플로가 실행 중인 것을 확인해야 합니다. 작업이 완료되면 Netliify에서 사이트의 대시보드를 확인하여 배포가 성공적으로 게시되었는지 확인할 수 있습니다.

### 자동 병합 워크플로 생성

요청을 자동으로 병합할 때와 병합하지 않을 시기를 고려해야 하므로 이 워크플로를 구축하는 것이 이전 워크플로우만큼 간단하지 않습니다.

워크플로우 파일을 작성하기 전에, 우리를 대신하여 요청을 할 수 있는 작업 권한을 부여하기 위해 리포지토리에 하나의 암호를 더 만들어야 합니다. 이를 위해서는 GitHub에 대한 개인 접근 토큰이 필요합니다.

GitHub 계정의 액세스 토큰 페이지로 바로 이동하려면 이 링크를 클릭하고 "새 토큰 생성" 버튼을 클릭한 다음 "자동화"와 같은 설명적인 메모를 제공합니다.

범위 섹션에서 리포 및 워크플로 범위만 선택합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/repo-checkbox.png?resize=730%2C222&ssl=1)

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/workflow-checkbox.png?resize=730%2C186&ssl=1)

페이지를 나가기 전에 토큰 생성 버튼을 클릭하고 토큰을 복사합니다.

앱 리포지토리에 `개인_토큰`이라는 새 암호를 추가하고 코드 편집기로 돌아가십시오.

/workflows 폴더에서 `automerge.yml` 파일을 만들고 아래 조각 내용을 붙여넣습니다.

```undefined
name: Auto Merge
on:
  pull_request:
    branches: [master] # run on pull requests to master branch
jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2 # setup repository in runner
      - name: Setup Node.js # setup Node.js un runner
        uses: actions/setup-node@v1
        with:
          node-version: '12'
      - uses: actions/cache@v2
        with:
          path: ~/.npm
          key: ${ runner.os }-node-${ hashFiles('**/package-lock.json') }
          restore-keys: ${ runner.os }-node-
      - run: npm ci # install dependencies
      - run: npm run build --if-present # build project
      - run: npm run test:unit # run tests
  mergepal-merge: # merge when build and testing is successful
    runs-on: ubuntu-latest
    needs:
      - build-and-test
    steps:
      - uses: actions/checkout@v2
      - uses: maxkomarychev/merge-pal-action@v0.5.1
        with:
          token: ${secrets.PERSONAL_TOKEN}
```

이제 풀 요청을 자동으로 병합하는 워크플로를 성공적으로 생성했으므로 모든 작업이 의도한 대로 작동하도록 몇 가지 조건을 설정해야 합니다.

앱의 루트 폴더에 `.mergepal.yml`이라는 파일을 만들고 아래 조각에서 코드를 붙여넣습니다.

```cpp
whitelist:
  - good-to-go
blacklist:
  - wip
method: merge #available options "merge" | "squash" | "rebase"
```

방금 만든 파일은 작업흐름에 추가한 재사용 가능한 작업인 `mergepal`의 구성 파일입니다. 구성은 풀 요청을 병합해야 하는 시기와 방법을 결정합니다.

- GitHub의 여러 레이블을 나열하는 데 `white list`를 사용할 수 있으며, 이 경우 꺼내기 요청이 자동으로 병합됩니다.
- `블랙리스트` 옵션 아래에 추가된 레이블로 인해 꺼내기 요청이 자동으로 병합되지 않음
- `method`를 사용하면 GitHub에서처럼 풀 요청에 대한 병합 방법을 지정할 수 있습니다.

## GitHub에 리포지토리 밀어넣기

GitHub에 새 레이블을 만들고 로컬 리포지토리를 GitHub에 푸시하는 두 가지 추가 작업이 있습니다.

리포지토리의 Pull requests 탭으로 이동하여 다음과 같은 새 테이블을 만듭니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/pull-request-tab-new-table.png?resize=730%2C304&ssl=1)

마지막 단계는 코드 편집기로 돌아가서 프로젝트를 GitHub 저장소로 푸시하는 것입니다. 자동 배포 워크플로우가 트리거되었어야 합니다.

자동 병합을 테스트하려면 로컬에서 변경 사항이 있는 새 분기를 만들고 분기를 GitHub로 밀어넣습니다. 꺼내기 요청을 만들고 새로 만든 레이블 중 하나를 테스트한 후 제출합니다.

다음은 실행 중인 워크플로의 스크린샷입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/workflow-example.png?resize=730%2C445&ssl=1)

GitHub의 저장소에 액세스할 수 있습니다.