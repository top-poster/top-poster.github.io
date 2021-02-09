---
layout: post
title: "Test Vue Component Unit with Jest and Vue Test Utilities (VTU)"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/jest.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/jest.png)

# a general outline

Vue Test Utilities (VTU) is an official library for unit testing in a Vue.js environment.
Jest is a Facebook-created test framework and is a test runner recommended by Vue Test Utilities.
Test the Vue application using two open sources.

> This article is described based on the Jest 24 or higher version.
Installation and settings of the Babel etc. are different when using the version 23 or lower.

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/npm-trends-com.jpg)

# Unit Test

Unit testing refers to verifying that defined program minimum units, such as states, methods, and components, are operating normally independently.
This improves the reliability of the entire program and reduces the burden of code refactoring.

> This article is not based on Test Drive Development (TDD).

## Understanding Tests

Let`s understand the test through a simple example.
There is a CommonJS-style `calc.js` module with an `addOne` function that adds `1` to one argument as follows:

```js
// calc.js

exports.addOne = function (a) {
return a + 1
}

```

This `addOne` expects numeric data to be `normal as desired` as an argument.
(The expected normal behavior here means returning the result of a numeric operation and is a very subjective criterion)
However, the following character data arguments return unexpected results:

```js
// main.js

const { addOne } = require('./calc.js')

console.log(
addOne(1), // 2
addOne('1') // '11'
)

```

Let`s create a condition that can be re-verified at any time through the test without manually checking the expected value through the console output one by one.
The following tests were based on Jest.
Skip the information about test settings and behavior.

```js
// calc.test.js

// Import test targets into the test environment.
const { addOne } = require('./calc.js')

// Test 1
test('If argument is a number', () => {
// expect the argument result of expect() to be the argument value of .toBe().
expect(addOne(1)).toBe(2)
expect(addOne(7)).toBe(8)
})

// Test 2
test('If argument is a character', () => {
expect(addOne('1')).toBe(2)
expect(addOne('7')).toBe(8)
})

```

When you run the test, the test fails at `If argument is a character` as follows:

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/unit-test-basic-failed.jpg)

To succeed in the above test, let`s modify (refactory) the `addOne` function.
Use `parseFloat` to modify the character data to convert to numeric data, as follows:

```js
exports.addOne = function (a) {
return parseFloat(a) + 1
}

```

When you run the test again, the test passes as follows:

Through this process, we can improve the reliability of our code.
Adding or modifying new logic can also reduce the burden of problems based on passing the test.

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/unit-test_basic-passed.jpg)

# Preferences

You can install it globally to use the Best CLI.

```bash
$ npm i -g jest

```

## Set Up a Quick Environment with Vue CLI

Vue CLI makes it easy to quickly install and set up Jest and Vue Test Utilities during project initialization.

```bash
$ npm i -g @vue/cli
$ vue create YOUR_PROJECT_NAME
$ cd YOUR_PROJECT_NAME

```

> With Vue CLI 3 or later, you can install it in a similar way.
Select/Disable with the [Space] key, and move on to the next question with the [Enter] key.

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/vue-cli-install-options.jpg)

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/vue-cli-install-options-unit-testing.jpg)

When installed with Vue CLI, you have a test sample (E.g. `tests/unit/example.spec.js`) already prepared for testing.
(Delete if not required)

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/vue-cli-scripts.jpg)

```bash
$ npm run test:unit

```

For the other examples below, I will add a script as follows.
(We recommend a global installation of Jest if the following scripts do not work in some environments):

```undefined
{
"scripts": {
// ...
"test": "jest"
}
}

```

You can start testing more easily without the keyword `run`.

```bash
$ npm t

```

Alternatively, if you do not have a separate Best CLI option to specify, you can run with the `jest` keyword immediately.

```bash
$ jest

```

## Installing and Setting Up Separately

This time, we will introduce the installation process of Jest and Vue Test Utilities in a separate Vue project.
The surrounding settings are included, but it`s not difficult, so let`s take a look at them one by one.

### Install Required Modules

Install several required modules for testing:

```bash
$ npm i -D jest @vue/test-utils vue-jest jest-serializer-vue babel-jest babel-core@bridge

```

<table><thead><tr><th>module</th><th>Description</th></tr></thead><tbody><tr><td>vue-jest</td><
 td>Compile the Vue file into JavaScript that Jest can execute.</td></tr><tr><td>jest-serializer-vue</td><td>Improved the saved Jest Snapshot for VueJS.
 <td></tr><tr><td>babel-jest</td><td>Compiles the JS/JSX file into JavaScript that Jest can run.</td><
 /tr><tr><td>babel-core@bridge</td><td>Install for compatibility with Babel version 6.
 <a href="https://github.com/facebook/jest/issues/6913#issuecomment-417637086" target="_blank" rel="noopener">I have a related issue</a>.</td><
 /tr></tbody></table>
 

With the relatively modern NuxtJS or Vue CLI, there are already `@babel/core` and `@babel/preset-env` inside.
Check your project to decide whether to install additional modules on the following modules:

<table><thead><tr><th>module</th><th>Description</th></tr></thead><tbody><tr><td>@babel/core</td>
 <td>This is version 7 of Babel.</td></tr><tr><td>@babel/preset-env</td><td>Specifies the support specification for Babel.</td></tr
 ></tbody></table>
 

```bash
$ npm i -D @babel/core @babel/preset-env

```

### Jest Configuration Options

You can set the Jest configuration options in the file `jest.config.js`.
The following configuration options are a basic example:
I`m attaching a brief description to the code, so please refer to it.
See here for more Jest configuration options and make sure to check the default!

```js
// jest.config.js

module.exports = {
// This is a list of extensions that Jest will search for, unless a file extension is specified.
// Specifies the extensions of commonly used modules.
moduleFileExtensions: [
'js',
'jsx',
'json',
'vue'
],
// map path aliases such as '@' or '~'.
// E.g. `import HelloWorld from '~/components/HelloWorld.vue';`
// Use the 'rootDir' token to reference the root path.
// TODO: Modify to the appropriate path for your project!
moduleNameMapper: {
'^~/(.*)$': '<rootDir>/src/$1',
'^@/(.*)$': '<rootDir>/src/$1'
},
// Do not import modules from matching paths.
// Use the 'rootDir' token to reference the root path.
// TODO: Modify to the appropriate path for your project!
modulePathIgnorePatterns: [
'<rootDir>/node_modules',
'<rootDir>/build',
'<rootDir>/dist'
],
// Specifies the conversion module of the file that matches the regular expression.
transform: {
'^.+\\.vue$': 'vue-jest',
'^.+\\.jsx?$': 'babel-jest'
},
// Specifies the modules required for the Jest Snapshot test.
snapshotSerializers: [
'jest-serializer-vue'
]
}

```

Alternatively, you can clean up the jest configuration options in `package.json`, not in a separate file.

```undefined
// package.json

{
"jest": {
"moduleFileExtensions": [
// ...
],
// ...
}
}

```

### Babel Configuration Options

Set the file `.babelrc` or `babel.config.js` as follows:

> The ideal configuration of Babel may vary depending on the project.

```js
// babel.config.js

module.exports = {
presets: ['@babel/preset-env']
}

```

In a test environment, Jest automatically specifies `test` if `process.env.NODE_ENV` is empty.
The `env` merge option allows you to configure the Babel that will only work in a test environment.

> Use the '--no-cache' Best CLI option if you want to:

```js
module.exports = {
presets: ['@babel/preset-env']
env: {
test: {
presets: [[
'@babel/preset-env', {
debug: true
}
]]
}
}
}

```

### adding test scripts

Finally, add the test script to `package.json` as follows:
It can be operated through `npm test` and `npm t` without the `run` keyword.

```undefined
{
"scripts": {
"test": "jest"
}
}

```

Alternatively, you can use the `--watch` Jest CLI option.
This is convenient because you can verify that the file has changed and only run tests related to the changed file again.

```undefined
{
"scripts": {
"test": "jest --watch"
}
}

```

```bash
$ npm test
# or
$ npm t
# or
$ jest --watch

```

## Improvements

### Vuetify

When using the Vuetify UI library, the following errors can occur regardless of whether or not the test has passed:

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/use-vuetify-error.jpg)

Related issues are still open, so continuous verification is recommended.
The file path `jest.setup.js` can be specified in the `setupFilesAfterEnv` Best Config option for easy resolution.

```js
// jest.config.js

module.exports = {
// ...
// Before each test file runs,
// The execution code for configuring or setting up the test framework
// Specifies a list of module paths.
setupFilesAfterEnv: [
'./jest.setup.js'
]
}

```

```js
// jest.setup.js

import Vue from 'vue'
import Vuetify from 'vuetify'

Vue.use(Vuetify)

```

In addition, if you are using components that are connected by default to the root component of `v-app`, such as Vuetify`s `v-dialog`.
The following warning messages are visible because the test cannot find the root component:

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/use-vuetify-warning.jpg)

This adds the rendering results of the root component to the file `jest.setup.js` discussed above:

```js
// jest.setup.js

import Vue from 'vue'
import Vuetify from 'vuetify'

Vue.use(Vuetify)

// Create a <div> element to be replaced by the <v-app> root component and the test component
const app = '<div id="app" data-app="true"><div></div></div>'
document.body.innerHTML += app

