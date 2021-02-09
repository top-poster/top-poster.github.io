---
layout: post
title: "Firebase authentication enables Google login to Sapper (Svelte)"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/firebase.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/firebase.png)

With Firebase authentication, you can create services that can easily log in to accounts such as Google, Facebook, Twitter, and GitHub.
I would like to implement Google login simply using Sapper (Svelte), which I am interested in recently.

The example results are available by clicking the GitHub Repository button above.

```bash
$ npx degit "ParkYoungWoong/firebase-auth-with-sapper" YOUR_PROJECT_NAME

```

# Sapper Installation

Sapper (Svelte APP makER) is a framework that supports the following functions that operate on Svelte:
It`s the same lineup as React`s Next.js and Vue`s Nuxt.js.

- Routing
- Server Side Rendering (SSR)
- Auto code segmentation
- Offline Support (Service Worker)
- Advanced project structure management

Create a new project using Degit as follows:

```bash
$ npx degit "sveltejs/sapper-template#rollup" YOUR_PROJECT_NAME
$ cd YOUR_PROJECT_NAME
$ npm install

```

After installation, Sapper has the following structure:

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/sapper-project-structure-1.jpg)

# Creating a Firebase project

Firebase is an integrated platform that supports a variety of functions, including authentication, real-time databases, storage, push, and hosting.
Log in to the Firebase page and proceed as follows:

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/create-firebase-project-1.jpg)

1) Log in to the Firebase console and create a new project.

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/create-firebase-project-2.jpg)

2) Follow the steps in Creating a Project, such as naming a project.

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/create-firebase-project-3.jpg)

3) When a new project is ready, press `Continue` to access the dashboard.

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/create-firebase-project-4.jpg)

4) Under "How to Log In" in "Authentication," select "Google" as the provider.

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/create-firebase-project-5.jpg)

5) Activate `Enable` and set `Project Support Email` before saving.

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/create-firebase-project-6.jpg)

6) If the provider status is `enabled`, you are ready.

# Installing and Initializing Firebase

To use the `firebase` command globally, globally install `firebase-tools` as follows:

```bash
$ npm i -g firebase-tools

```

If the installation is complete, you need to log in to Google next.

> This command connects the local machine to Firebase and grants access to the Firebase project.

```bash
$ firebase login

```

You must initialize the Firebase project:
Run the following command at the root of the project directory:

```bash
$ firebase init

```

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/firebase-init.jpg)

Select the desired Firebase CLI feature (select any feature because it only tests `authentication` right now).
Connect the Firebase project you created above, called `auth-test-with-sapper`.

> Create the firebase.json file required for Firebase hosting.
The default name of the directory that Firebase is looking for is 'public'.
You can also set up a public directory later by modifying the firebase.json file directly.

# Add My Apps in Firebase

Before applying Firebase authentication to the Sapper project,
First of all, you need to select the platform for your app (Sapper project) that Firebase will support as follows to obtain the SDK configuration:

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/firebase-add-app-1.jpg)

1) Approach `Project Settings`.

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/firebase-add-app-2.jpg)

2) Select the platform from `My App` to `Web`.

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/firebase-add-app-3.jpg)

2) Alternatively, you can choose directly from `Project Overview`.

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/firebase-add-app-4.jpg)

3) Set the name of my web app.
(You can add multiple apps within a single Firebase project)

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/firebase-add-app-5.jpg)

4) Code for adding Firebase SDK will be issued.
(This is a publicly available configuration)

> For information about security rules, see Firebase Security Rules.

To connect the SDK for authentication (Auth), go to https://firebase.google.com/docs/web/setup#available-libraries as written in the configuration above.

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/available-libraries.jpg)

Because we use authentication, we use firebase-auth.I will connect js`.
We add the code to the `src/template.html` of our Sapper project as follows:

```xml
<!-- src/template.html -->

<script src="https://www.gstatic.com/firebasejs/7.6.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/7.6.1/firebase-auth.js"></script>

<script>
// Your web app's Firebase configuration
var firebaseConfig = {
apiKey: "AIzaSyDEZzrvW7PlAgDmQWtw_hecYHTLi7NBWxA",
authDomain: "auth-test-with-sapper.firebaseapp.com",
databaseURL: "https://auth-test-with-sapper.firebaseio.com",
projectId: "auth-test-with-sapper",
storageBucket: "auth-test-with-sapper.appspot.com",
messagingSenderId: "148687957530",
appId: "1:148687957530:web:919b0b6018d8bbde5d2ba3"
};
// Initialize Firebase
firebase.initializeApp(firebaseConfig);
</script>

```

> Because this project uses authentication (login) only in client static files, install it as a CDN to disable it on the Sapper server.
https://stackoverflow.com/questions/56315901/how-to-import-firebase-only-on-client-in-sapper

# Components and Stores

For Google login, we will create components and store files as follows:

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/sapper-project-structure-2.jpg)

## user store

To make it writable to access the current user (`currentUser`) globally within the project:

```js
// src/store/user.js

import { writable } from 'svelte/store'

export const currentUser = writable(null)

```

## Sign InWithGoogle Components

Complete the components for Google Login as follows:
Define the style so that it is located on the right side of the Nav (Header).

