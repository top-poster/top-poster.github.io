---
layout: post
title: "Vuex - 5 - Actions / Context / Vue.js Devtools"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/vue.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/vue.png)

# Vuex Actions

Vuex`s actions are very similar to Mutation.
The operations should be handled synchronously and the actions asynchronously and should be separated as such.

Mutations focus on managing states, and when asynchronous operations are involved, it becomes extremely difficult to track program flow when variation is operated on each component.
Therefore, you should create actions to distinguish between asynchronous operations, and so on.

However, unless you need to distinguish between motivation and asynchronous processing, you may not feel the need for a difference between variation and action.
For your understanding, let`s make a simple asynchronous discount button.

First, we will install Vue.js devtools to make it easier to understand the difference between variation and action.

![image](https://heropy.blog/images/screenshot/vuex_vuejs_devtools_screenshot.jpg)

Vue.js devtools is a Chrome and Firefox extension for debugging applications using Vue.js.

Reboot the app once the installation is complete.

```bash
$ npm run dev

```

When you turn on the developer tool, the `Vue` tab appears at the top and is debugged as follows:

There is a Switch to Components tab where you can see the components used by the app.

![image](https://heropy.blog/images/screenshot/vuex_vue_devtools_components_panel_screenshot.jpg)

There is also a `Switch to Vuex` tab where you can see what happens in Vuex.

![image](https://heropy.blog/images/screenshot/vuex_vue_devtools_vuex_panel_screenshot.jpg)

Once the installation is complete, we will modify the code to apply the discount after 2 seconds by clicking the Discount Price button for asynchronous processing.

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
setTimeout(() => {
state.fruits.forEach(fruit => {
fruit.price *= (100 - payload.discountRate) / 100;
});
}, 2000);
}
}
});

```

Now press the button.
Each time a button is pressed, the developer tool `Vue.js devtools` panel immediately logs a `discountPrice` mutation, but in fact, the discounted price changes take place after 2 seconds.
This asynchronous processing in Mutations makes tracking difficult.

![image](https://heropy.blog/images/screenshot/vuex_vue_devtools_vuex_panel_async_mutations_screenshot.jpg)

Therefore, asynchronous processing should be written in Actions.
Add the `actions` property and create a `countPrice` action as follows:
Run the `setTimeout` function for asynchronous processing and call the mutation internally to `commit`.
Naturally, the `discountPrice` mutation removes asynchronous processing.

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
},
actions: {
discountPrice(context, payload) {
setTimeout(() => {
context.commit('discountPrice', payload);
}, 2000);
}
}
});

```

The `commit` method was used to invoke Mutation.
However, you must use the `dispatch` method to invoke Actions.

```xml
<!--BtnDiscount.vue-->
<!-- ... -->

<script>
export default {
methods: {
discountPrice() {
// Call Action
this.$store.dispatch('discountPrice', {
discountRate: 20
});
}
}
}
</script>

<!-- ... -->

```

Now that we`ve moved the asynchronous process from mutation to action, let`s actually get it working.
When you press the button, the `Vue.js devtools` panel does not immediately display the `countPrice` variation, but after 2 seconds.
In other words, asynchronous processing can be tracked only by action.

In addition, the method of `actions` has a factor called `context`, which allows access to `states`, `getters`, and `commits` in the store.
When you check `context` on the console, you can see:

![image](https://heropy.blog/images/screenshot/vuex_actions_context_object_screenshot.jpg)

In particular, if you call `commit` multiple times, you can modify it as follows:

```js
// store.js
// ...
actions: {
discountPrice({ commit }, payload) { // ES6 - Destructuring
setTimeout(() => {
commit('discountPrice', payload);
}, 2000);
}
}
// ...

```