```

When you mount a component in a test, use the attachTo option to specify the element to be replaced by the test component as a selector.
Specify the root component structure and its selectors for the project.
Be aware of `replaced` rather than being inserted as a child element of the selector.
The following example code uses the `attachTo` option:

```js
import { mount, createLocalVue } from '@vue/test-utils'
import Vuetify from 'vuetify'
import MyComponent from '../MyComponent'

const localVue = createLocalVue()
localVue.use(Vuetify)

describe('MyComponent', () => {
let wrapper
beforeEach(() => {
wrapper = mount(MyComponent, {
localVue,
attachTo: '#app > div',
vuetify: new Vuetify()
})
})
})

```

### Watchman

Watchman is a service that monitors file changes created by Facebook.
Install if you encounter problems with Jest`s `--watch` or `--watchAll` CLI options.

See Watchman Installation for installation information.
If you are using Homebrew, you can easily install it as follows:

```bash
$ brew update
$ brew install watchman

```

### No-undef error

Using global methods or objects such as `describe` or `test` in the test code may result in an error (no-undef) in ESLint.
The ESLint configuration option reads `env.Set to jest = true` to resolve.

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/eslint-error-no-undef.jpg)

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/eslint-error-no-undef-resolved.jpg)

### Unresolved~ Warning.

If you need to enable the Jest API`s predefined, installing Jest`s DefinitelyTyped can be a simple solution without setting the IDE option.

```bash
$ npm i -D @types/jest

```

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/unresolved-function-or-method.jpg)

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/unresolved-function-or-method-resolved.jpg)

### regeneratorRuntime~ 에러

When writing asynchronous code in a test, the following errors can occur:
There are many reasons for this error, but in many cases it can be solved simply by including @babel/polyfill.

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/regeneratorRuntime-error.jpg)

```bash
$ npm i -D @babel/polyfill

```

You can include it directly in the test or in the file `jest.setup.js`.
For setting up `jest.setup.js`, see the `Improvement > Vuetify` part.

```js
import '@babel/polyfill'

```

### jsdom upgrade

Starting with the Jest `25.1.0` version, the internally used jsdom was upgraded from v11 to v15.
However, if you want to use an older version of Jest or a newer version of jsdom (recommended, v16) at the point of writing this article, add the Jest configuration option with the following module:

<table><thead><tr><th>module</th><th>description</th></tr></thead><tbody><tr><td>jest-environment-jsdom-sixteen<
 /td><td>Specify jsdom version 16.</td></tr></tbody></table>
 

> The latest version of jsdom requires Node.js v10 or later.

```bash
# jest-environment-jsdom-MAJOR_VERSION
$ npm i -D jest-environment-jsdom-sixteen

```

```js
// jest.config.js

module.exports = {
// ...
// Specifies the test environment.
// default is 'jsdom' and configures a browser-like environment.
// Creating 'node' can provide an environment similar to NodeJS.
// If you are using the latest version of jsdom separately, specify the following post-installation options:
testEnvironment: 'jest-environment-jsdom-sixteen'
}

```

## Jest Coverage

You can check the coverage report of the test through Jest.
This report determines the extent to which the code for the test target (such as the Vue component) was executed through the test code you created.

### collectCoverage

Use the Jest configuration option `collectCoverage` to view the report.

```js
module.exports = {
// ...
collectCoverage: true
}

```

This report option significantly reduces test speed, so it is also recommended that you check as needed.
If you are uncomfortable modifying configuration options each time, you can use the Jest CLI option `--coverage` only if you are viewing the report.

```bash
$ jest --coverage

```

Now, let`s write a test code to verify the report.
Add one component simply as follows:

```xml
<!-- HelloJest.vue -->

<template>
<div>
{ reversedMsg }
</div>
</template>

<script>
export default {
data () {
return {
msg: 'Test coverage'
}
},
computed: {
reversedMsg () {
return this.reverseString(this.msg)
}
},
methods: {
reverseString (str) {
return str.split('').reverse().join('')
}
}
}
</script>

```

Now add the test code.
Something is wrong, but we will proceed with the test.

```js
// HelloJest.test.js

import { shallowMount } from '@vue/test-utils'
import HelloJest from '../HelloJest'

describe('HelloJest', () => {
test('1', () => {
shallowMount(HelloJest)
})
})

```

```bash
$ npm t
# or
$ jest --coverage

```

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/jest-coverage-without-test-code.jpg)

If you look at the test code carefully, there is no actual test code.
If you look at the test results, all items such as `Stmts` and `Branch` are marked as 100% coverage.

Each item is shown in the following table:
Percent (%) is the percentage of code executed (called) among all codes.

<table><thead><tr><th>items</th><th>description</th></tr></thead><tbody><tr><td>Statements(Stmts)</td>
 <td>Check execution of each statement</td></tr><tr><td>Branches</td><td>Check execution of each branch of conditional statements such as If and Switch</td></tr>
 <tr><td>Functions(Funcs)</td><td>Verify each function call</td></tr><tr><td>Lines</td><td>Verify the execution of each line
 (Similar to Statements)</td></tr><tr><td>Uncovered Line</td><td>Show lines of code that were not executed by testing</td></tr></tbody>
 </table>
 

All items are 100% because all code associated with `reversedMsg` was executed through the component mount.
The component will annotate some code and retest as follows:
You can now determine which part of the test target was not run through the test you created.

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/jest-coverage-vue-component-comment.jpg)

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/jest-coverage-without-test-code2.jpg)

### coverageReporters

If you want to see the report in more detail, you can change the report form to an HTML document using the Jest configuration option `coverageReporters`.
Modify the configuration options and run the test as follows:

```js
module.exports = {
// ...
collectCoverage: true
coverageReporters: ['html']
}

```

```bash
$ npm t

```

The report is no longer displayed in the terminal.
The specified option creates a new directory called `coverage` in the root path.
You can view the report by running the internal `index.html` file.

> If any changes occur, press Refresh.

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/jest-coverage-reporters-html-coverage-directory.jpg)

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/jest-coverage-reporters-html.gif)

### coverageThreshold

The jest configuration option `coverageThreshold` allows you to specify a minimum threshold for a report.
If this test does not meet the specified threshold, the test fails, which can help maintain a higher test reliability.

If you specify a positive number, the percentage is:
Negative means the number of unexecuted (called) code allowed.

To help you understand, let`s give you a simple example of not meeting the threshold.
If only one of the target`s three codes was executed in the test, two codes were not executed.
If the threshold is specified as `50` (positive), the test fails because the percentage of code executed is 33.33%.
If the threshold is specified as `-1` (negative), the test fails because it means that you will accept up to one unexecuted code.

```js
module.exports = {
// ...
collectCoverage: true
coverageReporters: ['html']
coverageThreshold: {
global: {
statements: 50,
branches: 50,
functions: 50,
lines: -1
}
}
}

```

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/jest-coverage-coverage-threshold.jpg)

## First Component Test

Let`s write a very simple test code to ensure that the test is working properly according to the preferences.

> The default project in this article is installed with the Vue CLI.
For the first installation, refer to the 'Quick Configuration with Vue CLI' part.

Create the file `src/components/HelloJest.vue` and create:

```xml
<!-- src/components/HelloJest.vue -->

<template>
<div>
{ msg }
</div>
</template>

<script>
export default {
data () {
return {
msg: 'Hello Jest~'
}
}
}
</script>

```

Jest uses `testRegex` configuration options.Explore all files in the project that match test` or `.spec`.
To test this component, add the `__tests__` directory and the internal `HelloJest.test.js` file to the `src/components/` path.
The final path is `src/components/__tests__/HelloJest.test.js`.

> Jest recommends creating the directory '__tests__' in the same path as the code to be tested.

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/first-test-directories.jpg)

Create the test as follows:

```js
// src/components/__tests__/HelloJest.test.js

import { shallowMount } from '@vue/test-utils'
import HelloJest from '../HelloJest'

describe('HelloJest', () => {
let wrapper
beforeEach(() => {
wrapper = shallowMount(HelloJest)
})
test('1', () => {
expect(wrapper.vm.msg).toBe('Hello Jest!')
})
})

```

Run the test.

```bash
$ npm t

```

The following error should occur:
It is now clear what modifications should be made to pass the test.

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/first-test-failed.jpg)

The test can also pass by invalid received and expected values.
Intentional failure can increase test reliability.
Especially if the test is complicated, you should be more careful.

# Jest

## Global members (Globals)

`describe` sets the scope of the test.
`test` sets the unit test.

```js
'describe('test range', () = {
test('Unit Test 1', () = {})
test('Unit Test 2', () = {})
})

```

### Hooks

Jest provides four global functions: `beforeAll`, `afterAll`, `beforeEach` and `afterEach`, which allow control of the front and back of the test code.

`beforeAll` and `afterAll` operate before and after within the declared `describe`.
`beforeEach` and `afterEach` operate around each `test` unit within the declared `describe`.

```js
describe('Jest Hooks', () => {
beforeAll(() => {
console.log('beforeAll')
})
afterAll(() => {
console.log('afterAll')
})
beforeEach(() => {
console.log('beforeEach')
})
afterEach(() => {
console.log('afterEach')
})

test('1', () => {
expect(1 + 1).toBe(2)
console.log('test 1 is done!')
})
test('2', () => {
expect(2 + 1).toBe(3)
console.log('test 2 is done!')
})
})

```

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/jest-hooks.jpg)

### Nesting

You can nest another `describe` within `describe`.

```js
describe('Vue Component', () => {
describe('Computed', () => {})
describe('Created', () => {})
describe('Mounted', () => {})
describe('Methods', () => {
describe('Sync', () => {})
describe('Async', () => {})
})
})

