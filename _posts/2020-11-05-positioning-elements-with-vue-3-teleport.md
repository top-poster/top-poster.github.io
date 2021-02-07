---
layout: post
title: "Vue 3 텔레포트를 사용한 위치 지정 요소"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/vue3teleport.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/vue3teleport.png?fit=730%2C487&ssl=1)

Vue 3에는 텔레포트라는 흥미로운 새로운 기능이 있습니다. Vue 3의 초기 단계에서 이 기능은 Portals라고 불렸지만 Vue 팀은 결국 텔레포트라고 부르기로 결정했다. 텔레포트는 개발자들이 Vue 애플리케이션에서 요소들을 한 장소에서 다른 장소로 옮기는 것을 가능하게 한다.

이것이 왜 필요한지 궁금하다면, 올바른 질문을 하는 것입니다. 모델, 알림 등의 구성 요소를 사용하는 경우 DOM에서 해당 구성 요소의 위치가 중요하다는 것을 알 수 있습니다. 예를 들어, 깊게 중첩된 요소 내에서 모달을 사용하는 경우 상위 요소의 CSS 속성은 모달의 스타일에 영향을 미칩니다.

이러한 동작은 애플리케이션의 다른 부분에서 모델과 같은 구성 요소를 호출할 수 있고, 우리는 모델 위치와 스타일을 일관성 있게 유지하기를 원하기 때문에 문제가 있습니다. 그것이 텔레포트가 우리를 위해 하는 일입니다. 이를 통해 우리는 정확히 DOM에서 모달 또는 HTML의 다른 부분을 렌더링할 위치를 지정할 수 있습니다. 글로벌 상태를 관리하거나 모달에 대한 새로운 구성요소를 만들 걱정 없이 말입니다.

## 작동 방식

`telport` 태그는 요소를 텔레포트할 DOM의 위치를 지정하는 `to` 특성을 사용합니다. 다른 응용 프로그램의 UI 구성 요소에 대한 간섭을 방지하려면 이 대상이 구성 요소 트리 외부에 있어야 합니다. 이러한 이유로 Vue 팀은 공용 디렉토리의 `index.html` 파일에서 본문 태그 아래에 배치할 것을 권장합니다.

## 전제조건

이 튜토리얼은 Vue 3에 대한 소개가 아니고 Vue 3의 텔레포트 기능에 초점을 맞추고 있기 때문에 Vue 3의 기본 사항에 대해서는 다루지 않겠습니다.

## Vue 앱 만들기

먼저 아래 명령을 사용하여 Vue CLI v4.5의 최신 버전을 설치해야 합니다.

```coffeescript
yarn global add @vue/cli@next
#OR
npm install -g @vue/cli@next
```

Vue 응용 프로그램을 만드는 프로세스는 변경되지 않았습니다. Vue CLI가 설치되어 있는 경우 아래 명령을 실행하여 `vueleport`라는 새 Vue 프로젝트를 생성합니다.

```undefined
vue create vueteleport
```

터미널 프롬프트에 따라 Vue 3 사전 설정을 선택하여 설정 프로세스를 완료합니다. 이 데모에 대한 모달을 생성하려면 `src/public/index.html` 파일을 아래 조각으로 업데이트하십시오.

```xml
<!-- src/public/index.html -->
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work
        properly without JavaScript enabled. Please enable it to
        continue.
      </strong>
    </noscript>
    <div id="app"></div>
    <div id="my-modal" style="border: green 3px solid"></div>
  </body>
```

여기서 우리가 한 일은 `마이모달`이라는 ID를 가진 div를 정의하는 것이다. 모든 프로젝트 파일이 들어 있는 앱 컨테이너 바로 아래에 렌더링됩니다. 여러분은 우리가 `my-modal` div 요소를 업데이트하여 약간의 색 테두리를 제공하는 것을 볼 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/vueteleporthomepage.png?resize=1962%2C1308&ssl=1)

녹색 수평선은 my-modal div 요소의 위치를 나타냅니다. Vue 구성 요소 내에서 ID CSS 선택기(예: `my-modal`)를 사용하여 요소를 `my-modal` div 요소로 `텔레포트`할 수 있다. 방금 지정한 대상으로 콘텐츠를 텔레포트하는 방법을 시연하기 위해 `HelloWorld.vue` 파일을 열고 간단한 모달을 만들어 보겠습니다.

