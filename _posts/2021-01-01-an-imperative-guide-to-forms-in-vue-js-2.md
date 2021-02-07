---
layout: post
title: "Vue.js의 양식에 대한 전체 안내서"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2021/01/Vue-logo.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Vue-logo.png?fit=730%2C487&ssl=1)

편집자 참고: Vue.js의 양식에 대한 이 가이드는 01/19/21에 업데이트되었습니다.

우리가 좋아하는 프레임워크에서 양식을 적절하게 사용하는 것을 배우는 것은 가치 있고, 그것은 개발 중에 시간과 에너지를 절약할 수 있습니다. 이 튜토리얼에서는 Vue.js 2.x 응용 프로그램의 폼에서 입력을 만들고 검증하고 활용하는 프로세스를 안내합니다.

이 튜토리얼을 따르기 위해서는 HTML과 Vue.js에 대한 지식이 필요합니다. 코드펜에서 전체 데모를 가지고 놀 수 있습니다.

## Vue.js 앱 설정

먼저 HTML 마크업을 기본으로 하는 간단한 Vue.js 앱을 만들겠습니다. 또한 사전 제작된 일부 스타일을 활용하기 위해 Bulma를 수입할 예정입니다.

```xml
<!DOCTYPE html>
<html>
<head>
  <title>Fun with Forms in Vue.js</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/bulma/0.4.4/css/bulma.min.css">
</head>

<body>
  <div class="columns" id="app">
    <div class="column is-two-thirds">
      <section class="section">
        <h1 class="title">Fun with Forms in Vue 2.0</h1>
        <p class="subtitle">
          Learn how to work with forms, including <strong>validation</strong>!
        </p>
        <hr>      
        
        <!-- form starts here -->
        <section class="form">

</section>
      </section>
    </div>
  </div>
  
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.3.4/vue.min.js"></script>

<script>
  new Vue({
    el: '#app'
  })
</script>

</body>
</html>
```

## 'v-model'을 사용하여 입력

v-model 명령을 사용하여 폼 입력 및 텍스트 영역 요소 값을 Vue 인스턴스 데이터에 바인딩할 수 있다. Vue 문서에 따르면, `v-model` 지시문을 사용하면 양식 입력 `text area`에 양방향 데이터 바인딩을 생성하고 요소를 선택할 수 있습니다. 입력 유형을 기준으로 요소를 업데이트하는 올바른 방법을 자동으로 선택합니다.

### 텍스트 입력 예제

간단한 텍스트 입력을 만들어 사용자의 전체 이름을 가져옵니다.

```xml
...
<section class="form">
  <div class="field">
    <label class="label">Name</label>
    <div class="control">
      <input v-model="form.name" class="input" type="text" placeholder="Text input">
    </div>
  </div>
</section>
...

<script>
new Vue({
  el: '#app',
  data: {
    form : {
      name: ''
    }
  }
})
</script>
```

위의 코드에서, 우리는 Vue 인스턴스에서 데이터 옵션을 정의하고 폼 객체를 정의하는데, 이것은 우리가 우리의 폼에 대해 원하는 모든 속성을 포함할 것이다. 우리가 정의하는 첫 번째 속성은 우리가 만든 텍스트 입력에 바인딩된 `이름`입니다.

양방향 바인딩이 존재하기 때문에 응용 프로그램 내 어디에서나 form.name 값을 사용할 수 있습니다. 텍스트 입력의 업데이트된 값이 될 것입니다. 양식 개체의 모든 속성을 볼 수 있는 섹션을 추가할 수 있습니다.

```js
...
<div class="columns" id="app">
  <!-- // ... -->

<div class="column">
    <section class="section" id="results">
      <div class="box">
        <ul>          
          <!-- loop through all the `form` properties and show their values -->
          <li v-for="(item, k) in form">
            <strong>{ k }:</strong> { item }
          </li>
        </ul>
      </div>
    </section>
  </div>
</div>
...
```

만약 당신이 제대로 따라가고 있다면, 당신은 아래의 바이올린과 같은 결과를 얻어야 합니다. 입력란에 입력해 보십시오.

v-model은 양식 입력의 값, 확인 또는 선택된 속성을 무시하고 Vue 인스턴스 데이터를 참 소스로 처리합니다. 즉, Vue 인스턴스에서 `form.name`에 대한 기본값을 지정할 수도 있습니다. 그것이 폼 입력의 초기 값이 될 것이다.