```

You can create multiple ranges and test them differently, as shown in the following example:

```js
import { shallowMount, mount } from '@vue/test-utils'
import HelloJest from '../HelloJest'

describe('HelloJest.vue', () => {
let wrapper

describe('ShallowMount', () => {
beforeEach(() => {
// Shallow Mount
// Does not include (rendering) subcomponents.
wrapper = shallowMount(HelloJest)
})
test('1', () => {
expect(wrapper.text()).toBe('Hello')
})
})

describe('Mount', () => {
beforeEach(() => {
// Mount
// Include subcomponents.
wrapper = mount(HelloJest)
})
test('1', () => {
expect(wrapper.text()).toBe('Hello World!')
})
})
})

```

### only, skip

There are `only` and `skip` members that can be used for `describe` and `test`.
If you want to run only a few tests, use `only`.
Use `skip` when you want to stop some tests.
Convenient because you do not need to delete or comment on each test and set.

```js
describe('Vue Component', () => {
describe.only('Computed', () => {
test('1', () => {})
test('2', () => {})
})
describe('Created', () => {
test('1', () => {})
test.only('2', () => {})
})
describe('Methods', () => {
test('1', () => {})
test('2', () => {})
})
})

```

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/jest-global-only-member.jpg)

```js
describe('Vue Component', () => {
describe.skip('Computed', () => {
test('1', () => {})
test('2', () => {})
})
describe('Created', () => {
test('1', () => {})
test.skip('2', () => {})
})
describe('Methods', () => {
test('1', () => {})
test('2', () => {})
})
})

```

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/jest-global-skip-member.jpg)

## Matchers

Matcher is used to determine the value received through `expect`.

Clean up each Matcher quickly.
Please refer to Matcher`s official documentation for more information.
Some Matcher contents have been cleaned up at the bottom.

The default usage is as follows:
`?` means an optional argument.

```js
'expect (received_value).MATCHER (expected value?');
```

Use the `.not` property in the middle to determine the opposite of the value received:

```js
'expect (received_value).not.MATCHER();

```

Common

<table><thead><tr><th></th><th>Matcher</th><th>Description</th><th>Remarks</th></tr></thead><tbody
 ><tr><td><a href="https://jestjs.io/docs/en/expect#tobevalue" target="_blank" rel="noopener">📎</a></td><td
 ><code>`.toBe(value)`</code></td><td>Compare received value with basic equality</td><td></td></tr><tr><td><
 a href="https://jestjs.io/docs/en/expect#toequalvalue" target="_blank" rel="noopener">📎</a></td><td><code>`.toEqual(
 Value)`</code></td><td>Deep equality comparison with received value</td><td><code>`{ a: undefined} === {}`</code><
 /td></tr><tr><td><a href="https://jestjs.io/docs/en/expect#tostrictequalvalue" target="_blank" rel="noopener">📎</a>
 </td><td><code>`.toStrictEqual(value)`</code></td><td>Strict deep equality comparison with received value</td><td><code>`{
 a: undefined} !== {}`</code></td></tr></tbody></table>
 

Data type

<table><thead><tr><th></th><th>Matcher</th><th>Description</th><th>Remarks</th></tr></thead><tbody ><tr><td><a href="https://jestjs.io/docs/en/expect#tobetruthy" target="_blank" rel="noopener">📎</a></td><td ><code>`.toBeTruthy()`</code></td><td>The received value is <a href="https://developer.mozilla.org/en/docs/Glossary/Truthy" target=" _blank" rel="noopener">Truthy</a> check value</td><td></td></tr><tr><td><a href="https://jestjs.io/ docs/en/expect#tobefalsy" target="_blank" rel="noopener">📎</a></td><td><code>`.toBeFalsy()`</code></td><td >Check if the received value is <a href="https://developer.mozilla.org/en/docs/Glossary/Falsy" target="_blank" rel="noopener">Falsy</a></td> <td></td></tr><tr><td><a href="https://jestjs.io/docs/en/expect#tobedefined" target="_blank" rel="noopener">📎 </a></td><td><code>`.toBeDefined()`</code></td><td>Check whether the received value is a defined value</td><td><code>` Values other than undefined`</code></td></tr><tr><td><a href="https://jestjs.io/docs/en/expect#tobeundefined" target="_blank" rel ="noopener">📎</a></td><td><code>`.toBeUndefined()`</code></td> <td>Check whether the received value is an undefined value</td><td><code>A value that is `undefined`</code></td></tr><tr><td><a href=" https://jestjs.io/docs/en/expect#tobenull" target="_blank" rel="noopener">📎</a></td><td><code>`.toBeNull()`</ code></td><td>Check if the received value is <code>`null`</code></td><td></td></tr><tr><td><a href=" https://jestjs.io/docs/en/expect#tobenan" target="_blank" rel="noopener">📎</a></td><td><code>`.toBeNaN()`</ code></td><td>Check if the received value is <code>`NaN`</code></td><td></td></tr><tr><td><a href=" https://jestjs.io/docs/en/expect#tobeinstanceofclass" target="_blank" rel="noopener">📎</a></td><td><code>`.toBeInstanceOf(class)`< /code></td><td>Check if the received value is an instance of a class</td><td><code>Use `instanceof`</code></td></tr><tr><td ><a href="https://jestjs.io/docs/en/expect#tobegreaterthannumber--bigint" target="_blank" rel="noopener">📎</a></td><td><code >`.toBeGreaterThan(number)`</code></td><td><code>` Received value &gt; check if number`</code></td><td></td></tr><tr><td><a href="https://jestjs.io/docs/en/expect#tobegreaterthanorequalnumber- -bigint" target="_blank" rel="noopener">📎</a></td><td><code>`.toBeGreaterThanOrEqual(number)`</code></td><td><code> `Check if the received value &gt;= number`</code></td><td></td></tr><tr><td><a href="https://jestjs.io/docs/ en/expect#tobelessthannumber--bigint" target="_blank" rel="noopener">📎</a></td><td><code>`.toBeLessThan(number)`</code></td> <td><code>` received value &lt; check if number`</code></td><td></td></tr><tr><td><a href="https://jestjs.io/docs/en/expect#tobelessthanorequalnumber- -bigint" target="_blank" rel="noopener">📎</a></td><td><code>`.toBeLessThanOrEqual(number)`</code></td><td><code> `Check if the received value &lt;= number`</code></td><td></td></tr><tr><td><a href="https://jestjs.io/docs/ en/expect#tobeclosetonumber-numdigits" target="_blank" rel="noopener">📎</a></td><td><code>`.toBeCloseTo(number, digit?)`</code></ td><td>Compare Received Value with Floating Point</td><td></td></tr><tr><td><a href="https://jestjs.io/docs/en/expect #tocontainitem" target="_blank" rel="noopener">📎</a></td><td><code>`.toContain(element)`</code></td><td> Make sure the element contains</td><td>Primitives</td></tr><tr><td><a href="https://jestjs.io/docs/en/expect #tocontainequalitem" target="_blank" rel="noopener">📎</a></td><td><code>`.toContainEqual(element)`</code></td><td> Deep check to see if the element is contained</td><td><code>`object`</code>, <code>`Array&lt;object&gt;`</code></td></tr>< tr><td><a href="https://jestjs.io/docs/en/expect#toma tchregexporstring" target="_blank" rel="noopener">📎</a></td><td><code>`.toMatch(Regular expression | String)`</code></td><td>Check if the received value matches the regular expression or contains a string</td><td></td></tr><tr><td><a href ="https://jestjs.io/docs/en/expect#tomatchobjectobject" target="_blank" rel="noopener">📎</a></td><td><code>`.toMatchObject(object) `</code></td><td>Check if the object is a subset of the values received</td><td><code>`object`</code>, <code>`Array&lt;object&gt;`</ code></td></tr></tbody></table>
 

Snapshots and Exceptions

<table><thead><tr><th></th><th>Matcher</th><th>Description</th><th>Remarks</th></tr></thead><tbody ><tr><td><a href="https://jestjs.io/docs/en/expect#tomatchsnapshotpropertymatchers-hint" target="_blank" rel="noopener">📎</a></td> <td><code>`.toMatchSnapshot()`</code></td><td>Creates or compares a snapshot of the received value</td><td></td></tr><tr> <td><a href="https://jestjs.io/docs/en/expect#tomatchinlinesnapshotpropertymatchers-inlinesnapshot" target="_blank" rel="noopener">📎</a></td><td>< code>`.toMatchInlineSnapshot(Inline Snapshot)`</code></td><td>Create or compare an inline snapshot of the received value</td><td><a href="https://developer. mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals" target="_blank" rel="noopener">template literal</a> snapshot</td></tr><tr><td>< a href="https://jestjs.io/docs/en/expect#tothrowerrormatchingsnapshothint" target="_blank" rel="noopener">📎</a></td><td><code>`.toThrowErrorMatchingSnapshot( )`</code></td><td>Creates or compares error snapshot when the received value (function) is called</td><td></td></tr><tr><td> <a href="https://jestjs.io/docs/en/expect#tothrowerrormatchinginlinesnapshoti nlinesnapshot" target="_blank" rel="noopener">📎</a></td><td><code>`.toThrowErrorMatchingInlineSnapshot()`</code></td><td>Received value (function) Create or compare error inline snapshots when is called</td><td><a href="https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals" target="_blank "rel="noopener">template literal</a> snapshot</td></tr><tr><td><a href="https://jestjs.io/docs/en/expect#tothrowerror" target="_blank" rel="noopener">📎</a></td><td><code>`.toThrow(error?)`</code></td><td>Received value (function) Check if an error occurs when is called</td><td>The received value is wrapped with a function<br><code>`expect(() =&gt; errFn(1, 2))`</code></td></tr></tbody></table>
 

