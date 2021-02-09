---
layout: post
title: "Vuex - 4 - Mutations / Payload"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/vue.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/vue.png)

# Vuex Mutations

Vuex mutation (Mutations) is a method of changing the state of a repository.

Let`s apply Mutation by adding a button to discount the price of fruits.
Add a `BtnDiscount.vue` component to the `src/components` directory to use the button.

```bash
vue-simple-app
#...
├-src
│ ├-assets
│ ├-components
│ │ ├-BtnDiscount.vue
│ │ ├-FruitsList.vue
│ │ └-FruitsPrice.vue
│ ├-css
│ └-store
#...

```

Creates a button component with a few styles added (`BtnDiscount.vue`).

```xml
<!--BtnDiscount.vue-->
<template>
<div class="btn">DISCOUNT PRICE</div>
</template>

<script>
export default {

}
</script>

<style scoped>
.btn {
max-width: 700px;
margin: 10px auto;
padding: 10px 0;
color: white;
text-align: center;
background: salmon;
border-radius: 6px;
cursor: pointer;
}
</style>

```

Use button components in `App.vue`.

```xml
<!--App.vue-->
<template>
<div id="app">
<h1 id="app-title">Fruits List</h1>
<div id="fruits-table">
<fruits-list></fruits-list>
<fruits-price></fruits-price>
</div>
<btn-discount></btn-discount>
</div>
</template>

<script>
import FruitsList from './components/FruitsList.vue';
import FruitsPrice from './components/FruitsPrice.vue';
import BtnDiscount from './components/BtnDiscount.vue';

export default {
// ...
components: {
'fruits-list': FruitsList,
'fruits-price': FruitsPrice,
'btn-discount': BtnDiscount
}
}
</script>

<!-- ... -->

```

Added `Discount Price` button to screen.

![image](https://heropy.blog/images/screenshot/vuex_mutations_btn_screenshot.jpg)

To transform the storage state, add a `countPrice` function to the `mutations` property, which discounts `10%` of the current fruit price.

```js
// store.js
// ...
export const store = new Vuex.Store({
state: {
// ...
},
getters: {
// ...
},
mutations: {
discountPrice(state) {
state.fruits.forEach(fruit => {
fruit.price *= .9;
});
}
}
});

```

We will add a click event to the button component (`BtnDiscount.vue`) so that we can apply the `discountPrice` variation.
The variable handler cannot be called directly, so it must be called using the `commit` method as shown below.

```xml
<!--BtnDiscount.vue-->
<template>
<div class="btn" @click="discountPrice">DISCOUNT PRICE</div>
</template>

<script>
export default {
methods: {
discountPrice() {
this.$store.commit('discountPrice');
}
}
}
</script>

<!-- ... -->

```

Pressing the `DISCOUNT PRICE` button displays a `10%` discount on the current price.

![image](https://heropy.blog/images/screenshot/vuex_mutations_discounted_screenshot.jpg)

# Payload

This time, we will call you using a factor called Payload so that you can enter a discount rate in Commit.
I will modify the code to call you while applying the discount rate.
We will apply it to discount 20%.

```xml
<!--BtnDiscount.vue-->
<template>
<div class="btn" @click="discountPrice">DISCOUNT PRICE</div>
</template>

<script>
export default {
methods: {
discountPrice() {
this.$store.commit('discountPrice', {
discountRate: 20
});
}
}
}
</script>

<!-- ... -->

```

Create a payload as an object to accurately indicate what the value means and is passed to the payload.

> In most cases, payloads should be objects that can contain multiple fields, and recorded variations are easier to understand.

You will now modify the variation in the repository to receive the factor.

```js
// store.js
// ...
export const store = new Vuex.Store({
state: {
// ...
},
getters: {
// ...
},
mutations: {
discountPrice(state, payload) {
state.fruits.forEach(fruit => {
fruit.price *= (100 - payload.discountRate) / 100;
});
}
}
});

```

If applied well, each time you click the button, you will see a 20% discounted price.