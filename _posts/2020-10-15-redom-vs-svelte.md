---
layout: post
title: "RE:DOM 대 스벨트"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/redom-vs-svelte.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/redom-vs-svelte.png?fit=730%2C487&ssl=1)

RE:DOM은 DOM을 조작하기 위한 라이브러리이며, Svelte는 라우팅과 전환이 있는 프레임워크이다. 이 게시물에서는 이러한 차이점에 대해 검토하고 다양한 종류의 애플리케이션에 대해 어떤 것이 더 나은 선택인지 확인합니다.

## RE를 사용하여 구성 요소 생성:DOM 및 스벨트

RE:DOM과 Svelte 모두 컨텐츠를 렌더링하기 위한 구성요소를 만들 수 있습니다.

### 스벨트

Svelte에서는 개별 파일에 구성 요소를 생성합니다. 예를 들어 다음과 같이 쓸 수 있습니다.

```xml
//App.svelte

<script>
    import Button from "./Button.svelte";
</script>

<style>
  main {
    font-family: sans-serif;
    text-align: center;
  }
</style>

<main>
    <h1>Hello</h1>
    <Button />
</main>
```

```xml
//Button.svelte

<script>
    let count = 0;

    function handleClick() {
      count += 1;
    }
</script>

<style>
    button {
      background: #ff3e00;
      color: white;
      border: none;
      padding: 8px 12px;
      border-radius: 2px;
    }
</style>

<button on:click={handleClick}>
  Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
```

Button에 Button 구성 요소를 `Button` 구성 요소를 생성했습니다.App.svelte 구성 요소 파일에 포함된 svelte입니다.

스크립트 태그에는 논리가 있고 스타일에는 각 구성 요소의 스타일이 있습니다. 파일의 나머지 부분에는 렌더링하려는 HTML 콘텐츠가 있습니다.

우리는 자바스크립트 식을 가새에 내장하고, 그것은 이벤트 핸들러를 연결하기 위한 자체 구문을 가지고 있다. `on:click` 속성을 사용하면 클릭 처리기를 연결할 수 있습니다.

우리가 `handleClick` 기능과 같이 `script` 태그에 있는 함수들을 HTML 템플릿에서 참조할 수 있다.

`script` 태그에 선언된 모든 변수는 `button` 요소에서와 같이 템플릿에서 참조하고 렌더링할 수 있습니다.

데이터 바인딩은 자동으로 수행되므로 직접 바인딩할 필요가 없습니다.

### RE:DOM

RE 포함:DOM에서는 다음을 실행하여 `redom` 패키지를 설치합니다.

```coffeescript
npm i redom
```

그런 다음 다음과 같은 간단한 구성 요소를 생성할 수 있습니다.

```js
//index.js

import { el, mount } from "redom";

const hello = el("h1", "Hello world");

mount(document.body, hello);
```

이것은 우리가 프로젝트를 만들기 위해 소포와 같은 모듈 번들을 사용할 것이라고 가정한다. 우리는 `el` 기능으로 구성 요소를 만들어 태그와 우리가 보여주고자 하는 HTML을 전달합니다. 그런 다음 `안녕` 부품을 몸에 장착하기 위해 `마운트`를 부른다.

텍스트 기능을 사용하여 텍스트 구성 요소를 생성할 수 있습니다.

```js
import { text, mount } from "redom";

const hello = text("hello");

mount(document.body, hello);

hello.textContent = "hi!";
```

초기 `textContent`를 `text` 함수에 전달하면 해당 값이 자동으로 업데이트됩니다.

하위 요소를 추가하려면 다음과 같이 씁니다.

```js
import { el, setChildren } from "redom";

const a = el("h1", "foo");
const b = el("h2", "bar");
const c = el("h3", "baz");

setChildren(document.body, [a, b, c]);
```

그런 다음 배열과 동일한 순서로 요소를 추가합니다.

RE를 생성할 수도 있습니다.클래스가 있는 DOM 구성 요소입니다. 이를 통해 구성요소 라이프사이클의 다양한 단계에서 코드를 실행하도록 라이프사이클 방법을 설정할 수 있습니다. 예를 들어 다음과 같이 쓸 수 있습니다.

```js
import { el, mount } from "redom";

class Hello {
  constructor() {
    this.el = el("h1", "Hello world");
  }
  onmount() {
    console.log("mounted Hello");
  }
  onremount() {
    console.log("remounted Hello");
  }
  onunmount() {
    console.log("unmounted Hello");
  }
}

class App {
  constructor() {
    this.el = el("app", (this.hello = new Hello()));
  }
  onmount() {
    console.log("mounted App");
  }
  onremount() {
    console.log("remounted App");
  }
  onunmount() {
    console.log("unmounted App");
  }
}

const app = new App();

mount(document.body, app);
```

위에는 앱 구성 요소 내에 중첩된 Hello 구성 요소가 있습니다. 중첩을 수행하기 위해 새 인스턴스를 만들어 두 번째 인수로 전달한다.

