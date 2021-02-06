---
layout: post
title: "Nuxt의 Composition API를 사용하여 서버측 애플리케이션 처리"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/handlingserversideappswithnuxtscompositionapi.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/handlingserversideappswithnuxtscompositionapi.png?fit=730%2C487&ssl=1)

최근 생태계에 Vue의 Composition API 채택이 증가하고 있다. 구성요소가 성장함에 따라 유지관리할 수 있는 기능과 구성요소 재사용을 장려하는 기능은 많은 대형 프로젝트에서 유용합니다. 서버측 렌더링으로 인해 Nux로 작성된 프로젝트는 Vue의 Composition API를 사용하는 데 어려움을 겪고 있습니다. Nuxt 팀은 이를 관찰하고 Nuxt로 작성된 프로젝트에 대한 구성 API를 작성하였다. 이 기사에서는 Nux Composition API의 작동 방식, Vue의 Composition API에 대한 활용도 및 프로젝트에 사용하는 모범 사례에 대해 살펴보겠습니다.

## 시작 중

프로젝트에 Nuxt의 Composition API를 추가하려면 플러그인을 설치하는 것처럼 설치하십시오.

```undefined
npm install @nuxtjs/composition-api --save

/*  For Yarn users */

yarn add @nuxtjs/composition-api
```

이 작업이 성공적으로 완료되면 Nux 구성 파일에서 모듈을 활성화합니다.

```css
{
  buildModules: [
    '@nuxtjs/composition-api'
  ]
}
```

## 응용 프로그램 데이터 처리

Nux 프로젝트에서 Vue의 Composition API를 사용할 때 사용자가 직면하는 주요 문제는 애플리케이션과 서버 간의 통신 중에 데이터가 반환되지 않는 경우입니다. API에서 검색하여 Nux 앱에 표시할 데이터가 있다고 가정해 보겠습니다.

```js
import { ref } from '@nuxtjs/composition-api'

function useTodo () {
  const todo = ref({})

  const fetchPost = async (id) => {
    fetch('https://gorest.co.in/public-api/todos/' + user_id)
      .then(response => response.json())
      .then(json => post.value = json)
  }

  return {
    todo,
    fetchTodo
  }  
}
```

Nux 애플리케이션에서 useTodo 기능을 가져오고 사용하려고 하면 데이터를 가져오는 기본 작업이 비동기적이면서 기본적으로 서버측 구현이 동기적으로 실행되기 때문에 API에서 검색된 데이터가 서버측으로 이동하지 못한다. 따라서 아래 코드 샘플은 빈 div를 반환합니다.

```xml
<template>
  <div>{ data.title }</div>
</template>

<script>
import { useTodo } from '@/composables'

export default defineComponent({
  setup (props, { root }) {
    const { todo, fetchTodo } = useTodo($root.route.params.user_id)

    fetchTodo()

    return { todo }
  }
})
</script>
```

Nuxt의 Composition API는 서버 측에서 데이터를 가져와 클라이언트 측에 중계하는 데 사용할 수 있는 useFetch라는 래퍼를 제공한다. 서버 측에서 한 번만 탐색하고 결과 데이터가 클라이언트 측 코드로 검색되어 초기 상태로 사용되도록 하기 때문에 여러 비동기 네트워크 호출을 방지하는 데 유용한 역할을 한다. 모의 REST API 샘플을 사용하여 `useFetch` 래퍼로 데이터를 검색하는 방법을 살펴보겠습니다.

```js
import { defineComponent } from '@nuxtjs/composition-api'
import { ref, useFetch } from '@nuxtjs/composition-api'

function useTodo (id) {
  const todo = ref({})

  const { fetch: fetchTodo } = useFetch(async () => fetch('https://gorest.co.in/public-api/todos/' + user_id)
    .then(response => response.json())
    .then(json => todo.value = json)
  )

  return {
    post,
    fetchTodo
  }  
}
```

위의 코드 블록에서 `useFetch`는 비동기 호출에 사용되는 가져오기 기능인 `fetchTodo`를 반환합니다. 이 통화가 처음 이루어지면 서버측 코드는 다음 작업관리 목록을 포함합니다.

```coffeescript
<div key="0">
  Aggredior the testimony of the multitude, therefore I will restore the color of whic   h I see.
</div>
```

## Next 컨텍스트를 사용하여 추가 모듈 액세스

Composition API는 또한 상점, 경로 또는 환경 변수와 같은 NuxT 컨텍스트에서 제공하는 속성에 액세스할 수 있는 기능을 제공합니다. 이것은 `useContext`라는 래퍼를 통해 이루어지며, 이 래퍼는 Nux 모듈 속성에도 액세스할 수 있다. 컨텍스트에서 nuxjs/axios 모듈을 사용하여 요청을 만드는 방법을 살펴보겠습니다. 먼저 다음에서 속성에 액세스하고자 하는 모듈을 설치합니다.

