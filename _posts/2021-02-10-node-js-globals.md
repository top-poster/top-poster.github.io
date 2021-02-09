---
layout: post
title: "New Node.js Development - 3 - Globals"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/nodejs.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/nodejs.png)

Let`s learn about the global objects in Node.js that are available in all modules, such as using a timer function or figuring out the directory name of the current module.

# REPL

The Read-Eval-Print-Loop (REPL) feature in Node.js is very useful for experimenting with Node.js code and debugging JavaScript code.

Read: Read the user`s input, parse the input into the JavaScript data structure, and save it to memory.
Evaluating the data structure.
Print: Print the results.
Loop: Repeat the command until the user presses `ctrl`+`c` twice.

REPL can be started by running `node` in the Shell as follows:

```bash
$ node

```

You can use simple operations or logs.

```bash
> 1 + ( 2 * 3 ) - 4
3
> console.log('hello REPL!');
hello REPL!

```

REPL also supports multiline expression.

```bash
> for (var i = 0; i < 4; i++) {
... console.log(i);
... }
0
1
2
3

```

## REPL Commands

`ctrl`+`c`: Exits the current command.
`ctrl`+`c` (number 2): Exit Node REPL.
`ctrl`+`d`: Exit Node REPL
`Up`/`Down` key: Allows you to modify previous commands by navigating the command history.
The `tab` key: Displays the specified command or data that starts with the currently created command.
`.help`: Displays a list of all REPL commissions.
`.break`: Terminate multi-line expressions.
`.clear`: Exits a multi-line representation.
*.save <filename>: Saves the current Node REPL session to a file. For example, `.save log.txt`, `.save session.js`.
`.load >`: Imports files into the current Node REPL session.
`.editor`: use editor mode (`ctrl`+`d`: complete editor mode, `ctrl`+`c`: cancel editor mode)

```bash
> .editor
// Entering editor mode (^D to finish, ^C to cancel)
function welcome(name) {
return `Hello ${name}!`;
}

welcome('Node.js');

// ^D(ctrl+d) complete
'Hello Node.js'

```

Use `_` (Underscore Variable) to refer to the value in the last result.

```bash
> 1 + 1
2
> _ * 2
4
>

```

# global

Returns an object with global information from Node.js.

> Typically, the top-level scope in a browser is a global scope. However, in Node.js, the top range is not a global range. The content inside the module is the region for that module.

```js
console.log(global);
console.log(window); // window is not defined

```

# Console

Node.js Console
Provides a debugging console that displays standard output (stdout) and standard errors (stderr) similar to the JavaScript console mechanism provided by a web browser.

## console.log()

Displays data in the debugging console with standard output (stdout).

```js
console.log('hello world!');

```

### Placeholders

Each Placeholder token is replaced by its Arguments value.
The following Placeholder tokens are supported:

`%s`: String
`%d`: numeric (integer, floating point value)
`%i`: Integer
`%f`: Floating point value
`%j`: JSON
`%o`: Generic JavaScript Object

```js
console.log('My name is %s, I am %d.', 'HEROPY', 19);

```

```bash
My name is HEROPY, I am 19.

```

# Timers

The timer in Node.js calls a given function after a certain period of time.
The timer specifies the reference time that the provided callback should run after a certain amount of time, rather than the exact time that you want it to run.
Timer callback runs at the earliest time that can be scheduled after a specified time.
However, it may be delayed due to operating system scheduling or other callback execution.

## setInterval()

Repeat the callback function every delay (ms).
Returns the Timeout object to be used by `clearInterval()`.
If the delay time is greater than or less than `2147483647`, the delay time is set to `1`.

```js
setInterval(callback, delay)

```

```js
let sec = 0;
setInterval(() => {
sec++;
console.log(sec);
}, 1000);

```

### clearInterval()

Cancels the Timeout object created by `setInterval()`.

```js
let sec = 0;
const timer = setInterval(() => {
sec++;
console.log(sec);

if (sec > 9) {
clearInterval(timer);
return;
}
}, 1000);

```

## setTimeout()

Runs a one-time callback function after a latency (ms).
Returns the Timeout object to be used by `clearTimeout()`.
Node.js does not guarantee the exact timing or order in which a callback occurs.
The callback function is called as close as possible to the specified time.
If the delay time is greater than or less than `2147483647`, the delay time is set to `1`.

### clearTimeout()

Cancels the Timeout object created with `setTimeout()`.

## setImmediate()

Immediately execute callback within the I/O cycle.
Returns the Timeout object to be used by `clearImmediate()`.
When you call `setImmediate()` multiple times, the callback function waits for execution in the order of generation, and always runs before any scheduled timer within the I/O cycle, regardless of how many timers exist.

```js
const fs = require('fs');

fs.readFile(__filename, () => {
setTimeout(() => {
console.log('timeout');
}, 0);
setImmediate(() => {
console.log('immediate');
});
console.log('start');
console.log('end');
});

```

```bash
start
end
immediate
timeout

```

### clearImmediate()

Cancels the Timeout object created by `setImmediate()`.

# Modules

