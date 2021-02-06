---
layout: post
title: "Vue.js 간소화: 구성 요소, 소품 및 슬롯"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/vue-skyline.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/vue-skyline.png?fit=730%2C487&ssl=1)

이 문서에서는 Vue.js, Vue CLI 및 Bootboot CSS를 사용할 예정입니다.

## 설치

Vue.js를 설치하려면(아직 없는 경우) 다음 명령을 실행합니다.

```coffeescript
npm install vue
```

Vue CLI를 설치하려면 다음을 실행하십시오.

```coffeescript
npm install -g @vue/cli
```

이제 다음 명령을 실행하여 Vue CLI를 사용하여 프로젝트(북앱)를 시작할 수 있습니다.

```undefined
vue create bookapp
```

프로젝트 디렉토리는 다음과 같습니다.

```undefined
|-- node_modules/
|-- public/
|-- src/
   |-- assets/
   |-- components/
   |-- App.vue
   |-- main.js
|-- .gitignore
|-- babel.config.js
|-- package.json
|-- README.md
|-- yarn.lock
```

부트스트랩 CSS를 설치하려면 다음을 실행하십시오.

```css
npm install bootstrap@4.0.0
```

그런 다음 다음과 같이 main.js 파일로 가져올 수 있습니다.

```coffeescript
import 'bootstrap';
import 'bootstrap/dist/css/bootstrap.min.css';
```

### 구성 요소란 무엇입니까?

"구성 요소는 단일 요소를 통해 액세스할 수 있는 그룹으로 캡슐화된 요소의 모음입니다." – Sarah Drasner

구성 요소는 재사용 가능한 코드 조각을 만드는 데 도움이 됩니다. 이것은 우리가 이전에 구현된 기능이 필요할 때마다, 우리는 그러한 기능을 복제하기 위해 동일한 코드 조각을 다시 반복할 필요가 없다는 것을 의미한다.

이 기사에서는 단일 파일 구성 요소를 만드는 데 초점을 맞출 것입니다.

슬롯과 소품은 구성 요소의 내용을 쉽게 동적으로 변경할 수 있도록 함으로써 이를 달성하는 데 도움이 됩니다.

## 소품이란 무엇인가?

간단히 말해서, 소품은 데이터로 채워진 구성 요소 내의 개구부입니다. 즉, 구성 요소에 받침대가 있는 경우 해당 받침대를 가져온 다른 구성 요소 또는 뷰에서 데이터를 가져올 수 있어야 합니다. 이 다른 구성 요소를 상위 구성 요소라고 할 수 있습니다.

앞서 만든 앱인 북앱을 이용해 (카드로) 책 목록을 만들 수 있도록 앱을 만들겠습니다. 각 카드는 장부 목록 내에서 재사용할 수 있는 구성요소가 됩니다.

자, 우리가 `BookList`라는 구성 요소를 만든다고 합시다.vue와 그 안에서 BookCard.vue라는 또 다른 부품을 수입합니다.vue는 상위 구성 요소로 알려져 있습니다. 따라서 BookCard.vue는 하위 컴포넌트로 알려지게 된다.

아래 코드 예제를 확인하십시오.

```xml
<!--BookList.vue-->
<template>
    <div class="container">
        <div>
            <div class="row">
                <div class="col-md-4">
                    <BookCard />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import BookCard from './BookCard.vue'

export default {
    components: {
        BookCard
    },
</script>
```

따라서 `src/` 폴더의 디렉터리 구조는 다음과 같아야 합니다.

```undefined
|-- src/
   |-- assets/
   |-- components/
       |-- BookCard.vue
       |-- BookList.vue
   |-- App.vue
   |-- main.js
```

소품은 상위에서 하위로 데이터를 전달하기 위해 제작되었습니다. 단방향 데이터 흐름에 맞게 설계되었습니다.

`BookList`의 이전 예제를 사용하여 소품을 실제로 확인해 보겠습니다.vue와 bookcard.vue.

BookCard.vue에서는 특정 책에 대한 정보를 담고 있는 BookData라고 하는 소품을 수락하고 싶다고 가정해 보자.

```xml
<!--BookCard.vue-->
<template>
    <div>
        <div class="card">
            <img :src="bookData.img_url" class="card-img-top">
            <div class="card-body">
                <p class="card-text">Title: {bookData.title}</p>
                <p class="card-text">Author: {bookData.author}</p>
                <p class="card-text">Description: {bookData.description}</p>
                <slot></slot>
                <slot name="button"></slot>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        bookData: {
            type: Object,
            required: true,
            default: () => {}
        }
    }
}
</script>
```

