---
layout: post
title: "Spruce를 사용한 Alpine.js의 상태"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/%F0%9F%8C%B2-1.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/%F0%9F%8C%B2-1.png?fit=730%2C487&ssl=1)

React 및 Vue.js와 같은 JavaScript 프레임워크를 사용한 경우 구성 요소 간에 데이터를 주고받고 공유하는 방법을 만드는 상태 관리 개념을 이미 잘 알고 있을 수 있습니다.

이상적으로는 사용자가 각 구성요소의 범위에서 데이터를 선언하고 수동으로 전달할 필요 없이 두 개 이상의 구성요소가 애플리케이션에 있을 때 상태 관리가 필요합니다. 일반적으로 사용자는 애플리케이션 상태/데이터의 단일 진실 소스 역할을 하는 일종의 저장소를 가지고 있습니다.

React와 Vue.js의 개념을 빌리는 비교적 새로운 자바스크립트 프레임워크인 Alpine.js를 입력하고, Spruce라는 라이브러리를 통해 상태 관리를 구현한다. 스프루스는 Alpine.js를 위한 경량 상태 관리 라이브러리이며, Alpine.js와 마찬가지로 스프루스는 단순하고 작은 설치 공간을 가지고 있다.

## 우리가 짓고 있는 것

이 문서에서는 두 가지 구성 요소, 즉 새로운 작업관리 추가를 위한 입력과 작업관리 목록을 표시하는 테이블로 구성된 간단한 작업관리 응용프로그램을 구축합니다. 이를 통해 두 개의 독립된 구성 요소 내부의 글로벌 스토어에서 상태에 액세스할 수 있는 기회를 얻을 수 있습니다.

## Spruce

시작하려면 `알파인-스프루즈-작업관리`라는 새 프로젝트 디렉터리를 생성해 보겠습니다.

```undefined
mkdir alpine-spruce-todo
```

그런 다음 프로젝트 디렉터리 내에 `index.html` 파일을 만듭니다.

```bash
cd alpine-spruce-todo
touch index.html
```

Alpine.js와 마찬가지로, Spruce는 CDN에서 설치하거나 npm 또는 Narn을 사용하여 설치할 수 있습니다. 이 튜토리얼에서는 CDN을 사용합니다. `index.html` 내에 다음 코드를 추가하십시오.

```xml
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Todo</title>

    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bulma@0.9.0/css/bulma.min.css"
    />

    <script src="https://cdn.jsdelivr.net/npm/@ryangjchandler/spruce@1.1.0/dist/spruce.umd.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/alpinejs@2.6.0/dist/alpine.min.js"></script>
  </head>
  <body>
    <section class="section">
      <div class="container">
        <div class="columns">
          <div class="column is-three-fifths is-offset-one-fifth">
            
```

우리는 CDN을 사용하기 때문에 알파인.js보다 먼저 스프루스를 불러와야 합니다. 스타일링에는 CDN에서 가져올 Bulma CSS를 사용하자.

## 글로벌 저장소 생성

Spruce를 사용하기 위해서는 글로벌 저장소를 정의해야 합니다. 이 저장소는 당사의 모든 애플리케이션 구성 요소에 대한 단일 진실 소스로 사용됩니다. 응용 프로그램의 글로벌 저장소를 생성해 보겠습니다.

본문을 닫는 태그 바로 앞에 다음 조각을 추가합니다.

```xml
<script>
  Spruce.store('todo', {
    todos: [],
  })
</script>
```

우리는 윈도우 스코프에서 스프루스를 사용할 수 있는 CDN 빌드를 사용하고 있습니다. 스프루스 변수를 사용하여 스토어 메소드를 호출하여 스토어를 만듭니다.

메소드는 저장소의 이름과 저장소의 상태(데이터)의 두 가지 인수를 사용합니다. 우리는 작업관리 애플리케이션을 만들고 있기 때문에 매장 이름을 `작업`으로 정하는 것이 타당하다. 상점에는 기본적으로 비어 있도록 설정된 작업관리 배열인 하나의 상태(`작업관리`)만 있습니다.

## 상태 액세스

글로벌 스토어가 마련되어 있으므로 이제 구성 요소에서 스토어에 액세스하는 방법을 결정해야 합니다. 우리에게 다행스럽게도, 스프루스는 `$스토어` 마법의 재산을 노출시킴으로써 이것을 원활하게 만든다.

"구성 요소가 여기에 있음" 텍스트를 다음 코드로 바꿉니다.

```xml
<div x-data="{}">
  <template x-if="$store.todo.todos.length">
    <div class="box mt-5">
      <table class="table is-fullwidth">
        <tbody>
          <template
            x-for="(todo, index) in $store.todo.todos"
            :key="index"
          >
            <tr>
              <td x-text="todo.title"></td>
            </tr>
          </template>
        </tbody>
      </table>
    </div>
  </template>
</div>
```

이것은 일반적인 Alpine.js 구성 요소이지만 구성 요소에 대한 범위를 선언하지 않았습니다. 글로벌 스토어의 데이터를 활용하여 $store.to.do를 사용하여 `to do` 스토어에 액세스하고 이후 스토어 속성을 이용하고자 하기 때문입니다.

먼저 `작업관리` 배열에 일부 작업이 포함되어 있는지 확인합니다. 그런 다음 `작업관리` 배열을 반복하여 테이블에 작업관리를 표시합니다.

## 저장소 상태 수정

우리는 상점에서 주(州)에 접근하는 방법을 알아봤다. 상태를 수정하려면 어떻게 해야 합니까? 이를 위해, 우리는 이미 했던 것과 유사한 단계를 따릅니다. 상태를 수정하기 위해 새 값을 상태에 재할당하기만 하면 됩니다. 하지만 우리는 우리의 주(州)로서 어레이를 가지고 일하고 있기 때문에 새로운 아이템을 어레이에 밀어 넣어야 할 것입니다.

이전 구성 요소 앞에 다음 코드를 추가합니다.

```js
<div x-data="todoInput()">
  <div class="field">
    <div class="control">
      <input
        type="text"
        class="input"
        x-model="newTodo"
        placeholder="What needs to be done?"
        @keyup.enter="addTodo"
      />
    </div>
  </div>
</div>
```

이것은 일반적인 Alpine.js 구성 요소이지만, 이번에는 구성 요소 자체의 범위가 있으며, 이를 `todoInput()`라는 별도의 함수로 추출합니다. 이 입력은 `새로운`에 바인딩되어 있습니다.데이터 입력 키를 누르면 "Add Todo" 메서드를 호출할 수 있습니다.

함수를 생성해 보겠습니다. 스프루스 저장소를 정의하기 위한 코드 뒤에 아래 조각을 추가하십시오.

```js
function todoInput() {
  return {
    newTodo: '',
    addTodo() {
      if (!this.newTodo) {
        return
      }

      this.$store.todo.todos.push({
        title: this.newTodo,
      })

      this.newTodo = ''
    }
  }
}
```

기능에서 매장에 접속하고 있기 때문에 이것을 활용해야 합니다. 우리는 단순히 새로운 작업관리를 `토도스` 배열에 추가하기만 하면 된다.

## 결론

본 튜토리얼에서는 상태 관리의 정의와 이를 사용해야 하는 이유 및 시기에 대해 다룹니다. 또한 Spruce를 사용하여 Alpine.js에서 상태 관리를 적용하는 방법도 배웠고, 스토어 상태에 액세스하고 수정하는 방법도 보았습니다.

스프루스에 대해 자세히 알아보려면 GitHubrepo를 참조하십시오.