### '텍스트 영역' 예제

이러한 기능은 일반 입력 상자가 작동하는 것과 동일한 방식으로 작동합니다.

```undefined
...
<div class="field">
  <label class="label">Message</label>
  <div class="control">
    <textarea class="textarea" placeholder="Message" v-model="form.message"></textarea>
  </div>
</div>
...
```

양식 모델의 해당 값은 다음과 같습니다.

```css
data: {
  form : {
    name: '',  
    message: '' // textarea value
  }
}
```

텍스트 영역(<textarea>{form.message}/textarea)의 보간은 양방향 바인딩에서 작동하지 않습니다. 대신 `v-model` 지시문을 사용하십시오.

### 선택 상자에 'v-model' 지시어 사용

v-model 명령어는 선택 상자에 쉽게 연결할 수도 있습니다. 정의된 모델은 선택한 옵션의 값과 동기화됩니다.

```xml
...
<div class="field">
  <label class="label">Inquiry Type</label>
  <div class="control">
    <div class="select">
      <select v-model="form.inquiry_type">
        <option disabled value="">Nothing selected</option>
        <option v-for="option in options.inquiry" v-bind:value="option.value">
          { option.text }
        </option>
      </select>
    </div>
  </div>
</div>
...
```

위의 코드에서 우리는 `v-for` 지시어를 사용하여 동적으로 옵션을 로드하기로 선택했다. Vue 인스턴스에서 사용 가능한 옵션도 정의해야 합니다.

```css
data: {
  form : {
    name: '',
    message: '',
    inquiry_type: '' // single select box value
  },
  options: {
    inquiry: [
      { value: 'feature', text: "Feature Request"},
      { value: 'bug', text: "Bug Report"},
      { value: 'support', text: "Support"}
    ]
  }
}
```

이 프로세스는 다중 선택 상자의 경우와 유사합니다. 차이점은 다중 선택 상자에 대해 선택한 값이 배열에 저장된다는 것입니다.

예를 들어:

```xml
...
<div class="field">
  <label class="label">LogRocket Usecases</label>
  <div class="control">
    <div class="select is-multiple">
      <select multiple v-model="form.logrocket_usecases">
        <option>Debugging</option>
        <option>Fixing Errors</option>
        <option>User support</option>
      </select>
    </div>
  </div>
</div>
...
```

해당 모델 속성 정의:

```undefined
data: {
  form : {
    name: '',
    message: '',
    inquiry_type: '',
    logrocket_usecases: [], // multi select box values
  }, 
  // ..
}
```

위의 예에서, 선택된 값은 `logrocket_usecase` 배열에 추가됩니다.

### 확인란 예제

부울(true/false) 값이 필요한 단일 확인란은 다음과 같이 구현할 수 있습니다.

```js
...
<div class="field">
  <div class="control">
    <label class="checkbox">
      <input type="checkbox" v-model="form.terms">
      I agree to the <a href="#">terms and conditions</a>
    </label>
  </div>
</div>
...
```

해당 모델 속성 정의:

```undefined
>data: {
  form : {
    name: '',
    message: '',
    inquiry_type: '',
    logrocket_usecases: [],
    terms: false, // single checkbox value
  }, 
  // ..
}
```

위의 예에서 `form.terms` 값은 확인란의 선택 여부에 따라 true 또는 false가 됩니다. 속성에 기본값인 false가 제공되므로 확인란의 초기 상태가 선택 해제됩니다.

여러 확인란의 경우 동일한 어레이에 간단히 바인딩할 수 있습니다.

```js
...
<div class="field"> 
  <label>
    <strong>What dev concepts are you interested in?</strong>
  </label>
  <div class="control">
    <label class="checkbox">
      <input type="checkbox" v-model="form.concepts"
        value="promises">
      Promises
    </label> 
    <label class="checkbox">
      <input type="checkbox" v-model="form.concepts" 
        value="testing">
      Testing
    </label>
  </div>
</div>
...

data: {
  form : {
    name: '',
    message: '', 
    inquiry_type: '', 
    logrocket_usecases: [],
    terms: false,
    concepts: [], // multiple checkbox values
  }, 
  // ..
}
```

### 라디오 버튼 예제

라디오 버튼의 경우 `모델` 속성은 선택한 `라디오` 버튼의 값을 취합니다.

