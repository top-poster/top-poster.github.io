---
layout: post
title: "Vue.js 앱에서 Vuetify를 사용하여 양식 유효성 검사를 구현하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/vuetify.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/vuetify.png?fit=730%2C487&ssl=1)

오늘날 대부분의 웹 사이트에는 사용자의 정보를 요청하는 등록, 로그인 또는 연락처 양식이 있습니다. 대부분의 양식은 오류 처리 시 충분한 피드백을 제공하지 않기 때문에, 이러한 양식을 검증하면 사용자의 전반적인 경험을 개선하는 데 도움이 됩니다.

예를 들어, 사용자가 양식에 오류가 있는 양식을 작성할 때, 사용자는 양식을 제출한 후에야 오류가 존재함을 알 수 있습니다. 즉, 양식을 완성한 후 다시 제출해야 합니다.

그러나 Vue.js의 재료 설계 구성요소 프레임워크인 Vuetify를 사용하여 품질 양식 검증을 구현할 수 있다. 이 블로그 게시물에서 Veutify를 사용하여 등록 양식을 작성하고 확인하는 방법에 대해 알아봅니다.

먼저 Vue.js 프로젝트를 만드는 것으로 시작하겠습니다.

## Vue.js 앱을 만드는 중

Vue CLI를 사용하여 Vue 앱을 만듭니다. 로컬 시스템에 Vue CLI가 설치되어 있지 않으면 아래 npm 명령을 실행하여 설치하십시오.

```coffeescript
npm install -g @vue/cli
```

명령 실행이 완료되면 아래 명령을 실행하여 Vue.js 프로젝트를 만들 수 있습니다.

```undefined
vue create vuetify-form-validation
```

나중에 기본 사전 설정 또는 수동 설정을 원하는지 묻는 메시지가 표시됩니다. 이 프로젝트에서는 기본 사전 설정을 사용합니다.

이제 Vuetify를 프로젝트에 추가하겠습니다.

## 프로젝트에 Vuetify 추가

Vuetify는 사용자에게 풍부하고 매력적인 웹 및 모바일 애플리케이션을 구축하는 데 필요한 모든 것을 제공합니다.

Vuetify를 추가하려면 프로젝트 폴더 cdvuetify-form-validation으로 이동하십시오.

그런 다음 아래 명령을 실행하여 Vuetify를 추가하십시오.

```undefined
vue add vuetify
```

Vuetify 명령이 실행되기 시작하면 원하는 사전 설정을 선택하라는 메시지가 다시 나타납니다. 기본 사전 설정을 선택합니다.

## 등록 양식 인터페이스 작성

이 문서에서는 다음 필드를 포함하는 등록 양식을 작성합니다.

- 이름
- 성
- 이메일
- 비밀번호
- 암호 다시 입력

또한 각 양식 필드의 유효성을 확인하는 방법에 대해서도 알아보겠습니다. 먼저 양식 페이지를 디자인해 보겠습니다. 구성 요소 폴더를 열고 `HelloWorld.vue` 파일을 삭제한 후 `RegistrationForm.vue`를 생성하십시오.

Vuetify 그리드 구성요소를 사용하여 페이지를 두 개의 열로 나눕니다.

RegistrationForm.vue 템플릿에 다음 코드를 씁니다.

```xml
<template>
  <v-container fluid class="fluid">
    <v-row justify="center" align="center" class="row">

      <v-col cols="6" sm="12" md="6" xs="12" class="text-center purple darken-2"
        style="height: 100vh">

        <v-row justify="center" align="center" style="height: 80vh">
          <v-col>
            <h1 class="white--text">LogRockets</h1>
          </v-col>
        </v-row>

      </v-col>

      <v-col cols="6" sm="12" md="6" xs="12" class="text-center" style="height: 100vh">
        <h1>Welcome to LogRockets</h1>
        <p>Please complete this form to create an account</p>
      </v-col>

    </v-row>
  </v-container>
</template>
```

섹션 전체를 `<v-container>`로 포장했습니다.

<v-container>는 웹 사이트를 중앙 또는 수평으로 패딩하고 유동성의 속성을 <v-container>에 바인딩할 수 있는 Vuetify 컨테이너로, 사이트 구축 시 원하는 화면 크기로 컨테이너를 완전히 확장할 수 있다. 또한 반응성을 높여줍니다.

우리는 또한 전체 구간을 포장하는 데 <v-row>를 사용했다.

<v-row>는 유연한 속성을 사용해서 우리가 원하는 방식으로 <v-cols>의 흐름을 제어할 수 있게 한다.