일반적으로 Svelte는 애플리케이션 개발 프레임워크를 의미하므로 구성 요소를 생성할 수 있는 더 많은 옵션을 제공합니다. RE:DOM은 DOM 조작 라이브러리로 사용해야 하므로 훨씬 더 제한적입니다.

## 구성 요소 업데이트 중

### 스벨트

Svelte에서는 다양한 방법으로 구성 요소를 업데이트할 수 있습니다.

스크립트 태그의 변수를 업데이트하면 새 값이 자동으로 렌더링됩니다. 예를 들어:

```xml
//Button.svelte

<script>
    let count = 0;

    function handleClick() {
      count += 1;
    }
</script>

<style>
    button {
      background: #ff3e00;
      color: white;
      border: none;
      padding: 8px 12px;
      border-radius: 2px;
    }
</style>

<button on:click={handleClick}>
  Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
```

버튼을 클릭하면 핸들 클릭 기능이 실행되어 카운트가 1씩 늘어납니다. 그러면 `count`의 최신 값이 템플릿에 렌더링됩니다. 우리는 단지 그것을 표시하고, 그 표현에서 알 수 있듯이, 우리는 또한 그것을 다른 것을 렌더링하기 위해 사용합니다.

우리는 또한 Svelte와 스타일 바인딩을 추가할 수 있다. 예를 들어 다음과 같이 쓸 수 있습니다.

```xml
<script>
    let size = 42;
    let text = "edit me";
</script>


<input type=range bind:value={size}>

<div>
    <span style="font-size: {size}px">{text}</span>
</div>
```

크기 변수에 바인딩되는 범위 슬라이더가 있습니다. 따라서 슬라이더 값을 변경하면 크기에 따라 값이 변경됩니다.

그리고 `span` 요소에서 `size` 값을 사용하여 `font-size` 스타일을 변경합니다. 따라서 슬라이더가 편집자 텍스트 크기를 변경합니다.

또한 클래스를 설정하는 특수 구문도 함께 제공됩니다.

예를 들어 다음과 같이 쓸 수 있습니다.

```xml
<script>
 let active = true;
</script>

<style>
  .active {
    background-color: red;
  }
</style>

<div>
  <div
    class="{active ? 'active' : ''}"
    on:click="{() => active = !active}"
  >
    toggle
  </div>
</div>
```

버튼을 클릭하면 활성 변수를 true와 false로 전환하여 활성 클래스를 켜거나 끌 수 있습니다.

우리는 같은 것을 하기 위해 이와 같은 것을 쓸 수도 있습니다.

```xml
<script>
 let active = true;
</script>

<style>
  .active {
    background-color: red;
  }
</style>

<div>
  <div
    class:active='{active}'
    on:click="{() => active = !active}"
  >
    toggle
  </div>
</div>
```

활성 클래스가 `class:active` 속성에 적용되는지 여부를 결정하는 식이 있습니다.

### RE:DOM

RE 포함:DOM, 구성 요소 업데이트가 더 제한적입니다. 요소의 속성을 설정하거나 스타일을 설정할 수 있습니다. 예를 들어, 다음과 같은 내용을 작성하여 `h1` 요소를 만들고 해당 색상을 빨간색으로 설정할 수 있습니다.

```js
import { el, setAttr, mount } from "redom";

const hello = el("h1", "Hello world!");

setAttr(hello, {
  style: { color: "red" },
  className: "hello"
});

mount(document.body, hello);
```

setAttr 함수는 두 가지 인수를 사용합니다. 첫째는 우리가 만든 요소이고 둘째는 객체이며, 키는 속성 이름이고 값은 속성 값입니다.

RE:DOM에는 요소의 스타일을 설정할 수 있는 `setStyle` 방법도 있습니다. 예를 들어 다음과 같이 쓸 수 있습니다.

```js
import { el, setStyle, mount } from "redom";

const hello = el("h1", "Hello world!");

setStyle(hello, { color: "green" });

mount(document.body, hello);
```

이렇게 하면 `h1` 요소가 녹색으로 설정됩니다.

다음은 구성 요소를 업데이트하는 주요 방법입니다.

## 개발자 경험

Svelte는 전체 앱 개발 프레임워크이며 RE:DOM은 DOM 조작을 위한 라이브러리입니다. 둘 다 쉽게 DOM을 조작할 수 있습니다.

스벨트는 우리가 코드를 표현적으로 쓸 수 있게 해주고, 보일러판을 없앨 수 있게 해준다. Svelte 프로젝트를 빨리 만들기 위해서는 Svelte 템플릿 레포를 복제해야 하는데, 이는 그리 편리하지 않습니다.

반면에 RE:DOM, 우리는 단지 npmiedom과 함께 redom 패키지를 설치한다.