toHave

<table><thead><tr><th></th><th>Matcher</th><th>Description</th><th>Remarks</th></tr></thead><tbody ><tr><td><a href="https://jestjs.io/docs/en/expect#tohavebeencalled" target="_blank" rel="noopener">📎</a></td><td ><code>`.toHaveBeenCalled()`</code></td><td>Check if the received value (function) was called</td><td></td></tr><tr><td ><a href="https://jestjs.io/docs/en/expect#tohavebeencalledtimesnumber" target="_blank" rel="noopener">📎</a></td><td><code>`. toHaveBeenCalledTimes(count)`</code></td><td>Check the number of calls of the received value (function)</td><td></td></tr><tr><td><a href ="https://jestjs.io/docs/en/expect#tohavebeencalledwitharg1-arg2-" target="_blank" rel="noopener">📎</a></td><td><code>`.toHaveBeenCalledWith (Argument 1, Argument 2, ...)`</code></td><td>Check whether the received value (function) was called with the corresponding argument</td><td></td></tr ><tr><td><a href="https://jestjs.io/docs/en/expect#tohavebeenlastcalledwitharg1-arg2-" target="_blank" rel="noopener">📎</a></td ><td><code>`.toHaveBeenLastCalledWith(argument1, argument2, ...)`</code></td><td>Check whether the last call of the received value (function) was called with the corresponding argument< /td><td></td></tr><tr><td><a href="https: //jestjs.io/docs/en/expect#tohavebeennthcalledwithnthcall-arg1-arg2-" target="_blank" rel="noopener">📎</a></td><td><code>`.toHaveBeenNthCalledWith(n Th, Argument 1, Argument 2, ...)`</code></td><td>Check whether the nth call of the received value (function) was called with the corresponding argument</td><td></ td></tr><tr><td><a href="https://jestjs.io/docs/en/expect#tohavereturned" target="_blank" rel="noopener">📎</a>< /td><td><code>`.toHaveReturned()`</code></td><td>Check that the received value (function) returns without error after calling</td><td><code >`undefined`</code> Include returns</td></tr><tr><td><a href="https://jestjs.io/docs/en/expect#tohavereturnedtimesnumber" target="_blank "rel="noopener">📎</a></td><td><code>`.toHaveReturnedTimes(number)`</code></td><td>The received value (function) is called without error. Check the number of times to return a value</td><td><code>`undefined`</code> Also includes return</td></tr><tr><td><a href="https:// jestjs.io/docs/en/expect#tohavereturnedwithvalue" target="_blank" rel="noopener">📎</a></td><td><code>`.toHaveReturnedWith(return value)`</code> </td><td>Compare whether there is the same value among the values returned by all calls to the received value (function)</td><td></td></tr><tr><td><a href= "https://jestjs.io/docs/en/expec t#tohavelastreturnedwithvalue" target="_blank" rel="noopener">📎</a></td><td><code>`.toHaveLastReturnedWith(return value)`</code></td><td> received Compare with the value returned by the last call of the value (function)</td><td></td></tr><tr><td><a href="https://jestjs.io/docs/en /expect#tohaventhreturnedwithnthcall-value" target="_blank" rel="noopener">📎</a></td><td><code>`.toHaveNthReturnedWith(nth, return)`</code></ td><td>Compare the value returned by the nth call of the received value (function)</td><td></td></tr><tr><td><a href="https:// jestjs.io/docs/en/expect#tohavelengthnumber" target="_blank" rel="noopener">📎</a></td><td><code>`.toHaveLength(number)`</code>< /td><td>Compare the received value with the <code>`.length`</code> attribute value</td><td></td></tr><tr><td><a href=" https://jestjs.io/docs/en/expect#tohavepropertykeypath-value" target="_blank" rel="noopener">📎</a></td><td><code>`.toHaveProperty(property path) , Value?)`</code></td><td>Check whether the property path exists in the received value or the corresponding value exists in the property path</td><td></td></tr></tbody ></table>
 

### toBe

The most basic Matcher, comparing the received and expected values to be the same.
Use to compare raw data (Primitives).

```js
const user = {
name: 'Neo',
age: 85
}
test('Name', () => {
expect(user.name).toBe('Neo')
})
test('Age', () => {
expect(user.age).toBe(85)
})

```

### toEqual과 toStrictEqual

`toEqual` and `toStictEqual` recursively compare all properties (elements) of an object or array data, which are called deep equivalent comparisons.
Where `deep` can be `go into the data and be meticulous.`

First of all, the following example using `toBe` fails:

```js
const user = {
name: 'Neo',
age: 85
}
test('User', () => {
expect(user).toBe({
name: 'Neo',
age: 85
})
})

```

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/jest-to-be-failed.jpg)

If you change `toBe` to `toStictEqual` as follows, the test will pass.

```js
// ...
test('User', () => {
// Passes
expect(user).toStrictEqual({
name: 'Neo',
age: 85
})
})

```

Although `toEqual` and `toStictEqual` are similar, there are some differences in severity as follows:
Only failed tests were marked with `//Fails`.

```js
test('Undefined', () => {
expect({ a: undefined }).toEqual({})
expect({ a: undefined }).toStrictEqual({}) // Fails

expect([undefined, 1]).toEqual([, 1])
expect([undefined, 1]).toStrictEqual([, 1]) // Fails
})
test('Class instance', () => {
class User {
constructor (name) {
this.name = name
}
}
expect(new User('Neo')).toEqual({ name: 'Neo' })
expect(new User('Neo')).toStrictEqual({ name: 'Neo' }) // Fails
})

```

### toBeCloseTo

When you convert a decimal number to a binary number in JavaScript, an infinite number of decimal places such as `0.1` or `0.2` decimal places will occur.
Therefore, the next test using `toBe` will fail.

```js
test('Floating point numbers', () => {
expect(0.1 + 0.2).toBe(0.3)
})

```

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/floating-point-number-operation.jpg)

If you change to `toBeCloseTo`, the test will pass.

```js
test('Floating point numbers', () => {
expect(0.1 + 0.2).toBeCloseTo(0.3) // Passes
})

```

### toContain과 toContainEqual

Both Matchers verify that the value (array) received contains certain elements.
There is a deep difference between `toContain` and `toContainEqual` and `toContainEqual` to determine whether or not the object data is included.

The following example makes it easier to understand the difference.

```js
test('Primitives', () => {
// Both Matchers can determine the inclusion of raw data in the array.
const numbers = [1, 2, 3]
expect(numbers).toContain(1)
expect(numbers).toContainEqual(1)

// toContainEqual cannot verify that it contains strings.
const string = 'Hello World'
expect(string).toContain('Hello')
expect(string).toContainEqual('Hello') // Fails
})

test('Objects', () => {
// toContain cannot verify that it contains object data.
const users = [
{ name: 'Neo' },
{ name: 'Evan' },
{ name: 'Lewis' }
]
expect(users).toContain({ name: 'Neo' }) // Fails
expect(users).toContainEqual({ name: 'Neo' })
})

test('Recommended', () => {
// Use toContainEqual for in-array inclusion checks.
const numbers = [1, 2, 3]
expect(numbers).toContainEqual(1)

// With strings is recommended to use toMatch.
const string = 'Hello World'
expect(string).toMatch('Hello')
})

```

### toThrow

Check if an error occurs when a function is called.
Note that the error-throwing function is not recognized as an error in the test itself when it is wrapped once as a separate function or when the function itself is used as a received value argument without a call.

The following example provides a more detailed understanding of `toThorrow`.

```js
function err (isPass) {
if (!isPass) {
throw new Error('I am your error.')
}
}

// Do not call the function that will cause the error directly!
test('Wrapping to a function', () => {
// Wrap with Anonymous Function
expect(() => { err() }).toThrow()
// Function argument without call
expect(err).toThrow()
// Fails
expect(err()).toThrow()
})

// Optionally, the following arguments are available:
test('Arguments', () => {
// substring of error message
expect(() => { err() }).toThrow('I am your error.')
expect(() => { err() }).toThrow('m your err')
// Regular expression matching error message pattern
expect(() => { err() }).toThrow(/^I.*errors?\.$/)
// Error instance matching error message
expect(() => { err() }).toThrow(new Error('I am your error.'))
// Error Class
expect(() => { err() }).toThrow(Error)
})

// The test fails if the function does not throw an error.
test('None error', () => {
// Fails - Received function did not throw
expect(() => { err(true) }).toThrow()
})

```

### toHaveBeenCalled

Verify that a function has ever been called in the test.
Importantly, the function must be being watched (spy) or simulated (mock) function.

In addition, `toHaveBeenCalled` only checks the call of a function, so even if the function throws an error, the test passes (when using a Try/Catch statement).
`toHaveReturned` is a good choice to make the test fail when throwing an error in the function.

```js
const user = {
name: 'Neo',
getName () {
return this.name
}
}

test('Called', () => {
jest.spyOn(user, 'getName') // now watch its function.
user.getName() // Called
expect(user.getName).toHaveBeenCalled() // Passes
})

```

