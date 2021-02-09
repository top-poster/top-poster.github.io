---
layout: post
title: "Webpack - 3 - Term Organization"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/webpack.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/webpack.png)

# Webpack Config

If you configure the `webpack.config.js` file while `following the Webpack foundation`, the following properties appear:
Let`s find out what each attribute means.

## entry

Where `Webpack` begins to read the file (entry point).
Interpret dependencies between modules such as `require` and `@import` and create a dependency tree to set one or more entry points.

```js
{
entry: './src/app.js'
}

```

```js
// Set at least one entry point
{
entry: [
'./src/app.js',
'./src/main.js'
]
}

```

```js
The `// property (key) can be used to set one or more entry points
{
entry: {
app: './src/app.js'
}
}

```

## output

The setting that returns the result (bundle) based on the dependency tree set by `entry`.

`output.path`: specifying the path where the output will be stored
`output.filename`: specifying the file name of the output

### Setting parameters

`[name]: `key` name of `entry`

```js
{
entry: './src/app.js'
output: {
path: '/dist',
filename: 'app.bundle.js'
}
}

```

```js
Use the `// parameter to name the bundle
{
entry: {
app: './src/app.js'
},
output: {
path: '/dist',
filename: '[name].bundle.js'
}
}

```

## resolve

Setting the interpretation method of a module called `require()`

`resolve.root`: Specify the path to start exploring modules (default: `node_modules/`)
`extensions`: specify the extension of the module to be explored

```js
{
resolve: {
extensions: ['', '.js', '.css']
}
}

```

## module

Set how modules are handled within the dependency tree.

`test`: setting up files that apply in regular expressions
`use`: Compile files applied by specified `loader` in `test`
`exlude`: Exclude folders or files that you do not want to compile
`include`: Specify a separate folder or file to compile

```js
{
module: {
rules: [
{
// Sass Loader
test: /\.sass$/,
use: ExtractTextPlugin.extract({
fallback: 'style-loader',
use: [
'css-loader',
'sass-loader'
],
publicPath: '/dist'
})
},
{
// Babel Loader
test: /\.js$/,
exclude: /node_modules/,
use: "babel-loader"
}
]
}
}

```

## plugins

Set up a variety of plug-ins to handle how the deliverables are handled, etc. after bundling

```js
{
plugins: [
// Use EJS Template
new HtmlWebpackPlugin({
title: 'Hello Webpack Project!',
minify: {
collapseWhitespace: true
},
hash: true,
template: path.join(__dirname, '/src/index.ejs')
}),
// Extracting to single file`
new ExtractTextPlugin('app.css')
]
}

```