```xml
<!-- src/components/HelloWorld.vue -->
<template>
  <div class="hello">
    <h1>{ msg }</h1>
        <button @click="showModal = true">Open Modal</button>
      <div v-if="showModal" id="myModal">
        <div class="modal-content">
          <span @click="showModal = false">&times;</span>
          <h3>This is the title</h3>
          <p>This is the awesome content</p>
        </div>
      </div>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      showModal: false,
    };
  },
};
</script>
```

이 시점에서 앱을 실행하면 다음과 같이 브라우저에 모달 열기 버튼이 나타납니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/openmodalbutton.png?resize=1964%2C1310&ssl=1)

그러나 브라우저에서 Modal 열기 버튼을 검사하면 해당 버튼이 앱 컨테이너에 깊이 중첩되어 있음을 알 수 있습니다. app의 ID를 가진 컨테이너 요소에서 hellow world의 ID를 가진 div 요소로 깊숙이 2단계까지 이동한다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/dividsayinghelloworld.png?resize=730%2C488&ssl=1)

앞서 우리는 콘텐츠를 텔레포트할 `index.html` 파일에 `my-modal` div 요소를 제공했다는 사실을 기억하라. 이 모달에 순간이동하기 위해 Vue의 <telport> 태그 안에 우리가 만든 모달들을 싸서 그것에 `to` 속성을 전달한다. 이 속성은 대상을 가리킵니다. 즉, 우리가 텔레포트로 이동하는 위치를 나타내는 식별자를 나타냅니다. 이 경우 할당한 CSS ID 선택기를 사용하여 div 요소를 식별하며 `my-modal`입니다.

```xml
<!-- src/components/HelloWorld.vue -->
<template>
  <div class="hello">
    <h1>{ msg }</h1>
    <teleport to="#my-modal">
      <button @click="showModal = true" >Open Modal</button>
      <div v-if="showModal" id="myModal">
        <div class="modal-content">
          <span @click="showModal = false">&times;</span>
          <h3>This is the title</h3>
          <p>This is the awesome content</p>
        </div>
      </div>
    </teleport>
  </div>
</template>
```

우리가 한 일은 뷰의 <텔레포트> 안에 있는 모달들을 싸고 모달들을 텔레포트로 이동시키는 위치를 가리키는 `to` 속성을 전달하는 것이다. 이 경우, 우리는 `index.html` 파일에 제공했던 `my-modal` div 요소로 순간이동한다.

브라우저에서 다시 확인해 보면, 우리는 이전 단계에서 경험했던 딥 중첩이 더 이상 존재하지 않으며 모형이 이 애플리케이션의 루트로 텔레포트되었음을 확인해야 한다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/mymodaldiv.png?resize=730%2C488&ssl=1)

여기서는 텔레포트 기능을 사용하여 이 응용 프로그램의 어디에서나 호출할 수 있는 완전히 분리된 모달 구성 요소를 구축했습니다. 여기 실행 중인 모형이 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/modalcomponentinaction.gif?resize=600%2C401&ssl=1)

텔레포트가 중요한 게 아냐 여러 소스에서 요소를 동일한 루트로 텔레포트하는 등의 추가 작업을 수행할 수 있습니다. 즉, 텔레포트할 다른 모든 요소의 `to` 속성에 대한 값을 지정하여 여러 요소를 동일한 `my-modal` 요소로 텔레포트할 수 있습니다.

텔레포트의 또 다른 중요한 특징은 부모로부터 CSS를 물려받지 않는다는 것입니다. 이렇게 하면 상위 구성요소와 완전히 독립적이며 지정된 컨텍스트 내에서 범위를 지정할 수 있습니다.

### 여러 텔레포트 콘텐츠를 동일한 대상에 마운트합니다.

위의 예에서는 HelloWorld 구성 요소에서 모달리티를 텔레포트하여 애플리케이션의 루트 index.html 파일에 렌더링하였다. 앱 내 다른 위치에 동일한 대상으로 순간이동하고자 하는 요소가 있다면 어떨까요? 네, 그렇게 하는 것이 가능합니다. 새 `src/components/Words.vue` 파일을 생성하고 아래 조각으로 업데이트해 보겠습니다.

```xml
<!-- src/components/Words.vue -->
<template>
  <h1 class="center">Hi, I am a text from the Words component</h1>
</template>
<script>
export default {
  name: "Words",
};
</script>
<style >
.center {
  text-align: center;
  border: 3px solid green;
}
</style>
```