### toHaveReturned

`toHaveReturned` is similar to `toHaveBeenCalled`, but it must have a return value as well as a function`s call of the function.
The function returns `undefined` by default without the keyword `return`, which `toHaveReturned` also confirms as the function`s return value.
Therefore, it can be used to verify that the function is called normally without any errors.
As with `toHaveBeenCalled`, the function must be being watched (spy) or simulated (mock) to be tested.

If you want to know what the return value is, use `toHaveReturnedWith`.

```js
const user = {
name: '', // None name..
getName () {
if (!this.name) {
throw new Error('Not Neo!')
}
return this.name
}
}

function displayName (user) {
try {
return user.getName()
} catch (err) {
// console.error(err)
}
}

test('Called', () => {
jest.spyOn(user, 'getName') // now watch its function.
displayName(user)
expect(user.getName).toHaveBeenCalled() // Passes
expect(user.getName).toHaveReturned() // Fails
})

```

### toHaveReturnedWith

The function is called and determines what value it returned.
If the function is called multiple times and each has a different return value, the test passes if the expected value matches the return of one of all calls.

```js
const users = {
names: ['Neo', 'Evan', 'Lewis', 'Emily'],
getName (order) {
return this.names[order]
}
}

test('Return value', () => {
jest.spyOn(users, 'getName')
users.getName(0) // Neo
users.getName(1) // Evan
users.getName(2) // Lewis
expect(users.getName).toHaveReturnedWith('Evan') // Passes
expect(users.getName).toHaveReturnedWith('Emily') // Fails
})

```

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/jest-to-have-returned-with.jpg)

### toHaveProperty

You can check the existence and value of the property that you want to find on the object.
When specifying the path (keyPath) of the property, you can use point notation or an array that lists the path of the property.

```js
const user = {
name: 'Neo',
age: 85
address: {
address1: Jongro 6, Jongno-gu, Seoul,
"Seoul Metropolitan Government,"
zonecode: '03187'
},
belongings: [
'phone',
'laptop',
{ mouse: 'MX Vertical' },
[100, 1000]
],
girlFriend: undefined,
getName () {
return this.name
}
}

// No tests fail below!
test('Key and Value', () => {
// verify that the age attribute exists on the user object.
expect(user).toHaveProperty('age')
You can also check the value of the //age property.
expect(user).toHaveProperty('age', 85)

// Point notation allows deeper entry.
expect(user).toHaveProperty('address.zonecode')
// You can also check the value of the attribute.
expect(user).toHaveProperty('address.zonecode', '03187')
// Instead of point notation, you can also use notation to list attribute paths in an array.
expect(user).toHaveProperty(['address', 'zonecode'], '03187')

// The notation that you list as an array can index elements if the value is array data.
expect(user).toHaveProperty(['belongings', 0], 'phone')
We could go deeper!
expect(user).toHaveProperty(['belongings', 2, 'mouse'], 'MX Vertical')
expect(user).toHaveProperty(['belongings', 3, 1], 1000)

// Be aware that there are attributes that have explicit undefined.
expect(user).toHaveProperty('girlFriend')
// Of course, you can check the method.
expect(user).toHaveProperty('getName')
})

```

## Asynchronous test pattern

In many cases, projects write asynchronous code.
You can easily test this asynchronous code in Jest.

As an example of a simple asynchronous function that runs after 0.5 seconds, the following test patterns are organized:
It won`t be difficult if you know about Promise.

```js
function asyncFn (hasToFail) {
return new Promise((resolve, reject) => {
setTimeout(() => {
if (hasToFail) {
reject(new Error('Fails!'))
}
resolve('Passes!')
}, 500)
})
}

// All the following tests pass:
describe('Async', () => {

// If test() uses Done as an argument,
// Wait for the test to be called.
// If don't call, the test fails after 5 seconds by default.
test('done', done => {
asyncFn().then(r => {
expect(r).toBe('Passes!')
done()
})
})

// When test() returns Promise,
// Wait for that Promise to be fulfilled.
// If rejected, the test fails.
test('return promise', () => {
return asyncFn().then(r => {
expect(r).toBe('Passes!')
})
})

// You can use two Bridge attributes (as in .not) between the received value (Expect) and the expected value (Matcher).
// .resolves waits for the Promise received to be fulfilled, and .rejects waits for rejection.
// Asertion (Expect syntax) must be returned before the test waits.
test('resolves', () => {
return expect(asyncFn()).resolves.toBe('Passes!')
})
test('rejects', () => {
return expect(asyncFn('hasToFail')).rejects.toThrow('Fails!')
})

// If you declare the callback of the test() as an async function,
// Wait for the test to match the created awit.
test('async/await', async () => {
const r = await asyncFn()
expect(r).toBe('Passes!')
})

})

```

The `test` function waits up to 5 seconds by default to complete the test.
The test will fail after 5 seconds, so you need to increase the waiting time depending on the situation.
Enter the time in milliseconds (ms) as the third argument of the `test` function.
The default is `5000`.

```js
This is the function used in the example above.
// increased the operating time from 0.5 seconds to 6 seconds.
function asyncFn (hasToFail) {
return new Promise((resolve, reject) => {
setTimeout(() => {
if (hasToFail) {
reject(new Error('Fails!'))
}
resolve('Passes!')
}, 6000) // 6 seconds.
})
}

describe('Async', () => {
// This test fails because it waits up to 5 seconds.
test('timeout 5000', async () => {
const h = await asyncFn()
expect(h).toBe('Passes!')
})

// This test passes because it waits up to 7 seconds.
// test (name, callback, time)
test('timeout 7000', async () => {
const h = await asyncFn()
expect(h).toBe('Passes!')
}, 7000)
})

```

# Vue Test Utils(VTU)

## Mountings

There are the following functions that connect and render Vue components for testing:

<table><thead><tr><th>function</th><th>return</th><th>features</th></tr></thead><tbody><tr><td>
 mount()</td><td>Wrapper</td><td>Mount (render subcomponents)</td></tr><tr><td>shallowMount()</td><td>Wrapper<
 /td><td>shallow mount (subcomponent stub)</td></tr><tr><td>render()</td><td>Promise<cheeriowrapper></cheeriowrapper></td><
 td>Render as string and return <a href="https://cheerio.js.org/" target="_blank" rel="noopener">Cheerio</a> Promise</td></
 tr><tr><td>renderToString()</td><td>Promise<string></string></td><td>Render as HTML</td></tr></tbody></
 table>
 

`render()` and `renderToString()` require installation of `@vue/server-test-utils`.

```bash
$ npm i -D @vue/server-test-utils

```

Import as follows:

```js
import { mount, shallowMount } from '@vue/test-utils'
import { render, renderToString } from '@vue/server-test-utils'

```

### mount vs shallowMount

There are `mount` and `shallowmount` functions that connect and render components in the test.
These functions return a wrapper object, which allows you to access the Vue instance (vm) or take advantage of the various methods provided.

Below is a very simple component that outputs `msg`.

> Refer to the 'Preferences > First Test' part for the basic structure of the project.
Although it is described as 'mount', we can see the same result even if 'shallowmount' is applied.
The differences are explained later.

```xml
<!-- src/components/HelloJest.vue -->

<template>
<h1>Hello { msg }</div>
</template>

<script>
export default {
data () {
return {
msg: 'Jest'
}
}
}
</script>

```

The following code tests the components above.

```js
// src/components/__tests__/Hello.test.js

import { mount } from '@vue/test-utils'
import HelloJest from '../HelloJest' // HelloJest.vue

// All tests below pass.
describe('HelloJest', () => {
let wrapper
beforeEach(() => {
// Mount the component and return the wrapper object.
// mount (component, optional)
wrapper = mount(HelloJest)
})

test('Vue instance', () => {
// Access the Vue instance with 'wrapper.vm'.
expect(wrapper.vm.msg).toBe('Jest')
})

test('Wrapper methods', () => {
// The wrapper object provides a variety of methods.
// .text() returns the text content of the rendered component.(similar to 'HTMLElement.innerText')
expect(wrapper.text()).toBe('Hello Jest')
})
})

```

It is very similar, but there is a big difference between `mount` and `shallowmount`.
`mount()` renders subcomponents as default mounts.
`shallowMount()` is a shallow mount that stubs subcomponents.

> Stub means an object that looks like it actually behaves.

Let`s take a look at the example first.
There are two parent/child components: `ChildComp.vue` and `ParentComp.vue`.

```xml
<!-- src/components/ChildComp.vue -->

<template>
<span>Child!</span>
</template>

```

```xml
<!-- src/components/ParentComp.vue -->

<template>
<div>
Hello <child-comp />
</div>
</template>

<script>
import ChildComp from './ChildComp'

export default {
components: {
ChildComp
}
}
</script>

```

Create a test code.
We do not create an Aspect syntax because we will only check the console.

```js
// src/components/__tests__/ParentComp.test.js

import { shallowMount, mount } from '@vue/test-utils'
import ParentComp from '../ParentComp'

describe('ParentComp', () => {
test('mount', () => {
const wrapper = mount(ParentComp)
console.log(wrapper.html())
})
test('shallowMount', () => {
const wrapper = shallowMount(ParentComp)
console.log(wrapper.html())
})
})

```

You can view the following results:

