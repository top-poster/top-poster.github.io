---
layout: post
title: "CSS를 사용하여 Vue.js 애플리케이션 스타일링"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-29-at-9.28.48-AM.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-29-at-9.28.48-AM.png?fit=730%2C485&ssl=1)

CSS를 이용한 스타일링 뷰 컴포넌트는 개발자들이 배경색, 폰트 크기, 패딩, 포지셔닝, 애니메이션, 다양한 화면 크기에 대한 반응형 디스플레이를 포함한 디자인 미관을 응용 프로그램에 더하는 데 도움을 줄 수 있다.

Vue 지시어를 사용하면 템플릿 내에서 클래스 및 스타일 바인딩을 관리할 수 있습니다. 구성요소 내에서 인라인 스타일링을 사용하거나 외부 CSS 파일을 사용하여 응용프로그램을 구성할 수도 있습니다.

이 기사에서는 간단한 웹 페이지를 구축하여 Vue 구성 요소를 CSS로 스타일링하는 다양한 방법을 살펴볼 것이다.

### 전제조건

이 튜토리얼을 살펴보기 전에 몇 가지 확인해야 할 사항이 있습니다. 우선, 코드 편집기가 필요합니다. 비주얼 스튜디오 코드를 선호합니다. 그런 다음 터미널에서 다음을 실행하여 Node.js 버전 10.x 이상이 설치되어 있는지 확인합니다.

```css
:node -v
```

Vue의 최신 CLI도 필요합니다. 최신 버전을 가져오려면 먼저 이전 CLI 버전을 제거하십시오.

```coffeescript
npm uninstall -g @vue/cli
 id="or">#or
yarn global remove @vue/cli
```

그런 다음 최신 버전을 설치하십시오.

```coffeescript
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

또는 다음과 같이 버전을 업데이트할 수 있습니다.

```coffeescript
npm update -g @vue/cli

# OR
yarn global upgrade --latest @vue/cli
>
```

이 데모에 대한 보고서는 여기서도 찾을 수 있습니다. 또한 추가 읽기를 위해 최신 Vue 구성 요소 라이브러리 목록을 확인하는 것이 좋습니다.

## Vue 프로젝트 설정

새 프로젝트를 생성하려면 다음을 실행하십시오.

```xml
vue create <projectname>
```

그런 다음 사전 설정을 선택하라는 메시지가 표시됩니다. 다음 중 하나를 선택할 수 있습니다.

- 기본 Babel + ESLint 설정과 함께 제공되는 기본 사전 설정
- Vue 3 미리 보기 또는
- "수동으로 기능 선택"을 선택하여 필요한 기능을 선택합니다.

다음으로 디렉토리를 변경합니다.

```bash
cd <projectname>
```

그리고 로컬 호스트에서도 볼 수 있도록 설정할 것입니다.

```coffeescript
npm run serve

or

yarn serve
```

## 뷰에서 '범위 지정' 특성을 사용하여 스타일 지정

아래의 `style` 태그에 첨부된 `scope` 속성은 여기서 정의된 CSS 스타일이 구성 요소/템플릿 외부에 적용되지 않고 이 템플릿에만 적용됨을 의미합니다.

먼저, "Navbar"라는 이름의 `navbar` 구성 요소를 생성합니다.

```xml
// @/components/Navbar.vue

<template>
    <div class="navbar">

        <div class="navLink">
            <a href="#">About</a>
            <a href="#">Services</a>
            <a href="#">Contact</a>
        </div>
    </div>
</template>
<script>
export default {
    name: 'Navbar'
}
</script>
<style scoped>
.navbar{
    background: #f44336;
    padding: 1rem;
    font-size: 1.5rem;
    border-bottom: 1px solid white;


}
.navLink{
text-align: center;
}
a{
    text-decoration: none;
    padding: 1rem;
    color:#fff;
    text-align: center;
}
@media only screen and (max-width: 600px) {
    .navLink{
        display: flex;
        flex-direction: column;
    }
}
</style>
>
```

위의 예에서는 navbar 구성 요소를 생성했습니다. 그 안에서, 우리는 `scope` 속성을 사용해서 navbar를 스타일링했습니다. 즉, 여기서 정의된 모든 CSS 스타일은 `navbar` 구성 요소에만 적용됩니다.

## 외부 CSS 파일과 연결

CSS가 많아지면서 애플리케이션이 커짐에 따라 CSS 스타일을 외부 CSS 파일로 구분하여 컴포넌트에 연결하기를 권하고 싶습니다. 이것은 코드를 정리하는 여러 가지 방법 중 하나에 불과합니다.

다음은 예입니다.

```xml
// @/components/Body.vue

<template>
    <div class="container"> 
        <div class="startPage">
        <h2>Cruz Page</h2>
        <button class="btn">Get started here </button>
    </div>
    </div>

</template>

<script>
export default {
    name: 'Body'
}
</script>

<style scoped src="../assets/css/startpage.css">
/* @import '../assets/css/startpage.css'; */
</style>
```

위의 구성 요소는 아래의 외부 CSS 파일에 연결됩니다.

```undefined
// @/assets/css/startpage.css';

