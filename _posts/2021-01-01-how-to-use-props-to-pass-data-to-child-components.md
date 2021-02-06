---
layout: post
title: "Vue.js: 전체 튜토리얼에서 소품을 사용하여 하위 구성 요소에 데이터를 전달하는 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/08/vuejsprops.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/08/vuejsprops.png?fit=730%2C412&ssl=1)

편집자 참고: 이 Vue 튜토리얼은 2021년 1/15에 업데이트되었습니다.

이 게시물에서는 Vue.js에서 상위 구성 요소에서 하위 구성 요소로 데이터가 전달되는 방법을 살펴본다.

## Vue에서 소품을 사용하기 위한 필수 구성 요소

이 게시물은 초보자를 포함한 모든 단계의 개발자에게 적합합니다. 여기 이 기사를 살펴보기 전에 당신이 이미 가져야 할 몇 가지 것들이 있다.

PC에는 다음이 필요합니다.

- Node.js 버전 10.x 이상이 설치되었습니다. 터미널/명령 프롬프트에서 아래 명령을 실행하여 이미 설치되어 있는지 확인할 수 있습니다.

```undefined
node -v
```

- 코드 편집기: Visual Studio Code를 사용하는 것이 좋습니다.
- 컴퓨터에 전체적으로 설치된 Vue의 최신 버전
- Vue CLI 3.0이 시스템에 설치되어 있습니다. 이렇게 하려면 먼저 이전 CLI 버전을 제거하십시오.

```coffeescript
npm uninstall -g vue-cli
```

그런 다음 새 드라이브를 설치합니다.

```coffeescript
npm install -g @vue/cli
```

- 여기에서 Vue 스타터 프로젝트 다운로드
- 다운로드한 프로젝트의 압축을 풉니다.
- 압축 해제된 파일로 이동하고 명령을 실행하여 모든 종속성을 최신 상태로 유지합니다.

```coffeescript
npm install
```

## 소품이란 무엇인가?

먼저, 소품을 정의해 봅시다. Vue 팀은 모든 구성 요소에 등록할 수 있는 사용자 지정 특성인 소품을 제공합니다. 작동 방식은 상위 구성 요소에 대한 데이터를 정의하고 값을 제공한 다음 해당 데이터가 필요한 하위 구성 요소로 이동하여 해당 값을 적절한 속성에 전달하여 데이터가 하위 구성 요소의 속성이 되도록 하는 것입니다.

구문은 다음과 같습니다.

```bash
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{ title }</h3>'
})
```

루트 구성 요소(App.vue)를 상위 구성 요소로 사용하고 데이터를 저장한 다음 소품을 등록하여 필요한 모든 구성 요소에서 이 데이터에 동적으로 액세스할 수 있습니다.

## 왜 소품을 사용해야 합니까?

왜 소품을 사용해야 하느냐고 묻죠? 두 개의 다른 구성 요소로 표시하려는 데이터 개체(예: 빌보드 톱 10 아티스트 목록)가 있지만 매우 다른 방식으로 표시하려는 경우 첫 번째 본능은 두 개의 개별 구성 요소를 생성하고 데이터 개체 내부에 배열을 추가한 다음 템플릿에 표시하는 것입니다.

이 솔루션은 훌륭하지만 구성 요소를 추가하면 비효율적인 솔루션이 됩니다. VS Code에서 연 시작 프로젝트로 이를 입증해 보겠습니다.

테스트를 엽니다.아래 이 코드 블록에 파일 및 복사:

