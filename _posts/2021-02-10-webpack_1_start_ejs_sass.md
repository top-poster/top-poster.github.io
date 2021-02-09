---
layout: post
title: "Webpack - 1 - Getting Started / EJS / SASS (SCSS)"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/webpack.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/webpack.png)

# Getting Started

Create an empty project directory and use the terminal to create `package.json`.

```bash
$ npm init -y

```

And install `webpack`.

```bash
$ npm i -D webpack

```

Create the file `webpack.config.js` and create it as shown below.

```js
// webpack.config.js
module.exports = {
entry: './src/app.js'
output: {
filename: './dist/app.bundle.js'
}
};

```

It created `src` directory and added `app.js` file.

```js
// app.js
console.log('Hello Webpack!');

```

Create a `dist` directory and add `index.html` file.

```xml
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Webpack Test</title>
</head>
<body>
<script src="app.bundle.js"></script>
</body>
</html>

```

Create a directory structure as below.

```bash
webpack-test
├-dist
│ └-index.html
├-src
│ └-app.js
├-package.json
└-webpack.config.js

```

To operate as `webpack`, modify the contents of `scripts` in the `package.json` file as shown below.

```undefined
// package.json
"scripts": {
"dev": "webpack -d --watch", // 개발용 (development)
"prod": "webpack -p" // 제품용 (production)
}

```

Practice

```bash
"# for development
$ npm run dev

# For Products
$ npm run prod

```

When the `app.bundle.js` file is created in the `dist` directory,
Open the `index.html` file in a web browser to view the console window with developer tools.

```shell
'Hello Webpack!'

```

# Using the EJS Template

Let`s try the EJS template.

Now that the test is over, delete the dist directory.
Create the `index.ejs` file in the `src` directory and create it as shown below.

```xml
<!-- index.ejs -->
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title><%= htmlWebpackPlugin.options.title %></title>
</head>
<body>

</body>
</html>

```

You can create a detecory structure as shown below.

```bash
webpack-test
├-src
│ ├-app.js
│ └-index.ejs
├-package.json
└-webpack.config.js

```

Then, install the plug-in below to use the EJS template.

HTML Webpack Plugin

```bash
$ npm i html-webpack-plugin -D

```

Change the contents of the file `webpack.config.js` as shown below.

```js
// webpack.config.js
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
entry: './src/app.js',
output: {
path: __dirname + '/dist',
filename: 'app.bundle.js'
},
plugins: [
new HtmlWebpackPlugin({
title: 'Hello Webpack Project!',
template: './src/index.ejs'
})
]
};

```

Run it and check it out.

```bash
$ npm run dev

```

The structure of the detector changes as shown below.

```bash
webpack-test
├-dist
│ ├-app.bundle.js
│ └-index.html
├-src
│ ├-app.js
│ └-index.ejs
├-package.json
└-webpack.config.js

```

Finally, let`s apply some of the plug-in options.

```js
// webpack.config.js
new HtmlWebpackPlugin({
title: 'Hello Webpack Project!',
minify: {
collapseWhitespace: true
},
hash: true,
template: './src/index.ejs'
})

```

`colapseWhitespace`: Remove spaces from the document`s text node.
`hash`: Adds a unique Webpack compilation hash to all CSS or JS files. It`s useful for invalidating the cache.

# Using Style, CSS, and Sass Loaders

## Loaders 란?

> You can preprocess files when importing or 'Loading'. Similar to "Task" in other build tools such as 'Gulp' or 'Grunt' and provides a powerful way to handle front-end build steps. You can convert the file to 'JavaScript' in another language, such as 'TypeScript', or you can transform the inline image into a data URL. You can also do things like import CSS files directly from the JavaScript module.

## style-loader 와 css-loader 사용

To process CSS, install `style-loader` and `css-loader` first.

`style-loader`: Outputs CSS as a `style` tag.
`css-loader`: interprets CSS in JavaScript and addresses all dependencies.

```bash
$ npm i style-loader css-loader -D

```

Add `module` to `webpack.config.js`.

```js
// webpack.config.js
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
entry: './src/app.js',
output: {
path: __dirname + '/dist',
filename: 'app.bundle.js'
},
module: {
rules: [
{
test: /\.css$/,
use: [
'style-loader',
'css-loader'
]
}
]
},
plugins: [
new HtmlWebpackPlugin({
title: 'Hello Webpack Project!',
template: './src/index.ejs'
})
]
};

```

Create an `app.css` file and add a simple css code.

```css
/* app.css */
body {
background: tomato;
}

```

Import into the file `app.js`.

```js
// app.js
var css = require('./app.css');

```

When you run the `dist/index.html` file, you can see that the background color has changed.

```bash
$ npm dev run

```

## Using the sass-loader

Install the sass-loader.

```bash
$ npm i sass-loader node-sass -D

```

After installation, modify the parts written with `css` to `scss`.

```js
// webpack.config.js
test: /\.scss$/,
use: [
'style-loader',
'css-loader'
'sass-loader'
]

```

```js
// app.js
var css = require('./app.scss');

```

Let`s modify the extension of the CSS file and change the content to Sass (Scss) grammar.

```undefined
/* app.sass */
body {
background: tomato;

p {
color: white;
}
}

```

Modify the `index.ejs` file accordingly.

```xml
<!-- index.ejs -->
<body>
<p>Hello Sass!</p>
</body>

```

Run it.

```bash
$ npm run dev

```

## Extract to a single file

We merged `require(`/app.css`) code into `app.bundle.js` file.
However, as the size of the css file grows, large amounts of code are read inline within the bundle file, separate calls can be loaded in parallel and will work faster.

Install the extract text plugin for webpack.

```bash
$ npm i extract-text-webpack-plugin -D

```

Please modify the contents of the file `webpac.config.js` as below.

```js
// webpack.config.js
var ExtractTextPlugin = require('extract-text-webpack-plugin');
// ...
{
test: /\.scss$/,
use: ExtractTextPlugin.extract({
fallback: 'style-loader',
use: [
'css-loader'
'sass-loader'
],
publicPath: '/dist/' // '../images/'
})
}
// ...

```

`fallback`: Loader to be used when CSS is not extracted
`use`: Loader to convert to CSS export module
`publicPath`: Reset Path

```js
// webpack.config.js
// ...
{
plugins: [
new HtmlWebpackPlugin({
// ...
}),
new ExtractTextPlugin('app.css')
]
}
// ...

```

Next, let`s look at the server configuration for development (`webpack-dev-server`) to be used locally.