소품이 어떻게 정의되는지 주목하십시오. 유형 및 필수 속성이 적절한 유효성 검사에 사용됩니다. 위에 표시된 것처럼 소품에도 기본값이 있을 수 있습니다. 문자열과 숫자는 그대로 직접 전달할 수 있습니다. 그러나 배열과 개체 기본값은 함수로 전달되어야 합니다.

검증이 필요하지 않았다면 다음과 같은 작업을 수행할 수도 있었습니다.

```xml
<script>
export default {
    props: ['bookData']
}
</script>
```

이제 하위 구성 요소(BookCard.vue)를 bookData라는 소품을 받아들이도록 수정했습니다. 이제 상위 구성 요소인 `BookList.vue`에서 해당 데이터를 하위 구성 요소로 전달해야 합니다.

먼저 부품으로 전달하고자 하는 품목(데이터) 목록이 필요합니다. 이 데이터는 데이터베이스 또는 API에서 가져온 것일 수 있습니다. 이 글을 쓰기 위해 나는 몇 가지 도서 자료를 하드 코딩할 것이다.

```xml
<!--BookList.vue-->
<script>
import BookCard from './BookCard.vue'

export default {
    components: {
        BookCard
    },
    data(){
        return {
            books: [
                {
                    'author': 'David Deustch',
                    'title': 'The beginning of Infinity',
                    'description': 'A book on philosphy about the origins of man',
                    'img_url': 'https://placeimg.com/640/480/any'
                },
                {
                    'author': 'Paul Coelho',
                    'title': 'The Alchemist',
                    'description': 'The true search for one\'s treasure',
                    'img_url': 'https://placeimg.com/640/480/any'
                },
                {
                    'author': 'Susan Kaye Quinn',
                    'title': 'Open Minds',
                    'description': 'Sci-Fi Futuristic book about mind reading',
                    'img_url': 'https://placeimg.com/640/480/any'
                },
                {
                    'author': 'Robert Kiyosaki',
                    'title': 'Rich Dad, Poor Dad',
                    'description': 'Motivational book on wealth building',
                    'img_url': 'https://placeimg.com/640/480/any'
                },
                {
                    'author': 'Dan Brown',
                    'title': 'The Da Vinci Code',
                    'description': 'Conspiracy theories about secrets of the holy grail',
                    'img_url': 'https://placeimg.com/640/480/any'
                },
                {
                    'author': 'Arthur Hailey',
                    'title': 'The money changers',
                    'description': 'Travel back in time and experience the banking system',
                    'img_url': 'https://placeimg.com/640/480/any'
                },
            ]
        }
    }
}
</script>
```

마크업 측면에서는 v-for로 데이터를 반복하고 소품을 통해 각 개체를 하위 구성 요소(북카드)로 전달하기만 하면 된다. 그게 어떻게 이루어지는지 보자.

참고: Vue.js에서 `camelCase` 또는 `PascalCase`에서 명명된 구성 요소는 마크업에서 케밥 케이스로 자동 변환됩니다. 그래서 `<book card/>>를 쓰는 대신 `<book card/>`를 쓸 수 있다.

소품도 그렇다. 우리는 우리의 소품을 `북데이터`로 명명했지만 마크업에서 `북데이터`로 지칭할 수 있다.

```xml
<!--BookList.vue-->
<template>
    <div class="container">
        <div>
            <div class="row">
                <div class="col-md-4" v-for="(book, i) in books" :key="i">
                    <book-card :book-data="book" />
                </div>
            </div>
        </div>
    </div>
</template>
```

마지막으로, 우리가 만든 이 간단한 앱을 보기 위해, 우리는 다음과 같이 App.vue 파일의 `BookList` 구성 요소를 부를 것이다.

```xml
<!--App.vue-->
<template>
  <div id="app">
    <book-list/>
  </div>
</template>

<script>
import BookList from './components/BookList.vue'

