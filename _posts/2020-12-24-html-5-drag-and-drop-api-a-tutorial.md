---
layout: post
title: "HTML 5 드래그 앤 드롭 API: 튜토리얼"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/HTML-5-Drag-Drop-API.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/HTML-5-Drag-Drop-API.png?fit=730%2C487&ssl=1)

이 튜토리얼에서는 HTML 5 드래그 앤 드롭 API에 대해 알아보겠습니다. 드래그 앤 드롭 API는 끌어다 놓을 수 있는 요소를 HTML로 가져와 개발자들이 한 장소에서 다른 장소로 끌어다 놓을 수 있는 풍부한 UI 요소를 포함하는 애플리케이션을 만들 수 있게 한다.

HTML 5의 드래그 앤 드롭 기능에 대해 알아보기 위해 Vue.js를 사용하여 간단한 칸반 보드를 구축하겠습니다.

칸반 보드는 사용자가 프로젝트를 처음부터 끝까지 시각적으로 관리할 수 있는 프로젝트 관리 도구입니다. Tello, Pivotal Tracker 및 Jira와 같은 도구는 Kanban 보드입니다.

## 전제조건

이 튜토리얼의 경우 다음이 필요합니다.

- HTML 및 JavaScript에 대한 기본 지식
- Vue.js 2.x의 기본 지식
- 시스템에 설치된 Vue CLI 4 이상
- 시스템에 설치된 Node.js 8.0.0 및 npm

## 칸반 보드 설정

칸반 보드는 Vue CLI 응용프로그램이 될 것입니다. 새 응용 프로그램을 생성하려면 다음 명령을 실행하십시오.

```undefined
vue create kanban-board
```

사전 설정을 선택하라는 메시지가 나타나면 Babel 및 ESLint만 포함하는 기본 사전 설정을 선택합니다.

설치를 완료한 후 Vue가 설치 과정에서 생성한 기본 구성 요소 `HelloWorld`를 삭제합니다. 또한 `App` 구성 요소를 비어 있고 베어 구성 요소 템플릿만 포함하도록 수정하십시오.

```xml
<template> <div></div> </template>
<script>
export default {
  name: 'App',
  components: {},
};
</script>
<style></style>
```

우리는 스타일링을 위해 Bootboot를 사용하겠지만 Bootboot CSS CDN만 있으면 됩니다. public/index.html의 `head` 섹션에 추가합니다.

```xml
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css"
    integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
```

## 칸반에서 UI 구성 요소 구축

칸반 보드를 건설한 후의 모습은 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/built-kanban-board.png?resize=730%2C285&ssl=1)

일반적으로 칸반 보드에는 기둥과 카드가 있습니다. 카드는 수행할 단일 항목 또는 태스크이며, 열은 특정 카드의 상태를 표시합니다.

우리는 세 가지 Vue 구성 요소를 만들 것입니다: 하나는 열용, 하나는 카드용, 다른 하나는 새 카드를 만들 것입니다.

### 카드 구성 요소 생성

우리가 만들 첫 번째 구성 요소는 카드 구성 요소입니다. 시작하려면 `/component` 디렉토리에 `Card.vue`라는 새 파일을 만드십시오.

새로 생성된 구성 요소에 다음을 추가하십시오.

```xml
<template>
  <div class="card">
    <div class="card-body">A Sample Card</div>
  </div>
</template>
<script>
export default {};
</script>
<style scoped>
div.card {
  margin-bottom: 15px;
  box-shadow: 0 0 5px #cccccc;
  transition: all ease 300ms;
  background: #fdfdfd;
}
div.card:hover {
  box-shadow: 0 0 10px #aaaaaa;
  background: #ffffff;
}
</style>
```

이렇게 하면 카드 구성요소가 작성되고 스타일링됩니다. 이 구성 요소는 구성 요소의 골격에 불과하기 때문에 끌 수 있는 기능을 아직 추가하지 않았습니다.

### 카드 구성 요소 추가

이름에서 알 수 있듯이, 이 구성 요소는 새 카드를 만들어 열에 추가할 책임이 있습니다.

/components 디렉터리에 `AddCard.vue` 파일을 생성하고 다음 파일을 추가하십시오.