또 콜스(cols)를 넘긴 `v-cols`를 추가해 페이지를 두 개의 열로만 나누고 싶어 6의 값을 부여했다. Vuetify `cols`는 12개의 열로 제한됩니다.
왼쪽 열은 값이 `보라`인 클래스 속성을 가지며, UI를 배경색으로 제공합니다. 높이 100vh의 스타일 속성을 갖고 있다. 왼쪽 열에는 텍스트가 있는 <h1> 태그가 있습니다.

오른쪽 열에는 텍스트 센터 값이 있는 클래스 속성과 높이 100vh 값이 있는 스타일의 받침대가 있습니다. 오른쪽 콜스 안에는 텍스트와 with 태그가 붙어 있다.

우리는 양식이 전체 웹사이트 페이지를 차지하기를 원하기 때문에 높이 100vh의 스타일 속성을 양쪽 열에 전달한다.

## 등록 양식 작성

자, 이제 우리의 형태를 만들어봅시다. 오른쪽 열 아래의 `<p> 태그 아래에 다음 코드를 기록합니다.

```undefined
<v-form ref="form" class="mx-2" lazy-validation>
</v-form
```

<v-form>은 Vuetify 양식 구성 요소이며 양식 입력의 유효성을 쉽게 확인할 수 있다.

## 양식 필드 추가

다음은 양식 입력 필드를 구현합니다. 첫 번째 필드는 이름과 성을 입력하는 필드입니다. 검증도 실행해 봅시다.

v-form 내부에 다음 코드를 작성합니다.

```xml
<v-row>
  <v-col cols="6">
    <v-text-field v-model="firstname" :rules="nameRules" label="First Name">
    </v-text-field>
  </v-col>
  <v-col cols="6">
    <v-text-field v-model="lastname" :rules="nameRules" label="Last Name">
    </v-text-field>
  </v-col>
</v-row>
```

위의 코드에서, 우리는 웹 페이지를 두 개의 열로 나눌 때처럼 `v-input`을 일렬로 포장하는 것으로 시작했다. 두 번째 열에 폼 입력을 원합니다.

다음 단계는 이름과 성 입력 필드를 추가하는 것이었습니다.

<v-input>은 입력으로부터 값을 받는 Vue.js 속성인 <v-model>의 속성을 가지고 있다. <v-input>은 :rules라는 속성을 가지는데, 이것은 nameRules라는 함수를 그 값으로 삼는다. nameRules` 함수는 입력 필드에 전달된 값이 유효한지 또는 잘못된지를 확인하는 조건을 설정하는 데 사용됩니다.

<v-input>의 속성은 `label`로, 기본적으로 입력 자리 표시자 및 레이블처럼 작동합니다. 값은 입력 유형입니다. v-inputify `v-input`에는 기본적으로 텍스트 유형이 있으므로 무시하거나 무시하지 않도록 선택할 수 있습니다.

성 필드도 라벨과 v-모델을 제외한 이름 필드와 조건이 같다.

## 이름 및 성 필드의 유효성을 검사하는 중

지금 앱을 서비스하고 두 입력란에 커서를 놓으면 아무 일도 일어나지 않습니다. 입력에 유효성 검사 기능이 할당되지 않았기 때문입니다.

이제 입력을 비워둘 수 없다는 두 입력에 필요한 유효성 검사를 추가할 예정입니다. 다음으로 선언한 `name Rules`에 대한 유효성 검사 기능을 작성하겠습니다.

템플릿 태그 외부의 스크립트 태그로 이동하여 아래 코드를 작성합니다.

```php
<v-text-field v-model="firstname" :rules="nameRules" label="First Name" required>
</v-text-field>

firstname: '',
lastname: '',
nameRules: [
  v => !!v || 'Name is required',
  v => (v && v.length <= 10) || 'Name must be less than 10 characters',
],
```

이 코드에서 우리는 두 이름 입력 필드의 값을 문자열로 보유하는 `이름` 변수와 `성` 변수를 선언하였다. 그 후 입력 태그의 `:rules` 속성에 변수로 전달한 `nameRules` 변수를 선언하였다.

nameRules 변수에는 입력 필드에 전달된 값이 유효한 값인지 확인하는 조건이 있습니다. 그렇지 않은 경우 nameRules는 항상 오류를 표시해야 합니다.

지금 앱을 실행하고 이름 및 성 필드에 입력한 후 커서를 제거하면 이름이 필요하다는 오류가 표시됩니다.

