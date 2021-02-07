---
layout: post
title: "Vue.js에서 가상 DOM의 작동 방식"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/how-virtual-dom-works-vue-js.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/how-virtual-dom-works-vue-js.png?fit=730%2C487&ssl=1)

## 도입

Vue.js는 빠른 로딩 속도와 쉬운 확장성으로 알려진 자바스크립트 프레임워크이다. 일부 기능은 가상 DOM을 사용하여 필요에 따라 실제 DOM을 업데이트하는 데 직접 연결할 수 있습니다.

가상 DOM을 이해하는 것은 Vue.js를 학습하기 위한 요구 사항은 아니지만, 확실히 초보 Vue 개발자들이 뒤에서 일어나는 몇 가지 일들을 이해하기 시작하는 데 도움이 될 수 있다.

## DOM 자체

```xml
<!DOCTYPE html>
<html>
<head>
<title>Page Title</title>
</head>
<body>
<h1>This is a Heading</h1>
<p>This is a paragraph.</p>
</body>
</html>
```

Vue.js 가상 DOM에 대해 이야기하기 전에, DOM 자체가 무엇인지 이해하는 것이 중요하다. DOM(Document Object Model)은 모든 마크업 언어(HTML)를 연결된 노드로 처리하는 인터페이스의 일종이다. 트리와 같은 구조로 저장된 마크업 요소를 위한 객체 인터페이스로 볼 수 있다.

DOM은 우리가 요소들의 스타일을 쓰고 바꿀 수 있게 하고 요소들 자체도 바꿀 수 있게 해줍니다. 이 작업은 문서의 헤드와 본문에 요소 태그 또는 CSS 스타일을 추가, 수정 또는 삭제하는 방식으로 수행됩니다. 이러한 태그는 노드와 개체로 표시되기 때문입니다. HTML DOM은 이렇게 작동합니다. Vue가 다른 DOM을 사용하는 이유는 무엇입니까?

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/wireframe-html-dom.png?resize=730%2C464&ssl=1)

### 기존 DOM이 충분하지 않은 이유

그래서 DOM으로 모든 것이 좋고 좋습니다. 그러나 애플리케이션이 성장함에 따라, 즉 이동할 노드, 더 많은 요소 및 이러한 요소와 통신할 수 있는 스크립트가 증가하면, DOM은 속도가 느려지고 처리 능력이 많이 소모됩니다.

검색을 수행하거나 심지어 DOM을 업데이트하려고 하는 것은 어려운 작업이 되고, 궁극적으로 응용 프로그램의 성능에 영향을 미칩니다. 이것이 가상 DOM이 생성된 이유입니다.

## Vue.js 가상 DOM

Vue.js 팀은 가상 DOM을 전통적인 DOM의 일종의 추상화처럼 만들기 위해 구축했다; 그것은 HTML DOM의 "lite" 버전이지만 초능력을 가지고 있다. 가상 DOM은 더 스마트하므로 기존 DOM보다 더 효율적인 방법을 찾을 수 있습니다.

이 작업을 수행하는 주된 방법은 문서에 대한 새로운 변경 또는 업데이트 후 전체 DOM을 다시 렌더링하지 않도록 하기 위한 다양한 확산 알고리즘을 통해서이다. 그것만으로도 DOM API 호출 빈도가 낮기 때문에 효율성과 리소스 관리가 크게 향상됩니다.

이 설명에 의한 가상 DOM은 실제 DOM과 Vue 인스턴스 사이에 위치합니다.

```coffeescript
new Vue({
 el: '#app',
 data: {
   text: 'hi LogRocket'
 },
 render(createElement) {
   return createElement(
    'h1',
    { attrs: { id: 'headers' } },
  this.text
  );
 }
});
```

렌더링 시 아래 요소를 반환합니다.

```xml
<div id=’app’>
 <h1 id=’headers’>hi LogRocket</h1>
</div>
```