export default {
  name: 'App',
  components: {
    BookList,
  }
}
</script>
```

이제 앱을 보면 다음과 유사하게 보일 것입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/the-alchemist.png?resize=730%2C305&ssl=1)

각 카드는 동일한 구성 요소이지만 동적으로 로드되는 서로 다른 콘텐츠를 포함합니다.

구성요소에 전달된 소품은 전달된 데이터가 동적으로 변경될 것으로 예상되는 경우에만 구성요소에 바인딩하면 됩니다. 즉, 다음과 같이 데이터를 소자에 바인딩함으로써 도서 소품을 전달했습니다.

```undefined
:book-data="book"
```

여기서 바인딩은 북데이터에 추가된 콜론(v-bind:) 기호(v-bind:)입니다.

데이터가 동적으로 변경될 필요가 없는 상황에서는 바인딩할 필요가 없습니다. book-data=book이라고 하면 string book을 객체가 아닌 소품으로 전달할 수 있습니다.

그러나 당사의 인증은 브라우저 콘솔에 오류를 발생시켜 개체가 문자열이 아닌 예상 개체임을 나타냅니다.

### 슬롯이란 무엇입니까?

간단히 말해 슬롯은 소품과 유사한 구성 요소 내의 개구부입니다. 소품과는 달리 데이터를 가져갈 뿐만 아니라 다른 마크업이나 다른 구성품도 가져갈 수 있다.

명확히 하자면, 그들의 가장 큰 차이점은 소품이 있으면 부모가 어떻게 데이터가 전달될지에 대한 통제 없이 오직 아이에게만 데이터를 전달할 수 있다는 것이다. 그러나 슬롯을 사용하면 상위 항목이 데이터를 렌더링하는 방법을 정확하게 결정할 수 있으며 다른 구성 요소를 전달할 수도 있습니다.

우리 북앱 프로젝트 계속해봅시다.

우리는 모든 책의 정보를 카드로 만들어 놓았지만, 각각의 책에 대한 더 많은 정보를 볼 수 있는 버튼을 추가하고자 합니다. 이를 위해 우리는 다음과 같은 `북카드` 구성요소의 슬롯을 열기로 결정할 수 있다.

```xml
<!--BookCard.vue-->
<template>
    <div>
        <div class="card">
            <img :src="bookData.img_url" class="card-img-top">
            <div class="card-body">
                <p class="card-text">Title: {bookData.title}</p>
                <p class="card-text">Author: {bookData.author}</p>
                <p class="card-text">Description: {bookData.description}</p>
                <slot></slot> <!--Slot opening-->
            </div>
        </div>
    </div>
</template>
```

이제 상위 구성 요소(`BookList.vue`)에서 원하는 항목을 하위 구성 요소의 슬롯에 전달할 수 있습니다.

```js
<template>
    <div class="container">
        <div>
            <div class="row">
                <div class="col-md-4" v-for="(book, i) in books" :key="i">
                    <book-card :book-data="book">
                        <template v-slot> <!--Accessing slot-->
                            <button class="btn-primary">Read more</button>
                        </template>
                    </book-card>
                </div>
            </div>
        </div>
    </div>
</template>
```

단추에 링크나 액션이 없지만 이제 인터페이스에 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/author-david.png?resize=730%2C463&ssl=1)

### 명명된 슬롯

앞의 슬롯 예에서는 이름이 없는 <슬롯>을 사용했습니다. 따라서 상위 구성 요소에서 참조할 때 이름을 지정하지 않고 `<template v-slot>/template>만 수행했습니다.

슬롯이 두 개 이상 필요한 상황에서는 슬롯 이름을 지정하는 것이 편리합니다.

예제 프로젝트를 사용하여 다른 버튼에 대해 슬롯을 하나 더 추가해 보겠습니다. 이를 `미리 보기` 버튼이라고 합니다.

```xml
<!--BookCard.vue-->
<template>
    <div>
        <div class="card">
            <img :src="bookData.img_url" class="card-img-top">
            <div class="card-body">
                <p class="card-text">Title: {bookData.title}</p>
                <p class="card-text">Author: {bookData.author}</p>
                <p class="card-text">Description: {bookData.description}</p>
                <slot></slot> <!--Slot opening-->
                <slot name="preview"></slot>
            </div>
        </div>
    </div>
</template>
```

그러면 상위 구성 요소(BookList.vue)에서 다른 버튼을 누르면 됩니다. 이번에만, 이름이 있어.

```js
<template>
    <div class="container">
        <div>
            <div class="row">
                <div class="col-md-4" v-for="(book, i) in books" :key="i">
                    <book-card :book-data="book">
                        <template v-slot>
                            <button class="btn-primary">Read more</button>
                        </template>
                        <template v-slot:preview> <!--Accessing the preview slot-->
                            <button class="btn-secondary">Preview</button>
                        </template>
                    </book-card>
                </div>
            </div>
        </div>
    </div>
</template>
```

