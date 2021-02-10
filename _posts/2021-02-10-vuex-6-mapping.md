---
layout: post
title: "Vuex - 6 - Helpers(Mapping)"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/vue.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/vue.png)

I will try to use Vuex Helpers by modifying the code I wrote before.
Helpers bind the Vuex store (Store) content to the component`s `computed` or `methods` properties to help you use it more intuitively.

You can map the states (State), Getters, Mutations, and Actions that Vuex.

- mapState
- mapGetters
- mapMutations
- mapActions

First, I will modify `FruitList.vue`.

```xml
<!--FruitsList.vue-->
<!-- ... -->

<script>
import { mapState, mapGetters } from 'vuex';

export default {
computed: {
...mapState([ // ES6 - Spread
'fruits'
]),
...mapGetters([ // ES6 - Spread
'upperCaseFruits'
])
}
}
</script>

<style></style>

```

`mapState` was used to bind the state.
We used `mapGetters` for Getters.

Grammarly, you cannot run a function in the properties of an object or in the place of the method, but the deployment operator (Spread / `...) before the function.The `) allows you to expand and use it as part of an object.
Helpers uses this approach to create.

First, check the results to see if there are any problems.
If there is a problem with the deployment operator, `Babel Preset` may be required.

```bash
$ npm install --save-dev babel-preset-es2015

```

```undefined
// .babelrc
{
"presets": ["es2015"]
}

```

If you checked if it came out well, I will modify the next components as well.

```xml
<!--FruitsPrice.vue-->
<!-- ... -->

<script>
import { mapState } from 'vuex';

export default {
computed: {
...mapState(['fruits'])
}
}
</script>

<style></style>

```

Use `mapActions` for `BtnDiscount.vue`.

In the old code, the discount rate was transferred to the `dispatch` method with the Payload factor.

```js
discountPrice() {
this.$store.dispatch('discountPrice', {
discountRate: 20
});
}

```

However, if you map with `mapActions`, you cannot directly use the Payload factor, so you can create separate data and use it as a factor when a click event occurs.

```xml
<!--BtnDiscount.vue-->
<template>
<div class="btn" @click="discountPrice(discountData)">DISCOUNT PRICE</div>
</template>

<script>
import { mapActions } from 'vuex';

export default {
data() {
return {
discountData: {
rate: 20
}
}
},
methods: {
...mapActions([
'discountPrice'
])
}
}
</script>

<!-- ... -->

```

Create an object called `discountData` as a forwarding factor for the event.
The `countData` object has the `rate` property, but the Store has the Payload written to use the `countRate` property.
I will modify the name.

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
fruit.price *= (100 - payload.rate) / 100; // discountRate>
> rate
});
}
},
actions: {
// ...
}
});

```

I modified it all using Helpers.
Check if it works well.

If the name you mapped is not optimized or appropriate, you can change the name of the method that maps within the component as follows.

```xml
<!--BtnDiscount.vue-->
<template>
<div class="btn" @click="price(discountData)">DISCOUNT PRICE</div>
</template>

<script>
import { mapActions } from 'vuex';

export default {
data() {
return {
discountData: {
rate: 20
}
}
},
methods: {
...mapActions({
price: 'discountPrice'
})
}
}
</script>

<!-- ... -->

```