```xml
<template>
  <div>
    <h1>Vue Top 20 Artists</h1>
    <ul>
      <li v-for="(artist, x) in artists" :key="x">
      <h3>{artist.name}</h3>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'Test',
  data (){
    return {
      artists: [
       {name: 'Davido', genre: 'afrobeats', country: 'Nigeria'},
       {name: 'Burna Boy', genre: 'afrobeats', country: 'Nigeria'},
       {name: 'AKA', genre: 'hiphop', country: 'South-Africa'},
       {name: 'Sarkodie', genre: 'hiphop', country: 'Ghana'},
       {name: 'Stormzy', genre: 'hiphop', country: 'United Kingdom'},
       {name: 'Lil Nas', genre: 'Country', country: 'United States'},
       {name: 'Nasty C', genre: 'hiphop', country: 'South-Africa'},
       {name: 'Shatta-walle', genre: 'Reagae', country: 'Ghana'},
       {name: 'Khalid', genre: 'pop', country: 'United States'},
       {name: 'ed-Sheeran', genre: 'pop', country: 'United Kingdom'}
      ]
    }
  }
}
</script>
```

구성 요소 폴더에 새 파일을 만듭니다. test2라고 합니다.아래의 코드 블록을 뷰잉하여 그 안에 붙여 넣습니다.

```xml
<template>
  <div>
    <h1>Vue Top Artist Countries</h1>
    <ul>
      <li v-for="(artist, x) in artists" :key="x">
      <h3>{artist.name} from {artist.country}</h3>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'Test2',
  data (){
    return {
      artists: [
       {name: 'Davido', genre: 'afrobeats', country: 'Nigeria'},
       {name: 'Burna Boy', genre: 'afrobeats', country: 'Nigeria'},
       {name: 'AKA', genre: 'hiphop', country: 'South-Africa'},
       {name: 'Sarkodie', genre: 'hiphop', country: 'Ghana'},
       {name: 'Stormzy', genre: 'hiphop', country: 'United Kingdom'},
       {name: 'Lil Nas', genre: 'Country', country: 'United States'},
       {name: 'Nasty C', genre: 'hiphop', country: 'South-Africa'},
       {name: 'Shatta-walle', genre: 'Reagae', country: 'Ghana'},
       {name: 'Khalid', genre: 'pop', country: 'United States'},
       {name: 'ed-Sheeran', genre: 'pop', country: 'United Kingdom'}
      ]
    }
  }
}
</script>
<style scoped>
li{
    height: 40px;
    width: 100%;
    padding: 15px;
    border: 1px solid saddlebrown;
    display: flex;
    justify-content: center;
    align-items: center;
  }  
a {
  color: #42b983;
}
</style>
```

방금 만든 새 구성 요소를 등록하려면 App.vue 파일을 열고 아래 코드를 복사하십시오.

```xml
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Test/>
    <test2/>
  </div>
</template>
<script>
import Test from './components/Test.vue'
import Test2 from './components/Test2'
export default {
  name: 'app',
  components: {
    Test, Test2
  }
}
</script>
```

VS Code 터미널에서 이 명령을 사용하여 개발 환경에서 애플리케이션을 서비스합니다.

```coffeescript
npm run serve
```

다음과 같이 보입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/08/top20artists.gif?resize=449%2C394&ssl=1)

구성 요소가 5개 정도 더 있으면 모든 구성 요소의 데이터를 계속 복사해야 합니다. 상위 구성 요소의 데이터를 정의한 후 속성 이름과 함께 필요한 모든 하위 구성 요소로 가져올 수 있는 방법이 있다고 상상해 보십시오.

## 상위 구성 요소의 데이터 정의

루트 구성 요소를 상위 구성 요소로 선택했으므로 먼저 루트 구성 요소 내부에서 동적으로 공유할 데이터 개체를 정의해야 합니다. 처음부터 이 게시물을 팔로우한 경우 앱을 엽니다.스크립트 섹션 내에서 데이터 개체 코드 블록을 vue 파일로 복사하십시오.