이제 다음과 같은 이점이 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/beginning-of-infinity.png?resize=730%2C305&ssl=1)

"<**template** v-slot:preview>를 사용하여 슬롯에 액세스했습니다. `**template** #preview`와 같은 짧은 형식을 사용할 수도 있다. 이는 Vue 2.6 이상에 새로 추가된 명명된 슬롯에서만 가능합니다.

### 범위 지정 슬롯

원웨이 데이터 흐름을 위한 소품이 만들어지지만, 스코프 슬롯을 통해 아이의 데이터를 부모에게 다시 전달할 수도 있습니다.
어떻게 하는지 봅시다.

이전의 예제 프로젝트를 사용하여 미리 보기라는 이름을 붙일 계산된 속성이 있다고 가정해 보겠습니다.북카드 부품 내부에 있는 슬러그. 이 민달팽이는 미리보기 버튼에 `href` 링크로 전달되어야 합니다. 범위 지정 슬롯을 사용하여 이 작업을 어떻게 수행할 수 있는지 확인하십시오.

하위 구성 요소(`BookCard.vue`)에는 몇 가지 새로운 속성(방법 및 계산됨)이 포함되어 있습니다. 메소드 속성을 사용하면 함수를 정의할 수 있고 계산된 속성을 사용하면 동적 값을 가질 수 있습니다.

이 경우 `미리보기`라는 계산된 속성이 있습니다.슬러그, 모부품에 넘기고 싶다.

```xml
<!--BookCard.vue-->
<script>
export default {
    props: {
        bookData: {
            type: Object,
            required: true,
            default: () => {}
        }
    },
    methods: {
        computePreviewSlug(){
            return this.bookData.title.split(' ').join('-') +'-'+ this.bookData.author.split(' ').join('-')
        },
    },
    computed: {
        previewSlug(){
            return this.computePreviewSlug()
        },
    }
}
</script>
```

이제 명명된 슬롯에서 다음과 같은 새 속성을 바인딩할 수 있습니다.

```xml
<!--BookCard.vue-->
<template>
    <div>
        <div class="card">
            <img :src="bookData.img_url" class="card-img-top">
            <div class="card-body">
                <p class="card-text">Title: {bookData.title}</p>
                <p class="card-text">Author: {bookData.author}</p>
                <p class="card-text">Description: {bookData.description}</p>
                <slot></slot> 
                <slot name="preview" :previewSlug="previewSlug"></slot>
```

```xml
<!--Binding previewSlug Property to Slot-->
```

```undefined
            </div>
        </div>
    </div>
</template>
```

이렇게 하면 `미리`가 된다.상위 구성 요소에서 사용할 수 있는 Slug` 데이터입니다. 즉시 다음과 같이 사용할 수 있습니다.

```xml
<!--BookList.vue-->
<template>
    <div class="container">
        <div>
            <div class="row">
                <div class="col-md-4" v-for="(book, i) in books" :key="i">
                    <book-card :book-data="book">
                        <template v-slot>
                            <button class="btn-primary">Read more</button>
                        </template>
                        <template v-slot:preview="slotProps"> 
                          <a :href="slotProps.previewSlug"><button class="btn-secondary">Preview</button></a>
                          <!--Using the previewSlug data by accessing the slot prop-->
                        </template>
                    </book-card>
                </div>
            </div>
        </div>
    </div>
</template>
```

여기서 우리는 미리보기를 구속한다.`href` 소유물에 대한 슬러그. 그러면 고유한 슬러그 링크가 미리 보기 단추에 추가됩니다.
버튼을 클릭하면 다음과 같은 링크가 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/localhost-8080.png?resize=730%2C63&ssl=1)

## 결론

재사용 가능성은 코딩의 표준 관행이며 코드를 훨씬 더 깨끗하고 쉽게 유지관리할 수 있습니다. Vue.js 구성 요소는 소품 및 슬롯을 사용하는 동적 데이터 바인딩의 길을 제공함으로써 이를 달성하는 데 도움이 된다.

다음은 이 기사에서 사용한 더미 프로젝트에 대한 Github 저장소에 대한 링크입니다: https://github.com/IDTitanium/dummy-bookapp.