![image](https://heropy.blog/images/screenshot/vue-test-with-jest/vtu-mount-vs-shallowMount.jpg)

`mount` is also rendered with subcomponents, so `<span>Child!</span>` is output.

The `shallowmount` replaces the subcomponents with stub components (`<child-comp-stub></child-comp-stub>`) to make them appear to be functional.
This does not include subcomponents in the test, enabling independent testing.
This is especially useful if the test components have many subcomponents.

### Mounting options

You can specify the following additional options while mounting components in the test:

```js
'shallowMount (component, optional)

```

<table><thead><tr><th></th><th>options</th><th>description</th><th>type</th><th>default</th></ tr></thead><tbody><tr><td><a href="https://vue-test-utils.vuejs.org/api/options.html#context" target="_blank" rel=" noopener">📎</a></td><td><code>`context`</code></td><td>only <a href="https://vuejs.org/v2/guide/ render-function.html#Functional-Components" target="_blank" rel="noopener">Functional components</a> specify <code>`context`</code> to pass</td><td>Object< /td><td></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/options.html#slots" target=" _blank" rel="noopener">📎</a></td><td><code>`slots`</code></td><a href="https://vuejs. org/v2/guide/components-slots.html" target="_blank" rel="noopener">Specify the object to be inserted into Slots</a></td><td>Object</td><td>< /td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/options.html#scopedslots" target="_blank" rel="noopener" >📎</a></td><td><code>`scopedSlots`</code></td> <a href=" of <td>components https://vuejs.org/v2/guide/components -slots.html#Scoped-Slots" target="_blank "rel="noopener">Specify the object to be inserted into Scoped Slots</a></td><td>Object</td><td></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/options.html#stubs" target="_blank" rel="noopener">📎</a></td><td><code >`stubs`</code></td><td>Specify subcomponents to be stubbed</td><td>Object / Array</td><td></td></tr><tr>< td><a href="https://vue-test-utils.vuejs.org/api/options.html#mocks" target="_blank" rel="noopener">📎</a></td>< td><code>`mocks`</code></td><td>Specify a mock object or function</td><td>Object</td><td></td></tr>< tr><td><a href="https://vue-test-utils.vuejs.org/api/options.html#localvue" target="_blank" rel="noopener">📎</a></ td><td><code>`localVue`</code></td><td>Specify a local copy of the created Vue with <code>`createLocalVue`</code></td><td><code >`createLocalVue()`</code></td><td></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/ api/options.html#attachto" target="_blank" rel="noopener">📎</a></td><td><code>`attachTo`</code></td><td><code >When using `window`</code> or DOM-Events, specify the element selector to be replaced by the component to be mounted</td><td>HTMLElement / String< /td><td><code>`null`</code></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api /options.html#attachtodocument" target="_blank" rel="noopener">📎</a></td><td><code>`attachToDocument`</code></td><td>(Deprecated) When using <code>`window`</code> or DOM-Events, it is recommended to use the <code>`attachTo`</code> option instead of <br></td><td>Boolean</td>< td><code>`false`</code></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/options.html #attrs" target="_blank" rel="noopener">📎</a></td><td><code>`attrs`</code></td><td><a href to be received from parent component ="https://kr.vuejs.org/v2/api/index.html#vm-attrs" target="_blank" rel="noopener"><code>`$attrs`</code> object</a Specify ></td><td>Object</td><td></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org /api/options.html#propsdata" target="_blank" rel="noopener">📎</a></td><td><code>`propsData`</code></td><td>Component Specifies the props to be passed when the is mounted</td><td>Object</td><td></td></tr><tr><td><a href="https://vue-test- utils.vuejs.org/api/options.html#listeners" target="_blank" rel="noopener" >📎</a></td><td><code>`listeners`</code></td><td><a href="https://kr.vuejs.org/v2 received from parent component /api/index.html#vm-listeners" target="_blank" rel="noopener"><code>specify `$listeners`</code> object</a></td><td>Object</ td><td></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/options.html#parentcomponent" target="_blank "rel="noopener">📎</a></td><td><code>`parentComponent`</code></td><td>Specify parent component</td><td>Object</ td><td></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/options.html#provide" target="_blank "rel="noopener">📎</a></td><td><code>`provide`</code></td><td><a href="https://vuejs.org/v2 /api/#provide-inject" target="_blank" rel="noopener">Specify <code>`provide`</code> object</a> to be used in <code>`inject`</code></ td><td>Object</td><td></td></tr></tbody></table>
 

### createLocalVue

Returns the local Vue class to be used by the test.
A kind of fake Vue object that can be used to add mix-ins, plug-ins, etc. without contaminating the global Vue class.

For example, if your component is using VueRouter`s `router-link` or Vuex(Store)`s `state`, `actions`, etc., you can connect as an example:
Of course, you can stub the `router-link` or replace the store with a mock object for testing without actually creating the same environment.
The key is to create an environment in which Vue components can operate in a test.

```js
import { mount, createLocalVue } from '@vue/test-utils'
import VueRouter from 'vue-router'
import Vuex from 'vuex'

import App from '@/App.vue'
import router from '@/router.js'
import store from '@/store.js'

const localVue = createLocalVue()
localVue.use(VueRouter)
localVue.use(Vuex)

const wrapper = mount(App, {
localVue,
router,
store
})

```

The test example above is similar to the following familiar settings:

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Vuex from 'vuex'

import App from './App.vue'
import router from './router.js'
import store from './store.js'

Vue.use(VueRouter)
Vue.use(Vuex)

new Vue({
router,
store
render: h => h(App)
}).$mount('#app')

```

## Wrappers

Mounting components with `mount` or `shallowmount` in the test returns the wrapper object.

Clean up the properties and methods available in the wrapper object.
Please refer to the member`s official document for details.

The default usage is as follows:
`?` means an optional argument.

```js
const wrapper = shallowMount(Component)

const vm = wrapper.vm
const html = wrapper.html()

expect(vm.msg).toBe('Hello Jest~')
expect(html).toBe('<h1 class="title">Hello Jest~</h1>')

```

And among the methods organized below,
`wrapper.find()` returns another wrapper of the found result.
`wrapper.findAll()` returns a wrapper array of found results.

Wrapper and WrapperArray cleaned up in `Distinguish` because some of the available members are different.
Empty classification is only for Wrapper!

<table><thead><tr><th></th><th>member</th><th>description</th><th>division</th></tr></thead><tbody ><tr><td><a href="https://vue-test-utils.vuejs.org/api/wrapper/#properties" target="_blank" rel="noopener">📎</a>< /td><td><code>`.vm`</code></td><td>Return Vue instance (read-only)</td><td></td></tr><tr> <td><a href="https://vue-test-utils.vuejs.org/api/wrapper/#properties" target="_blank" rel="noopener">📎</a></td>< td><code>`.element`</code></td><td>Return root element (read-only)</td><td></td></tr><tr><td>< a href="https://vue-test-utils.vuejs.org/api/wrapper-array/#properties" target="_blank" rel="noopener">📎</a></td><td> <code>`.wrappers`</code></td><td>Return an array of all wrappers in the found result (read-only)</td><td>WrapperArray</td></tr><tr> <td><a href="https://vue-test-utils.vuejs.org/api/wrapper-array/#properties" target="_blank" rel="noopener">📎</a></td ><td><code>`.length`</code></td><td>Return array length (read-only)</td><td>WrapperArray</td></tr><tr>< td><a href="https://vue-test-utils.vuejs.org/api/wrapper-array/#at" target="_blank" rel="noopener">📎</a></td> <td><cod e>`.at(number)`</code></td><td>Zero based array indexing (negative numbers from the back), return wrapper</td><td>WrapperArray</td></tr> <tr><td><a href="https://vue-test-utils.vuejs.org/api/wrapper/#attributes" target="_blank" rel="noopener">📎</a></ td><td><code>`.attributes(attributes?)`</code></td><td>Returns the HTML attribute object or attribute value of the root element</td><td></td>< /tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/wrapper/#classes" target="_blank" rel="noopener">📎</a ></td><td><code>`.classes(class?)`</code></td><td> Returns an array of HTML class attribute values or presence or absence of attribute values of the root element</td>< td></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/wrapper/#contains" target="_blank" rel=" noopener">📎</a></td><td><code>`.contains(selector | Components)`</code></td><td>Check if a specific CSS selector or component is included</td><td>Wrapper / WrapperArray</td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/wrapper/#destroy" target="_blank" rel="noopener">📎</a></td><td><code> `.destroy()`</code></td><td>Destroy Vue instance (<code>`beforeDestroy`</code> and <code>`destroyed`</code> lifecycle hooks work)< /td><td>Wrapper / WrapperArray</td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/wrapper/#emitted" target ="_blank" rel="noopener">📎</a></td><td><code>`.emitted()`</code></td><td><code>`$emit(name , Value)`</code> return result object</td><td></td></tr><tr><td><a href="https://vue-test-utils.vuejs .org/api/wrapper/#emittedbyorder" target="_blank" rel="noopener">📎</a></td><td><code>`.emittedByOrder()`</code></td> <td><code><$emit(name, value)`</code> return result object</td><td></td></tr><tr><td><a href=" https://vue-test-utils.vuejs.org/api/wrapper/#exists" target="_blank" rel="noopener">📎</a></td><td><code>`.exists ()`</code></td><td>Wrapper(<code>`.find()`</code>) or WrapperArray(<code> Returns the existence of `.findAll()`</code>)</td><td></td></tr><tr><td><a href="https://vue-test- utils.vuejs.org/api/wrapper/#find" target="_blank" rel="noopener">📎</a></td><td><code>`.find(Selector | Component)`</code></td><td>Return a specific CSS selector or component wrapper</td><td></td></tr><tr><td><a href="https ://vue-test-utils.vuejs.org/api/wrapper-array/#filter" target="_blank" rel="noopener">📎</a></td><td><code>`. filter(number)`</code></td><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter" target Array filter similar to ="_blank" rel="noopener">Array.prototype.filter</a></td><td>WrapperArray</td></tr><tr><td><a href= "https://vue-test-utils.vuejs.org/api/wrapper/#findall" target="_blank" rel="noopener">📎</a></td><td><code>`. findAll(Selector | Component)`</code></td><td>Return the WrapperArray of a specific CSS selector or component</td><td></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/wrapper/#html" target="_blank" rel="noopener">📎</a></td><td><code> `.html()`</code></td><td>Return HTML as string</td><td></td></tr><tr><td><a href="https: //vue-test-utils.vuejs.org/api/wrapper/#get" target="_blank" rel="noopener">📎</a></td><td><code>`.get() Works the same as `</code></td><td><code>`.find()`</code>, If not, error</td><td></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org/api/wrapper/#is "target="_blank" rel="noopener">📎</a></td><td><code>`.is(selector | Component)`</code></td><td>Check if it matches a specific CSS selector or component</td><td>Wrapper / WrapperArray</td></tr><tr><td><a href ="https://vue-test-utils.vuejs.org/api/wrapper/#isempty" target="_blank" rel="noopener">📎</a></td><td><code>` .isEmpty()`</code></td><td>check if child element does not exist</td><td>Wrapper / WrapperArray</td></tr><tr><td><a href ="https://vue-test-utils.vuejs.org/api/wrapper/#isvisible" target="_blank" rel="noopener">📎</a></td><td><code>` .isVisible()`</code></td><td>(Deprecaed) Check if the element is visible (if <code>`display: none`</code> or <code>`visibility: hidden`</code> <code>`false`</code>)</td><td></td></tr><tr><td><a href="https://vue-test-utils.vuejs.org /api/wrapper/#isvueinstance" target="_blank" rel="noopener">📎</a></td><td><code>`.isVueInstance()`</code></td><td >(Deprecaed) Check if it is a Vue instance</td><td>Wrapper / WrapperArray</td></tr><tr><td><a href="https://vue-test-utils.vuejs.org /api/wrapper/#name" target="_blank" rel="noopener">📎</a></td><td><code>`.name()`</code></td><td > Component name or root element
 

# Test double

As we learn about the test, we start to see some difficult terms like Mock, Spy, and Stub.
These terms are commonly referred to as test double.
Just as the stunt double of film production replaces the real actor, it replaces the real object for testing.

Each has a slightly different meaning, and it is difficult to grasp the exact intention just by explaining it.
All of them can be organized as `fake objects for testing`.
Some documents may be mixed.

<table><thead><tr><th>Term</th><th>Description</th></tr></thead><tbody><tr><td>Dummy</td>
 <td>Object to fill data without actually working</td></tr><tr><td>Stub</td><td>It doesn't actually work, but it's prepared to appear to work.
 Objects that return only values</td></tr><tr><td>Fake</td><td>Objects that actually work but are not suitable for product</td></tr><tr>
 <td>Mock</td><td>Object that actually works but returns only prepared values</td></tr><tr><td>Spy</td><td>calls, etc.
 Object that monitors various situations by recording some information</td></tr></tbody></table>
 

# Create a simulated function (Mocking)

In test environments, functions that are actually implemented are often not available.
There are many external factors that prevent you from focusing on the test itself, such as functions that rely on external APIs that incur network costs, or unnecessarily changing many states (data) after the function runs.

What you need is a Mock function.
Because it`s literally `fake`, you can make sure that you return certain results right away, intentionally throw errors, monitor how many times a function has been called, etc. at no network cost.
This allows you to minimize disruptions and focus entirely on testing.

Personally, when I first encountered the word "Mock" in the test, it was very difficult to understand, but in fact, it is a word that I can usually encounter such as "Mock test," "Mock-up," and so on.

> [Method]: Try to imitate the real thing.

Let`s simply configure the components using the actual Todo API.
The following example takes Todo information to Axios and outputs a title (`title`) to the screen.

```xml
<!-- HelloJest.vue -->

<template>
<div>
{ todo.title }
</div>
</template>

<script>
import axios from 'axios'

export default {
data () {
return {
todo: {}
}
},
created () {
this.fetchTodo()
},
methods: {
async fetchTodo () {
// https://jsonplaceholder.typicode.com/
const { data } = await axios.get('https://jsonplaceholder.typicode.com/todos/1')
this.todo = data
}
}
}
</script>

```

The following is the `data` value of the actual response:
Check only `title` properties!

```undefined
{
"completed": false,
"id": 1,
"title": "delectus aut autem",
"userId": 1
}

```

You will now create the test as follows:
Note that you should consider the approximate amount of time the content can be reflected on the screen, as this should be through actual requests/response.
Therefore, `setTimeout` was written to be tested after 1 second.
If the network is slow, we may need to increase the time to 2 seconds or 3 seconds.

```js
// HelloJest.test.js

import { shallowMount } from '@vue/test-utils'
import HelloJest from '../HelloJest'

describe('HellJest', () => {
let wrapper
beforeEach(() => {
wrapper = shallowMount(HelloJest)
})
test('Imported Text Rendering normally', don = {
setTimeout(() => {
expect(wrapper.text()).toBe('delectus aut autem')
done()
}, 1000) // 1 second
})
})

```

At normal network speeds, the above tests pass.
However, because it uses actual requests/response, if the network is offline, the test fails, which is due to external factors.
Also, even though the network is online, testing can fail due to a variety of external factors such as very slow speed, problems with the API server, etc.

Then to eliminate these external factors (network costs), let`s make `axios.get()` a mock function and return only the required values.
The actual request creates the same `todo` object, which contains the `title` property, because it is responded to.
Modify as follows:

```js
// HelloJest.test.js

import { shallowMount } from '@vue/test-utils'
import axios from 'axios'
import HelloJest from '../HelloJest'

describe('HellJest', () => {
let wrapper
beforeEach(() => {
// Fake Response Results to Return
const response = {
data: {
title: 'delectus aut autem'
}
}
// Mock components before they are mounted.
// axios.get() uses mockResolvedValue because it returns Promise.
axios.get = jest.fn().mockResolvedValue(response)
// The above code is as follows:
// axios.get = jest.fn(() => new Promise(resolve => resolve(response)))

// Mount Component!
wrapper = shallowMount(HelloJest)
})
test('Imported Text Rendering normally', () = > {
// No longer requires setTimeout because you do not need to rely on the network (because it is a simulated request/response).
expect(wrapper.text()).toBe('delectus aut autem')
})
})

```

I`ll continue to explain.

## jest.fn()

As shown above, `jest.fn()` returns a mock function, which can be combined to create an implementation of the simulated function to be returned.
The mock function also monitors calls by default and has several additional methods available.

First, let`s find out about the function implementation based on the example we wrote above.
In fact, `axios.get` imports data asynchronously and returns Promise objects.
We make `axios.get` a mock function to avoid using network costs, but the logic of the components must work, so we have to create a structure in which Promise is returned.
Therefore, the first argument of `jest.fn()` creates a callback function where Promise can be returned as follows:
`jest.fn()` returns a mock function.You can use `mockImplementation(`)`.

> The various methods available for the mock function are organized below.

```js
axios.get = jest.fn(() => new Promise(resolve => resolve(response)))
// Or
// axios.get = jest.fn().mockImplementation(() => new Promise(resolve => resolve(response)))

```

Because there is no specific implementation and you only need to return the Promise you want.
The above method.You can replace it with the `mockResolvedValue()` method.

```js
// axios.get = jest.fn(() => new Promise(resolve => resolve(response)))
// Or
// axios.get = jest.fn().mockImplementation(() => new Promise(resolve => resolve(response)))
axios.get = jest.fn().mockResolvedValue(response)

```

The simulated function also monitors the call immediately after it is created.
Since `axios.get` is called from component `fetchTodo` and `fetchTodo` is called from `created` Hook,
Eventually, you can determine whether a call is made as follows:
(As a result, ``axios.get`` is called from the `created` hook, it must be made as a mock function before mounting.)

```js
// HelloJest.test.js

describe('HellJest', () => {
let wrapper
beforeEach(() => {
// ...
// axios.get = jest.fn(() => new Promise(resolve => resolve(response)))
axios.get = jest.fn().mockResolvedValue(response)
wrapper = shallowMount(HelloJest)
})
test('check import call', () = {
// determine whether the mock function is called.
expect(axios.get).toHaveBeenCalled() // Passes
})
})

```

## jest.spyOn()

`jest.spyOn()` can be very useful for call surveillance without overwriting the actual function as a mock function.

In the existing example, the component calls `fetchTodo` from the `created` Hook to get the desired data.
To verify that `fetchTodo` has been called since Mounting, use `jest.spyOn()` as follows:
(Because ``fetchTodo`` is called from the `created` hook, surveillance must be started before Mounting)

> The monitored target (method) is a simulated function.

```js
'const spy = jest.spyOn' (object, 'method name')

```

```js
// HelloJest.test.js

describe('HellJest', () => {
let wrapper
const spy = {}
beforeEach(() => {
// ...
// spy on the 'fetchTodo' method before components are mounted.
spy.fetchTodo = jest.spyOn(HelloJest.methods, 'fetchTodo')
wrapper = shallowMount(HelloJest)
})
test('check import call', () = {
// Spy can check for calls.
expect(spy).toHaveBeenCalled() // Passes
})
})

```

If the existing example `fetchTodo` needs to be implemented differently for testing, you can write the following example:
Like `jest.fn()`, `jest.spyOn()` also returns a mock function.You can use `mockImplementation(`)`.

```js
// HelloJest.test.js

describe('HellJest', () => {
let wrapper
const spy = {}
beforeEach(() => {
// ...
// Plant a spy and create a mock implementation.
spy.fetchTodo = jest.spyOn(HelloJest.methods, 'fetchTodo')mockImplementation(num => num + 1)
wrapper = shallowMount(HelloJest)
})
test('check import call', () = {
expect(spy.fetchTodo).toHaveBeenCalled() // Passes
expect(wrapper.vm.fetchTodo(1)).toBe(2) // Passes
})
})

```

To add one more explanation, `spy.fetchTodo` affects the next test because the call information was added through the previous test (`Confirm Import Call`).
Therefore, it is recommended to initialize the simulated function call information using `jest.clearAllMocks()` in the `afterEach` Hook as follows:

```js
// HelloJest.test.js

describe('HellJest', () => {
let wrapper
const spy = {}
beforeEach(() => {
// ...
spy.fetchTodo = jest.spyOn(HelloJest.methods, 'fetchTodo')mockImplementation(num => num + 1)
wrapper = shallowMount(HelloJest)
})
afterEach(() => {
// Initialize call information for all mock functions.
jest.clearAllMocks()
// You want to initialize only the desired simulated functions.
// spy.fetchTodo.mockClear()
})
test('check import call', () = {
expect(spy.fetchTodo).toHaveBeenCalled() // Passes
expect(wrapper.vm.fetchTodo(1)).toBe(2) // Passes
})
test('check import call next test', () = {
// If you do not initialize the call information of the mock function, the number of calls will be 3 instead of 1.
expect(spy.fetchTodo),toHaveBeenCalledTimes(1) // Passes
})
})

```

## Mock functions

The simulated functions returned through `jest.fn()` and `jest.spyOn()` can use several methods:
You can also see familiar methods from above.

<table><thead><tr><th></th><th>member</th><th>description</th></tr></thead><tbody><tr><td>< a href="https://jestjs.io/docs/en/mock-function-api#mockfnmockcalls" target="_blank" rel="noopener">📎</a></td><td><code> `.mock.calls`</code></td><td>Array of call information of mock function<br>(including arguments)</td></tr><tr><td><a href=" https://jestjs.io/docs/en/mock-function-api#mockfnmockresults" target="_blank" rel="noopener">📎</a></td><td><code>`.mock. results`</code></td><td>Array of result information from which the mock function was called<br>(Including result type and return value)</td></tr><tr><td><a href ="https://jestjs.io/docs/en/mock-function-api#mockfnmockinstances" target="_blank" rel="noopener">📎</a></td><td><code>`. mock.instances`</code></td><td>an array of instances created by the mock function<br>(if the mock function is a constructor)</td></tr><tr><td><a href ="https://jestjs.io/docs/en/mock-function-api#mockfngetmockname" target="_blank" rel="noopener">📎</a></td><td><code>`. getMockName()`</code></td><td>Return the name of the mock function<br>(default <code>`"jest.fn()"`</code>)</td></tr ><tr><td><a href="https://jestjs.io/docs/en/mock-function-api#mockfnmocknamevalue" tar get="_blank" rel="noopener">📎</a></td><td><code>`.mockName(name)`</code></td><td>Specify the name of the mock function </td></tr><tr><td><a href="https://jestjs.io/docs/en/mock-function-api#mockfnmockclear" target="_blank" rel="noopener"> 📎</a></td><td><code>`.mockClear()`</code></td><td>Mock function call information (<code>`.mock.xxx`</code> >) Initialize an array</td></tr><tr><td><a href="https://jestjs.io/docs/en/mock-function-api#mockfnmockreset" target="_blank" rel ="noopener">📎</a></td><td><code>`.mockReset()`</code></td><td>Reset all state of mock function</td></ tr><tr><td><a href="https://jestjs.io/docs/en/mock-function-api#mockfnmockrestore" target="_blank" rel="noopener">📎</a>< /td><td><code>`.mockRestore()`</code></td><td>Initialize all state of the mock function and restore it to the actual function<br>(<code>`jest.spyOn( )`</code> only implementation can be restored)</td></tr><tr><td><a href="https://jestjs.io/docs/en/mock-function-api #mockfnmockreturnthis" target="_blank" rel="noopener">📎</a></td><td><code>`.mockReturnThis()`</code></td><td> code>return `this`</code></td></tr><tr><td><a href="https://jestjs.io/doc s/en/mock-function-api#mockfnmockimplementationfn" target="_blank" rel="noopener">📎</a></td><td><code>`.mockImplementation(implementation)`</code>< /td><td>write the implementation part of the mock function</td></tr><tr><td><a href="https://jestjs.io/docs/en/mock-function-api#mockfnmockreturnvaluevalue "target="_blank" rel="noopener">📎</a></td><td><code>`.mockReturnValue(value)`</code></td><td>< code>Return `value`</code></td></tr><tr><td><a href="https://jestjs.io/docs/en/mock-function-api#mockfnmockresolvedvaluevalue" target="_blank" rel="noopener">📎</a></td><td><code>`.mockResolvedValue(value)`</code></td><td>Asynchronous mock function implemented( Resolved), return the specified <code>`value`</code></td></tr><tr><td><a href="https://jestjs.io/docs/en/mock-function -api#mockfnmockrejectedvaluevalue" target="_blank" rel="noopener">📎</a></td><td><code>`.mockRejectedValue(value)`</code></td><td>Asynchronous If the mock function is Rejected, return the specified <code>`value`</code></td></tr><tr><td><a href="https://jestjs.io/docs/ en/mock-function-api#mockfnmockimplementationoncefn" target="_blank" rel="noopener">📎</a></t d><td><code>`.mockImplementationOnce(implementation)`</code></td><td>(one time only) write the implementation of mock function</td></tr><tr><td> <a href="https://jestjs.io/docs/en/mock-function-api#mockfnmockreturnvalueoncevalue" target="_blank" rel="noopener">📎</a></td><td><code >`.mockReturnValueOnce(value)`</code></td><td>(Only once) Returns the <code>`value`</code> specified by the mock function</td></tr><tr> <td><a href="https://jestjs.io/docs/en/mock-function-api#mockfnmockresolvedvalueoncevalue" target="_blank" rel="noopener">📎</a></td><td ><code>`.mockResolvedValueOnce(value)`</code></td><td>(Only once) When the asynchronous mock function is resolved, the specified <code>`value`</code> is returned</code></td><td> td></tr><tr><td><a href="https://jestjs.io/docs/en/mock-function-api#mockfnmockrejectedvalueoncevalue" target="_blank" rel="noopener">📎< /a></td><td><code>`.mockRejectedValueOnce(value)`</code></td><td>(Only once) The specified <code>` value when the asynchronous mock function is rejected Return `</code></td></tr></tbody></table>
 

### mockClear

`.mockClear()` initializes all information about a simulated function call.
The call information is stored in an array of `.mock.calls` and `.mock.results`.

```js
const module = {
api: value => value + 'DEF'
}

test('mockClear', () => {
jest.spyOn(module, 'api')
// Or
// module.api = jest.fn().mockImplementation(module.api)

console.log(module.api.mock.calls) // []
console.log(module.api.mock.results) // []

Module.api ('ABC') // Called

expect(module.api).toHaveBeenCalledTimes(1) // Passes
console.log(module.api.mock.calls) // [['ABC']]
console.log(module.api.mock.results) // [{ type: 'return', value: 'ABCDEF' }]

module.api.mockClear() // Initialize call information for mock function

expect(module.api).toHaveBeenCalledTimes(0) // Passes
console.log(module.api.mock.calls) // []
console.log(module.api.mock.results) // []
})

```

### mockReset

`.mockReset()` initializes all states of the mock function.
The state here contains all call information and the simulated function implementation.
Therefore, the initialized mock function returns `undefined`.

> .mockReset() = .mockClear() + Reset

```js
const module = {
api: value => value + 'DEF'
}

test('mockReset', () => {
jest.spyOn(module, 'api')mockImplementation(() => 'Fake value..')
// Or
// module.api = jest.fn().mockImplementation(() => 'Fake value..')

expect(module.api('ABC')).ToBe('Fake value..') // Called
```

### mockRestore

`.mockRestore()` initializes all states of the mock function and restores the mock function implementation to the original function implementation.
You can only restore it if you used `jest.spyOn()`.

> .mockRestore() = .mockReset() + Restore

```js
const module = {
api: value => value + 'DEF'
}

test('mockRestore', () => {
jest.spyOn(module, 'api')mockImplementation(() => 'Fake value..') // Only `jest.spyOn()`

expect(module.api('ABC')).ToBe('Fake value..') // Called
```

# References

https://jestjs.io/docs/en/getting-started
https://vue-test-utils.vuejs.org/guides/
https://medium.com/@syalot005006/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-%EC%8B%A4%EC%88%98-%EA%B3%84%EC%82%B0-%EC%98%A4%EB%A
https://martinfowler.com/bliki/TestDouble.html