간단하지만 이 구성 요소는 녹색 테두리가 있는 div 요소로 둘러싸인 텍스트를 표시합니다. 브라우저에서 이 구성 요소를 단독으로 렌더링하면 다음과 같은 출력이 생성됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/wordscomponent.png?resize=730%2C488&ssl=1)

이제 우리는 이 콘텐츠를 기존의 `마이모달` 타겟으로 순간이동시켜 같은 루트에 있는 모달과 함께 보여주는 것이다. 이를 위해 `앱`을 업데이트하겠습니다.vue`는 아래 조각과 함께 제공됩니다.

```xml
<!-- src/App.vue -->
<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Vue 3 Teleport Demo" />
  <teleport to="#my-modal">
    <Words />
  </teleport>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import Words from "./components/Words.vue";
export default {
  name: "App",
  components: {
    HelloWorld,
    Words,
  },
};
</script>
</style>
```

여기서 우리가 한 일은 워드 구성 요소를 앱 뷰에 가져와서 모달에 대해 앞에서 지정한 my-modal 타겟으로 순간이동시키는 것이다. 이제 브라우저에서 워드 구성 요소를 검사하면 모달과 동일한 대상으로 텔레포트되었음을 확인해야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/h1center.png?resize=730%2C487&ssl=1)

이것이 바로 텔레포트 기능을 통해 Vue의 동일한 대상에 여러 콘텐츠를 탑재할 수 있게 하는 방법입니다. Vue 텔레포트를 사용하면 응용 프로그램의 모든 구성 요소 내에서 레이아웃 요소를 제어할 수 있습니다. 확장자별로 텔레포트를 사용하여 프로그램의 머리말 또는 바닥글 내용을 프로그램의 모든 구성 요소에서 업데이트할 수 있습니다. 이 특정 사용 사례는 응용프로그램에 복잡성을 가져올 수 있으므로 주의하여 사용해야 합니다.

## 텔레포트 소품

- `to` — `<텔레포트` 내의 모든 콘텐츠가 렌더링될 대상 요소를 지정하는 필수 소품입니다. `to` 소품의 값은 기존 CSS `class` amisid 또는 `HTMLEment`를 식별하는 유효한 쿼리 선택기여야 합니다.

```xml
<!-- ok -->
<teleport to="#some-id" />
<teleport to=".some-class" />
<teleport to="[data-teleport]" />

<!-- Wrong -->
<teleport to="h1" />
<teleport to="some-string" />
```

- `사용 안 함` — 이름에서 알 수 있듯이 이 소품은 텔레포트 기능을 사용하지 않는 데 사용되는 부울입니다. true로 설정하면 `teleport`의 내용이 지정된 위치에 렌더링됩니다. 따라서 외부 대상 또는 위치로 "텔레포트"되지 않습니다. 이전 예제를 사용하여 모달에 대해 텔레포트를 비활성화하려면 다음과 같이 `비활성화` 프로포트를 추가할 수 있습니다.

```xml
<!-- src/components/HelloWorld.vue -->
<teleport to="#my-modal" :disabled="true">
  <button @click="showModal = true" >Open Modal</button>
  <div v-if="showModal" id="myModal">
    <div class="modal-content">
      <span @click="showModal = false">&times;</span>
      <h3>This is the title</h3>
      <p>This is the awesome content</p>
    </div>
  </div>
</teleport>
```

콘텐츠를 지정된 CSS 선택기가 아닌 `본문`으로 직접 텔레포트할 수 있다는 점에 유의해야 한다. 예는 다음과 같습니다.

```xml
<!-- src/components/HelloWorld.vue -->
<teleport to="#body">
  <button @click="showModal = true">Open Modal</button>
  <div v-if="showModal" id="myModal">
    <div class="modal-content">
      <span @click="showModal = false">&times;</span>
      <h3>This is the title</h3>
      <p>This is the awesome content</p>
    </div>
  </div>
</teleport>
```

## 결론

이 튜토리얼에서는 새로운 Vue 3 텔레포트 기능에 대해 살펴봤습니다. 이 기능의 기본 개념을 소개하고 모달 사용 사례를 시연했습니다. 텔레포트 API를 더 잘 이해하기 위해 텔레포트 수용 소품 및 이를 사용하여 이 기능의 기능을 수정하는 방법에 대해 설명하였다. Vue Telepot에 대해 더 자세히 알아보려면 여기에서 언제든지 공식 문서를 확인하십시오.