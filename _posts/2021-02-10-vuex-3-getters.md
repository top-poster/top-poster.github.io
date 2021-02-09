---
layout: post
title: "Vuex - 3 - Getters"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/vue.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/vue.png)

# Vuex Getters

You can use Getters when you use the storage state (State) in a calculated state.

> The attribute 'getters' is similar to the attribute 'computed' in Vue.

You want to capitalize all fruit names.
Modify `store.js` to capitalize `name` attribute information in the `fruit` data (`-fruit name`) as follows:

Reprocess the `fruit` (array) data of `state` by map method.
It can be used internally by transferring `state` as a factor in the attribute `upperCaseFruits`.

```js
// store.js
// ...

export const store = new Vuex.Store({
state: {
fruits: [
// ...
]
},
getters: {
upperCaseFruits: state => {
return state.fruits.map(fruit => {
return {
name: `- ${fruit.name.toUpperCase()}` // ES6 - Template Strings
}
});
}
}
});

```

Use the `upperCaseFruit` set in `getters` in the `FruitList.vue` component.

```xml
<!--FruitsList.vue-->
<template>
<div id="fruits-list">
<h1>Fruits Name</h1>
<ul>
<li v-for="fruit in upperCaseFruits">
{ fruit.name }
</li>
</ul>
</div>
</template>

<script>
export default {
computed: {
fruits() {
return this.$store.state.fruits;
},
upperCaseFruits() {
return this.$store.getters.upperCaseFruits;
}
}
}
</script>

<style></style>

```

The list of fruit names is capitalized and the `-` symbol is added before each name.

![image](https://heropy.blog/images/screenshot/vuex_getters_screenshot.jpg)

For optimized storage usage, such as utilization in other components, be careful not to calculate the storage state directly from `compacted` as follows!

```xml
<!--FruitsList.vue-->
<template>
<div id="fruits-list">
<h1>Fruits Name</h1>
<ul>
<li v-for="fruit in upperCaseFruits">
{ fruit.name }
</li>
</ul>
</div>
</template>

<script>
export default {
computed: {
fruits() {
return this.$store.state.fruits;
},
// Bad case!
upperCaseFruits() {
return this.$store.state.fruits.map(fruit => {
return {
name: `- ${fruit.name.toUpperCase()}`
}
});
}
}
}
</script>

<style></style>

```