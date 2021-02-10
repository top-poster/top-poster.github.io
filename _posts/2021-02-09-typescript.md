---
layout: post
title: "TypeScript at a glance (updated)"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/typescript.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/typescript.png)

# Changes

## February 2020

- I added the following parts.
keyof <Interface/Indexable Types>
Type Aliases
- keyof <Interface/Indexable Types>
- Type Aliases
- I modified some contents and typos.

## March 2020

- I added the following parts.
Unknown Type £Type Default/Type Declaration £
Intersection £Type Base /Type Declaration >
Function type <
Class Type <
Interface Extensions >
Function
This <Function>
Explicit this <function>
Overloads <Function>
- Unknown Type £Type Default/Type Declaration £
- Intersection £Type Base /Type Declaration >
- Function type <
- Class Type <
- Interface Extensions >
- Function
- This <Function>
- Explicit this <function>
- Overloads <Function>
- Deleted part title `Index signature` for table of contents flow.(We didn`t delete the content)
- I modified some contents and typos.

## April 2020

- I added the following parts.
TS Node <Development Environment>
Module
Export and import 모
Ambient module declaration <
Definitely Typed(@types) <모듈>
TypeRoots and types options <
- TS Node <Development Environment>
- Module
- Export and import 모
- Ambient module declaration <
- Definitely Typed(@types) <모듈>
- TypeRoots and types options <
- Added multiple links to compilation options.
- I modified some contents and typos.

## June 2020

- I added the following parts.
Constraints <
Conditional Types <
infer <Generic>
Partial <TS Utility Type>
Required <TS Utility Type>
Readonly <TS Utility Type>
Record <TS Utility Type>
Pick <TS Utility Type>
Omit <TS Utility Type>
EXClude TS TS Utility Type
Extract <TS Utility Type>
NonNullible <TS Utility Type>
Parameters > TS Utility Type
ConstructorParameters >
ReturnType <TS Utility Type>
InstanceType <TS Utility Type>
ThisParameterType <TS Utility Type>
OmitThisParameter <TS Utility Type>
ThisType TS TS Utility Type
- Constraints <
- Conditional Types <
- infer <Generic>
- Partial <TS Utility Type>
- Required <TS Utility Type>
- Readonly <TS Utility Type>
- Record <TS Utility Type>
- Pick <TS Utility Type>
- Omit <TS Utility Type>
- EXClude TS TS Utility Type
- Extract <TS Utility Type>
- NonNullible <TS Utility Type>
- Parameters > TS Utility Type
- ConstructorParameters >
- ReturnType <TS Utility Type>
- InstanceType <TS Utility Type>
- ThisParameterType <TS Utility Type>
- OmitThisParameter <TS Utility Type>
- ThisType TS TS Utility Type
- Added information about `in` operator to `Type Guard` part.
- I modified some contents and typos.

# TypeScript Overview

TypeScript is an Apache licensed open source developed and maintained by Microsoft.
It was first released in October 2012 as a JavaScript Superset compiled into regular JavaScript.

## Why TypeScript?

Strong type systems used in systematic and refined languages such as C# and Java can provide high readability and code quality, and error occurs in non-runtime compilation environments, making fatal errors easier to catch.

JavaScript, on the other hand, is a dynamic programming language without a type system, and JavaScript variables can have multiple types of values, such as strings, numbers, and bulins.
This can be described as a weak type of language, providing an environment that can be developed relatively flexibly, while also having the disadvantage of easily failing runtime environments.

In addition, TypeScript applies a strong type system to these JavaScripts so that most errors can be checked while entering code in the compilation environment environment.

## How to Use TypeScript

JavaScript says.TypeScript can be written to a file with the `.ts` extension, as it is written to a file with the `.ts` extension, and then compiled into a JavaScript file through the TypeScript compiler.

```bash
$ tsc sample.ts
# compiled to `sample.js`

```

## Features of TypeScript

- Cross Platform Support: Available on any platform where JavaScript runs.
- Object-oriented language: Provides powerful features such as classes, interfaces, modules, and allows you to write pure object-oriented code.
- Static type: Because it uses static type, errors can be checked while entering code (but with the help of an editor or plug-in)
- DOM Control: You can add or delete elements by controlling DOM, such as JavaScript.
- Support for the latest ECMAScript features: Easily support the latest JavaScript syntax over ES6.

# Development Environment

## VSCode와 WebStorm

Visual Studio Code (VSCode) and WebStorm have built-in typescript support, so you can recognize typescript files (such as `.ts`, `tsconfig.json`, etc.) without any additional setup, and take advantage of a variety of features, such as code checking, quick fixes, execution, and debugging.
However, the compiler is not included and must be installed separately (E.g. `npm install typescript`)

![image](https://heropy.blog/images/screenshot/typescript/typescript-vscode-error.jpg)

![image](https://heropy.blog/images/screenshot/typescript/typescript-webstorm-error.jpg)

## Install Compiler

To use the `tsc` command, you can globally install type scripts as follows:
When you specify a typescript file as a path, it compiles it.

```bash
$ npm install -g typescript
$ tsc --version
$ tsc ./src/index.ts

```

Alternatively, if you wish to use it only in a single project, you can run it with the `npx tsc` command after a typical regional installation.

```bash
$ npm install -D typescript
$ npx tsc --version
$ npx tsc ./src/index.ts

```

### Compiler Options

You can specify various options for compilation of type scripts.

- Official Document: https://www.typescriptlang.org/docs/handbook/compiler-options.html
- Korean translation of official documents: https://typescript-kr.github.io/pages/Compiler%20Options.html
- vomvoru`s blog: https://vomvoru.github.io/blog/tsconfig-compiler-options-kr/

```bash
$ tsc ./src/index.ts --watch --strict true --target ES6 --lib ES2015,DOM --module CommonJS

```

Alternatively, you can manage options with the `tsconfig.json` file as shown below.
You can add the option `include` and `exlude` together to set which paths to include and exclude in the compilation.

> With VSCode and WebStorm, creating a 'tsconfig.json' file in the project root path will analyze the configuration options by the editor.

```undefined
{
"compilerOptions": {
"strict": true,
"target": "ES6",
"lib": ["ES2015", "DOM"],
"module": "CommonJS"
},
"include": [
"src/**/*.ts"
],
"exclude": [
"node_modules"
]
}

```

```bash
$ tsc --watch

```

## TypeScript Playground

https://www.typescriptlang.org/play/index.html

This is the REPL provided on the official page of the TypeScript, and you can immediately see how the content you create is converted to JavaScript according to the compiler options.

![image](https://heropy.blog/images/screenshot/typescript/typescript-playground.jpg)

## Repl.it

https://repl.it/languages/typescript

You can easily configure typescript projects managed by files and directories.
It is good to test type scripts with simple projects.

![image](https://heropy.blog/images/screenshot/typescript/typescript-repl.jpg)

## Parcel

If you want to test type scripts quickly in your local environment, Parcel Bundler is a good choice.
Organize your project simply as follows:

```bash
$ mkdir typescript-test
$ cd typescript-test
$ npm init -y
$ npm install -D typescript parcel-bundler

```

![image](https://heropy.blog/images/screenshot/typescript/typescript-test-with-parcel-bundler.jpg)

Create the file `tsconfig.json` and add the options you want.
Here`s an example.

```undefined
{
"compilerOptions": {
"strict": true,
},
"exclude": [
"node_modules"
]
}

```

Create the `main.ts` file and enter the type script code you want.

```undefined
function add(a: number, b: number) {
return a + b;
}
const sum: number = add(1, 2);
console.log(sum);

```

Create an `index.html` file and ` as follows.Connect the `.ts` file, not js`.
The Parcel Bundler automatically compiles the type script when it is built.

```xml
<!doctype html>
<html>
<head>
<meta charset="UTF-8">
<title>TypeScript Test</title>
</head>
<body>
<script src="main.ts"></script>
</body>
</html>

```

Finally, specify `index.html` as the entry file and build it with a Parcel bundler as follows:

```bash
$ npx parcel index.html
# Server running at http://localhost:1234

```

## TS Node

If you want to test in NodeJS environment, use TS Node.
Organize your project simply as follows:

```bash
$ mkdir typescript-test
$ cd typescript-test
$ npm init -y
$ npm install -D typescript @types/node ts-node

```

> '@types/node' is a type declaration module for Node.js API.
For more information on '@types', refer to the 'module' part.

![image](https://heropy.blog/images/screenshot/typescript/typescript-test-with-ts-node.jpg)

Create the file `tsconfig.json` and add the options you want.
Here`s an example.

```undefined
{
"compilerOptions": {
"strict": true,
"module": "CommonJS"
},
"exclude": [
"node_modules"
]
}

```

Create the `main.ts` file and enter the type script code you want.

```undefined
console.log('TypeScript on NodeJS!');

```

Run `main.ts` using TS Node.

```bash
$ npx ts-node main.js
# TypeScript on NodeJS!

```

# Type Default (Types)

## Specify Type

TypeScript can specify types such as `: TYPE` in general variables, parameters, and properties of objects.

```undefined
function someFunc(a: TYPE_A, b: TYPE_B): TYPE_RETURN {
return a + b;
}
let some: TYPE_SOME = someFunc(1, 2);

```

In the following example,
Parameters of `add` function `a` and `b` should be of type `number`.
We specified that the variable `sum` should also be of type `number` because the return value of the function executed is inferred to a number.

```undefined
function add(a: number, b: number) {
return a + b;
}
const sum: number = add(1, 2);
console.log(sum); // 3

```

The results compiled by JavaScript are as follows:

```js
"use strict";
function add(a, b) {
return a + b;
}
const sum = add(1, 2);
console.log(sum);

```

## Type Error

If you specify that the variable `sum` should be of type `string` rather than `number` as follows, an error occurs when you write the code without even compiling it.

```undefined
function add(a: number, b: number) {
return a + b;
}
const sum: string = add(1, 2);
console.log(sum);

```

![image](https://heropy.blog/images/screenshot/typescript/typescript-error-ts2322.jpg)

You can see the error code TS2322 in the image above, and you can easily find information about the error code by searching for it.

![image](https://heropy.blog/images/screenshot/typescript/search-typescript-error-code.jpg)

## Type declaration

### Boolean: Boolean

Indicates a simple true/false value.

```undefined
let isBoolean: boolean;
let isDone: boolean = false;

```

### Numeric: Number

All floating point values are available.
Binary and octal literals introduced in ES6 are also supported.

```undefined
let num: number;
let integer: number = 6;
let float: number = 3.14;
let hex: number = 0xf00d; // 61453
let binary: number = 0b1010; // 10
let octal: number = 0o744; // 484
let infinity: number = Infinity;
let nan: number = NaN;

```

### String: String

Indicates a string.
Supports single quotes (```) and double quotes (```) as well as template strings for ES6.

```undefined
let str: string;
let red: string = 'Red';
let green: string = "Green";
let myColor: string = `My color is ${red}.`;
let yourColor: string = 'Your color is' + green;

```

### Array: Array

Indicates a general array that has values sequentially.
An array can declare a type in two ways:

```undefined
// Array with only strings
let fruits: string[] = ['Apple', 'Banana', 'Mango'];
// Or
let fruits: Array<string> = ['Apple', 'Banana', 'Mango'];

// Array with only numbers
let oneToSeven: number[] = [1, 2, 3, 4, 5, 6, 7];
// Or
let oneToSeven: Array<number> = [1, 2, 3, 4, 5, 6, 7];

```

You can also declare a union type (multi-type) `array with strings and numbers at the same time`.

```undefined
let array: (string | number)[] = ['Apple', 1, 2, 'Banana', 'Mango', 3];
// Or
let array: Array<string | number> = ['Apple', 1, 2, 'Banana', 'Mango', 3];

```

If you cannot affirm the value of an array of items, you can use `any`.

```undefined
let someArr: any[] = [0, 1, {}, [], 'str', false];

```

You can also use the interface or custom type.

```undefined
interface IUser {
name: string,
age: number,
isValid: boolean
}
let userArr: IUser[] = [
{
name: 'Neo',
age: 85,
isValid: true
},
{
name: 'Lewis',
age: 52,
isValid: false
},
{
name: 'Evan',
age: 36,
isValid: true
}
];

```

It is not useful, but you can also create specific values on behalf of a type, such as:

```undefined
let array = 10[];
array = [10];
array.push(10);
array.push(11); // Error - TS2345

```

You can also create a read-only array.
You can use either the `readonly` keyword or the `readonly array`

```undefined
let arrA: readonly number[] = [1, 2, 3, 4];
let arrB: ReadonlyArray<number> = [0, 9, 8, 7];

arrA[0] = 123; // Error - TS2542: Index signature in type 'readonly number[]' only permits reading.
arrA.push(123); // Error - TS2339: Property 'push' does not exist on type 'readonly number[]'.

arrB[0] = 123; // Error - TS2542: Index signature in type 'readonly number[]' only permits reading.
arrB.push(123); // Error - TS2339: Property 'push' does not exist on type 'readonly number[]'.

```

### Tuple: Tuple

The Tuple type is very similar to the array.
Differences represent fixed length arrays of a given type.

```undefined
let tuple: [string, number];
tuple = ['a', 1];
tuple = ['a', 1, 2]; // Error - TS2322
tuple = [1, 'a']; // Error - TS2322

```

You can use the data as a single Tuple type, rather than as individual variables:

```undefined
// Variables
let userId: number = 1234;
let userName: string = 'HEROPY';
let isValid: boolean = true;

// Tuple
let user: [number, string, boolean] = [1234, 'HEROPY', true];
console.log(user[0]); // 1234
console.log(user[1]); // 'HEROPY'
console.log(user[2]); // true

```

Furthermore, the following Tuple-type arrays (two-dimensional arrays) can be used using the above method:

```undefined
let users: [number, string, boolean][];
// Or
// let users: Array<[number, string, boolean]>;

users = [[1, 'Neo', true], [2, 'Evan', false], [3, 'Lewis', true]];

```

You can also replace the type with the value.

```undefined
let tuple: [1, number];
tuple = [1, 2];
tuple = [1, 3];
tuple = [2, 3]; // Error - TS2322: Type '2' is not assignable to type '1'.

```

The Tuple represents a fixed length array of a given type, but it is limited to the Assignment (Assign).
You can`t stop putting the value through `.push()` or `.splice()`.

```undefined
let tuple: [string, number];
tuple = ['a', 1];
tuple = ['b', 2];
tuple.push(3);
console.log(tuple); // ['b', 2, 3];
tuple.push(true); // Error - TS2345: Argument of type 'true' is not assignable to parameter of type 'string | number'.

```

You can also use the keyword `readonly` to create a read-only tuple, as used in an array.

```undefined
let a: readonly [string, number] = ['Hello', 123];
a[0] = 'World'; // Error - TS2540: Cannot assign to '0' because it is a read-only property.

```

### Enum: Enum

Enum is a type that can be named in a set of numeric or string values, which is useful if the type of value is set to a certain range.

Basically it starts with `0` and the value increases by `1`.

```undefined
enum Week {
Sun
Mon,
Tue,
Wed,
Thu,
Fri,
Sat
}

```

![image](https://heropy.blog/images/screenshot/typescript/typescript-enum-example1.jpg)

You can change the value manually, increasing the value by `1` again.

![image](https://heropy.blog/images/screenshot/typescript/typescript-enum-example2.jpg)

The interesting part of Enum type is that it supports reverse mapping.
This means that the members can be accessed by the listed members (such as `Sun` and `Mon`) as values.

Outputs `Week` to console.

```undefined
enum Week {
// ...
}
console.log(Week);
console.log(Week.Sun); // 0
console.log(Week['Sun']); // 0
console.log(Week[0]); // 'Sun'

```

![image](https://heropy.blog/images/screenshot/typescript/typescript-enum-console-log.jpg)

In addition to enumerating numeric values, Enum can be initialized with string values as follows:
This method does not support reverse mapping and has the disadvantage of having to initialize individually.

```undefined
enum Color {
Red = 'red',
Green = 'green',
Blue = 'blue'
}
console.log(Color.Red); // red
console.log(Color['Green']); // green

```

### All Types: Any

Any means all types.
Therefore, you can assign any type of value, just like a typical JavaScript variable.
When developing using external resources, it can be useful if the type cannot be determined inevitably.

```undefined
let any: any = 123;
any = 'Hello world';
any = {};
any = null;

```

You can also use it to represent an array that contains various values.

```undefined
const list: any[] = [1, true, 'Anything!'];

```

To strictly prohibit the use of Any to maintain the advantages of a strong type system, the compile option `noImplicitAny`: true` can cause errors when using Any.

### Unknown type: Unknown

Unknown, the top-level type like Any, means unknown type.
Like Any, you can assign any type of value to Unknown, but you cannot assign Unknown to any other type.

> In general, Unknown requires either type affirmations or type guards.
Information about type assertions or guards is organized in other parts.

```undefined
let a: any = 123;
let u: unknown = 123;

let v1: Boolean = a; // All types (any) can be assigned anywhere.
let v2: number = u; // unknown type cannot be assigned to any type other than any.
let v3: any = u; // OK!
let v4: number = u as number; // type to assign.

```

This can be useful in APIs that can return various types.

> It's better to use a clearer type than Unknown.

```undefined
type Result = {
success: true,
value: unknown
} | {
success: false,
error: Error
}

export default function getItems(user: IUser): Result {
// Some logic...
if (id.isValid) {
return {
success: true,
value: ['Apple', 'Banana']
};
} else {
return {
success: false,
error: new Error('Invalid user.')
}
}
}

```

### Object: Object

By default, it represents all types returned by the `typeof` operator as `object`.

> If the compiler option sets the strict type check to 'true', it does not include 'null'.

```undefined
let obj: object = {};
let arr: object = [];
let func: object = function () {};
let nullValue: object = null;
let date: object = new Date();
// ...

```

It is not very useful because it is a different type of parent.
To be more accurate, you can specify individual types for object properties as follows:

```undefined
let userA: { name: string, age: number } = {
name: 'HEROPY',
age: 123
};

let userB: { name: string, age: number } = {
name: 'HEROPY',
age: false, // Error
email: 'thesecon@gmail.com' // Error
};

```

If you want to use it repeatedly, we recommend using `interface` or `type`.

```undefined
interface IUser {
name: string,
age: number,
}

let userA: IUser = {
name: 'HEROPY',
age: 123
};

let userB: IUser = {
name: 'HEROPY',
age: false, // Error
email: 'thesecon@gmail.com' // Error
};

```

### Null과 Undefined

By default, Null and Undefined are subtypes of any type, which can be assigned to each type as follows:
You can even assign it to each other`s types.

```undefined
let num: number = undefined;
let str: string = null;
let obj: { a: 1, b: false } = undefined;
let arr: any[] = null;
let und: undefined = null;
let nul: null = undefined;
let voi: void = null;
// ...

```

This prevents the compilation option `strictNullChecks`: true` from assigning strictly null and undefined types to each other.
However, you can assign Undefined to Void.

```undefined
let voi: void = undefined; // ok

```

### Void

Void is typically used by functions that do not return a value.
The position `: void` is where the function specifies the return type.

```undefined
function hello(msg: string): void {
console.log(`Hello ${msg}`);
}

```

A function that does not return a value actually returns `undefined`.

```undefined
function hello(msg: string): void {
console.log(`Hello ${msg}`);
}
const hi: void = hello('world'); // Hello world
console.log(hi); // undefined

```

```undefined
// Error - TS2355: A function whose declared type is neither 'void' nor 'any' must return a value.
function hello(msg: string): undefined {
console.log(`Hello ${msg}`);
}

```

### Never

Never indicates a value that will never occur, and no type can be applied.

```undefined
function error(message: string): never {
throw new Error(message);
}

```

You can usually view Never if you incorrectly declare an empty array of types as follows:

```undefined
const never: [] = [];
never.push(3); // Error - TS2345: Argument of type '3' is not assignable to parameter of type 'never'.

```

### Union

If more than one type is allowed, this is called a union.
Type is distinguished by `|` (vertical bar), and `()` is optional.

```undefined
let union: (string | number);
union = 'Hello type!';
union = 123;
union = false; // Error - TS2322: Type 'false' is not assignable to type 'string | number'.

```

### Intersection

`

> If you can understand the union you've seen above as if it's an 'Or', the intersection is an 'And'.

```undefined
If the existing types can be combined, the intersection can be used.
interface IUser {
name: string,
age: number
}
interface IValidation {
isValid: boolean
}
const heropy: IUser = {
name: 'Heropy',
age: 36,
isValid: true // Error - TS2322: Type '{ name: string; age: number; isValid: boolean; }' is not assignable to type 'IUser'.
};
const neo: IUser
```

### Function

You can specify the type using the arrow function.
Enter the type of argument and the type of return value.

```undefined
'// myFunc has two numeric type arguments, and it is a function that is a function that returns a numeric type.
let myFunc: (arg1: number, arg2: number) => number;
myFunc = function (x, y) {
return x + y;
};

// No arguments, no returns.
let yourFunc: () => void;
yourFunc = function () {
console.log('Hello world~');
};

```

## Type Inference

If there is no explicit type declaration, type scripts provide type inference.
The concept is very simple.

> [Inference]: Drawing other judgments on the basis of some judgment.

```undefined
let num = 12;
num = 'Hello type!'; // TS2322: Type '"Hello type!"' is not assignable to type 'number'.

```

When initializing the variable `num`, the number `12` was allocated and deduced to the number type, so the string type value `Hello type!` cannot be assigned, which causes an error.

The cases where type scripts deduce types are as follows.

- Initialized Variables
- Parameters with default values set
- Function with return value

```undefined
// Initialized Variable `num`
let num = 12;

// parameter 'b' with default value set
function add(a: number, b: number = 2): number {
// Function with return value (`a + b`)
return a + b;
}

```

> Type inference does not mean a type declaration that is not strict.
Therefore, it is not necessary to specify the type everywhere, and in many cases it can provide better code readability.

## Type affirmations

If type scripts exceed the type categories that can be determined by type inference, you can instruct them not to make any further inference.
This is called `type affirmation`, which means that the programmer has a better understanding of the type than the type script.

> [Confirmation]: Don't hesitate to speak straight.

Let`s look at the next example.

The parameter `val` of the function can be a string (String) or a number (Number) in the union type.
And we can infer (we) that the parameter `isNumber` is Boolean, which is a value that determines whether it is numeric or not through the name.
Therefore, we can clearly see that if `isNumber` is `true`, `val` will be a number and `toFixed` can be used.
However, TypeScript returns the following error (in the compilation phase) saying "toFixed" cannot be used if `val` is a string because the above cannot be inferred by the name `isNumber`.

```undefined
function someFunc(val: string | number, isNumber: boolean) {
// some logics
if (isNumber) {
val.toFixed(2); // Error - TS2339: ... Property 'toFixed' does not exist on type 'string'.
}
}

```

Therefore, we can affirm in two ways that `val` is a number when `isNumber` is `true`.
The second approach (`<number>val`) can cause problems with certain parsing using JSX, which is not available at all in the `.tsx` file.

```undefined
function someFunc(val: string | number, isNumber: boolean) {
// some logics
if (isNumber) {
// 1. Variable as type
(val as number).toFixed(2);
// Or
// 2. Type variables
// (<number>val).toFixed(2);
}
}

```

> Type affirmations are like programmers telling TypeScript, "I know, so trust me!"

### Non-null affirmation operator

The Non-null assertion operator with `!` can affirm that the operand is not a nullish (`null` or `undefined`) value, which is useful because it can be used simply by variables or attributes.

If you look at the `fnA` function in the following example, the parameter `x` is treated as a numeric type using `toFixed` within the function, but an error occurs because it can be `null` or `undefined`.
It can be solved by type affirmation or `if` condition, but as the last function `!The Non-null affirmation operator using ` can be used to simplify cleanup.

```undefined
// Error - TS2533: Object is possibly 'null' or 'undefined'.
function fnA(x: number | null | undefined) {
return x.toFixed(2);
}

// if statement
function fnD(x: number | null | undefined) {
if (x) {
return x.toFixed(2);
}
}

// Type assertion
function fnB(x: number | null | undefined) {
return (x as number).toFixed(2);
}
function fnC(x: number | null | undefined) {
return (<number>x).toFixed(2);
}

// Non-null assertion operator
function fnE(x: number | null | undefined) {
return x!.toFixed(2);
}

```

This is especially useful for using DOMs that are difficult to check in a compilation environment.
Of course, you can also use general type assertions.

```undefined
// Error - TS2531: Object is possibly 'null'.
document.querySelector('.menu-item').innerHTML;

// Type assertion
(document.querySelector('.menu-item') as HTMLDivElement).innerHTML;
(<HTMLDivElement>document.querySelector('.menu-item')).innerHTML;

// Non-null assertion operator
document.querySelector('.menu-item')!.innerHTML;

```

## Type Guards

In some cases, the type affirmation may be used multiple times to ensure each type of `val` as shown in the following example:

```undefined
function someFunc(val: string | number, isNumber: boolean) {
if (isNumber) {
(val as number).toFixed(2);
isNaN(val as number);
} else {
(val as string).split('');
(val as string).toUpperCase();
(val as string).length;
}
}

```

In this case, providing a type guard ensures a type within a specific scope that the type script can infer.
Type Guard is a function that specifies the type of `NAME is TYPE` type as the return type.
In the following example, the type alcohol part is `val is number`.
It`s much cleaner without type affirmations.

> [££]: A word that describes the state of a subject, the nature, and so on.

```undefined
'// Type Guard
function isNumber(val: string | number): val is number {
return typeof val === 'number';
}

function someFunc(val: string | number) {
if (isNumber(val)) {
val.toFixed(2);
isNaN(val);
} else {
val.split('');
val.toUpperCase();
val.length;
}
}

```

There are more type guards that can be provided as well as the above method.
Type guard that directly uses `typeof`, `in`, and `instanceof` operators.
It is recommended from a relatively simple logic.

> Only 'number', 'string', 'boolean', and 'symbol' can be recognized as type guards.
The right-sided object ('val') of the 'in' operator must be of type 'any'.

```undefined
// If you use the `typeof` operator directly, it will work as a type guard, even if you do not provide (abstract) 'isNumber' as in the previous example.
// https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/typeof
function someFuncTypeof(val: string | number) {
if (typeof val === 'number') {
val.toFixed(2);
isNaN(val);
} else {
val.split('');
val.toUpperCase();
val.length;
}
}

// Use the 'in' operator to provide a type guard without any additional abstraction.
// https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/in
function someFuncIn(val: any) {
if ('toFixed' in val) {
val.toFixed(2);
isNaN(val);
} else if ('split' in val) {
val.split('');
val.toUpperCase();
val.length;
}
}

// also provides a type guard using the 'instanceof' operator without any abstraction.
// https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/instanceof
class Cat {
meow() {}
}
class Dog {
woof() {}
}
function sounds(ani: Cat | Dog) {
if (ani instanceof Cat) {
ani.meow();
} else {
ani.woof();
}
}

```

# Interface

An interface is a kind of rule and structure that defines multiple objects in a typescript.
Use with the keyword `interface` as follows:

> In 'IUser', 'I' was used as an alias for the interface.

```undefined
interface IUser {
name: string,
age: number
isAdult: boolean
}

let user1: IUser = {
name: 'Neo',
age: 123,
isAdult: true
};

// Error - TS2741: Property 'isAdult' is missing in type '{ name: string; age: number; }' but required in type 'IUser'.
let user2: IUser = {
name: 'Evan',
age: 456
};

```

`:`(colon), `,`(comma), or symbols may not be used.

```undefined
interface IUser {
name: string,
age: number
}
// Or
interface IUser {
name: string;
age: number;
}
// Or
interface IUser {
name: string,
age: number
}

```

Using `?` for a property can be defined as an optional property:
Optional properties are described separately in the Optional part, but in a simple way, they are defined as `properties, not required`.

```undefined
interface IUser {
name: string,
age: number
isAdult?: boolean // Optional property
}

// error does not occur without initializing 'isAdult'.
let user: IUser = {
name: 'Neo',
age: 123,
};

```

## Readonly properties

The keyword `readonly` allows you to define read-only properties that must maintain initialized values.

```undefined
interface IUser {
readonly name: string,
age: number
}

// Initialize
let user: IUser = {
name: 'Neo',
age: 36,
};

user.age = 85; // Ok
user.name = 'Evan'; // Error - TS2540: Cannot assign to 'name' because it is a read-only property.

```

If all properties are `readonly`, you can leverage utility or assertion type.

```undefined
// All readonly properties
interface IUser {
readonly name: string,
readonly age: number
}
let user: IUser = {
name: 'Neo',
age: 36
};
user.age = 85; // Error
user.name = 'Evan'; // Error


// Readonly Utility
interface IUser {
name: string,
age: number
}
let user: Readonly<IUser> = {
name: 'Neo',
age: 36
};
user.age = 85; // Error
user.name = 'Evan'; // Error


// Type assertion
let user = {
name: 'Neo',
age: 36
} as const;
user.age = 85; // Error
user.name = 'Evan'; // Error

```

## Function Type

When defining a function type as an interface, use what is called a call signature.
The call signature specifies the parameter of the function and the return type as follows:

```undefined
interface IName {
(PARAMETER: PARAM_TYPE): RETURN_TYPE // Call signature
}

```

Let`s take a look at a simple example.
The interface `IGetUser` defines the function type, which has one parameter `name` (name does not need to match), and must return the type `IUser`.

```undefined
interface IUser {
name: string,
}
interface IGetUser {
(name: string): IUser
}

// The parameter name does not need to match the interface.
// In addition, type inference allows you to provide parameters in an implicit manner in order.
const getUser: IGetUser = function (n) { // n is name: string
// Find user logic..
// ...
return user;
};
getUser('Heropy');

```

## Class Type

When defining classes with interfaces, use the keyword `implements`.

```undefined
interface IUser {
name: string,
getName(): string
}

class User implements IUser {
constructor(public name: string) {}
getName() {
return this.name;
}
}

const neo = new User('Neo');
neo.getName(); // Neo

```

The basic usage is not difficult.
However, if you use a class that you define as an argument, you might encounter the following problems:
Because in the following example, interface `ICat` is not a callable structure.

```undefined
interface ICat {
name: string,
}

class Cat implements ICat {
constructor(public name: string) {}
}

function makeKitten(c: ICat, n: string) {
return new c(n); // Error - TS2351: This expression is not constructable. Type 'ICat' has no construct signatures.
}
const kitten = makeKitten(Cat, 'Lucy');
console.log(kitten);

```

To this end, you can provide a construction signature.
The configuration signature is similar to the call signature shown above, but the keyword `new` must be used.

```undefined
interface IName {
new (PARAMETER: PARAM_TYPE): RETURN_TYPE // Construct signature
}

```

Modify the example you saw above as follows:
By defining a callable interface with a configuration signature called `ICatConstructor`, you can see that it is functioning smoothly.

```undefined
interface ICat {
name: string,
}
interface ICatConstructor {
new (name: string): ICat;
}

class Cat implements ICat {
constructor(public name: string) {}
}

function makeKitten(c: ICatConstructor, n: string) {
return new c(n); // ok
}
const kitten = makeKitten(Cat, 'Lucy');
console.log(kitten);

```

I prepared a similar but more interesting example.
It is sufficient if you have checked the part where the error occurs and understood the contents.

```undefined
interface IFullName {
firstName: string,
lastName: string
}
interface IFullNameConstructor {
new(firstName: string): IFullName; // Construct signature
}


function makeSon(c: IFullNameConstructor, firstName: string) {
return new c(firstName);
}
function getFullName(son: IFullName) {
return `${son.firstName} ${son.lastName}`;
}


// Anderson family
class Anderson implements IFullName {
public lastName: string;
constructor (public firstName: string) {
this.lastName = 'Anderson';
}
}
const tomas = makeSon(Anderson, 'Tomas');
const jack = makeSon(Anderson, 'Jack');
getFullName(tomas); // Tomas Anderson
getFullName(jack); // Jack Anderson


// Smith family?
class Smith implements IFullName {
public lastName: string;
constructor (public firstName: string, agentCode: number) {
this.lastName = `Smith ${agentCode}`;
}
}
const smith = makeSon(Smith, 7); // Error - TS2345: Argument of type 'typeof Smith' is not assignable to parameter of type 'IFullNameConstructor'.
getFullName(smith);

```

## Indexable types

We can define the type of a particular property (such as a method) through an interface, but in structures that have numerous properties or contain arbitrary properties that cannot be affirmed, traditional methods alone have limitations. Let`s look at index signatures that are useful in this situation.

Indexable types are indexed by `number` such as `arr[2]` or by `character` such as `obj[`name`]`.
The interface that defines these indexable types can have an index signature.
The index signature specifies the name and type of indexer to use for indexing, and the return value of the indexing result, as shown in the following structure:
The type of indexer can only be `string` and `number`.

```undefined
interface INAME {
[INDEXER_NAME: INDEXER_TYPE]: RETURN_TYPE // Index signature
}

```

> The number (characters) that point to a location in an array (object) is called index, and the use of indexes to access each array element (object attribute) is called indexing (each value that constitutes an array is called element).

To help you understand, look at the following example:
The interface `IItem` has an index signature, and there is an `item` with the `IItem` as its type (interface), and the value returned when indexing `item` as a number such as `item[0]` or `item[1]`.
An error occurs when indexing `item` as a character such as `item[`0`].

```undefined
interface IItem {
[itemIndex: number]: string // Index signature
}
let item: IItem = ['a', 'b', 'c']; // Indexable type
console.log(item[0]); // 'a' is string.
console.log(item[1]); // 'b' is string.
console.log(item['0']); // Error - TS7015: Element implicitly has an 'any' type because index expression is not of type 'number'.

```

For your information, if you use Union as the return type of indexing result, you can use it as follows.

```undefined
interface IItem {
[itemIndex: number]: string | boolean | number[]
}
let item: IItem = ['Hello', false, [1, 2, 3]];
console.log(item[0]); // Hello
console.log(item[1]); // false
console.log(item[2]); // [1, 2, 3]

```

Let`s take a look at an example of indexing by text.
Interface `IUser` has an index signature, and there is `user` with `IUser` as its type, and the value returned when indexing `user` as a text such as `user[`name`]`, `user[`email`]`, or `user[`isValid`] is a `Neo` or `thesecon@gmail.com` as a text.
In addition, if you index a number such as `user[0]` or `user[0`], the number is converted to a string before indexing, so you can return the value as follows:

```undefined
interface IUser {
[userProp: string]: string | boolean
}
let user: IUser = {
name: 'Neo',
email: 'thesecon@gmail.com',
isValid: true,
0: false
};
console.log(user['name']); // 'Neo' is string.
console.log(user['email']); // 'thesecon@gmail.com' is string.
console.log(user['isValid']); // true is boolean.
console.log(user[0]); // false is boolean
console.log(user[1]); // undefined
console.log(user['0']); // false is boolean

```

Index signatures are useful when you use properties that are not defined in the interface, such as the following example:
However, be aware that the property must have a return value defined in the index signature.
In the following example, an error occurs because the `isAdult` property does not return a defined `string` or `number` type.

```undefined
interface IUser {
[userProp: string]: string | number
name: string,
age: number
}
let user: IUser = {
name: 'Neo',
age: 123,
email: 'thesecon@gmail.com',
isAdult: true // Error - TS2322: Type 'true' is not assignable to type 'string | number'.
};
console.log(user['name']); // 'Neo'
console.log(user['age']); // 123
console.log(user['email']); // thesecon@gmail.com

```

### keyof

The attribute name can be used as a type by using `keyof` in Indexable Types.
Attribute names of indexable types are applied as union types.
Let`s take a quick look at an example.

```undefined
interface ICountries {
KR: 'South Korea,'
US: 'USA,
CP: 'China'
}
let country: keyof ICountries; // 'KR' | 'US' | 'CP'
country = 'KR'; // ok
country = 'RU'; // Error - TS2322: Type '"RU"' is not assignable to type '"KR" | "US" | "CP"'.

```

Indexing through `keyof` also gives you access to individual values of the type.

```undefined
interface ICountries {
KR: 'South Korea,'
US: 'USA,
CP: 'China'
}
let country: ICountries[keyof ICountries]; // ICountries['KR' | 'US' | 'CP']
country = 'Korea';
Country = 'Russia'; // Error - TS2322: Type 'Russia' is not acceptable to type 'South Korea' | 'USA' | 'China'.

```

## Interface Extensions

The interface can also be inherited using the `extends` keyword, just like the class.

```undefined
interface IAnimal {
name: string,
}
interface ICat extends IAnimal {
meow(): string
}

class Cat implements ICat { // Error - TS2420: Class 'Cat' incorrectly implements interface 'ICat'. Property 'name' is missing in type 'Cat' but required in type 'ICat'.
meow() {
return 'MEOW~'
}
}

```

You can also create multiple interfaces with the same name.
This is useful when adding content to an existing interface.

```undefined
interface IFullName {
firstName: string,
lastName: string
}
interface IFullName {
middleName: string
}

const fullName: IFullName = {
firstName: 'Tomas',
middleName: 'Sean',
lastName: 'Connery'
};

```

# Type Aliases

You can create a new type combination using the keyword `type`.
To give an alias (name) by combining one or more types, to be exact, to create an alias that refers to each type of combination.
Typically, a union is used a lot to form more than one combination.

> TUser used T as an alias for Type.

```undefined
type MyType = string;
type YourType = string | number | boolean;
type TUser = {
name: string,
age: number
isValid: boolean
} | [string, number, boolean];

let userA: TUser = {
name: 'Neo',
age: 85,
isValid: true,
};
let userB: TUser = ['Evan', 36, false];

function someFunc(arg: MyType): YourType {
switch (arg) {
case 's':
return arg.toString(); // string
case 'n':
return parseInt(arg); // number
default:
return true; // boolean
}
}

```

# Generic

Generic provides a way to declare a type at the time of use, not at the time of declaration of a function or class, for reuse purposes.

> It's easy to understand that you use the type as an argument.

The following example is intended for the `toArray` function to return the value received as an array as an argument.
An error occurs in a function call with String type as an argument because the parameter only accepts Number type.

```undefined
function toArray(a: number, b: number): number[] {
return [a, b];
}
toArray(1, 2);
toArray('1', '2'); // Error - TS2345: Argument of type '"1"' is not assignable to parameter of type 'number'.

```

We used the Union method to make it more general-purpose.
Now you can get the String type as an argument, but it`s less readable and there`s a new problem.
If you look at the third call, you can receive the number and string type at the same time unintentionally.

```undefined
function toArray(a: number | string, b: number | string): (number | string)[] {
return [a, b];
}
toArray(1, 2); // Only Number
toArray('1', '2'); // Only String
toArray(1, '2'); // Number
```

Use Generic this time.
Start by writing `£` on the right side of the function name.
`T` is an identifier that will be converted to a user-supplied type with a type variable.
The third call can now intentionally receive the Number and String type simultaneously (or an error will occur if the union is not used).

> Type variables can be specified by any name, just like parameters.

```undefined
function toArray<T>(a: T, b: T): T[] {
return [a, b];
}

toArray<number>(1, 2);
toArray<string>('1', '2');
toArray<string | number>(1, '2');
toArray<number>(1, '2'); // Error

```

Using type inference, type may not be provided at the time of use.

```undefined
function toArray<T>(a: T, b: T): T[] {
return [a, b];
}

toArray(1, 2);
toArray('1', '2');
toArray(1, '2'); // Error

```

## Constraints

You can also create generics that use interfaces or type aliases.
The following example does not have separate Constraints, allowing all types.

```undefined
interface MyType<T> {
name: string,
value: T
}

const dataA: MyType<string> = {
name: 'Data A',
value: 'Hello world'
};
const dataB: MyType<number> = {
name: 'Data B',
value: 1234
};
const dataC: MyType<boolean> = {
name: 'Data C',
value: true
};
const dataD: MyType<number[]> = {
name: 'Data D',
value: [1, 2, 3, 4]
};

```

To allow only the type variables `T` to be `string` and `number`, you can add constraints using the keyword `extends` as shown in the example below.
The basic grammar is as follows:

```undefined
T extends U

```

```undefined
interface MyType<T extends string | number> {
name: string,
value: T
}

const dataA: MyType<string> = {
name: 'Data A',
value: 'Hello world'
};
const dataB: MyType<number> = {
name: 'Data B',
value: 1234
};
const dataC: MyType<boolean> = { // TS2344: Type 'boolean' does not satisfy the constraint 'string | number'.
name: 'Data C',
value: true
};
const dataD: MyType<number[]> = { // TS2344: Type 'number[]' does not satisfy the constraint 'string | number'.
name: 'Data D',
value: [1, 2, 3, 4]
};

```

A type declaration that uses the keyword `type` and `interface` can be divided into `identifier` and `type implementation` based on the `=` symbol as shown in the following example.
Constraints are limited to `extends` used in the `Identifier` area.

```undefined
type U = string | number | boolean;

// type identifier = type implementation
type MyType<T extends U> = string | T;

// interface identifier {type implementation}
interface IUser<T extends U> {
name: string,
age: T
}

```

## Conditional Types

Unlike constraints, `extends` used in the `type implementation` area can use a conditional ternary operator.
This is called Conditional Types and has the following grammar:

```undefined
T extends U ? X : Y

```

```undefined
type U = string | number | boolean;

// type identifier = type implementation
type MyType<T> = T extends U ? string : never;

// interface identifier {type implementation}
interface IUser<T> {
name: string,
age: T extends U ? number : never
}

```

```undefined
'// 'T' is limited to 'boolean' type.
interface IUser<T extends boolean> {
name: string,
age: Textends true ? string : number, // return 'string' if type 'T' is 'true', or return 'number' if not.
isString: T
}

const str: IUser<true> = {
name: 'Neo',
age: '12', // String
isString: true
}
const num: IUser<false> = {
name: 'Lewis',
age: 12, // Number
isString: false
}

```

You can also use the trinomial operator continuously as follows:

```undefined
type MyType<T> =
T extends string ? 'Str' :
T extends number ? 'Num' :
T extends boolean ? 'Boo' :
T extends undefined ? 'Und' :
T extends null ? 'Nul' :
'Obj';

```

### infer

Use the keyword `infer` to determine whether a type variable is an Inference.
The basic grammar is as follows:

> If 'U' is an inferable type, then it is true or false.

```undefined
T extends infer U ? X : Y

```

It`s not useful, but let`s look at a very simple example for understanding.
The basic structure is the same as the conditional type shown above.

```undefined
type MyType<T> = T extends infer R ? R : null;

const a: MyType<number> = 123;

```

Here, the type variable `R` becomes the type `number` received from `MyType<number>` and checks whether type inference is possible through the keyword `infer`.
`number` type can be inferred of course, so it returns `R`. (If `R` cannot be inferred of type, `null` is returned.)
As a result, `MyType<number>` can return `number` and variable `a` can assign `123`.

This time, let`s take a look at a more complex but useful example.
`ReturnType` returns what type the function returns.

> Please refer to the 'TS Utility Type > ReturnType' part.

```undefined
type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;

function fn(num: number) {
return num.toString();
}

const a: ReturnType<typeof fn> = 'Hello';

```

In the example above, `typeofn` is `(num: number) = string` and the return type is `string`.
Therefore, `R` returns `R` because it is `string` and type inference is possible through the `infer` keyword.
That is, returns `string`.

For more information on the `infer` keyword, see the Type reference in conditional types part of the official document.
The contents of the document are summarized as follows:

- Keyword `infer` can only be used in conditional type `extends` clause, not constraint `extends`
- The `infer` keyword can use the same type variable in multiple locations
Reasoning as a union type in a typical covariate location
inferring from the function argument contra-variant position to intersection type
- Reasoning as a union type in a typical covariate location
- inferring from the function argument contra-variant position to intersection type
- For multiple call signatures (overloading functions), infer from the last signature

> For more information on covariance and anti-covariance, see the following post.
Covariance and anti-covariance in TypeScript (strictFunctionTypes)

# Function

The basic function use was discussed above.
Let`s take a look at the main characteristics of the TypeScript function.

## this

One of the most important things to do with a function is `this`.
In some cases, `this` in a function loses the context we want, such as referring to a global object (sloppy mode), or becoming `undefined` (strict mode).

```undefined
const obj = {
a: 'Hello~',
b: function () {
console.log(this.a); // obj.a.
// Inner function
function b() {
console.log(this.a); // global.a.
}
}
};

```

This causes problems, especially when using `non-recallable methods`.
First, let`s look at the next example.
In object data `obj`, the `b` method refers to the `a` attribute through `this`.

```undefined
const obj = {
a: 'Hello~',
b: function () {
console.log(this.a);
}
};

```

If you use (assign) the `non-recallable method` based on the object above as shown in the example below, `this` will lose the valid context and cannot reference `a`.

> In many cases, the callback function is applicable.

```undefined
obj.b(); // Hello~

const b = obj.b;
b(); // Cannot read property 'a' of undefined

function someFn(cb: any) {
cb();
}
someFn(obj.b); // Cannot read property 'a' of undefined

setTimeout(obj.b, 100); // undefined

```

In this situation, let`s find out how the `this` context can be maintained normally and referred to the `a` property.

The first is to connect `this` directly using the bind method.

> Because the bind, call, apply method does not check the argument type by default, you must specify 'strict: true' (or 'strictBindCallApply: true') in the compiler option to check the type normally.

```undefined
obj.b(); // Hello~

const b = obj.b.bind(obj);
b(); // Hello~

function someFn(cb: any) {
cb();
}
someFn(obj.b.bind(obj)); // Hello~

setTimeout(obj.b.bind(obj), 100); // Hello~

```

The second method is to use the arrow function.
Use the arrow function to invoke the method while maintaining a valid context:

> The arrow function captures 'this' where the function was created, not where it was called.

```undefined
obj.b(); // Hello~

const b = () => obj.b();
b(); // Hello~

function someFn(cb: any) {
cb();
}
someFn(() => obj.b()); // Hello~

setTimeout(() => obj.b(), 100); // Hello~

```

If you define a method member of a class, you can use the arrow function rather than the prototype method.

```undefined
class Cat {
constructor(private name: string) {}
getName = () => {
console.log(this.name);
}
}
const cat = new Cat('Lucy');
cat.getName(); // Lucy

const getName = cat.getName;
getName(); // Lucy

function someFn(cb: any) {
cb();
}
someFn(cat.getName); // Lucy

```

Note that each instance is created with an individual `getName`, which is inefficient to use the arrow function in a typical method call, but if the method is primarily used as a callback, the generated `getName` reference of the arrow function can be much more efficient than the new closure call of the prototype.

![image](https://heropy.blog/images/screenshot/typescript/typescript-compare-prototype-and-arrow-function.jpg)

> Each method is a trade-off for memory and performance.
It is recommended that you choose according to the situation.

### Explicit this

In the following example, the `cat` object that can be captured by `this` in `someFn` function is passed and executed through the call method, but in strict mode, `this` is implicitly `any` type, causing errors.

> 'Strict mode' refers to the case of 'strict: true' (or 'noImplicitThis: true') in the compiler option.

```undefined
interface ICat {
name: string
}

const cat: ICat = {
name: 'Lucy'
};

function someFn(greeting: string) {
console.log(`${greeting} ${this.name}`); // Error - TS2683: 'this' implicitly has type 'any' because it does not have a type annotation.
}
someFn.call(cat, 'Hello'); // Hello Lucy

```

In this case, you can explicitly declare the type of `this`.
Declares `this` as the first fake parameter:

```undefined
interface ICat {
name: string
}

const cat: ICat = {
name: 'Lucy'
};

function someFn(this: ICat, greeting: string) {
console.log(`${greeting} ${this.name}`); // ok
}
someFn.call(cat, 'Hello'); // Hello Lucy

```

## Overloads

TypeScript`s `Overloads` refers to multiple functions that have the same name but different parameter types and return types.
Function overload lets you create and manage functions of various structures.

In the example below, the `add` function has two declarations and one implementation.
Note that the number of parameters in the function declaration and implementation must be the same.

> 'any' is frequently used for function implementation.

```undefined
function add(a: string, b: string): string; // 함수 선언
function add(a: number, b: number): number; // 함수 선언
function add (a: any, b: any): any { // function implementation
return a + b;
}

add('hello ', 'world~');
add(1, 2);
add('hello ', 2); // Error - No overload matches this call.

```

You can also leverage overloads in method definitions such as interfaces or type aliases.
You can define dynamic parameters and return values for a function declaration by either type affirmation or type guard.

```undefined
interface IUser {
name: string
age: number,
getData(x: string): string[];
getData(x: number): string;
}

let user: IUser = {
name: 'Neo',
age: 36,
getData: (data: any) => {
if (typeof data === 'string') {
return data.split('');
} else {
return data.toString();
}
}
};

user.getData('Hello'); // ['h', 'e', 'l', 'l', 'o']
user.getData(123); // '123'

```

If you look at DOM-type declarations such as `HTMLDivElement`, you can see the following:

```undefined
/** Provides special properties (beyond the regular HTMLElement interface it also has available to it by inheritance) for manipulating <div> elements. */
interface HTMLDivElement extends HTMLElement {
/**
* Sets or retrieves how the object is aligned with adjacent text.
*/
/** @deprecated */
align: string;
addEventListener<K extends keyof HTMLElementEventMap>(type: K, listener: (this: HTMLDivElement, ev: HTMLElementEventMap[K]) => any, options?: boolean | AddEventListenerOptions): void;
addEventListener(type: string, listener: EventListenerOrEventListenerObject, options?: boolean | AddEventListenerOptions): void;
removeEventListener<K extends keyof HTMLElementEventMap>(type: K, listener: (this: HTMLDivElement, ev: HTMLElementEventMap[K]) => any, options?: boolean | EventListenerOptions): void;
removeEventListener(type: string, listener: EventListenerOrEventListenerObject, options?: boolean | EventListenerOptions): void;
}

```

![image](https://heropy.blog/images/screenshot/typescript/typescript-webstorm-declaration-of-usages.jpg)

![image](https://heropy.blog/images/screenshot/typescript/typescript-vscode-go-to-definition.jpg)

# Class

Unlike the class` constructor method and the class member, the properties declare a type separately on the class body, such as `name: string;`.

> Class body is bracketed `{}It means an area bound by a '.

```undefined
class Animal {
name: string;
constructor(name: string) {
thisname = name;
}
}
class Cat extends Animal {
getName(): string {
return `Cat name is ${this.name}.`;
}
}
let cat: Cat;
cat = new Cat('Lucy');
console.log(cat.getName()); // Cat name is Lucy.

```

## Class modifiers

Let`s take a look at the class modifiers related to TypeScript.

There are access controllers available in class members (attributes, methods).
Let`s understand the difference between each access controller.

> Access modifiers are keywords in object-oriented languages that set the accessibility of classes, methods, and other members.

<table><thead><tr><th>Access Controller</th><th>Meaning</th><th>Scope</th></tr></thead><tbody><tr><
td><code>`public`</code></td><td>Freely accessible from anywhere (optional)</td><td>Properties, methods</td></tr><tr><td
 ><code>`protected`</code></td><td>Available within me and derived descendant classes</td><td>Properties, methods</td></tr><tr><td
 ><code>`private`</code></td><td>Available only in my class</td><td>Properties, methods</td></tr></tbody></table>

 

The following modifiers can be used with the above access controller:
For `static`, typescript can use static methods as well as static properties.

<table><thead><tr><th>Formula</th><th>Meaning</th><th>Scope</th></tr></thead><tbody><tr><td>
 <code>`static`</code></td><td>Use statically</td><td>Properties, general methods</td></tr><tr><td><code>`readonly
 `</code></td><td>Use as read-only</td><td>Properties</td></tr></tbody></table>

 

Then first, let`s look at the differences between each access controller.

In the following example, `name` property of `Animal` class is `public`, so it is `this` from the derived child class (`Cat`.refer to as `name` or `cat.There is no problem accessing `name`.
(Free access from anywhere (missing)

```undefined
class Animal {
// use public modifier (missible)
public name: string;
constructor(name: string) {
thisname = name;
}
}
class Cat extends Animal {
getName(): string {
return `Cat name is ${this.name}.`;
}
}
let cat = new Cat('Lucy');
console.log(cat.getName()); // Cat name is Lucy.

cat.name = 'Tiger';
console.log(cat.getName()); // Cat name is Tiger.

```

In the following example, `name` property of `Animal` class is `protected`, so it is derived from `Cat`.You can refer to it as `name`, but an instance of it as `cat.It is not accessible by name`.
(accessible within my derived descendant class)

```undefined
class Animal {
// use the protected modifier
protected name: string;
constructor(name: string) {
thisname = name;
}
}
class Cat extends Animal {
getName(): string {
return `Cat name is ${this.name}.`;
}
}
let cat = new Cat('Lucy');
console.log(cat.getName()); // Cat name is Lucy.
console.log(cat.name); // Error - TS2445: Property 'name' is protected and only accessible within class 'Animal' and its subclasses.

cat.name = 'Tiger'; // Error - TS2445: Property 'name' is protected and only accessible within class 'Animal' and its subclasses.
console.log(cat.getName());

```

In the following example, the attribute `name` of class `Animal` is `private`, so it is `this` from the derived class `Cat`.It can`t be referred to as `name`, but it can be referred to as `cat.It is also not accessible by name`.
(Only accessible from my class)

```undefined
class Animal {
// Use private modifiers
private name: string;
constructor(name: string) {
thisname = name;
}
}
class Cat extends Animal {
getName(): string {
return `Cat name is ${this.name}.`; // Error - TS2341: Property 'name' is private and only accessible within class 'Animal'
}
}
let cat = new Cat('Lucy');
console.log(cat.getName());
console.log(cat.name); // Error - TS2341: Property 'name' is private and only accessible within class 'Animal'.

cat.name = 'Tiger'; // Error - TS2341: Property 'name' is private and only accessible within class 'Animal'.
console.log(cat.getName());

```

The following is an example of an error in instance creation because `protected` was used in the constructor method.

```undefined
class Animal {
name: string;
protected constructor(name: string) {
this.name = name;
}
}
const cat = new Animal('Dog'); // Error - TS2674: Constructor of class 'Animal' is protected and only accessible within the class declaration.

```

The interesting part is that the constructor method uses the access controller at the same time as the argument type declaration, which can be defined as an attribute member immediately.
Be careful not to omit the access controller.

```undefined
class Cat {
constructor(public name: string, protected age: number) {}
getName() {
return this.name;
}
getAge() {
return this.age;
}
}

const cat = new Cat('Neo', 2);
console.log(cat.getName()); // Neo
console.log(cat.getAge()); // 2

```

Let`s take a look at `static` and `readonly`.

In ES6, only static methods could be created with `static`; in TypeScript, static properties could also be created.
Static attributes are used as type declarations of attributes in the class body, and unlike the static method, values cannot be initialized in the class body, which requires initialization in the `constructor` or method.

```undefined
class Cat {
static legs: number;
constructor() {
Cat.legs = 4; // Init static property.
}
}
console.log(Cat.legs); // undefined
new Cat();
console.log(Cat.legs); // 4

class Dog {
// Init static method.
static getLegs() {
return 4;
}
}
console.log(Dog.getLegs()); // 4

```

If you use `readonly`, the attribute is `read only`.

```undefined
class Animal {
readonly name: string;
constructor(n: string) {
this.name = n;
}
}
let dog = new Animal('Charlie');
console.log(dog.name); // Charlie
dog.name = 'Tiger'; // Error - TS2540: Cannot assign to 'name' because it is a read-only property.

```

And `static` and `readonly` can be used with access controllers.
You must create an Access Controller first.

```undefined
class Cat {
public readonly name: string;
protected static eyes: number;
constructor(n: string) {
this.name = n;
Cat.eyes = 2;
}
private static getLegs() {
return 4;
}
}

```

## Abstract class

Abstract classes are basic classes that other classes can derive from, and are very similar to interfaces.
`abstract` can be used not only for classes but also for properties and methods.
You must create an instance from a derived descendant class because an abstract class cannot create its own instance.

```undefined
// Abstract Class
abstract class Animal {
abstract name: string; // must be implemented in the derived class.
abstract getName(): string; // must be implemented in the derived class.
}
class Cat extends Animal {
constructor(public name: string) {
super();
}
getName() {
return this.name;
}
}
new Animal(); // Error - TS2511: Cannot create an instance of an abstract class.
const cat = new Cat('Lucy');
console.log(cat.getName()); // Lucy

// Interface
interface IAnimal {
name: string;
getName(): string;
}
class Dog implements IAnimal {
constructor(public name: string) {}
getName() {
return this.name;
}
}

```

The difference between an abstract class and an interface is that it allows detailed implementation of properties or method members.

```undefined
abstract class Animal {
abstract name: string;
abstract getName(): string;
// Abstract class constructor can be made protected.
protected constructor(public legs: string) {}
getLegs() {
return this.legs
}
}

```

# Optional

Let`s take a look at several optional concepts using the keyword `?

## Parameters

First of all, you can specify optional parameters when declaring a type.
In the following example, `?You have specified `y` as an optional parameter using the keyword `.
Therefore, there is no error even if `y` does not have an argument to receive.

```undefined
function add(x: number, y?: number): number {
return x + (y || 0);
}
const sum = add(2);
console.log(sum);

```

The example above is exactly the following example:
This means that using the keyword `?` is equivalent to adding `| undefined`.

```undefined
function add(x: number, y: number | undefined): number {
return x + (y || 0);
}
const sum = add(2, undefined);
console.log(sum);

```

## Properties and Methods

The keyword `?` can also be used for properties and methods type declarations.
The following is an example from the interface part:
Declaring `isAdult` as an optional attribute will no longer cause errors.

```undefined
interface IUser {
name: string,
age: number,
isAdult?: boolean
}

let user1: IUser = {
name: 'Neo',
age: 123,
isAdult: true
};

let user2: IUser = {
name: 'Evan',
age: 456
};

```

It can also be used in Type or Class.

```undefined
interface IUser {
name: string,
age: number,
isAdult?: boolean
validate?(): boolean
}
type TUser = {
name: string,
age: number,
isAdult?: boolean
validate?(): boolean
}
abstract class CUser {
abstract name: string;
abstract age: number;
abstract isAdult?: boolean;
abstract validate?(): boolean;
}

```

## Chaining

The following example results in an error when the property `str` is `undefined` because the `toString` method is not available.
The problem can be solved by affirming that the attribute `str` is a string, but it is simpler to use the optional chaining operator `?You can use `.

Please refer to the MDN document for detailed instructions.
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining

```undefined
obj?.prop;
obj?.[expr];
arr?.[index];
func?.(args);

```

```undefined
// Error - TS2532: Object is possibly 'undefined'.
function toString(str: string | undefined) {
return str.toString();
}

// Type Assertion
function toString(str: string | undefined) {
return (str as string).toString();
}

// Optional Chaining
function toString(str: string | undefined) {
return str?.toString();
}

```

Especially `

```undefined
// Before
if (foo
```

## Nullish Merge Operator

In general, Falsey checks (checking `0`, ```, `NaN`, `null`, `undefined`) are often made using logical operators `||`.
Using a value of `0` or `"" as an effective value may result in undesired results, which may be useful in this case, the nullish coalescing operator `?You can use ` in the TypeScript.

Please refer to the MDN document for detailed instructions.
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator

```undefined
const foo = null ?? 'Hello nullish.';
console.log(foo); // Hello nullish.

const bar = false ?? true;
console.log(bar); // false

const baz = 0 ?? 12;
console.log(baz); // 0

```

# Module

In order to understand the modules in TypeScript, you must understand the JavaScript module first.
Many of the official documents of TypeScript contain descriptions of this JavaScript module, and we are only going to look at the differences in the module concept of TypeScript.

## Export and import

If you do not understand the export and import of JavaScript modules, please refer to the following MDN documents first.
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/export
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/import

TypeScript can export common variables, functions, and classes, as well as interfaces or type aliases to modules as follows:

```undefined
// myTypes.ts

// Export Interface
export interface IUser {
name: string,
age: number
}

// Export Type Alias
export type MyType = string | number;

```

```undefined
Import '// Declared Modules (myTypes.ts)
import { IUser, MyType } from './myTypes';

const user: IUser = {
name: 'HEROPY',
age: 85
};

const something: MyType = true; // Error - TS2322: Type 'true' is not assignable to type 'MyType'.

```

TypeScript provides export and import syntax for CommonJS/AMD/UMD modules such as `export = ABC;` and `import ABC = request(`abc`) ;`.
This provides a Default Export feature that exports only one object from one module, such as the `export default` on the ES6 module.

Eventually, the CommonJS/AMD/UMD modules can be imported from TypeScript as follows:
In addition, providing the compile option with `esModuleInterop: true` also allows the ES6 module to use the Default Import method.

```undefined
// CommonJS/AMD/UMD
import ABC = require('abc');
// or
import * as ABC from 'abc';
// or `"esModuleInterop": true`
import ABC from 'abc';

```

## Ambient module declaration

Let`s find out about the use of external JavaScript modules in TypeScript.
Let`s create a simple project and install and use Lodash as an external module.

> Please refer to the Development Environment / TS Node part for the type script project creation of the 'Module' part.

```bash
$ npm install lodash

```

Write a very simple code for console output using the Lodash module`s `camelCase` API in `main.ts`.
However, an error occurs during the `import` step as follows:
This is because there is no Ambient module declaration from the module that the TypeScript compiler can check.

```undefined
// main.ts

import * as _ from 'lodash'; // Error - TS2307: Cannot find module 'lodash'.

console.log(_.camelCase('import lodash module'));

```

Unlike type scripts that have both an implementation and a type declaration, if you use a JavaScript module (E.g. Lodash) that only has an implementation, you need a type declaration of a module that the compiler can understand, most of which is made into a `.d.ts` file.

Then let`s make a type declaration about Lodash.
Create the file `lodash.d.ts` in the root path as follows:

![image](https://heropy.blog/images/screenshot/typescript/typescript-module-ambient1.jpg)

The underlying structure is simple.
Use the keyword `module` to specify the module name so that you can import the module (Import).
And within that range, you only need to declare a variable with a type (`_`) and export it.

> The 'declare' keyword should be declared so that the TypeScript compiler can understand it!

```undefined
// lodash.d.ts

// Ambient module declaration
declare module 'lodash' {
// 1. Type (Interface) Declaration
interface ILodash {
camelCase(str?: string): string
}

// 2. Declaration of variables with type (interface)
const _: ILodash;

// 3. Export (CommonJS)
export = _;
}

```

For this type declaration to be included in the compilation process, use the reference tag (`reference />`) and `path` properties using the `//` (triple-slash directives) as follows:
Before we move on, let`s take a look at some of the characteristics of the reference tag.

- Imported as a reference tag is not a module implementation but a type declaration, so it should not be imported as the `import` keyword.
- The triple slash indicator is a simple comment when compiled into JavaScript.
- The `path` property specifies the relative path of the type declaration to be imported and must enter an extension.
- The `types` property specifies the module name, such as `/// ference types="lodash" />`, based on the compile option `typeRoots` and Definitely Typeped(`@types`).

> Compilation options 'typeRoots' and Definitely Typeped ('@types') are discussed later.

```undefined
'// Reference Tag (Triple-slash directive)
/// <reference path="./lodash.d.ts" />

import * as _ from 'lodash';

console.log(_.camelCase('import lodash module'));

```

Verify that the console output is normal.

```bash
$ npx ts-node main.ts
# importLodashModule

```

### Definitely Typed(@types)

In the previous part, we used Lodash`s `camelCase` method, and we are going to use `snakeCase` additionally.
However, since we did not declare the type for `snakeCase` in `lodash.d.ts`, the error occurs as follows.

```undefined
// main.ts

/// <reference path="./lodash.d.ts" />

import * as _ from 'lodash';

console.log(_.camelCase('import lodash module'));
console.log(_.snakeCase('import lodash module')); // Error - TS2339: Property 'snakeCase' does not exist on type 'ILodash'.

```

This can be solved simply by declaring a type for `snakeCase` in `lodash.d.ts`.

```undefined
// lodash.d.ts

declare module 'lodash' {
interface ILodash {
camelCase(str?: string): string
snapCase(str?:string): adding string // type declaration
}

const _: ILodash;
export = _;
}

```

However, creating a type declaration (typing, typing) is very inefficient for every module used in the project.
So we can use the Definitely Typered, which is made from the contribution of many users.
Many types of modules are defined and are being added continuously.

Install and use `npm install -D @types/module name`.
Search with `npm info @types/module name` to see if a type declaration of the desired module exists.

Install the Lodash type declaration as follows:

```bash
$ npm i -D @types/lodash

```

Now, delete `lodash.d.ts` because you don`t need it anymore!
Delete the reference tag (Triple-slash directive) of `main.ts` as well!
You can use a variety of Lodash APIs without any additional settings.

```undefined
// main.ts

import * as _ from 'lodash';

console.log(_.camelCase('import lodash module'));
console.log(_.snakeCase('import lodash module'));
console.log(_.kebabCase('import lodash module'));

```

```bash
$ npx ts-node main.ts
# importLodashModule
# import_lodash_module
# import-lodash-module

```

The principle of motion is simple.
The type declaration module ("@types/lodash") is installed on the path "node_modules/@types".
All type declarations in this path are automatically included in the compilation via Import Modules (Import).

![image](https://heropy.blog/images/screenshot/typescript/typescript-module-declaration-for-camelcase-in-lodash.jpg)

### TypeRoots and types options

As you can see above, there are situations where you don`t have to think about type declaration as follows when using the JavaScript module.

- Modules written in typescript from the beginning
- JavaScript modules that provide type declarations (such as `.d.ts` files) together
- JavaScript Module with Type Declaration Contributed to Definitely Typeted (`@types/module`)

However, you should also consider the following situations in which you are forced to write (typing, typing) your own type declaration:

- JavaScript module not found type declaration
- You need to modify the type declaration you have.

You can create and provide a type declaration yourself, such as `lodash.d.ts`, which you wrote above, and use the compilation option `typeRoots` as a way to manage it more easily.

To test the `typeRoots` option, create a new project, install Lodash, and create a `main.ts` file as shown below.

> Please refer to the Development Environment / TS Node part for the type script project creation of the 'Module' part.

```bash
$ npm install lodash

```

There is also an error in the `Import` stage.

```undefined
// main.ts

import * as _ from 'lodash'; // Error - TS2307: Cannot find module 'lodash'.

console.log(_.camelCase('import lodash module'));

```

To address this,
Create the `index.d.ts` file in the `types/lodash` path as shown below.
The option to compile the file `tsconfig.json` provides `typeRoots`: ["./types]`.
Before we move on, let`s take a look at some of the features of the `typeRoots` option.

- The default is `"typeRoots": ["./node_modules/@types"]`.
- The `typeRoots` option first navigates the `index.d.ts` file in the specified path.
- If you do not have an `index.d.ts` file, navigate to the path and file name created in the `types` or `typing` properties of `package.json`.
- Compilation error occurs if type declaration cannot be found.

```undefined
// types/lodash/index.d.ts

declare module 'lodash' {
interface ILodash {
camelCase(str?: string): string
}
const _: ILodash;
export = _;
}

```

![image](https://heropy.blog/images/screenshot/typescript/typescript-module-ambient2.jpg)

It will now work normally.

```bash
$ npx ts-node main.ts
# importLodashModule

```

The `typeRoots` option allows you to manage type declarations from multiple modules in the `types` directory.
Directory names are free to use not only `types`, but also `@types`, `_types`, and `types`.

Additionally, the compiler option `types` allows you to create only the module names to be whitelisted.
If you write "types": ["lodash"] then the "types" directory will only use Lodash`s type declaration.
Writing `"types": []` means not using type declarations from all modules in the `types` directory.
If you do not use the `types` option, you will use the type declaration of all modules in the `types` directory.

Adding and deleting module names will help you understand how they work.

> In general, you do not need to create the 'types' option.

![image](https://heropy.blog/images/screenshot/typescript/typescript-module-types-compile-option.jpg)

# TS Utility Type

There are several global utility types provided by TypeScript.
We included a simple example to help you understand.
See Utility Types for more information.

> Type variable 'T' is an abbreviation for Type, U' is another type, and 'K' is an abbreviation for Key.
For better understanding, type variables are 'TYPE' or 'TYPE1', 'U' is 'TYPE2' and 'K' is 'KEY'.

<table><thead><tr><th>Utility name</th><th>Description (representative type)</th><th>Type variable</th></tr></thead><tbody> <tr><td><code>`Partial`</code></td><td> Return a new type by selectively changing all properties of <code>`TYPE`</code> (interface)</td ><td><code>`&lt;TYPE&gt;`</code></td></tr><tr><td><code>`Required`</code></td><td><code > Return a new type with all attributes of `TYPE`</code> changed to required (interface)</td><td><code>`&lt;TYPE&gt;`</code></td></tr> <tr><td><code>`Readonly`</code></td><td> Return a new type with all properties of <code>`TYPE`</code> changed to read-only (interface)</td><td><code>`&lt;TYPE&gt;`</code></td></tr><tr><td><code>`Record`</code></td><td>< code>Returns a new type specifying `KEY`</code> as an attribute and <code>`TYPE`</code> as the type of the attribute value (interface)</td><td><code>`&lt ;KEY, TYPE&gt;`</code></td></tr><tr><td><code>`Pick`</code></td><td><code>`TYPE`</code > Return a new type of property selected from <code>`KEY`</code> (interface)</td><td><code>`&lt;TYPE, KEY&gt;`</code></td></ tr><tr><td><code>`Omit`</code></td></td> Omit attributes from <td><code>`TYPE`</code> to <code>`KEY`</code> And return the selected new type (interface)</td><td><code>`&lt;TYPE, KEY&gt;`</code></td></tr><tr ><td><code>`Exclude`</code></td><td><code>`TYPE1`</code> returns new type except <code>`TYPE2`</code> (union) </td><td><code>`&lt;TYPE1, TYPE2&gt;`</code></td></tr><tr><td><code>`Extract`</code></td> <td>Return a new type extracted from <code>`TYPE2`</code> from <code>`TYPE1`</code> (union)</td><td><code>`&lt;TYPE1, TYPE2&gt;` </code></td></tr><tr><td><code>`NonNullable`</code></td><td><code><code> in `TYPE`</code> Return new type except null`</code> and <code>`undefined`</code> (union)</td><td><code>`&lt;TYPE&gt;`</code></td></tr><tr><td><code>`Parameters`</code></td><td><code> Return the parameter type of `TYPE`</code> as a new tuple type (function, Tuple)</td><td><code>`&lt;TYPE&gt;`</code></td></tr><tr><td><code>`ConstructorParameters`</code></td> Return parameter type of <td><code>`TYPE`</code> as new tuple type (class, tuple)</td><td><code>`&lt;TYPE&gt;`</code></td></tr><tr><td><code>`ReturnType`</code></td><td> Return the return type of <code>`TYPE`</code> as a new type (function) </td><td><code>`&lt;TYPE&gt;`</code></td></tr><tr><td><code>`InstanceType`</code></td><td ><code>Return the instance type of `TYPE`</code> (class)</t d><td><code>`&lt;TYPE&gt;`</code></td></tr><tr><td><code>`ThisParameterType`</code></td><td>< code>Explicit <code>`this`</code> of `TYPE`</code> Returns parameter type as a new type (function)</td><td><code>`&lt;TYPE&gt;`< Explicit <code> of /code></td></tr><tr><td><code>`OmitThisParameter`</code></td><td><code>`TYPE`</code> `this`</code> Return a new type with parameters removed (function)</td><td><code>`&lt;TYPE&gt;`</code></td></tr><tr><td><code>`ThisType`</code></td> Specify the <code>`this`</code> context of <td><code>`TYPE`</code>, no separate return ! (Interface)</td><td><code>`&lt;TYPE&gt;`</code></td></tr></tbody></table>

 

### Partial

Returns a new type with all attributes of `TYPE` changed to optional (`?`)

> See the 'Options 속 Properties and Methods' part.

```undefined
Partial<TYPE>

```

```undefined
interface IUser {
name: string,
age: number
}

const userA: IUser = { // TS2741: Property 'age' is missing in type '{ name: string; }' but required in type 'IUser'.
name: 'A'
};
const userB: Partial<IUser> = {
name: 'B'
};

```

In the example above, `Partialial` can be understood as follows:

```undefined
interface INewType {
name?: string,
age?: number
}

```

### Required

Returns a new type with all attributes of the `TYPE` changed to required.

```undefined
Required<TYPE>

```

```undefined
interface IUser {
name?: string,
age?: number
}

const userA: IUser = {
name: 'A'
};
const userB: Required<IUser> = { // TS2741: Property 'age' is missing in type '{ name: string; }' but required in type 'Required<IUser>'.
name: 'B'
};

```

In the example above, `Requiredir` can be understood as follows:

```undefined
interface IUser {
name: string,
age: number
}

```

### Readonly

Returns a new type with all attributes of the `TYPE` changed to `readonly`

> See the 'Interface > Read-only Properties' part.

```undefined
Readonly<TYPE>

```

```undefined
interface IUser {
name: string,
age: number
}

const userA: IUser = {
name: 'A'
age: 12
};
userA.name = 'AA';

const userB: Readonly<IUser> = {
name: 'B'
age: 13
};
userB.name = 'BB'; // TS2540: Cannot assign to 'name' because it is a read-only property.

```

In the example above, `Readonly<` can be understood as follows:

```undefined
interface INewType {
readonly name: string,
readonly age: number
}

```

### Record

Returns a new type that specifies `KEY` as the attribute (Key) and `TYPE` as the type of attribute (Type).

```undefined
Record<KEY, TYPE>

```

```undefined
type TName = 'neo' | 'lewis';

const developers: Record<TName, number> = {
neo: 12,
lewis: 13
};

```

In the example above, `Record <TName, number>` can be understood as follows:

```undefined
interface INewType {
neo: number,
lewis: number
}

```

### Pick

Returns a new type of property selected from `TYPE` to `KEY`.
`TYPE` must be an interface or object type with properties.

```undefined
Pick<TYPE, KEY>

```

```undefined
interface IUser {
name: string,
age: number
email: string,
isValid: boolean
}
type TKey = 'name' | 'email';

const user: Pick<IUser, TKey> = {
name: 'Neo',
email: 'thesecon@gmail.com',
age: 22 // TS2322: Type '{ name: string; email: string; age: number; }' is not assignable to type 'Pick<IUser, TKey>'.
};

```

In the example above, the `Pick`, TKey` can be understood as follows:

```undefined
interface INewType {
name: string,
email: string,
}

```

### Omit

As opposed to the "Pick" we saw above,
Skip the attribute from `TYPE` to `KEY` and return the remaining selected new type.
`TYPE` must be an interface or object type with properties.

```undefined
Omit<TYPE, KEY>

```

```undefined
interface IUser {
name: string,
age: number
email: string,
isValid: boolean
}
type TKey = 'name' | 'email';

const user: Omit<IUser, TKey> = {
age: 22,
isValid: true,
name: 'Neo' // TS2322: Type '{ age: number; isValid: true; name: string; }' is not assignable to type 'Pick<IUser, "age" | "isValid">'.
};

```

In the example above, `Omit<, TKey` can be understood as follows:

```undefined
interface INewType {
// name: string,
age: number
// email: string,
isValid: boolean
}

```

### Exclude

Union `TYPE1` returns a new type except Union `TYPE2`.

```undefined
Exclude<TYPE1, TYPE2>

```

```undefined
type T = string | number;

const a: Exclude<T, number> = 'Only string';
const b: Exclude<T, number> = 1234; // TS2322: Type '123' is not assignable to type 'string'.
const c: T = 'String';
const d: T = 1234;

```

### Extract

Union `TYPE1` returns a new type extracted from Union `TYPE2`.

```undefined
Extract<TYPE1, TYPE2>

```

```undefined
type T = string | number;
type U = number | boolean;

const a: Extract<T, U> = 123;
const b: Extract<T, U> = 'Only number'; // TS2322: Type '"Only number"' is not assignable to type 'number'.

```

### NonNullable

Union `TYPE` returns a new type except `null` and `undefined`.

```undefined
NonNullable<TYPE>

```

```undefined
type T = string | number | undefined;

const a: T = undefined;
const b: NonNullable<T> = null; // TS2322: Type 'null' is not assignable to type 'string | number'.

```

### Parameters

Returns the parameter type of function `TYPE` to a new Tuple type.

```undefined
Parameters<TYPE>

```

```undefined
function fn(a: string | number, b: boolean) {
return `[${a}, ${b}]`;
}

const a: Parameters<typeof fn> = ['Hello', 123]; // Type 'number' is not assignable to type 'boolean'.

```

In the example above, `Parameters <typeofn>` can be understood as follows:

```undefined
[string | number, boolean]

```

### ConstructorParameters

Returns the parameter type of class `TYPE` to a new tuple type.

```undefined
ConstructorParameters<TYPE>

```

```undefined
class User {
constructor (public name: string, private age: number) {}
}

const neo = new User('Neo', 12);
const a: ConstructorParameters<typeof User> = ['Neo', 12];
const b: ConstructorParameters<typeof User> = ['Lewis']; // TS2741: Property '1' is missing in type '[string]' but required in type '[string, number]'.

```

In the example above, `Constructor Parameters <type of User>` can be understood as follows:

```undefined
[string, number]

```

### ReturnType

Returns the return type of function `TYPE` to a new type.

```undefined
ReturnType<TYPE>

```

```undefined
function fn(str: string) {
return str;
}

const a: ReturnType<typeof fn> = 'Only string';
const b: ReturnType<typeof fn> = 1234; // TS2322: Type '123' is not assignable to type 'string'.

```

### InstanceType

Returns the instance type of class `TYPE`.

```undefined
InstanceType<TYPE>

```

```undefined
class User {
constructor(public name: string) {}
}

const neo: InstanceType<typeof User> = new User('Neo');

```

### ThisParameterType

Returns the explicit `this` parameter type of function `TYPE` to a new type.
Returns an unknown type if the function `TYPE` does not have an explicit `this` parameter.

> Refer to the 'Function > This > Explicit This' part.

```undefined
ThisParameterType<TYPE>

```

```undefined
// https://www.typescriptlang.org/docs/handbook/utility-types.html#thisparametertype

function toHex(this: Number) {
return this.toString(16);
}

function numberToString(n: ThisParameterType<typeof toHex>) {
return toHex.apply(n);
}

```

In the example above, the explicit `this` type of function `toHex` is `Number`.
With reference to that type, declare the type of parameter `n` of function `numberToString`.
This prevents other types of `this` from binding to `toHex`.

### OmitThisParameter

Returns a new type that removes the explicit `this` parameter of the function `TYPE`.

```undefined
OmitThisParameter<TYPE>

```

```undefined
function getAge(this: typeof cat) {
return this.age;
}

// Existing Data
const cat = {
age: 12 // Number
};
getAge.call(cat); // 12

// New Data
const dog = {
age: '13' // String
};
getAge.call(dog); // TS2345: Argument of type '{ age: string; }' is not assignable to parameter of type '{ age: number; }'.

```

In the example above, the function `getAge` designed based on data `cat` cannot use a new data `dog` with some different types as `this`.
However, `OmitThisParameter` allows us to create a new type of function that eliminates explicit `this`.
Data `dog` can be used without modifying `getAge` directly.

```undefined
const getAgeForDog: OmitThisParameter<typeof getAge> = getAge;
getAgeForDog.call(dog); // '13'

```

> Note that 'this.age' can now contain any value.

### ThisType

Specifies the `this` context (Context) of the `TYPE` and does not return a separate type.

```undefined
ThisType<TYPE>

```

```undefined
interface IUser {
name: string,
getName: () => string
}

function makeNeo(methods: ThisType<IUser>) {
return { name: 'Neo', ...methods } as IUser;
}
const neo = makeNeo({
getName() {
return this.name;
}
});

neo.getName(); // Neo

```

The method `getName`, which is used as an argument for the function `makeNeo`, is internal `this`.Because `name` is used, `ThisType` explicitly sets the `this` context.
However, because `ThisType` does not return a separate type, the type for the `makeNeo` return value (`{ name: `Neo`, ...methods}`) is not normally inferred.
Therefore, it is necessary to assert the type separately, such as `as IUser`, to call `neo.getName` normally.

# References

https://www.typescriptlang.org/docs/home.html
https://www.tutorialsteacher.com/typescript
https://hyunseob.github.io/2017/12/12/typescript-type-inteference-and-type-assertion/
https://jsdev.kr/t/typescript-interface/3168
https://github.com/microsoft/TypeScript/wiki/`this`-in-TypeScript
https://github.com/Microsoft/TypeScript-Handbook/issues/180
https://medium.com/naver-fe-platform/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EB%9F%AC%EA%B0%80-%EB%AA%A8%EB%93%88-%ED%83%80%EC%9E%85-%EC%84%A0%EC%96%B8%EC%9D%84-%EC%B0%B8%EC%A1%B0%ED%95%98%EB%8A%94-%EA%B3%BC%EC%A0%95-5bfc55a88bb6