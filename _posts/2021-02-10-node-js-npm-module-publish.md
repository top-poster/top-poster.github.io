---
layout: post
title: "Deploying My NPM Package (Module)"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/npm.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/npm.png)

As more modules are installed with `npm installxx` for development, I wanted to provide my frequently used code in the same way.
However, it is easier to copy and paste the code, so I started to have a chance to postpone it.
At the same time, you cleaned up the courses required for NPM deployment.

# NPM Package (Module) Deployment

![image](https://heropy.blog/images/screenshot/node-js-npm-module-publish/node-js-npm-module-publish1.jpg)

> The module is a file or directory in the 'node_modules' directory that can be loaded by the Node.js 'require()' function.
Not all modules are packages because they do not need to have a 'package.json' file.
Only modules with 'package.json' files are packaged.

## Package Structure

The following package structures are recommended:

- `dist/` is the directory for deployment. You can create a file for deployment with a bundleer such as Webpack or Parcel or Gulp.
- `examples/` is the example directory to provide. It`s even better if you provide a Demo page with an example file.
- `lib/` contains the original file for package creation.

```undefined
my_npm_module/
├-- dist/
├-- examples/
├-- lib/
├-- .gitignore
├-- .npmignore
├-- LICENSE
└-- package.json

```

## Package Type

As shown in the following example, `AMD`, `UMD`, `CommonJS`, and `ES Module` are designated.

```undefined
dist/
├-- my_module.js (UMD)
├-- my_module.min.js (UMD, compressed)
├-- my_module.common.js (CommonJS, default)
└-- my_module.esm.js (ES Module)

```

For more information on the package format, see the 10 minute introduction to JavaScript module, module format, module loader and module bundler.

Please refer to output.libraryTarget if you use the package bundleer or UMD if you use Parcel.

## package.json settings

For better understanding, some examples have referenced axios/package.json.

### name

A package (module) name within 214 lowercase letters that will be used as part of a URL or Command Line, set a name that is concise and intuitive, but unique so that it does not overlap with other modules.

### version

Specifies the version of the semantic versioner for npm (SemVer) that can be analyzed.

```undefined
{
"version": "0.19.0-beta.1"
}

```

### description

Specifies the description of the project (package).
(This is helpful when using npm search.)

```undefined
{
"description": "Promise based HTTP client for the browser and node.js"
}

```

### keywords

Specifies the keyword of the project (package) as an array.
(This is helpful when using npm search.)

```undefined
{
"keywords": [
"xhr",
"http",
"ajax",
"promise",
"node"
]
}

```

### homepage

Specify the URL to the project homepage as follows:

```undefined
{
"homepage": "https://github.com/axios/axios"
}

```

### bugs

Specifies the URL for the issue tracker (tracking system) and email address that will be reported when there is a problem with the package.

```undefined
{
"bugs": {
"url": "https://github.com/axios/axios/issues",
"email": "issues@axios.com"
}
}

```

### license

You license the package so that you know how to allow it to be used and its restrictions.
Open Source Licenses

```undefined
{
"license": "MIT"
}

```

Use the SPDX license expression if the package is multiple public licenses.

```undefined
{
"license": "(MIT OR Apache-2.0)"
}

```

It is no longer used as follows:

```undefined
// Not valid metadata
{
"licenses" : [
{
"type": "MIT",
"url": "https://www.opensource.org/licenses/mit-license.php"
},
{
"type": "Apache-2.0",
"url": "https://opensource.org/licenses/apache2.0.php"
}
]
}

```

### author

Specifies the name of the author.

```undefined
{
"author": {
"name": "Steven Wanderski",
"email": "steven@bxcreative.com",
"url": "http://stevenwanderski.com"
}
}

```

You can reduce it to one string.
The NPM parses automatically.

```undefined
{
"author": "Steven Wanderski <steven@bxcreative.com> (http://stevenwanderski.com)"
}

```

### contributors

If there are multiple creators, they can be used instead of the `author` option (field).

```undefined
{
"contributors": [
"Evan <evan@zillinks.com> (https://zillinks.com)",
"Lewis <lewis@zillinks.com> (https://zillinks.com)",
"Neo <neo@zillinks.com> (https://zillinks.com)"
]
}

```

### main

Specifies the default entry point for the program.
The package is named `axios`, and when the user uses `require` or `import `axios` the exports object is returned from the main module of the entry point.

```undefined
{
"main": "dist/my_module.min.js"
}

```

### repository

Specifies the place (address) where the code exists.
The `npm docs` command makes it easy to locate the package store.

```bash
$ npm docs axios

```

```undefined
{
"repository": {
"type": "git",
"url": "https://github.com/axios/axios.git"
}
}

```

### files

An array of files (directories) to be included together when a package is installed as a dependency.
If omitted, all files are included except those set to Auto Exclude.

```undefined
{
"files": [
"dist",
"lib",
"!dist/test"
]
}

```

### browser

When using the client (client-side), you must use it instead of the `main` option (field) to provide hints to the JavaScript bundler or component tools.
For more information, see Package browser field spec.

```undefined
{
"browser": "./browser/specific/main.js"
}

```

### dependencies

Specifies the dependency module to be included in the package`s deployment.

```undefined
{
"dependencies": {
"follow-redirects": "^1.4.1",
"is-buffer": "^2.0.2"
}
}

```

### peerDependencies

Specifies the compatibility module for the package.
(Not included in deployment since npm@3, instead a warning message will be displayed if no compatibility module is present)

```undefined
{
"name": "bootstrap",
"peerDependencies": {
"jquery": "1.9.1 - 3",
"popper.js": "^1.12.9"
}
}

```

### engines

Specifies the Node version in which the package works.

```undefined
{
"engines": {
"node": ">=0.10.3 <0.12",
"npm" : "~1.0.20"
}
}

```

## README.md

Create a document to provide in the package.
You can organize the definitions, usage, examples, and more for the package.
NPM and most repositories support markdown grammar.

The following is part of axios/README.md.

```undefined
## Browser Support

![Chrome](https://raw.github.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png) | ![Firefox](https://raw.github.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png) | ![Safari](https://raw.github.com/alrra/browser-logos/master/src/safari/safari_48x48.png) | ![Opera](https://raw.github.com/alrra/browser-logos/master/src/opera/opera_48x48.png) | ![Edge](https://raw.github.com/alrra/browser-logos/master/src/edge/edge_48x48.png) | ![IE](https://raw.github.com/alrra/browser-logos/master/src/archive/internet-explorer_9-11/internet-explorer_9-11_48x48.png) |
--- | --- | --- | --- | --- | --- |
Latest ✔ | Latest ✔ | Latest ✔ | Latest ✔ | Latest ✔ | 11 ✔ |

[![Browser Matrix](https://saucelabs.com/open_sauce/build_matrix/axios.svg)](https://saucelabs.com/u/axios)

## Installing

Using npm:

```

## LICENSE

You can include open source licenses in your repository so that others can easily contribute.
Create a `LICENSE` or `LICENSE.md` (Markdown) file (all capital letters) throughout the package.

The following is a sample of the MIT License:

```undefined
MIT License

Copyright (c) 2019 HEROPY <thesecon@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

```

If GitHub manages the repository, you can select and submit a license template.
For more information, see Adding a license to a repository.

## .npmignore

Specifies the files or directories that the package does not distribute.
Without the `.npmignore` `.You can also use only the `gitignore` file.

However, it is recommended that each file be created separately for quick package distribution and efficient project storage management.

gitignore.io Template Source makes it easy and quick.You can create an npmignore` or `.gitignore` file.

`.gitignore` of axios

```undefined
*.iml
.idea
.tscache
.DS_Store
node_modules/
typings/
coverage/
test/typescript/axios.js*
sauce_connect.log
package-lock.json

```

`.npmignore` of axios

```undefined
**/.*
*.iml
coverage/
examples/
node_modules/
typings/
sandbox/
test/
bower.json
CODE_OF_CONDUCT.md
COLLABORATOR_GUIDE.md
CONTRIBUTING.md
COOKBOOK.md
ECOSYSTEM.md
Gruntfile.js
karma.conf.js
webpack.*.js
sauce_connect.log

```

## NPM Login

![image](https://heropy.blog/images/screenshot/node-js-npm-module-publish/node-js-npm-module-publish2.jpg)

User registration (membership) and login are required to deploy modules to NPM.
Let`s take a quick look at some of the CLI that we need to use.

`npm adduser` or `npm login`

Join as an NPM member (recommended to proceed on the NPM homepage)
Login NPM if you have an ID

> Sign up and get your e-mail authentication!

`npm logout`

Log out if you have an ID logged in to the NPM

`npm whoami`

Show logged in ID (username)

## Pre-deployment testing

You can test in your local environment before deployment.
Create a new test directory (`npm_install_test/`) based on the package directory you created (`my_npm_module/`) as follows:

```bash
$ cd ..
$ mkdir npm_install_test
$ cd npm_install_test
$ npm init -y

```

I will proceed `npm install` with relative path to check if my package works properly.
The relative path is my package directory, not my file, as follows:
If the package is written properly, it will work well according to the `main` option of `package.json`, right?

```bash
$ npm install ../my_npm_module

```

I will bring my package within the new test project and test it to see if it works properly.
As shown in the following example, if the functions I want work normally, I am ready to distribute them.

```js
import myModule from 'my_npm_module';
// const myModule = require('my_npm_module');

const message = myModule.run();
console.log(message);
// 'Hello world!'

```

## Deployment

You will now deploy the module you created to the NPM.

`npm publish`

If the deployment is normal, the following message can be seen based on the virtual module `my_npm_module`:

```bash
# ...
npm notice === Tarball Details ===
npm notice name: my_npm_module
npm notice version: 0.1.0
npm notice package size: 591.2 kB
npm notice unpacked size: 6.3 MB
npm notice shasum: ab1234aa331abcd30f0f8d854ab6390123456789
npm notice integrity: sha512-AbcDEfGHuBczK[...]27123Cd65mMTQ==
npm notice total files: 422
npm notice
+ my_npm_module@0.1.0

```

It may take some time to reflect in the actual NPM search results.
Therefore, `https://www.npmjs.You can access by `com/package/my_npm_module`.
Verify that the configured module is working properly.

## Use

It`s all over now.
You can install and use it as if you usually use other packages.

```bash
$ npm install my_npm_module

```

# References

https://docs.npmjs.com/about-package-readme-files
https://help.github.com/articles/adding-a-license-to-a-repository/
https://blog.outsider.ne.kr/829
https://heropy.blog/2018/02/18/node-js-npm/
https://trustyoo86.github.io/webpack/2018/01/10/webpack-configuration.html
https://medium.com/webpack/the-state-of-javascript-modules-4636d1774358