---
layout: post
title: "Parcel - Easy, Fast, Powerful Web App Bundler - SASS / PostCSS / Babel / Production"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/parcel.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/parcel.png)

Parcel.js, which appeared in the summer of 2017, is a new web application bundler.
Unlike Webpack or Gulp, which was used a lot before, you can build it quickly without any additional settings.
You can reduce the amount of time you spend learning bundlers for builds.

# Characteristics

## Run Quick Build

You build it as soon as you specify the Entry File without any setup.

```bash
$ parcel index.html

```

## Fast Bundle Speed

Multi-core compilation is possible, and even if you restart, you can rebuild quickly using cache.

First build speed: 4.21 seconds

![image](https://heropy.blog/images/screenshot/parcel_build_speed_before.jpg)

Build speed after restart: 0.769 seconds

![image](https://heropy.blog/images/screenshot/parcel_build_speed_after.jpg)

## Bundling Based on Assets

Supports certain types of asets, such as HTML, CSS, and JavaScript.
Similar types of asets are printed in the same bundle, and different types of asets are made into child bundles and referred to the parent bundle.
For example, if you import a SCSS file from a `main.js` file (`import`./scss/main.scss`) it is made from a different bundle (`.js` file and `.css` file) and leaves a reference.

## Automatic conversion

It includes and supports the most commonly used transpilers such as Babel and PostCSS (especially Autoprepixers).
Inside the module, `.Automatically convert settings files such as `babelrc` and `.postcssrc`.

## HMR(Hot Module Replacement)

Built-in Hot Module Replication (HMR) that automatically updates modules without refreshing pages during runtime.
(Refreshes if automatic update fails)

## Split code without setting (Splitting

Dynamic import() function syntax proposal

The Dynamic import function `import()` splits code to build.
This is used similar to `require()`, `import` and returns Promise.
This means that you can load modules asynchronously.
Let`s apply one of the examples below to use the Dynamic import function.

```js
// msg.js
export const msg = {
hello: 'Hello Parcel Dynamic import!!'
};

```

```js
// main.js
import('./msg')
.then(function (module) {
console.log(module.msg.hello); // 'Hello Parcel Dynamic import!!'
});

```

Using the Dynamic import function as shown above, files are split as shown below.
One is the bundle of `main.js` and the other is `msg.`Bundle of js`.

![image](https://heropy.blog/images/screenshot/parcel_splitting_js_file.jpg)

## Productions

When the development is complete and the deployment build (productization) is completed, it automatically activates and executes Minify and Ugly.
Of course, you can deactivate it as follows:

```bash
$ parcel build entry.js --no-minify

```

# Installation

Now that we`ve identified the features, we`ll install them ourselves and use them.
Install as Global.

NPM Installation

```bash
$ npm install -g parcel-bundler

```

Install Yarn

```bash
$ yarn global add parcel-bundler

```

# Getting Started with Projects

I will make a simple Parcel app.
Create a folder called `Parcel-App` and create `package.json`.

```bash
$ mkdir Parcel-App
$ cd Parcel-App
$ npm init

```

Please make the following structure.

```bash
Parcel-App
├-src
│ ├-main.css
│ └-main.js
├-index.html
└-package.json

```

```xml
<!-- index.html -->
<!doctype html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport"
content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>Document</title>
</head>
<body>
<h1>Hello Parcel.js!</h1>
<script src="./src/main.js"></script>
</body>
</html>

```

```css
/*main.css*/
h1 {
color: tomato;
}

```

```js
// main.js
require('./main.css');
console.log('Hello Parcel!');

```

You just need to select the entry file to build with Parcel.

```bash
$ parcel index.html

```

You can view the built pages by accessing http://localhost:1234 from your browser.
If you want to change the port number, you can overwrite it with the `-p port` option.

```bash
$ parcel index.html -p 9876

```

You can connect to http://localhost:9876

## Using SCSS (SASS)

I will install `node-sass` module to compile SCSS.

```bash
$ npm install --save node-sass

```

If the installation is complete, I will check if the module is working properly.
Let`s change the project structure slightly.

```bash
Parcel-App
├-src
│ ├-scss
│ │ ├-main.scss
│ │ └-_config.scss
│ └-main.js
├-index.html
└-package.json

```

```undefined
/*_config.scss*/
$color-primary: royalblue;

```

```undefined
/*main.scss*/
@import 'config';

h1 {
color: $color-primary;
}

```

I created the `scss` directory, so please change the import path.

```js
// main.js
require('./scss/main.scss');
console.log('Hello Parcel!');

```

```bash
$ parcel index.html

```

Does it work well?
You can use it as soon as you install the module, even if you don`t have any additional settings.

## Using PostCSS

This time, I will convert CSS to PostCSS and use it.
During PostCSS plugins, add an autoprepixer that provides Vendor Prefix.

```bash
$ npm install --save autoprefixer

```

so that the editor can interpret the file correctly.Create a postcssrc` file.

```bash
Parcel-App
# ...
│ └-main.js
├-.postcssrc
├-index.html
└-package.json

```

```undefined
/*.postcssrc*/
{
"plugins": {
"autoprefixer": true
}
}

```

Let me check if Vendor Prefix is applied well.
I will apply FlexBox function that is used a lot recently.

```undefined
/*main.scss*/
@import 'config';

h1 {
color: $color-primary;
display: flex;
justify-content: center;
align-items: center;
}

```

```bash
$ parcel index.html

```

Let`s open the built CSS file in the `dist` directory to see if Vendor Prefix is applicable.

```css
/*dist/a9b13cfdff4b53240fb7925101bb19f2.css*/
h1 {
color: royalblue;
display: -webkit-box;
display: -ms-flexbox;
display: flex;
-webkit-box-pack: center;
-ms-flex-pack: center;
justify-content: center;
-webkit-box-align: center;
-ms-flex-align: center;
align-items: center; }

```

Vendor Prefixes such as `-webkit-` and `-ms-` are applied.

## Using the Babel

Let`s install Babel and use ES6 grammar.

```bash
$ npm install --save babel-preset-env

```

```bash
Parcel-App
# ...
│ └-main.js
├-.babelrc
├-.postcssrc
├-index.html
└-package.json

```

"In the project route.Creates a `babelrc` setup file and enables preset activation for ES2015+ use.

```undefined
/*.babelrc*/
{
"presets": ["env"]
}

```

As expected, I will modify the structure a little bit.

```bash
Parcel-App
├-src
│ ├-js
│ │ ├-main.js
│ │ └-msg.js
│ └-scss
│ ├-main.scss
│ └-_config.scss
├-.babelrc
# ...

```

```js
// msg.js
export const msg = {
hello: 'Hello Parcel with Babel!!'
};

```

```js
// main.js
import '../scss/main.scss';
import { msg } from './msg';

console.log(msg.hello);

```

Modify the call path to match the changed structure.

```xml
<!-- index.html -->
<!-- ... -->
<h1>Hello Parcel.js!</h1>
<script src="src/js/main.js"></script>
</body>
</html>

```

```bash
$ parcel index.html

```

Can you see the message `Hello Parcel with Babel!!` in the developer tool console?

If the development is complete and you want to build (productize) for deployment, enter as follows:
Automatically execute Minify and Ugly of the code.

```bash
$ parcel build index.html

```