```xml
<template>
  <div class="">
    <button
      class="btn btn-sm btn-info w-100"
      v-if="!inAddMode"
      @click="inAddMode = true"
    >
      Add Card
    </button>
    <form action="#" class="card p-3" ref="form" v-else>
      <div class="form-group">
        <input
          type="text"
          name="title"
          id="title"
          class="form-control"
          placeholder="Something interesting..."
          v-model="cardData"
        />
      </div>
      <div class="d-flex justify-content-center">
        <button type="submit" class="btn w-50 btn-primary mr-3">Save</button>
        <button type="reset" class="btn w-50 btn-danger">
          Cancel
        </button>
      </div>
    </form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      inAddMode: false,
      cardData: '',
    };
  },
  methods: {},
};
</script>
<style></style>
```

이에 대한 기능은 다음 섹션에 구축될 예정입니다.

### 열 구성 요소 생성

이 구성 요소가 마지막으로 생성됩니다. 이 구성 요소에는 카드 목록이 표시되고 "카드 추가 구성 요소"도 포함되어 새 카드를 열로 직접 만들 수 있습니다.

`열`을 만듭니다.구성 요소 디렉토리의 vue` 파일을 입력하고 다음 코드를 추가합니다.

```xml
<template>
  <div class="col-md-3 card column" ref="column">
    <header class="card-header">
      <h3 class="col">Column Name</h3>
    </header>
    <div class="card-list"></div>
  </div>
</template>
<script>
export default {};
</script>
<style scoped>
div.column {
  padding: 0;
  padding-bottom: 15px;
  margin: 0 15px;
  box-shadow: 0 0 10px #cccccc;
}
div.card-list {
  padding: 0 15px;
}
header {
  margin-bottom: 10px;
}
header h3 {
  text-align: center;
}
</style>
```

기능을 추가하고 작업할 구성 요소를 구성하기 전에 브라우저의 끌어서 놓기 기능이 어떻게 작동하는지 간략히 살펴보겠습니다.

## HTML 5 Drag and Drop API란?

드래그 작업은 사용자가 드래그 가능 요소 위로 마우스를 이동한 다음 요소를 드롭 가능 요소로 이동할 때 시작됩니다.

기본적으로 끌어서 놓을 수 있는 HTML 요소는 이미지와 링크뿐입니다. 다른 요소를 끌어다 놓을 수 있게 하려면 요소에 끌어다 놓을 수 있는 속성을 추가하거나 JavaScript에서 요소를 선택하고 끌어다 놓을 수 있는 속성을 `true`로 설정하여 기능을 명시적으로 생성해야 합니다.

> 요소에서 드래그 게이블을 'true'로 설정하면 드래그 게이블 속성이 요소에 추가되었음을 알 수 있습니다.

```xml
<!-- Making an element draggable in HTML -->
<div draggable="true">This is a draggable div in HTML</div>

<script>
// Making an element draggable in javascript
const div = document.querySelector('div');
div.draggable = true;
</script>
```

요소를 끄는 목적은 페이지의 한 섹션에서 다른 섹션으로 데이터를 전송하는 것입니다.

이미지의 경우 전송 중인 데이터는 이미지 URL 또는 이미지의 기본 64 표현입니다. 링크의 경우 전송되는 데이터는 URL이며, 브라우저의 URL 표시줄로 이동하여 해당 URL로 이동할 수 있습니다.

같은 맥락에서 데이터를 전송할 수 없다면 요소를 끌어도 소용이 없습니다. 끌어서 놓기 작업을 통해 전송할 데이터는 끌어서 놓기 데이터스토어에 저장됩니다. 드래그 앤 드롭 작업 시 데이터를 저장하고 액세스할 수 있는 `데이터 전송` API를 통해 액세스할 수 있습니다.

데이터 전송 개체는 끌어서 놓기를 통해 전송할 항목을 추가할 수 있는 공간을 제공하기 때문에 이러한 목적으로 사용됩니다.

드래그 작업이 시작될 때(드래그스타트 이벤트가 발송될 때) 데이터를 드래그 데이터 저장소에 추가할 수 있으며, 드롭 작업이 완료된 후에(드롭 이벤트가 발송될 때) 데이터를 수신할 수 있습니다.

요소를 끌어다 놓았을 때부터 떨어뜨릴 때까지 끌리는 요소에 대해 두 가지 이벤트가 트리거됩니다. 즉, 요소를 떨어뜨린 후 끌기 시작과 끌기 끝입니다.

드래그 게이블 요소는 아무 데나 놓을 수 없습니다. 요소를 끌어다 놓을 수 있도록 명시적으로 요소를 끌어다 놓을 수 있도록 해야 하는 것과 마찬가지로, 드롭을 사용할 수 있어야 합니다.

요소 드롭을 활성화하려면 "드래그오버" 이벤트를 수신하고 기본 브라우저 작업을 방지해야 합니다.

```xml
<!-- Make a section drop-enabled -->
<section class="section"></section>
<script>
const section = document.querySelector('.section');
section.addEventListener('dragover', (e) => {
  e.preventDefault();
});
</script>
```

요소를 드롭 사용 요소 위로 끌면 다음 이벤트가 드롭 사용 요소에서 트리거됩니다.

`드래기터`: 요소가 드롭 가능 요소 위로 끌면 한 번 트리거됩니다.
드래그오버: 요소가 드롭 가능 요소 위에 유지되는 한 이 작업은 계속 트리거됩니다.
`Drop`: 끌어온 요소가 드롭 가능 요소에 삭제된 후에 트리거됩니다.

> 데이터 전송 객체에 저장된 데이터는 드롭 이벤트가 트리거될 때만 액세스할 수 있으며, 드래거터나 드래거버에서는 액세스할 수 없습니다. 이에 대한 자세한 내용은 여기에서 확인할 수 있습니다.

## 구성 요소 구성

구성 요소에 끌어서 놓기 기능을 추가하기 전에 `app state`에 대해 살펴보겠습니다.

여기서의 응용 프로그램 상태는 `앱` 구성 요소에 저장되어 소품으로 `컬럼` 구성 요소에 전달될 수 있습니다. 반면 Column 구성 요소는 렌더링 시 필요한 소품을 Card 구성 요소로 전달합니다.

`App`을 수정합니다.vue는 상태 및 구성요소 구성을 반영합니다.

```xml
// App.vue
<template>
  <div class="container-fluid">
    <h2 class="m-5">
      Vue Kanban Board
    </h2>
    <div class="row justify-content-center">
      <Column
        v-for="(column, index) in columns"
        :column="column"
        :key="index"
      />
    </div>
  </div>