## `__`dirname

Indicates the directory name (path) of the current module.

```js
console.log(__dirname);

```

```bash
/Users/myDir/Project/

```

## `__`filename

Indicates the file name (path) of the current module.

```js
console.log(__filename);

```

```bash
/Users/myDir/Project/app.js

```

## module

In each module, the `module` is a reference to the object representing the current module and is not a Global but a Region (Local) for each module.

### module.exports

What the module declares is a Scope that must be referenced within the module.
If you want to refer to the module content outside the module, you can use `module.exports` to make it possible for external reference.

`require()` is required to refer to a specific module externally.

`name.js`:

```js
const myName = 'HEROPY';
module.exports = myName; // 내보내기

```

`hello.js`:

```js
Skip 'const name = request('./name') ; // import / '.js'.
console.log(`Hello ${name}`);

```

```bash
Hello HEROPY

```

### exports

`exports` is a short cut of `module.exports`.
But it`s not the same. Let`s look at the next example.

`exports.js`:

```js
const array = [1, 2, 3];
exports = array;

```

`module-exports.js`:

```js
const array = [1, 2, 3];
module.exports = array;

```

`app.js`:

```js
const e = require('./exports');
const me = require('./module-exports');

console.log(`exports: ${e[1]}`);
console.log(`module.exports: ${me[1]}`);

```

```bash
exports: undefined
module.exports: 2

```

`module.exports`에서 `.exports` is the property of the `module` object and can have a value (general data type).
However, `exports` must have a property or method with one object data.
Therefore, in the example above, `exports.You can modify js` to use it as follows:

`exports.js`:

```js
const array = [1, 2, 3];
exportsarray = array;

```

`app.js`:

```js
const e = require('./exports');

console.log(`exports: ${e.array[1]}`);

```

If this approach is inconvenient, we recommend using `module.exports`.

## require()

Use the defined module via `require()`.

`msg.js`:

```js
// Module Definition
module.exports = 'Hello World!';

```

`log.js`:

```js
Using the '// Module
const msg = require('./msg');
console.log(msg);

```

```bash
Hello World!

```

# Process

The `process` object provides and controls information about the Node.js process.

## process.arch

Node.js processes such as `arm` and `ia32` return the processor architecture identifier currently running.

```bash
'x64'

```

## process.env

Returns an object with user experience information (variable).

```js
const PORT = process.env.PORT || 3000;

```

Because `process.env` is an object, you can set a value (an environment variable) and it is converted to a string and assigned.

```js
process.env.MY_VARIABLE = true;
console.log(process.env.MY_VARIABLE);

```

```bash
`true`

```

The Windows operating system is not case sensitive, so be careful.

```js
process.env.MY_VARIABLE = true;
console.log(process.env.my_variable);

```

```bash
`true`

```

You can delete environment variables using `delete`.

```js
delete process.env.MY_VARIABLE;
console.log(process.env.MY_VARIABLE);

```

```coffeescript
undefined

```

## process.exit()

Instruct Node.js to terminate the process.
`code` is the default value that `0` can be omitted, `0` means `normal (success) termination` and `1` means `unnormal (failure) termination`.

```js
process.exit(1);

```

## process.nextTick()

`process.nextTick()` adds a callback to the `NextTick Queue`.

```js
process.nextTick(callback)

```

> A tick is to pull an event out of the 'Event Loop Queue' and run that event.

This is not a simple alias for `setTimeout(fn, 0)`, `setImmediate()`.
Use to give users the opportunity to assign event handlers before I/O occurs after creating objects.

What is an event loop?

## process.platform

The Node.js process returns the operating system platform identifier that is running.
``darwin``, ``freebsd``, ``linux``, ``sunos`` 또는 ``win32``

```bash
'darwin'

```

## process.memoryUsage()

Returns an object describing the memory usage of the Node.js process in bytes.

```bash
{
rss: 11145216,
heapTotal: 7184384,
heapUsed: 4982952,
external: 8644
}

```

`HeapTotal`, `HeapUsed`: Indicates the memory usage of V8.
`external`: Indicates the memory usage of C++ objects bound to JavaScript objects managed by V8.
`rss`: Resident Set Size (rss) is the amount of space occupied by the main memory device (subset of total allocated memory) that contains the heap, code segment, and stack of the process.

> Heap is where objects, strings, and closers are stored. The variables are stored in Stack, and the actual JavaScript code is in the code segment.

## process.uptime()

Returns the time (sec) that the current Node.js process has run.

```js
console.log(
process.uptime()
Math.floor(process.uptime())
)

```

```bash
130.072
130

```

## process.versions

Returns an object listing Node.js and the version of its dependencies.

```bash
{
http_parser: '2.7.0',
node: '8.9.4',
v8: '6.1.534.50',
uv: '1.15.0',
zlib: '1.2.11',
ares: '1.10.1-DEV',
modules: '57',
nghttp2: '1.25.0',
openssl: '1.0.2n',
icu: '59.1',
unicode: '9.0',
cldr: '31.0.1',
tz: '2017b'
}

```

# Buffers