## 이메일 필드 구현

다음 필드는 이메일 필드와 이메일 필드의 유효성 검사입니다. 성 필드 아래에 다음 코드를 작성합니다.

```xml
<v-row>
  <v-col cols="12">
    <v-text-field v-model="email" :rules="emailRules" label="Email" required>
    </v-text-field>
  </v-col>
</v-row>
```

e-메일 필드에서는 e-메일의 값을 취하는 v-model, 유효성 확인 기능이 있는 :rule 속성, 사용자에게 어떤 입력인지 알려주는 라벨을 전달한다.

## 전자 메일 필드 유효성 검사

다음 코드를 작성하여 전자 메일 유효성 검사를 트리거합니다.

```php
email: '',
emailRules: [
  v => !!v || 'E-mail is required',
  v => /^(([^<>()[\]\\.,;:\s@']+(\.[^<>()\\[\]\\.,;:\s@']+)*)|('.+'))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(v) || 'E-mail must be valid',
 ],
```

그 안에서, 우리는 e-메일 값을 모두 포함하는 e-메일 변수와 서버에 보내기 전에 값 입력이 유효한지, 잘못된지를 확인하는 e-메일 규칙 변수를 선언했습니다.

또한 입력이 비어 있는지 확인하고 입력되어 있으면 사용자에게 오류 메시지를 자동으로 알리는 확인 메시지가 표시됩니다.

## 암호 및 확인란 필드 구현

마지막으로 암호 및 확인란 필드와 해당 확인 사항을 구현하겠습니다. 아래에 코드를 쓰세요.

```xml
<v-row>
  <v-col cols="6">
    <v-text-field v-model="password" type="password" :rules="passwordRules"
     label="Password" required></v-text-field>
  </v-col>
</v-row>

<v-checkbox v-model="firstcheckbox" :rules="[v => !!v || 'You must agree to continue!']"
    label="I agree with Terms and Conditions" required></v-checkbox>

<v-checkbox v-model="seccheckbox" :rules="[v => !!v || 'You must agree to receive!']"
    label="I want to receive LogRocket Emails" required></v-checkbox>
```

암호 필드에는 입력 필드 값을 사용하는 `v-model` 속성과 유효성 검사 기능을 사용하는 `:rule` 속성이 있습니다.

v-checkbox에는 확인란 입력 필드의 값을 사용하는 v-model 속성도 있습니다. 확인란:규칙 속성은 유효성 검사 규칙을 변수를 통과시키지 않고 직접 가져옵니다.

## 암호 필드 확인 중

아래 코드를 작성하여 암호 필드의 유효성을 확인하십시오.

```js
password: '',
  passwordRules: [
      v => !!v || 'Password is required',
      v => /(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{6,}/.test(v) || 'Password must contain at least lowercase letter, one number, a special character and one uppercase letter',
  ],
    firstcheckbox: false,
```

passwordRule 변수는 우리가 선언한 이메일 및 이름 규칙 배열과 동일하지만 각 기능마다 수행해야 할 고유한 작업이 주어집니다. 암호 규칙은 암호에 대문자, 소문자, 특수 문자 및 숫자가 포함되어 있는지 확인합니다.

## 버튼 유효성 검사 선언 중

Veutify 유효성 검사를 통해 `:rule prop`를 통과하지 않고도 입력 필드의 유효성을 계속 확인할 수 있습니다. 예는 다음과 같습니다.

```undefined
<v-btn class="purple darken-2 white--text mt-5"  @click="submitForm"> Register </v-btn>

methods: {
  submitForm () {
    this.$refs.form.validate();
  },
},
```

위의 코드는 Vuetify 버튼 구성 요소입니다. 버튼 구성 요소에는 클릭 이벤트에 연결된 `submitForm` 기능이 있습니다. 사건이 터질 때마다 부에티파이(Vuetify)를 이렇게 부른다.입력 필드에 전달된 값이 유효한지 확인하는 $ref.form.diff` 메서드입니다. 입력 필드가 비어 있으면 사용자에게 오류 메시지를 표시합니다.

이것은 Vuetify를 사용하여 폼의 유효성을 확인할 수 있는 또 다른 방법입니다.

## 결론

이 블로그 게시물에서는 Vue 앱에서 Vuetify를 사용하여 양식의 유효성을 검사하는 방법과 Vuetify 양식의 유효성을 검사할 수 있는 두 가지 방법을 배웠습니다. 코드를 확인하거나 자세히 알아보려면 Github를 방문하십시오.