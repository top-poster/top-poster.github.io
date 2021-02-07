---
layout: post
title: "Vue 3는 RC에 있습니다. 여기 당신이 알아야 할 것이 있습니다."
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/new-features-in-vue-3.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/new-features-in-vue-3.png?fit=730%2C487&ssl=1)

오랫동안 기다려온 Vue 3가 드디어 출시 후보(RC) 단계에 접어들었는데, 이는 버그 수정과 개선만을 위한 새로운 기능이나 새로운 변경 사항이 구현되지 않는다는 것을 의미한다.

Vue가 마이그레이션 가이드와 함께 게시한 이 새 버전에 대한 업데이트된 문서도 있습니다. 여전히 베타 버전이지만, 두 가지 리소스 모두 읽기 쉽고 구성이 잘 되어 있어 시작할 수 있습니다.

Vue 3로 실험하는 것은 이전 버전을 사용하는 것만큼 쉽습니다. 도중에 버그나 문제가 발견되면 Vue 문제 도우미를 사용하여 보고할 수 있습니다.

이 가이드에서는 프레임워크의 현재 상태에 대해 설명하고 Vue 3을 사용하는 데 도움이 되는 몇 가지 팁과 기본 지식을 제공합니다. 다음 내용을 다룹니다.

- 성과
- 나무 흔들기 지지대
- 구성 API
- 텔레포트
- 단편들
- 향상된 TypeScript 지원
- 기타 중단 변경 사항
- 실험 특징
- Vue 2에서 마이그레이션
- Vue 3 시작하기

시작해 봅시다!

## 성과

Vue 3의 하이라이트 중 하나는 퍼포먼스이다. 전체적으로 Vue 3는 약 55%, 업데이트 속도는 최대 133%, 메모리 사용량은 54%로 감소했습니다. 이 작업은 TypeScript를 사용하여 DOM 구현을 완전히 다시 쓰는 방식으로 수행됩니다.

기타 성능 향상 기능으로는 다음이 있습니다.

- 보다 효율적인 구성요소 초기화
- SSR가 2-3배 더 빠름
- 컴파일러에 대한 빠른 경로
- 업데이트 성능이 1.3-2배 빠름

## 나무 흔들기 지지대

트리 쉐이킹(Tree Shakeing)은 사용되지 않거나, 사용되지 않거나, 사용되지 않는 코드를 제거하여 앱의 빌드 크기를 줄이는 프로세스입니다.

Vue 3의 빌드 크기는 13.5kb입니다. Composition API 지원을 제외한 모든 항목을 제거하면 11.75kb로 슬림화됩니다.

모든 종소리와 휘파람을 불어도 Vue 3는 22.5kb에 불과해 나무 흔들림으로 인해 모든 기능이 포함된 Vue 2보다 여전히 가볍다.

## 구성 API

이것은 Vue의 가장 큰 변화 중 하나입니다. 완전히 부가적이며 이전 옵션 API에 대한 변경 사항을 제공합니다. Composition API가 발전하는 동안 Options API는 계속 지원됩니다.

```js
// src/components/UserRepositories.vue

export default {
  components: { RepositoriesFilters, RepositoriesSortBy, RepositoriesList },
  props: {
    user: { type: String }
  },
  setup(props) {
    console.log(props) // { user: '' }

    return {} // anything returned here will be available for the rest of the component
  }
  // the "rest" of the component
}
```

## 텔레포트

`텔레포트`는 많은 DOM 부모들과 함께 HTML을 어디에 렌더링할지 결정할 수 있는 새로운 Vue 3 기능입니다. 이제 전역 상태나 다른 구성 요소를 만들지 않고도 DOM의 다른 위치에 HTML을 렌더링할 수 있습니다.

```coffeescript
app.component('modal-button', {
  template: `
    &lt;button @click="modalOpen = true"&gt;
        Open full screen modal! (With teleport!)
    &lt;/button&gt;

    &lt;teleport to="body"&gt;
      &lt;div v-if="modalOpen" class="modal"&gt;
        &lt;div&gt;
          I'm a teleported modal! 
          (My parent is "body")
          &lt;button @click="modalOpen = false"&gt;
            Close
          &lt;/button&gt;
        &lt;/div&gt;
      &lt;/div&gt;
    &lt;/teleport&gt;
  `,
  data() {
    return { 
      modalOpen: false
    }
  }
})
```

이 버튼을 클릭하면 Vue는 본문 태그 안의 모달 내용을 본문 태그의 자식처럼 렌더링합니다.

## 단편들

Vue 3은 이제 조각이라고 하는 다중 루트 노드 구성 요소를 지원합니다. 이전 버전의 Vue에서는 지원되지 않습니다.

```undefined
&lt;!-- Layout.vue --&gt;
&lt;template&gt;
  &lt;header&gt;...&lt;/header&gt;
  &lt;main v-bind="$attrs"&gt;...&lt;/main&gt;
  &lt;footer&gt;...&lt;/footer&gt;
&lt;/template&gt;
```

## 향상된 TypeScript 지원

Vue 3의 코드베이스는 TypeScript로 작성되었습니다. 이렇게 하면 자동으로 최신 유형 정의를 생성, 테스트 및 번들할 수 있습니다.