예는 다음과 같습니다.

```js
...
<div class="field">
  <label><strong>Is JavaScript awesome?</strong></label>

<div class="control">
    <label class="radio">
      <input v-model="form.js_awesome" type="radio" value="Yes">
      Yes
    </label>
    <label class="radio">
      <input v-model="form.js_awesome" type="radio" value="Yeap!">
      Yeap!
    </label>
  </div>
</div>
...

data: {
  form : {
    name: '',
    message: '',
    inquiry_type: '',
    logrocket_usecases: [],
    terms: false,
    concepts: [],
    js_awesome: '' // radio input value
  }, 
  // ..
}
```

## Vee-validate를 사용하여 사용자 입력 확인

사용자 정의 유효성 검사 로직을 작성할 수 있지만, 입력의 유효성을 쉽게 검사하고 해당 오류를 표시하는 훌륭한 플러그인이 이미 있습니다. 이 플러그인은 ve-valid입니다.

Vue.js에 대한 이 양식 유효성 검사 라이브러리를 사용하면 선언적 스타일로 또는 구성 함수를 사용하여 입력 및 양식 UI를 검증할 수 있습니다.

우선, 우리는 우리의 앱에 플러그인을 포함시켜야 합니다. 이것은 실이나 npm으로 할 수 있지만, 우리의 경우에는 CDN을 통해서도 괜찮습니다.

```xml
<script src="https://unpkg.com/vee-validate@2.0.0-rc.18/dist/vee-validate.js"></script>
```

플러그인을 설정하려면 Vue 인스턴스 바로 위에 배치합니다.

```css
Vue.use(VeeValidate);
```

이제 입력에 v-validate 명령을 사용하여 규칙을 문자열 값으로 전달할 수 있습니다. 각 규칙은 파이프로 구분할 수 있습니다.

간단한 예를 들어 앞에서 정의한 `이름` 입력의 유효성을 살펴보겠습니다.

```undefined
...
<div class="field">
  <label class="label">Name</label>
  <div class="control">
    <input name="name" 
      v-model="form.name" 
      v-validate="'required|min:3'" 
      class="input" type="text" placeholder="Full name">
  </div> 
</div>
...
```

위의 예에서, 우리는 두 가지 규칙을 정의했습니다. 첫째는 이름 필드가 필요하다는 것입니다(`필수`). 둘째는 이름 필드에 입력된 값의 최소 길이는 3이어야 합니다(`min:3`).

> 팁: 규칙은 더 많은 유연성을 위한 개체로 정의할 수 있습니다. 예: 'v-dp="{required:true, min:3}"

이러한 규칙이 전달되지 않을 때 오류에 액세스하려면 플러그인에 의해 생성된 오류 도우미 개체를 사용할 수 있습니다. 예를 들어, 입력 바로 아래에 오류를 표시하기 위해 다음을 추가할 수 있습니다.

```undefined
<p class="help is-danger" v-show="errors.has('name')">
  { errors.first('name') }
</p>
```

위의 코드에서 우리는 `ve-validate` 플러그인의 두 가지 도우미 방법을 활용하여 다음과 같은 오류를 표시한다.

- .has는 입력란에 매개 변수로 전달된 오류가 있는지 확인할 수 있도록 도와줍니다. 부울 값(참/거짓)을 반환합니다.
- .first again`은 매개 변수로 전달된 필드와 관련된 첫 번째 오류 메시지를 반환합니다.

다른 유용한 방법으로는 `.collect()the `.all()the` 및 `.any()the. 여기서 더 읽어보실 수 있습니다.

vee-validate가 필드를 식별하는 데 사용하는 것이므로 입력 필드에 이름 속성을 정의해야 합니다.

마지막으로 입력 필드에 `is-danger` 클래스(Bulma에서 제공)를 추가하여 필드에 대한 유효성 검사 오류가 있는 경우를 나타낼 수 있습니다. 입력 필드가 제대로 채워지지 않은 경우 사용자가 바로 볼 수 있으므로 사용자 환경에 매우 유용합니다.

전체 필드 표시는 다음과 같습니다.

```undefined
...
<div class="field">
  <label class="label">Name</label>
  <div class="control">
    <input name="name" 
      v-model="form.name" 
      v-validate="'required|min:3'" 
      v-bind:class="{'is-danger': errors.has('name')}"
      class="input" type="text" placeholder="Full name">
  </div>
  <p class="help is-danger" v-show="errors.has('name')">
    { errors.first('name') }
  </p> 
