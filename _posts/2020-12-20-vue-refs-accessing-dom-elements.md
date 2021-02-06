---
layout: post
title: "Vue 참조 튜토리얼: Vue.js 앱에서 DOM 요소 액세스"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/07/vue-refs-access-dom-elements.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/vue-refs-access-dom-elements.png?fit=730%2C487&ssl=1)

이 튜토리얼에서는 Vue.js 구성 요소의 HTML 요소를 참조하는 방법에 대해 설명합니다.

다음 사항에 대해 자세히 설명합니다.

- Vue.js란?
- Vue.js의 참조란?
- Vue.js에서는 ref를 어떻게 사용합니까?
- Vue.js의 "this.refs"
- Vue.jsref의 작동 방식
- Vue `$refs
- Vue.js에서 DOM 요소 가져오기
- Vue.js에 HTML 요소 표시
- HTML 입력 값 표시
- Vue 요소의 URL 표시
- Vue.js의 조건 처리

이 Vue refs 튜토리얼은 Vue를 사용하는 개발자의 모든 단계(초보자 포함)에 적합합니다. 시작하기 전에 이미 갖춰야 할 몇 가지 전제 조건은 다음과 같습니다.

- Node.js 버전 10.x 이상입니다. 터미널/명령 프롬프트에서 `node -v`를 실행하여 버전 확인
- npm 6.7 이상
- 코드 편집기. VS Code를 적극 권장합니다.
- 컴퓨터에 전체적으로 Vue.js 3 설치
- Vue CLI 3.0이 시스템에 설치되어 있습니다. 이렇게 하려면 먼저 `npm install-gvue-cli`를 사용하여 이전 CLI 버전을 제거한 다음 `npm install -g @vue/cli`를 사용하여 새 버전을 설치하십시오.
- Vue 스타터 프로젝트 다운로드
- 다운로드한 프로젝트의 압축을 풀고 탐색한 다음 `npm install`을 실행하여 모든 종속성을 최신 상태로 유지합니다.

## Vue.js란?

Vue.js는 230명 이상의 커뮤니티 구성원의 기여로 Evan You와 Vue 핵심 팀에 의해 만들어진 진보적인 자바스크립트 프레임워크이다. 뷰는 87만 개 이상의 프로젝트에 사용되며 깃허브에는 175,000개 이상의 별이 있다.

Vue.js는 뷰 레이어에만 초점을 맞춘 접근 가능한 코어 라이브러리이다. 또한 응답성이 뛰어난 웹 환경을 쉽게 구축할 수 있도록 지원하는 광범위한 라이브러리 에코시스템을 갖추고 있습니다.

## Vue.js의 참조란?

참조는 응용 프로그램의 템플릿에서 HTML 요소 또는 하위 요소에 대한 참조를 등록하거나 표시하는 데 사용되는 Vue.js 인스턴스 속성입니다.

Vue 템플릿의 HTML 요소에 ref 속성을 추가하면 해당 요소 또는 Vue 인스턴스의 하위 요소를 참조할 수 있습니다. 또한 DOM 요소에 직접 액세스할 수도 있습니다. DOM 요소는 읽기 전용 특성이며 개체를 반환합니다.

## Vue.js에서는 ref를 어떻게 사용합니까?

ref 속성은 상위 `$ref` 특성의 키 역할을 하여 DOM 요소를 선택할 수 있도록 합니다.

예를 들어, 입력 요소에 ref 속성을 넣으면 상위 DOM 노드가 `이`로 표시됩니다.$ref.input` 또는 `this.ref["input"]라고 말할 수 있습니다.

Vue.js의 `$refs`를 참조하는 빠른 시각적 가이드를 보려면 다음 비디오 튜토리얼을 참조하십시오.

### 뷰의 this.refs

요소의 참조에 대한 메서드를 정의하여 DOM 요소를 조작할 수 있습니다. 예를 들어 `this`를 사용하여 입력 요소에 집중할 수 있습니다.

```bash
this.$refs["input"].focus()
```

이러한 방식으로 참조는 `문서`와 같이 사용될 수 있다.JavaScript의 쿼리 선택기(.element`) 또는 jQuery의 `$(.element`)`입니다. $refs는 Vue.js 인스턴스 내부와 외부에서 모두 액세스할 수 있습니다. 그러나 데이터 속성은 아니므로 사후 대응적이지 않습니다.

브라우저의 템플릿 검사에서는 HTML 특성이 아니라 Vue 템플릿 속성이기 때문에 표시되지 않습니다.

## Vue.jsref의 작동 방식

처음부터 이 게시물을 따른 경우 시작 프로젝트를 다운로드하여 VS Code에서 열었어야 합니다. `구성 요소` 폴더를 열고 이 폴더를 `테스트`에 복사합니다.vue` 파일:

```xml
<template>
  <div>
    <h2>Hello this is for refs man!</h2>
    <p>You have counted {this.counter} times</p>
    <input type="text" ref="input">
    <button @click="submit">Add 1 to counter</button>
  </div>
</template>
<script>
export default {
  name: 'Test',
  data(){
    return {
      counter: 0
    }
  },
  methods: {
    submit(){
      this.counter++;
      console.log(this.ref)
    }
  }
}
</script>
```

이제 다음 명령을 사용하여 개발 서버에서 이 작업을 실행하십시오.

```coffeescript
npm run serve
```

### Vue '$refs

클릭 시 업데이트되는 간단한 카운터가 사용자 인터페이스에 표시되지만 브라우저에서 개발자 도구를 열면 정의되지 않은 로그가 기록됩니다.