Vue는 이제 JSX를 지원하며 클래스 구성 요소는 TypeScript에서 계속 지원됩니다. 즉, 전문 기술에 따라 TypeScript 또는 JavaScript를 사용하여 Vue 앱을 생성할 수 있습니다.

```undefined
interface Book {
  title: string
  author: string
  year: number
}

const Component = defineComponent({
  data() {
    return {
      book: {
        title: 'Vue 3 Guide',
        author: 'Vue Team',
        year: 2020
      } as Book
    }
  }
})
```

## 기타 중단 변경 사항

Vue 3에서는 하나의 기사에서 적절히 다루기에는 너무 많은 변화들을 도입한다. 다음 사항에 대한 자세한 내용은 공식 문서를 참조하십시오.

- 이제 글로벌 및 내부 API에서 트리 흔들기 지원
- 렌더 함수 API 변경됨
- 이제 Global Vue API가 애플리케이션 인스턴스를 사용합니다.
- 기능 구성 요소는 일반 기능만 사용하여 생성됩니다.
- SFC(Single-File Component)의 `function` 특성이 더 이상 사용되지 않습니다.
- 모델 구성 요소 옵션과 v-bind의 동기화 수식어가 v-model 인수를 위해 제거됩니다.
- 비동기 구성요소를 생성하려면 `DefineAsyncComponent` 메서드가 필요합니다.
- 구성 요소 데이터 옵션을 항상 함수로 선언하는 것이 좋습니다.
- 범위 지정 슬롯 속성은 "$슬롯"으로 대체됩니다.
- 새로운 속성 강제화 전략

## 실험 특징

Vue 3는 Options API에 대한 여러 가지 우수한 기능과 향상된 기능을 제공하며 몇 가지 실험적인 기능도 제공합니다. 개발 단계에서 이러한 기능을 사용하여 Vue 핵심 팀에 피드백을 보낼 수 있습니다.

### 서스펜스

아직 실험 단계임에도 불구하고 서스펜스는 필수적인 기능입니다. 조건이 충족되면 폴백 콘텐츠를 렌더링하는 구성 요소로 작용하며 컨텐트를 조건부로 렌더링하는 데 사용됩니다.

`suspense`는 API 호출을 통해 내용을 완전히 로드하거나 처리 중에 로드 메시지를 표시하려는 경우에 유용합니다.

```undefined
<Suspense>
  <template >
    <Suspended-component />
  </template>
  <template #fallback>
    Loading...
  </template>
</Suspense>
```

위의 예는 API 요청을 처리하기 위해 suspense 및 fallback을 사용하는 간단한 예입니다.

### <스크립트 설정>

기존 방식:

```xml
<script>
import { ref } from 'vue'
export default {
  setup() {
    const variable = ref(0)
    const inc = () => variable.value++
    return {
      variable,
      inc,
    }
  },
}
</script>
```

새로운 방법:

```xml
<script setup>
  import { ref } from 'vue'

  export const count = ref(0)
  export const inc = () =&gt; count.value++
</script>
```

설정 방식을 사용하지 않으면 모든 것이 작동하며 코드가 더 간결해진다.

### <선발>

이것은 또한 Vue 3에서 소개된 매우 유용한 기능으로, 개발자들이 우리의 `스타일` 선언에 포함된 구성요소 상태를 참조할 수 있게 한다. 가장 기본적인 예는 사후 대응 속성을 선언하고 <스타일> 태그 내에서 액세스하는 것입니다.

```xml
<template>
  <div class="text-color">This is my text </div>
<template>

<script>
  export default {
    data() {
      return {
          color: 'green'
      }
    }
  }
</script>

<style vars="{ color }">
  .text-color {
    color: var(--color);
  }
</style>
```

vars 및 var 속성을 사용하여 반응형 값을 가진 CSS 속성을 선언할 수 있습니다.

## Vue 2에서 마이그레이션

Vue 2로 작성된 프로젝트가 있다면 마이그레이션이 어떻게 처리되는지 궁금할 수 있습니다. Vue 팀은 Vue 3과 역호환되는 Vue 2에 대한 마지막 업데이트를 발표할 것입니다. 또한 팀에서는 앱이 중단되는 변경 사항에 대한 감가상각 경고도 추가합니다. 이 LTS(장기 지원) 버전은 18개월 동안 지속되며, 이는 보안 업데이트를 계속 사용할 수 있다는 것을 의미합니다.

아직 베타 버전인 마이그레이션 가이드에는 Vue 3용 호환성 빌드 및 명령줄 마이그레이션 도구가 포함되어 있어 자동 마이그레이션에 도움이 되고 필요한 경우 힌트를 제공합니다.

## Vue 3 시작하기

```undefined
npm init vite-app hello-vue3


const HelloVueApp = {
  data() {
    return {
      message: 'Hello Vue!!'
    }
  }
}

Vue.createApp(HelloVueApp).mount('#hello-vue')
//--------------------------------------------------

&lt;div id="hello-vue" class="demo"&gt;
  { message }
&lt;/div&gt;
```

## 결론

본 튜토리얼에서는 Vue 3와 함께 소개된 새로운 기능과 새로운 변화에 대해 살펴 보았습니다. 또한 "vite" 라이브러리를 사용하여 Vue 3 프로젝트를 시작하는 방법과 이전 Vue 2 프로젝트를 Vue 3으로 마이그레이션하는 방법을 시연했습니다.