</template>
<script>
import Column from './components/Column';
export default {
  name: 'App',
  components: {
    Column,
  },
  data() {
    return {
      columns: [
        {
          name: 'TO-DO',
          cards: [
            {
              value: 'Prepare breakfast',
            },
            {
              value: 'Go to the market',
            },
            {
              value: 'Do the laundry',
            },
          ],
        },
        {
          name: 'In Progress',
          cards: [],
        },
        {
          name: 'Done',
          cards: [],
        },
      ],
    };
  },
};
</script>
<style>
h2 {
  text-align: center;
}
</style>
```

여기서는 `열` 구성요소를 가져오고, 데이터를 `열`로 저장한 상태에서 반복하면서 각 열에 대한 데이터를 `열` 구성요소로 전달합니다. 이 경우 "실행", "진행 중" 및 "완료"의 세 개의 열만 있으며 각 열에는 카드 배열이 있습니다.

다음으로, 소품을 받아 적절하게 표시하도록 `Column` 구성 요소를 업데이트합니다.

```xml
// Column.vue
<template>
  <div class="col-md-3 card column" ref="column">
    <header class="card-header">
      <h3 class="col">{ column.name }</h3>
      <AddCard />
    </header>
    <div class="card-list">
      <Card v-for="(card, index) in column.cards" :key="index" :card="card" />
    </div>
  </div>
</template>
<script>
import Card from './Card';
import AddCard from './AddCard';
export default {
  name: 'Column',
  components: {
    Card,
    AddCard,
  },
  props: {
    column: {
      type: Object,
      required: true,
    },
  },
};
</script>

...
```

Column 구성 요소는 App 구성 요소에서 소품을 수신하고 해당 소품으로 카드 구성 요소 목록을 렌더링합니다. 우리는 또한 새로운 카드를 열에 직접 추가할 수 있어야 하기 때문에 여기서 `Add Card` 구성 요소를 사용합니다.

마지막으로, Card 구성요소를 업데이트하여 Column에서 수신한 데이터를 표시합니다.

```xml
// Card.vue
<template>
  <div class="card" ref="card">
    <div class="card-body">{ card.value }</div>
  </div>