</div>
...
```

지금까지 진행 중인 작업은 아래 피드의 결과 탭에서 확인할 수 있습니다.

vee-contract는 일반적인 사용 사례에 적합한 미리 정의된 규칙을 많이 가지고 있습니다.  사용 가능한 규칙의 전체 목록은 여기에서 확인할 수 있습니다. 일반 규칙에서 다루지 않는 요구 사항이 양식에 있는 경우에도 사용자 정의 유효성 검사 규칙을 정의할 수 있습니다.

### 'Validator.extend()'를 사용하여 사용자 지정 유효성 검사 규칙 생성

Validator.extend() 방법을 사용하여 사용자 지정 규칙을 생성할 수 있습니다. 사용자 지정 규칙은 계약 또는 특정 구조를 준수해야 합니다.

메시지를 보낼 때 사용자가 공손해야 하는 유효성 검사 방법을 추가해 보겠습니다.

```js
VeeValidate.Validator.extend('polite', { 
  getMessage: field => `You need to be polite in the ${field} field`,
  validate: value => value.toLowerCase().indexOf('please') !== -1
});
```

검증자는 다음 두 가지 속성으로 구성됩니다.

- getMessage(field, params): 유효성 검사가 실패할 때의 오류 메시지인 문자열을 반환합니다.
- validate(값, 매개 변수): 부울, 개체 또는 약속을 반환합니다. 부울이 반환되지 않으면 `유효(부울)` 속성이 개체 또는 약속에 있어야 합니다.

부귀도 현지화를 염두에 두고 만들어졌다. 변환 및 로컬리제이션에 대한 메모를 전체 `값 확인` 사용자 정의 규칙 문서에서 볼 수 있습니다.

## 이벤트 핸들러를 포함한 양식 제출

양식 제출을 처리하기 위해 Vue의 제출 이벤트 핸들러를 사용할 수 있습니다. 메서드 이벤트 핸들러의 경우 "v-on" 방법을 사용합니다. 또한 기본 작업을 방지하기 위해 `.prevent` 수식어를 꽂을 수도 있습니다. 이 경우에는 양식을 제출할 때 페이지가 새로 고쳐집니다.

```xml
...
<form v-on:submit.prevent="console.log(form)"> 
  ...
  <div class="field is-grouped">
    <div class="control">
      <button class="button is-primary">Submit</button>
    </div>
  </div>
</form>
...
```

위의 예에서는 양식 제출 시 전체 양식 모델을 콘솔에 기록하기만 하면 됩니다.

사용자가 잘못된 양식을 제출할 수 없도록 `비 검증`을 통해 최종 터치 한 번을 추가할 수 있습니다. `errors.any()를 사용하여 이 작업을 수행할 수 있습니다.

```undefined
<button 
  v-bind:disabled="errors.any()"
  class="button is-primary">
  Submit
</button>
```

위 코드에서는 양식에 오류가 있는 경우 제출 버튼을 비활성화합니다.

좋습니다! 이제 우리는 처음부터 양식을 작성하고 유효성 검사를 추가했으며 양식의 입력 값을 사용할 수 있습니다. 코드펜에서 작업한 전체 코드를 찾을 수 있습니다.

## 최종 참고 사항

기타 몇 가지 중요한 사항:

- .lazy, .number, .trim 등의 폼 입력에 v-model에 대한 수식어가 있습니다. 여기에서는 그들에 대한 모든 것을 읽을 수 있습니다.
- 동적 속성은 `v-bind:value`를 사용하여 입력 값에 바인딩할 수 있습니다. 자세한 내용은 값 바인딩의 문서를 참조하십시오.

이 가이드에서는 Vue.js 앱에서 폼을 만들고, 폼을 검증하고, 필드 값으로 작업하는 방법에 대해 알아보았다. Vue.js에서 양식을 만드는 한 가지 방법을 설명했지만 다른 옵션이 있습니다. 이 튜토리얼에서는 Vue 공식으로 양식을 작성하는 방법을 보여 줍니다.

Vue.js의 양식에 대해 질문이나 의견이 있으십니까? 또는 다른 프레임워크의 양식과 어떻게 비교합니까? 얼마든지 댓글을 달지 말고 얘기하자!