.startPage {
    height: 600px;
    background-color: #f44336;
    text-align: center;
}
h2{
    padding-top: 10rem;
    font-size: 4rem;
}
.btn {
    background: black;
    color: #fff;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    -ms-border-radius: 5px;
    -o-border-radius: 5px;
    outline: none;
    padding: 0.7rem 2rem;
    border: none;
    margin-top: 2rem;
}
```

외부 파일을 사용할 때 원본 파일 자체를 통해 연결하거나 `스타일` 태그로 가져올 수 있습니다. 이 예에서는 Vue 애플리케이션의 자산 폴더에 생성한 외부 CSS 파일을 링크했습니다.

## Vue.js에서 글로벌 스타일 사용

앱의 여러 구성 요소에 적용하고자 하는 스타일이 있습니다. 이를 효율적으로 수행하기 위해 스코어 또는 외부 파일로 스타일링하지 않고 글로벌 스타일링을 사용할 것입니다(이것도 작동 가능함). 폰트, 컬러, 배경색, 테두리 반지름, 여백 등 일반적인 스타일링을 하고 싶다면 글로벌 스타일링이 최선의 선택이다.

아래 예에서는 App.vue `style` 태그에 글로벌 스타일을 추가할 예정입니다.

```xml
// @/src/App.vue

<template>
  <div>
    <Navbar/>
    <Body/>
  </div>
</template>
<script>
import Navbar from './components/Navbar.vue'
import Body from './components/Body.vue'

export default {
  name: 'App',
  components: {
    Navbar,
    Body

  }
}
</script>
<style>

* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}
h1,h2,h3,h4,h5,h6{
  color: #fff;
}
</style>
```

아시겠지만, 우리는 CSS 와일드카드 선택기(별표라고 발음됨)를 사용하여 우리 응용 프로그램의 모든 요소를 선택하였습니다. 이 경우 모든 요소의 여백과 패딩은 0으로, 박스 사이징은 경계상자로 설정됐다.

## 인라인 스타일링 사용

인라인 CSS는 스타일 속성을 사용하여 특정 HTML 요소에 고유한 스타일을 적용하는 데 사용된다.

다음은 간단한 예입니다.

```xml
<h1 style="color: red; text-align: center;">I am a footer</h1>
```

Vue.js에서는 HTML 태그의 스타일 속성을 바인딩하여 인라인 스타일을 요소에 추가할 수 있습니다. 참고로 `:style`은 `v-bind:style`의 줄임말이다.

인라인 스타일링은 개체 구문 사용 또는 어레이 구문 사용이라는 두 가지 방법으로 수행할 수 있습니다. 우리는 아래 둘 다에 대해 토론할 것입니다.

### 객체 구문

객체 구문은 CSS 속성 이름을 객체의 키 이름으로, 값을 각 CSS 특성의 값으로 넣어 인라인 스타일링을 사용할 수 있게 한다. 개체 구문을 사용할 때는 아래 예와 같이 camelCase 또는 "kebab-case"를 사용하십시오.

```xml
// @/components/Footer.vue

<template>
    <footer 
      :style="{backgroundColor: bgColor, color: textColor, 
              height: `${height}px`, textAlign: align, 
              padding: `${padding}rem`
              }">
          <p> &copy; 2020</p>
    </footer>
</template>
<script>
export default {
    data(){
        return{
            bgColor: 'black',
            textColor: 'white',
            height: 200,
            align: 'center',
            padding: 5
        }
    }
}
</script>
<style>

</style>
```

이 예에서는 `footer` 구성 요소를 생성한 다음 객체 구문을 사용하여 바닥글 요소를 스타일링했습니다.

객체 구문 접근 방식을 사용하면 애플리케이션이 커질수록 템플릿이 더 깔끔해 보이도록 스타일 객체에 직접 바인딩하는 것이 좋습니다. 다음 예에서 확인하십시오.

```xml
<template>
        <footer :style="footerStyles">
          <p> &copy; 2020</p>
        </footer>
</template>
<script>
export default {
    data(){
        return{
            footerStyles:{
                backgroundColor: 'black',
                color: 'white',
                height: '200px',
                textAlign: 'center',
                padding: '5rem'
            }

        }
    }
}
</script>
<style>

</style>
```

### 배열 구문

인라인 스타일링의 다른 방법은 배열 구문을 사용하여 여러 스타일 객체를 추가하는 것입니다. 다음 예에서는 배열 구문인 `textColor`에 텍스트 색상을 빨간색으로 변경하는 개체를 추가합니다.

```xml
<template>
        <footer :style="[footerStyles1, textColor]">
        <p> &copy; 2020</p>
        </footer>
</template>
<script>
export default {
    data(){
        return{
            footerStyles1:{
                backgroundColor: 'black',
                height: '200px',
                textAlign: 'center',
                padding: '5rem'
            },
            textColor:{
                color: 'red',
            }

        }
    }
}
</script>
<style>

</style>
```

위의 예와 같이 여러 스타일 개체를 추가하려면 어레이 구문을 사용하는 것이 가장 좋습니다. 클래스를 동적으로 스타일링하려면 개체 구문이 인라인 스타일링에 가장 좋습니다.

## 결론

본 튜토리얼에서는 스코프 스타일링, 외부 CSS 파일에 대한 링크, 글로벌 스타일링, 객체 및 어레이 구문을 통한 인라인 스타일링을 포함하여 Vue.js 애플리케이션을 스타일링할 수 있는 다양한 방법에 대해 배웠다. 우리는 또한 다양한 스타일링 방법을 적용하는 방법을 설명하기 위해 간단한 웹사이트를 만들었다.