Vue 가상 DOM은 Vue 인스턴스를 확장하는 JavaScript 개체인 Vue 구성 요소로 이루어져 있습니다. Vue는 실제 DOM에 비해 속도와 효율성이 크게 다르기 때문에 가상 DOM을 사용합니다. 가상 DOM은 실제 DOM보다 크기가 작기 때문에 매우 효율적입니다.

### 작동 방식

조건문이 있는 양식을 사용하여 가상 DOM의 작동 방식을 살펴보겠습니다.

```js
<form>
 <div>
 <div class=”form-group” :class=”{‘form-group — error’: $v.name.$error }”>
 <label class=”form__label”>Full Name</label>
 <input class=”form__input” v-model.trim=”$v.name.$model”/>
 </div>
  <span class=”error” v-if=”!$v.name.required”>Field is required</span>
 </div>
</form>
```

위의 코드 블록에 이름이 입력되지 않았을 때 오류 클래스를 나타내는 v-if 문이 있는 `span`이 어떻게 있는지 주목하십시오. 이 코드에서는 상자에 "필수 필드"를 나타내는 이름을 입력하지 않으면 작은 범위 줄이 나타납니다. v-if 조건이 true를 반환할 경우 가상 DOM의 변경 사항은 이 `span` 요소를 조건부로 추가하는 것입니다.

이 줄을 읽기 전에 실제 DOM과 가상 DOM이 동일하게 읽힙니다. 이 조건이 충족된 후에는 두 상태(실제 DOM과 가상 DOM에서) 간에 차이가 발생하며, 이 차이에서 변경 사항을 다시 렌더링하지 않고 실제 DOM에 적용할 변경 사항 패치를 출력합니다. 이러한 방식으로 두 DOM은 즉시 동일한 DOM으로 돌아갑니다.

### 가상 DOM을 HTML 요소에 마운트하는 방법

몰랐을 수도 있지만 Hello world와 같은 새 프로젝트에 대해 Vue.js 명령을 실행할 때마다 다음과 같은 문제가 발생합니다.

```undefined
vue create hello-world
```

기본적으로 Vue의 가상 DOM을 이미 사용하고 있으며, `main.js` 파일로 이동할 때 이를 확인할 수 있습니다. 이 코드는 아래의 이 코드 블록과 다소 유사해야 합니다.

```coffeescript
import Vue from 'vue'
import App from './App'

new Vue({
  el: '#app',
  components: { App }
});
```

여기서 요소는 앱의 앱 구성 요소인 앱 ID를 가진 모든 요소임을 알 수 있습니다.vue 파일 구성요소 내에서 `el` 옵션을 사용하여 원하는 요소를 특정 대상으로 지정할 수 있으며, 해당 요소는 HTML DOM에 마운트됩니다.

## 몇 가지 교훈

Vue.js에서 가상 DOM으로 배후에서 어떤 일이 벌어지는지 엿볼 수 있어 신선하다. 또한 우리가 쓰는 모든 코드가 가상 DOM에 의해 구문 분석되고, 명령어나 템플릿 섹션 내의 이벤트와 같은 것들이 실제 DOM에서 그렇게 끝나지 않는다는 것을 인식하는 것이 중요하다.

Vue는 이러한 툴을 사용하여 처리하며, 생성된 패치가 실제 DOM에 도달합니다. 브라우저에서 앱을 쉽게 검사하여 지침이 표시되지 않는지 확인할 수 있습니다.

## 결론

이 게시물은 Vue.js에서 가상 DOM이 작동하는 방식에 대한 개요이며, 따라야 할 몇 가지 그림 및 하나를 마운트하는 방법에 대한 설명입니다. 이 글을 읽고 나서 여러분이 뷰 프로젝트를 만들 때마다 매일 사용하는 것들의 개념에 대해 감사해했으면 하는 것이 제 바람입니다. 해피 해킹!