이는 Vue가 이 구문을 오류로 보지 않지만 오류라고 보기 때문에 구문을 올바르게 이해하는 것이 매우 중요합니다. 우리가 이미 Vuerefs에 대해 알고 있는 것에 따르면, 그들은 물체를 돌려주지만, 정의되지 않은 반응으로 미루어 볼 때, 무언가가 잘못되었다.

아래 코드를 `테스트`에 복사합니다.vue` 파일:

```xml
<template>
  <div>
    <h2>Hello this is for refs man!</h2>
    <p>You have counted {this.counter} times</p>
    <input type="text" ref="input">
    <button @click="submit">Add 1 to counter</button>
  </div>
</template>
<script>
export default {
  name: 'Test',
  data(){
    return {
      counter: 0
    }
  },
  methods: {
    submit(){
      this.counter++;
      console.log(this.$refs)
    }
  }
}
</script>
```

이 옵션을 실행하고 검사하면 이제 개체가 반환됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/running-test.vue-file.png?resize=730%2C244&ssl=1)

코드 블록을 빨리 보면 올바른 구문이 드러납니다. 템플릿 내부에서는 ref라고 하지만 Vue 인스턴스에서 이를 참조하면 $refs라고 합니다. 정의되지 않은 상태로 반환되지 않도록 주의해야 합니다. 참조된 요소의 가능한 모든 속성(템플릿에 있는 요소 포함)에 액세스할 수 있습니다.

## Vue.js에서 DOM 요소 가져오기

관심 있는 속성 중 몇 가지를 로깅해 보겠습니다. 네 시험 말이야vue` 파일은 다음과 같아야 합니다.

```xml
<template>
  <div>
    <h2>Hello this is for refs man!</h2>
    <p>You have counted {this.counter} times</p>
    <input type="text" ref="input">
    <button @click="submit">Add 1 to counter</button>
  </div>
</template>
<script>
export default {
  name: 'Test',
  data(){
    return {
      counter: 0
    }
  },
  methods: {
    submit(){
      this.counter++;
      console.log(this.$refs)
    }
  }
}
</script>
<style scoped>
p , input, button{
  font-size: 30px;
}
input, button{
  font-size: 20px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
```

브라우저의 응용 프로그램은 다음과 같아야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/test-application-browser.gif?resize=730%2C382&ssl=1)

### Vue.js에 HTML 요소 표시

HTML 요소를 DOM에 그대로 표시하려면 제출 메서드로 이동하여 `methods` 코드를 다음과 같이 변경하십시오.

```coffeescript
methods: {
    submit(){
      this.counter++;
      console.log(this.$refs.input)
    }
  }
```

여기서의 입력은 이전에 요소 내에서 만든 참조 이름입니다(`ref="input"`). 원하는 이름일 수 있습니다.

### HTML 입력 값 표시

HTML 요소 입력 값(사용자 인터페이스의 텍스트 상자에 입력된 문자열)을 표시하려면 `제출` 메서드로 이동하여 코드를 다음과 같이 변경합니다.

```coffeescript
methods: {
    submit(){
      this.counter++;
      console.log(this.$refs.input.value)
    }
  }
```

입력한 문자열이 정확하게 표시되며, 이 문자열은 바닐라 JavaScript 및 jQuery가 얻을 수 있는 쿼리 선택과 유사성을 보여줍니다.

### Vue 요소의 URL 표시

요소를 찾을 수 있는 웹 페이지도 Vue ref와 함께 표시할 수 있는 많은 것들 중 하나입니다. 제출 방법으로 이동하여 코드를 다음과 같이 변경합니다.

```coffeescript
methods: {
    submit(){
      this.counter++;
      console.log(this.$refs.input.baseURI)
 }
}
```

반환된 개체에 대한 정보만 참조로 액세스하고 기록할 수 있는 다른 많은 것들이 있습니다.

### Vue.js의 조건 처리

Vue.jsref는 또한 "v-for" 지시어가 사용되는 조건문과 같이 DOM에서 둘 이상의 요소를 출력하는 요소 내부에서 사용될 수 있다. 개체 대신 참조는 호출될 때 항목 배열을 반환합니다. 이를 설명하기 위해 다음과 같은 간단한 목록을 만드십시오.

```xml
<template>
  <div>
    <p v-for="car in 4" v-bind:key="car" ref="car"> I am car number {car}</p>
    <button @click="submit">View refs</button>
  </div>
</template>
<script>
export default {
  name: 'Test',
  data(){
    return {
    }
  },
  methods: {
    submit(){
      console.log(this.$refs)
    }
  }
}
</script>
```

개발 서버에서 다시 실행하면 다음과 같이 나타납니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/app-with-conditionals.png?resize=730%2C341&ssl=1)

이 튜토리얼의 전체 코드는 GitHub에서 찾을 수 있습니다.

## 결론

이 게시물은 Vue.js의 DOM에서 HTML 요소를 참조하는 사용자에게 노출되었습니다. 이제 값, 하위 노드, 데이터 속성 및 해당 요소를 포함하는 기본 URL과 같은 모든 요소 속성을 기준으로 이러한 요소에 액세스하고 기록할 수 있습니다.

또한 이를 달성할 수 있는 방법도 소개받았습니다. Vue 인스턴스가 초기화되고 구성 요소가 렌더링된 후 ref가 채워지는 것이 중요하므로 계산된 속성에서 ref를 사용하는 것은 하위 노드를 직접 조작할 수 있는 기능이 있기 때문에 중단된다. 해피 해킹!