https://redom.js.org/redom.min.js의 스크립트를 사용하여 앱에 추가할 수도 있습니다. 즉, RE를 추가하는 데 빌드 도구가 필요하지 않습니다.앱 프로젝트의 DOM입니다.

두 도구 모두 최신 JavaScript 구문을 지원합니다. RE 이후:DOM은 라이브러리이며, 앱을 만들 수 있는 CLI 프로그램이 없습니다. 반면, 처음부터 Svelte 프로젝트를 만들어 구축할 CLI 프로그램은 없습니다. 이는 매우 편리하지 않습니다.

## 성과

많은 테스트에 따르면 RE:DOM이 Svelte보다 빠릅니다. AJ Meygani의 벤치마크를 통해 Svelte가 RE보다 느린 것을 알 수 있습니다.DOM은 여러 면에서. 이는 더 최근의 벤치마크에서도 비슷하게 나타난다.

그러나 이 차이는 불과 몇 밀리초밖에 되지 않으므로 대부분의 경우 그다지 두드러지지 않을 수 있습니다. RE: 다시 한 번 강조할 가치가 있습니다.DOM은 경량 라이브러리인 반면, Svelte는 완전한 앱 개발 프레임워크이기 때문에 이것은 타당하다.

## 라우팅

RE:DOM은 렌더링할 구성 요소에 URL을 매핑하는 라우터가 함께 제공되는 반면, Svelte는 자체 라우터가 함께 제공되지 않습니다.

### 스벨트

Svelte 앱에 라우팅을 추가하려면 다음을 실행하여 `svelt-routing` 패키지를 설치해야 합니다.

```coffeescript
npm i svelte-routing
```

그런 다음 다음과 같이 씁니다.

```xml
//routes/About.svelte

<p>about</p>
```

```xml
//routes/Blog.svelte

<p>blog</p>
```

```xml
//routes/BlogPost.svelte

<script>
  export let id;
</script>

<p>blog post {id}</p>
```

```xml
//routes/Home.svelte

<p>home</p>
```

```xml
//App.svelte

<script>
  import { Router, Link, Route } from "svelte-routing";
  import Home from "./routes/Home.svelte";
  import About from "./routes/About.svelte";
  import Blog from "./routes/Blog.svelte";
  import BlogPost from "./routes/BlogPost.svelte";

  export let url = "";
</script>

<Router url="{url}">
  <nav>
    <Link to="/">Home</Link>
    <Link to="about">About</Link>
    <Link to="blog">Blog</Link>
    <Link to="blog/1">Blog Post</Link>
  </nav>
  <div>
    <Route path="blog/:id" let:params>
      <BlogPost id="{params.id}" />
    </Route>
    <Route path="blog" component="{Blog}" />
    <Route path="about" component="{About}" />
    <Route path="/"><Home /></Route>
  </div>
</Router>
```

라우터를 추가하기 위해 라우터 구성요소를 추가하고 라우트를 추가한다.

우리는 `let:params` 속성과 `id` prop를 사용하여 URL 매개 변수를 전달하여 `id` URL 매개 변수를 수락한다.

`:id`는 URL 매개 변수 자리 표시자입니다. 매개 변수가 있다면 블로그포스트스벨트에서처럼 소품으로 얻을 수 있다. 변수 선언 앞에 `수출` 키워드가 있다면 그것은 하나의 지지입니다.

### RE:DOM

RE를 사용하려면:DOM의 라우터는 다음과 같이 쓸 수식:

```js
//index.js

import { el, router, mount } from "redom";

class Home {
  constructor() {
    this.el = el("h1");
  }
  update(data) {
    this.el.textContent = `Hello ${data}`;
  }
}

class About {
  constructor() {
    this.el = el("about");
  }
  update(data) {
    this.el.textContent = `About ${data}`;
  }
}

class Contact {
  constructor() {
    this.el = el("contact");
  }
  update(data) {
    this.el.textContent = `Contact ${data}`;
  }
}

const app = router(".app", {
  home: Home,
  about: About,
  contact: Contact
});

mount(document.body, app);

const data = "world";

app.update("home", data);
app.update("about", data);
app.update("contact", data);
```

우리는 `라우터` 기능을 사용하여 경로를 생성한다.

개체의 키는 경로에 대한 URL입니다. `app.update`는 원하는 경로로 이동합니다. 업데이트 방법의 매개 변수에서 얻은 데이터를 경로에 전달합니다.

일반적으로 말해서 RE:DOM의 라우터는 확실히 Svelte의 라우터보다 유연성이 떨어진다.

## 결론

RE:DOM은 경량 DOM 조작 라이브러리입니다. 비록 우리가 그것으로 구성 요소를 만들 수 있지만, 복잡한 구성 요소를 만들 목적으로는 그다지 표현력이 좋지 않습니다. 따라서, 우리는 DOM을 가지고 조작하는 것만을 고수해야 합니다.

반면, Svelte는 라우팅에 대한 `svelt-routing` 패키지를 추가하는 한 완전한 앱 프레임워크이다.