</template>
<script>
export default {
  name: 'Card',
  props: {
    card: {
      type: Object,
      required: true,
    },
  },
};
</script>
```

카드 구성 요소는 칼럼에서 필요한 모든 데이터를 수신하여 표시합니다. 우리는 여기에 카드 요소에 대한 참조도 추가하고 있습니다. 이 기능은 JavaScript를 통해 카드 요소에 액세스할 때 유용합니다.

위의 내용을 완료한 후 응용 프로그램은 다음 사항을 확인해야 합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/in-progress-kanban-board.png?resize=730%2C278&ssl=1)

## 끌어서 놓기 기능 추가

드래그 앤 드롭 기능을 추가하는 첫 번째 단계는 드래그 앤 드롭 구성 요소와 드롭 대상을 식별하는 것입니다.

사용자는 카드의 활동 진행에 따라 카드를 한 열에서 다른 열로 끌 수 있어야 합니다. 따라서 드래그 게이블 구성 요소는 카드 구성 요소이고 드롭 대상은 칼럼 구성 요소입니다.

### 카드를 끌 수 있게 만들기

카드 구성 요소를 드래그할 수 있도록 다음 작업을 수행해야 합니다.

- 끌 수 있는 특성을 `true`로 설정합니다.
- `DataTransfer` 객체를 사용하여 전송할 데이터를 설정합니다.

끌기 쉬운 것을 참(true)으로 설정하는 것은 가능한 일찍 일어나야 하며, Vue life cyles hook에 따르면 이를 위한 안전한 장소는 마운트된 후크 안에 있다. `Card` 구성 요소의 마운트된 후크에 다음을 추가하십시오.

```xml
// Card.vue
<script>
export default {
  name: 'Card',
  props: {...},

  mounted() {
    this.setDraggable();
  },

  methods: {
    setDraggable() {
      // Get Card element.
      const card = this.$refs.card;
      card.draggable = true;
      // Setup event listeners.
      card.addEventListener('dragstart', this.handleDragStart);
      card.addEventListener('dragend', this.handleDragEnd);
    },
  },
</script>
```

위에서, 우리는 카드 구성요소를 드래그할 수 있도록 처리하는 `setDraggable` 방법을 만들었다.

setDraggable에서는 앞 절에 추가된 참조에서 카드를 받아 draggable 속성을 true로 설정합니다.

또한 데이터 전송 객체를 사용하여 드래그 데이터 저장소에 데이터를 추가하는 데 유용한 이벤트 수신기를 설치하고 있습니다.

바로 그거 할 수 있도록 이벤트 청취자를 만들어보도록 하겠습니다.

```undefined
// Card.vue
<script>
export const CardDataType = 'text/x-kanban-card';

export default {
...
  methods: {
    setDraggable() {...},
    handleDragStart(event) {
      const dataTransfer = event.dataTransfer;
      // Set the data to the value of the card which is gotten from props.
      dataTransfer.setData(CardDataType, this.card.value);
      dataTransfer.effectAllowed = 'move';
      // Add visual cues to show that the card is no longer in it's position.
      event.target.style.opacity = 0.2;
    },
    handleDragEnd(event) {
      // Return the opacity to normal when the card is dropped.
      event.target.style.opacity = 1;
    }
  }
}
</script>
```

드래그 앤 드롭 API 개요 섹션에서 `dragstart` 이벤트가 전송될 때만 드래그 데이터 저장소에 데이터를 추가할 수 있음을 상기한다. 따라서 handleDragStart 방식으로 데이터를 추가해야 합니다.

데이터 전송 객체는 발송된 드래그 이벤트에서 수신되며, 세트데이터를 사용하여 드래그 작업 시 이동될 데이터를 소품에서 수신되는 카드 값으로 설정한다.

데이터를 설정할 때 필요한 중요한 정보는 형식입니다. 이것은 어떤 문자열도 될 수 있습니다. 우리의 경우 텍스트/x칸반카드로 설정되어 있다. 우리는 이 데이터 포맷을 저장하여 내보내고 있습니다. 카드가 떨어진 후 데이터를 받을 때 Column에 필요하기 때문입니다.

마지막으로 카드의 불투명도를 `0.2`로 줄여 사용자에게 카드가 원래 위치에서 끌려나오고 있다는 피드백을 제공한다. 드래그가 완료되면 불투명도를 `1`로 되돌립니다.

그 카드는 이제 끌 수 있다. 그러나 드롭 대상을 추가하지 않았으므로 아무 곳에나 드롭할 수 없습니다. 바로 그거 하자.

### 열을 드롭다운 사용 가능으로 설정

끌어서 놓기 API의 개요에서 우리는 `끌기` 이벤트를 청취해야 열이 삭제되도록 할 수 있다. 카드가 칼럼 위로 끌려갈 때 드래그오버 이벤트가 트리거된다.

카드가 칼럼 구성 요소로 들어가면 바로 `드래거터` 이벤트가 트리거되고, 카드를 칼럼에 넣은 후 드롭 이벤트가 트리거됩니다.

따라서, 열에 카드 드롭이 가능하도록 하려면, 이러한 이벤트를 청취해야 합니다.

먼저 `열` 구성 요소를 업데이트하여 드롭을 사용하도록 설정합니다.

```undefined
// Column.vue
<template>...</template>
<script>
import Card { CardDataType } from './Card';
import AddCard from './AddCard';
export default {
  name: 'Column',
  components: {...},
  props: {...},
  mounted() {
    this.enableDrop();
  },
  methods: {
    enableDrop() {
      const column = this.$refs.column;
      column.addEventListener('dragenter', this.handleDragEnter);
      column.addEventListener('dragover', this.handleDragOver);
      column.addEventListener('drop', this.handleDrop);
    },
    /**
     * @param {DragEvent} event
     */
    handleDragEnter(event) {
      if (event.dataTransfer.types.includes[CardDataType]) {
        // Only handle cards.
        event.preventDefault();
      }
    },
    handleDragOver(event) {
      // Create a move effect.
      event.dataTransfer.dropEffect = 'move';
      event.preventDefault();
    },
    /**
     * @param {DragEvent} event
     */
    handleDrop(event) {
      const data = event.dataTransfer.getData(CardDataType);
      // Emit a card moved event.
      this.$emit('cardMoved', data);
    },
  },
};
</script>
```

여기서는 `Column` 구성 요소가 마운트된 후 드롭을 활성화하는 데 필요한 모든 이벤트 수신기를 설정합니다.

이 세 가지 이벤트 중 가장 먼저 트리거되는 이벤트는 드래그 앤터(dragenter)로, 드래그 앤서블 요소를 열로 끌어오면 바로 트리거된다. 응용 프로그램의 경우 카드만 열에 삭제하기를 원하므로, `드래그너` 이벤트에서는 카드 구성 요소에 정의된 카드 데이터 유형을 포함하는 데이터 유형에 대해서만 기본값을 사용할 수 있습니다.

드래거버 이벤트에서는 드롭 효과를 무브먼트로 설정했다.

> 이동은 항목(카드)이 한 장소(한 열)에서 다른 곳으로 이동 중임을 나타냅니다. 다른 효과로는 복사, 연결 및 없음이 있습니다. 이에 대한 자세한 내용은 MDN에서 확인할 수 있습니다.

드롭 이벤트에서, 우리는 `데이터 전송` 객체에서 전송된 데이터를 얻는다. 만일 우리가 `드래거터` 이벤트에서 데이터 유형을 확인하지 않았다면, 여기 있는 데이터는 끌려온 것에 따라 임의의 것이 될 수 있다.

그러나 여기서 우리는 전송되는 데이터가 카드 구성요소의 `드래그스타트` 이벤트에 명시된 카드의 내용임을 확신한다.

다음으로, 상태를 업데이트하고 카드를 현재 열로 이동시켜야 합니다. 우리의 애플리케이션 상태는 앱 구성 요소에 있기 때문에 전송한 데이터를 전달하여 드롭 수신기에 카드 이동 이벤트를 내보내고 앱 구성 요소에서 카드 이동 이벤트를 수신합니다.

Vue에서 사용자 지정 이벤트 발송에 대해 알아보려면 Vue 공식 문서를 확인하십시오.

이제 `앱`을 업데이트하십시오.vue는 카드 Moved 이벤트를 청취합니다.

```xml
// App.vue

<template>
  <div class="container-fluid">
    ...
    <div class="row justify-content-center">
      <Column
        v-for="(column, index) in columns"
        :column="column"
        :key="index"
        @cardMoved="moveCardToColumn($event, column)"
      />
    </div>
  </div>
</template>

<script>
import Column from './components/Column';
export default {
  name: 'App',
  components: {...},
  data() {
    return {...}
  },
  methods: {
    moveCardToColumn(data, newColumn) {
      const formerColumn = this.columns.find(column => {
        // Get all the card values in a column.
        const cardValues = column.cards.map((card) => card.value);
        return cardValues.includes(data);
      })
      // Remove card from former column.
      formerColumn.cards = formerColumn.cards.filter(
        (card) => card.value !== data
      );
      // Add card to the new column.
      newColumn.cards.push({ value: data });
    },
  },
}
</script>
```

여기서는 @cardMoved를 통해 카드 Moved 이벤트를 청취하고 카드 ToColumn을 호출합니다. Card Moved 이벤트는 $event를 통해 액세스할 수 있는 값(카드 데이터)을 발산하며, 우리는 또한 카드가 떨어진 현재 열(이것이 이벤트가 발송된 장소)을 통과하고 있습니다.

`CardToColumn으로 이동` 기능은 카드가 이전에 있던 열을 찾고 해당 열에서 카드를 제거한 다음 새 열에 카드를 추가하는 세 가지 작업을 수행합니다.

## 칸반 보드 완료

튜토리얼에서 여기까지 오게 된 것을 축하합니다! 드래그 앤 드롭 기능이 추가되었으므로 남은 작업은 "카드 추가" 기능을 만드는 것뿐입니다.

`AddCard.vue`를 다음과 같이 업데이트하십시오.

```xml
<template>
  <div class="">
    <button
      class="btn btn-sm btn-info w-100"
      v-if="!inAddMode"
      @click="inAddMode = true"
    >
      Add Card
    </button>
    <form
      action="#"
      class="card p-3"
      @submit.prevent="handleSubmit"
      @reset="handleReset"
      ref="form"
      v-else
    >
      ...
    </form>
  </div>
</template>
<script>
export default {
  data() {
    return {...};
  },
  methods: {
    handleSubmit() {
      if (this.cardData.trim()) {
        this.cardData = '';
        this.inAddMode = false;
        this.$emit('newcard', this.cardData.trim());
      }
    },
    handleReset() {
      this.cardData = '';
      this.inAddMode = false;
    },
  },
};
</script>
```

우리는 "카드 추가" 양식이 제출되거나 재설정될 때 실행할 기능을 만들었습니다.

재설정되면 `cardData`(입력 필드에 입력된 현재 데이터)를 지우고 `InAddMode`를 `false`로 설정합니다.

양식이 제출되면 카드 데이터도 지워져 새로운 항목이 추가되면 이전 데이터가 존재하지 않게 되고, inAddMode도 false로 설정해 새로운 카드 이벤트를 내보낸다.

상태는 앱 컴포넌트에 저장되며 어떻게든 앱 컴포넌트에 카드 추가 정보를 알려야 하므로 앱 컴포넌트에 도달할 수 있는 이벤트를 내보내야 한다는 점을 기억하라.

카드 추가 구성 요소는 칼럼 구성 요소에서 사용되므로 칼럼 구성 요소에서 새 카드 이벤트를 청취해야 합니다.

새 카드 이벤트를 수신하도록 `컬럼` 구성 요소를 업데이트합니다.

```js
<template>
  <div class="col-md-3 card column" ref="column">
    <header class="card-header">
      <h3 class="col">{ column.name }</h3>
      <AddCard @newcard="$emit('newcard', $event)"></AddCard>
    </header>
    ...
</template>
...
```

여기서는 새로운 카드 이벤트를 재방출하여 실제 조치가 이루어지는 앱 구성 요소에 도달할 수 있도록 하고 있다.

> 사용자 지정 Vue 이벤트는 거품이 끼지 않으므로 앱 구성 요소는 직접 하위 구성 요소가 아니기 때문에 추가 카드 구성 요소에서 발생하는 새 카드 이벤트를 청취할 수 없습니다.

이제 `앱` 구성 요소를 업데이트하여 `새 카드` 이벤트를 처리하십시오.

```xml
// App.vue

<template>
  <div class="container-fluid">
    ...
    <div class="row justify-content-center">
      <Column
        v-for="(column, index) in columns"
        :column="column"
        :key="index"
        @cardMoved="moveCardToColumn($event, column)"
        @newcard="handleNewCard($event, column)"
      />
    </div>
  </div>
</template>

<script>
import Column from './components/Column';
export default {
  name: 'App',
  components: {...},
  data() {
    return {...}
  },
  methods: {
    moveCardToColumn(data, newColumn) {...},
    handleNewCard(data, column) {
      // Add new card to column.
      column.cards.unshift({ value: data });
    },
  },
};
</script>
```

여기서는 Column 구성 요소에서 발송된 new card 이벤트를 듣고, 데이터를 받아 card를 생성한 column에 추가한다.

## 결론

이 기사에서는 HTML 5 드래그 앤 드롭 API가 무엇인지, 어떻게 사용하는지, Vue.js 응용 프로그램에서 구현하는 방법에 대해 다루었다.

그러나 드래그 앤 드롭 기능과 이 튜토리얼은 다른 프런트 엔드 프레임워크와 바닐라 자바스크립트에서 모두 사용할 수 있다.

이 기사의 코드는 여기에서 찾을 수 있습니다.