```xml
<script>
import Test from './components/Test.vue'
import Test2 from './components/Test2'
export default {
  name: 'app',
  components: {
    Test, Test2
  },
  data (){
    return {
      artists: [
       {name: 'Davido', genre: 'afrobeats', country: 'Nigeria'},
       {name: 'Burna Boy', genre: 'afrobeats', country: 'Nigeria'},
       {name: 'AKA', genre: 'hiphop', country: 'South-Africa'},
       {name: 'Sarkodie', genre: 'hiphop', country: 'Ghana'},
       {name: 'Stormzy', genre: 'hiphop', country: 'United Kingdom'},
       {name: 'Lil Nas', genre: 'Country', country: 'United States'},
       {name: 'Nasty C', genre: 'hiphop', country: 'South-Africa'},
       {name: 'Shatta-walle', genre: 'Reagae', country: 'Ghana'},
       {name: 'Khalid', genre: 'pop', country: 'United States'},
       {name: 'Ed Sheeran', genre: 'pop', country: 'United Kingdom'}
      ]
    }
  }
}
</script>
```

## 뷰에서 소품 받기

데이터를 정의한 후 두 개의 테스트 구성 요소로 이동하여 해당 구성 요소의 데이터 개체를 삭제합니다. 구성 요소의 소품을 받으려면 해당 구성 요소 내에서 받을 소품을 지정해야 합니다. 두 테스트 구성 요소 안으로 들어가 아래 그림과 같이 스크립트 섹션에 사양을 추가하십시오.

```css
<script>
export default {
  name: 'Test',
  props: ['artists']
}    
```

## 뷰에서 소품 등록

Vue 엔진에서 일부 하위 구성 요소에 동적으로 전달할 소품이 있음을 알려면 Vue 인스턴스에 이를 표시해야 합니다. 이 작업은 템플릿 섹션에서 다음과 같이 수행합니다.

```xml
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Test v-bind:artists="artists"/>
    <test2 v-bind:artists="artists"/>
  </div>
</template>
```

여기서는 v-bind 지시어를 사용하여 스크립트 섹션의 데이터 개체 어레이 이름인 아티스트와 테스트 구성 요소의 프로필 이름인 아티스트를 바인딩합니다. 이 경우, 다음과 같은 지시 없이 설정합니다.

```xml
    <Test artists="artists"/>
    <test2 artists="artists"/>
```

어떠한 출력도 표시되지 않으며 Vue 컴파일러나 ESLint도 오류 또는 주의로 플래그를 지정하지 않으므로 주의해야 하며 모든 동적 바인딩에 v-bind를 사용해야 합니다.

## 뷰에서 소품 사용

소품을 설정한 후에는 데이터가 동일한 구성 요소 내부에 정의된 것처럼 구성 요소 내부에서 소품을 사용할 수 있습니다. 이것은 당신이 메소드 콜을 설정하고 우리의 데모 케이스에서 "this.artists"에 쉽게 접근할 수 있다는 것을 의미한다.

## 강력한 타이핑 소품

또한 소품을 강하게 입력하여 구성 요소가 수신할 데이터 유형만 정확히 수신하는지 확인할 수 있습니다. 예를 들어 데모에서는 다음과 같은 인증을 설정하여 구성 요소로 전달되는 어레이만 확인할 수 있습니다.

```xml
<script>
export default {
  name: 'Test',
  props: {
    artists: {
      type: Array
    }
  }
}
</script>
```

따라서 문자열이라는 잘못된 유형을 추가할 때마다 콘솔에서 입력한 유형이 예상한 유형이 아니라는 경고가 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/08/vue-warn-error.png?resize=730%2C174&ssl=1)

여기서 이 튜토리얼의 전체 코드를 확인할 수 있습니다.

## 결론

이 게시물에서는 Vue 소품 및 이러한 소품이 데이터 개체의 재사용을 위한 플랫폼을 생성하여 DRY(자신을 반복하지 않음) 접근 방식을 장려하는 데 어떻게 도움이 되는지 살펴봤습니다. 또한 Vue 프로젝트에서 소품을 설정하는 방법도 배웠습니다. 자세한 내용은 뷰의 소품 공식 설명서를 참조하십시오. 해피 해킹!