```coffeescript
npm install @nuxtjs/axios
```

다음으로 Nux 구성 파일에 추가합니다.

```js
// nuxt.config.js

export default {
  modules: [
    '@nuxtjs/axios',
  ],

  axios: {
    // proxy: true
  }
}
```

그런 다음 모듈을 사용하기 위해 `$axios` 속성을 호출한다. 이전 `fetchTodo` 예제의 기본 `fetch` 메서드를 `@nux/axios`로 바꿔 보겠습니다.

```undefined
import { defineComponent } from '@nuxtjs/composition-api'
import { ref, useFetch, useContext } from '@nuxtjs/composition-api'

function useTodo (id) {
  const todo = ref({})
  const { @axios } = useContext()

  const { fetch: fetchTodo } = useFetch(async () =>
      todo.value = await $axios.$get('https://gorest.co.in/public-api/todos/' + id)

  return {
    post,
    fetchTodo
  }  
}
```

## 헤더 및 HTML 특성 업데이트

Composition API는 응용 프로그램의 헤더 속성 및 메타 태그와 직접 상호 작용할 수 있는 `use meta() 후크를 제공합니다. useMeta()를 사용하면 헤더의 상태를 하나의 구성 요소로만 수정하고 제한할 수 있습니다. 이 기능이 어떻게 작동하는지 살펴보겠습니다.

```coffeescript
<template>
  <div>
    Welcome Back
    <nuxt />
  </div>
</template>

import {
  defineComponent,
  useContext,
  useMeta,
  watchEffect,
} from "nuxt-composition-api";

export default defineComponent({
  name: "Home",
  head: {},
  setup() {
    const { route } = useContext();
    const { title } = useMeta();
    watchEffect(() => {
      title.value = route.value.path;
    });
  },
});
</script>
```

위의 코드 샘플에서는 useMeta() 태그를 사용하여 헤더 값을 "useContext" 래퍼에 의해 정의된 경로로 설정합니다. 경로를 정의하고 헤더를 개인화함으로써 한 단계 더 나아갈 수 있습니다.

```xml
// StudentProfile.vue

<template>
  <div>
    Student's Profile
    <nuxt-link to="/Grades">Grades</nuxt-link>
  </div>
</template>
<script lang="ts">
import { defineComponent, useMeta } from "nuxt-composition-api";
export default defineComponent({
  name: "StudentProfile",
  head: {},
  setup() {
    const { title } = useMeta();
    title.value = "Here is your profile";
  },
});
</script>
```

`useMeta` 태그를 통해 `title.value`에 할당된 값은 보고 있는 경로에 대한 헤더를 정의합니다. 또한 use Meta 태그가 작동하려면 head 속성을 선언하는 것이 중요합니다.

```xml
// Grades.vue

<template>
  <div>
    Grades
    <nuxt-link to="/StudentProfile">Student's Profile</nuxt-link>
  </div>
</template>
<script lang="ts">
import { defineComponent, useMeta } from "nuxt-composition-api";
export default defineComponent({
  name: "Grades",
  head: {},
  setup() {
    const { title } = useMeta();
    title.value = "View your grades here";
  },
});
</script>
```

브라우저에서는 다음과 같이 표시됩니다.

## Composition API를 통한 성능 주의사항

Nuxt의 Composition API는 팩트 확인이 엄격하기 때문에 사용 시 준수해야 할 규칙이 있습니다. 글로벌 컴포넌트 안에 적용할 때 use Async나 ssrRef와 같은 도우미 기능과 함께 고유 키를 사용하도록 하는 것이 그 규칙 중 하나다. 관련 기능에 고유 키를 부과하지 않으면 상수나 변수가 할당되지 않은 값을 채택할 수 있습니다. 아래의 코드 샘플을 살펴보십시오.

```js
function useTodo() {

// Only one unique key is generated, no matter how many times this function is called.

    const todo = ssrRef('')

    return todo
}

const a = useTodo()
const b = useTodo()

b.value = 'Buy Gas'
```

위의 코드 샘플에서 고유 키가 설정되지 않은 경우, a의 값도 클라이언트측에서 "가스 구입"으로 초기화된다. 이 문제는 `사용 작업` 기능에 키가 할당된 경우 방지할 수 있습니다.

```js
function useTodo(path: string) {
    const task = useAsync(
        () => fetch(`https://api.com/slug/${path}`).then(r => r.json()),
        path,
    )

    return {
        task
    }
}

export default useTodo
```

## 결론

Nuxt의 Composition API는 서버측 애플리케이션의 구성을 보다 원활하게 처리할 필요가 있기 때문에 만들어졌습니다. 아직 도입 초기 단계이지만, Vue의 Composition API와 유사한 성공을 거두는 것은 놀라운 일이 아닙니다. 이 게시물의 예에 대한 코드 데모를 살펴봐야 하는 경우 여기에서 확인할 수 있습니다.