The Buffer module provides a way to handle streams of binary data.

> Buffer means temporary storage space for bundles of data (Chunk) that are sent from one place to another to overcome the speed difference in I/O performance.

## Buffer.alloc()

Create a new, secure buffer of the desired length (Size).

```js
Buffer.alloc(size, fill, encoding)

```

> The existing 'new Buffer()' has been 'deprescribed' since v6.x.

```js
const buf = Buffer.alloc(4); // 4byte
console.log(buf);

```

```bash
<Buffer 00 00 00 00>

```

## Buffer.from()

Creates a new buffer filled with `String`, `Array`, or `Buffer`.

```js
Buffer.from()

```

> The existing 'new Buffer()' has been 'deprescribed' since v6.x.

Creates a new buffer with a string called `hello world`.
When generating a string, the encoding can be set as the second argument, which can be omitted, and the default value is `utf8`

```js
var buf1 = Buffer.from('hello world', 'utf8');
console.log(buf1);

```

```bash
<Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>

```

## Buffer.concat()

```js
Buffer.concat([buffer1, buffer2], totalLength)

```

Combine buffer objects into a single Buffer object (subjects).

```js
var buf1 = Buffer.from('abc'); // 3
var buf2 = Buffer.from('def'); // 3
var buf3 = Buffer.concat([buf1, buf2], 6);

console.log(
buf3,
buf3.toString()
)

```

```bash
<Buffer 61 62 63 64 65 66>
'abcdef'

```

If you do not specify `totalLength`, it is calculated automatically. However, additional loops are executed to calculate `totalLength`, so if you know the value, it is faster to provide it.
If the length of the buffer`s bond exceeds `totalLength`, the result is truncated.

```js
var buf1 = Buffer.from('abc'); // 3
var buf2 = Buffer.from('def'); // 3
var buf3 = Buffer.concat([buf1, buf2], 4);

console.log(
buf3,
buf3.toString()
)

```

```bash
<Buffer 61 62 63 64>
'abcd'

```

## Buffer.isBuffer()

Check if it is a Buffer object.

```js
Buffer.isBuffer(obj)

```

```js
var fs = require('fs')

fs.readFile('./abc.txt', function (err, data) {
console.log(
Buffer.isBuffer(data)
);
});

```

```bash
true

```

## buf.copy()

Copy and paste the Buffer object into the Target Buffer object.

```js
buf.copy(targetBuf, targetStart, sourceStart, sourceEnd)

```

`target`: Specifies the target buffer to paste the copied buffer.
`targetStart`: Specifies the starting position of the copied buffer. The default is `0`.
`sourceStart`: Specifies the starting location of the buffer to copy. The default is `0`.
`sourceEnd`: Specifies the end location of the Buffer object to copy. Default value is `buf`.Length`.

```js
var buf1 = Buffer.from('abcdef');
var buf2 = Buffer.from('ABCDEF');
buf1.copy(buf2, 1, 0, 2);
// buff2: Paste To
// 1: Where should I start with the target?
// 0: Where do you want me to copy it from?
// 2: How far should I copy the buff1?

console.log(
buf2,
buf2.toString()
);

```

```bash
<Buffer 41 61 62 44 45 46>
'AabDEF'

```

## buf.length

Returns the allocated memory in bytes.

```js
var buf1 = Buffer.from('abc');
var buf2 = Buffer.from('가나다');

console.log(
buf1.length, // 3byte
buf2.length // 9byte
);

```

```bash
3
9

```

> Hangul is '3byte' and spacing is '1byte'.

## buf.toString()

Decodes Buffer objects into strings based on character encoding.

```js
buf.toString(encoding, start, end)

```

`encoding`: Sets the character encoding to decode. The default is `utf8`.
`start`: Specifies the starting location to decode. The default is `0`.
`end`: Specifies the end location to decode. Default value is `buf`.Length`.

`abc.txt`:

```undefined
abcdefghijklmnopqrstuvwxyz

```

`app.js`:

```js
var fs = require('fs')

fs.readFile('./abc.txt', function (err, data) {
console.log(
data,
data.toString()
);
});

```

```bash
<Buffer 61 62 63 64 65 66 67 68 69 6a 6b 6c 6d 6e 6f 70 71 72 73 74 75 76 77 78 79 7a>
abcdefghijklmnopqrstuvwxyz

```

## buf.write()

Creates the specified string in the Buffer object.

```js
buf.write(string, offset, length, encoding)

```

```js
'var data = 'Ganadara';
var buf = Buffer.alloc(12);
buf.write(data);

console.log(
buf,
buf.toString()
);

```

```bash
<Buffer ea b0 80 eb 82 98 eb 8b a4 eb 9d bc>
"Ganadara."

```

# References

https://nodejs.org/dist/latest-v8.x/docs/api/
https://vimeo.com/96425312
http://voidcanvas.com/setimmediate-vs-nexttick-vs-settimeout/
https://developer.mozilla.org/ko/docs/Web/JavaScript/EventLoop
https://www.tutorialspoint.com/nodejs/nodejs_repl_terminal.htm