```xml
<!-- src/components/SignInWithGoogle.svelte -->

<script>
// Imports the current user from the store.
// Because 'currentUser' is writeable object data, you must refer to the store using the prefix '$'.
// `$currentUser`
import { currentUser } from '../store/user';

// Log in with Google
const signInWithGoogle = async () => {
await firebase.auth().signInWithPopup(
new firebase.auth.GoogleAuthProvider()
// You can also log in via Facebook or Twitter.
// new firebase.auth.FacebookAuthProvider()
// new firebase.auth.TwitterAuthProvider()
// new firebase.auth.GithubAuthProvider()
);
};

// Logout
const signOut = async () => {
await firebase.auth().signOut();
};
</script>

<div class="sign-in-with-google">
{#if $currentUser}
<div class="user">
<div class="user__name">{$currentUser.displayName}</div>
<div class="user__photo">
<img
src={$currentUser.photoURL}
alt={$currentUser.displayName}
width="28" />
</div>
</div>
<div
class="sign-out"
on:click={signOut}>
Sign out
</div>
{:else}
<div
class="sign-in"
on:click={signInWithGoogle}>
Sign in with Google
</div>
{/if}
</div>

<style>
.sign-in-with-google {
height: 56px;
position: absolute;
top: 0;
right: 1em;
display: flex;
align-items: center;
}
.sign-in-with-google .user {
display: flex;
align-items: center;
}
.sign-in-with-google .user .user__name {
font-size: 13px;
font-weight: 700;
margin-right: 6px;
}
.sign-in-with-google .user .user__photo {
width: 28px;
height: 28px;
border: 1px solid lightgray;
border-radius: 50%;
overflow: hidden;
margin-right: 10px;
}
.sign-in-with-google .sign-out {
font-size: 13px;
cursor: pointer;
}
.sign-in-with-google .sign-out:hover {
text-decoration: underline;
}
</style>

```

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/user-information.jpg)

The `SignInWithGoogle.svelte` component you created uses in `src/components/Nav.svelte` as follows:
The `nav` element must have the `position` property to ensure normal placement.

```xml
<!-- src/components/Nav.svelte -->

<script>
import SignInWithGoogle from './SignInWithGoogle.svelte';
// ...
</script>

<style>
nav {
/* ... */
position: relative;
}
</style>

<nav>
<!-- ... -->
<SignInWithGoogle />
</nav>

```

## UserObserver Component

Create the following components to detect the user`s status (login, logout):

```xml
<!-- src/components/UserObserver.svelte -->

<script>
import { onMount } from 'svelte';
import { currentUser } from '../store/user';

// Run from the 'onMount' hook to operate in the Client environment.
onMount(() => {
// Run callback based on the login user's status conversion (login, logout).
firebase.auth().onAuthStateChanged(user => {
if (user) {
// You can assign users immediately because they are writable objects.
$currentUser = user
} else {
// Initialize if no user exists.
$currentUser = null
}
});
});
</script>

```

To ensure that the `UserObserver.svelte` component that you create is always operational, use it in `src/routes/_layout.svelte` as follows:

```xml
<!-- src/routes/_layout.svelte -->

<script>
import UserObserver from '../components/UserObserver.svelte';
// ...
</script>

<UserObserver />

```

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/sign-in.jpg)

# Add Login UI with FirebaseUI

FirebaseUI is the official library that provides the Firebase Authentication UI.
You can easily create login UI such as Google, Facebook, Twitter, etc. without any additional UI production.

First, install the FirebaseUI as follows:

```xml
<!-- src/template.html -->

<!-- ... -->
<script src="https://www.gstatic.com/firebasejs/ui/4.3.0/firebase-ui-auth.js"></script>
<link type="text/css" rel="stylesheet" href="https://www.gstatic.com/firebasejs/ui/4.3.0/firebase-ui-auth.css" />
<!-- ... -->

```

Modify the `SignInWithGoogle.svelte` component to allow the FirebaseUI to take effect as follows:

```xml
<!-- src/components/SignInWithGoogle.svelte -->

<script>
import { onMount, tick } from 'svelte'
import { currentUser } from '../store/user';

// When the component is mounted, check the current user.
// If no user exists, create a Google login button.
onMount(() => {
if (!$currentUser) {
createLoginButton();
}
});

// create login button
function createLoginButton() {
// FirebaseUI config.
const uiConfig = {
signInOptions: [
firebase.auth.GoogleAuthProvider.PROVIDER_ID,
// We can also provide login UI such as Facebook or Twitter.
// firebase.auth.FacebookAuthProvider.PROVIDER_ID,
// firebase.auth.TwitterAuthProvider.PROVIDER_ID,
// firebase.auth.GithubAuthProvider.PROVIDER_ID
],
callbacks: {
signInSuccessWithAuthResult: (authResult, redirectUrl) => false
}
};

// Initialize FirebaseUI
const ui = firebaseui.auth.AuthUI.getInstance() || new firebaseui.auth.AuthUI(firebase.auth());
// Search for the element (ID selector) and render the UI.
ui.start("#firebaseui-auth-container", uiConfig);
}

// ...

const signOut = async () => {
await firebase.auth().signOut();
// Wait for rendering based on current user state change (logout).
await tick();
// Create a login button.
createLoginButton();
};
</script>

<div class="sign-in-with-google">
<!-- ... -->
{:else}
You must remove the click event from the <!--.sign-in>. -->
<div class="sign-in">
<!-- Where the FirebaseUI is rendered -->
<div id="firebaseui-auth-container"></div>
</div>
{/if}
</div>

<style>
/* ... */

/* reset firebaseui margin
```

![image](https://heropy.blog/images/screenshot/firebase-auth-with-sapper/sign-out.jpg)

# References

https://firebase.google.com/docs/web/setup