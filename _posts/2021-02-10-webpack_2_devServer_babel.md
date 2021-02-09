---
layout: post
title: "Webpack - 2 - Dev Server / Babel"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/webpack.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/webpack.png)

Now we`re going to learn about Webpack Dev Server.
We will configure `Babel` to use ES6 or higher grammar.

# To configure a project:

```bash
$ npm init -y

```

Configure the directory and create a file as below.

```bash
webpack-test
├-src
│ ├-js
│ │ ├-app.js
│ │ └-hello.js
│ ├-scss
│ │ ├-app.scss
│ │ └-hello.scss
│ └-index.ejs
├-package.json
└-webpack.config.js

```

```undefined
// package.json
{
"name": "webpack-test",
"version": "1.0.0",
"description": "",
"main": "index.js",
"scripts": {
"dev": "webpack -d --watch",
"prod": "webpack -p"
},
"keywords": [],
"author": "",
"license": "ISC",
"devDependencies": {
"css-loader": "^0.28.7",
"extract-text-webpack-plugin": "^3.0.1",
"html-webpack-plugin": "^2.30.1",
"node-sass": "^4.5.3",
"sass-loader": "^6.0.6",
"style-loader": "^0.19.0",
"webpack": "^3.8.1"
}
}

```

```js
// webpack.config.js
var HtmlWebpackPlugin = require('html-webpack-plugin');
var ExtractTextPlugin = require('extract-text-webpack-plugin');
var path = require('path');

module.exports = {
entry: path.join(__dirname, '/src/js/app.js'),
output: {
path: path.join(__dirname, '/dist'),
filename: 'app.bundle.js'
},
resolve: {
extensions: ['.js']
},
module: {
rules: [
{
test: /\.scss$/,
use: ExtractTextPlugin.extract({
fallback: 'style-loader',
use: [
'css-loader',
'sass-loader'
],
publicPath: '/dist'
})
}
]
},
plugins: [
new HtmlWebpackPlugin({
title: 'Hello Webpack Project!',
minify: {
collapseWhitespace: true
},
hash: true,
template: path.join(__dirname, '/src/index.ejs')
}),
new ExtractTextPlugin('app.css')
]
};

```

```xml
<!-- index.ejs -->
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title><%= htmlWebpackPlugin.options.title %></title>
</head>
<body>
<div class="container">
<h1>Hello Webpack!</h1>
</div>
</body>
</html>

```

```js
// app.js
require('../scss/app.scss');
require('./hello');

```

```js
// hello.js
console.log('Hello Webpack!');

```

```undefined
/* app.scss */
body {
background: tomato;
}
@import 'hello.scss';

```

```undefined
/* hello.scss */
.container {
h1 {
color: white;
}
}

```

If the configuration is complete, install the package.

```bash
$ npm i

```

# install webpack-dev-server

`webpack-dev-server` provides development servers for use locally.
Use this feature to watch the source file and recompile the bundle whenever the content changes.
It is convenient because it does not refresh frequently.

```bash
$ npm i webpack-dev-server -D

```

Modify the `dev` command to execute `webpack-dev-server`.

```undefined
// package.json
"scripts": {
"dev": "webpack-dev-server",
"prod": "webpack -p"
},

```

Add the `devServer` property as part of `webpack.config.js`.

```js
// webpack.config.js
module: {
// ...
},
devServer: {
contentBase: path.join(__dirname, 'dist'),
compress: true,
port: 9000
},
plugins: [
// ...
]

```

`contentBase`: Directory settings to provide static files, monitor source files, and recompile bundles whenever changes occur.
`Compress`: Whether to use `gzip` compression
`port`: Port settings

# Installing the Babel

```bash
$ npm i babel-loader babel-core -D

```

```js
// webpack.config.js
module: {
rules: [
{
// ...
},
{
test: /\.js$/,
exclude: /node_modules/,
use: "babel-loader"
}
]
},

```

`exlude`: setting for conditions to exclude

## creating .babelrc

"In the project route.Create a `babelrc` file to create settings and activate some plug-ins.

```bash
webpack-test
├-src
│ ├-js
│ │ ├-app.js
│ │ └-hello.js
│ ├-scss
│ │ ├-app.scss
│ │ └-hello.scss
│ └-index.ejs
├-.babelrc
├-package.json
└-webpack.config.js

```

`Envpreset` automatically sets the required Babel plug-ins based on supported environments.

```bash
npm i babel-preset-env -D

```

If `presets` does not have configuration options (`env`), it works the same as `babel-preset-latest` (`babel-preset-es2015`, `babel-preset-es2016) and `babel-preset-es2017`.

```undefined
// .babelrc
{
"presets": ["env"]
}

```

Now that setup is complete, use ES2015+ grammar.

```js
// app.js
import '../scss/app.scss';
import './hello';

```

```bash
$ npm run dev

```