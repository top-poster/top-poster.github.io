---
layout: post
title: "Intersection Observer - Observing the Visibility of Elements"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/javascript.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/javascript.png)

By default, Intersection observer observes the intersection of the browser viewport and the element you set, and provides the ability to distinguish whether the element is included in the viewport or not, and more easily, whether it is the element you see now on your screen.

Because this functionality runs asynchronously, it can be used without problems such as rendering performance or event continuous calls from event-based element observations such as `scroll`.

# IntersectionObserver

![image](https://heropy.blog/images/screenshot/intersection-observer/intersection-observer-summary.jpg)

Initialize the Observer with an instance (`io`) created by `new IntersectionObserver()` and specify the Element to observe.
The constructor has two arguments: `callback`, `options`.

```js
const io = new IntersectionObserver(callback, options) // 관찰자 초기화
io.observe(element) // registering what to observe (element)

```

## callback

When the target is registered or visibility changes, the observer executes a callback.
Callback has two arguments (`entries` and `observer`

```js
const io = new IntersectionObserver((entries, observer) => {}, options)
io.observe(element)

```

### entries

`entries` is an array of IntersectionObserverEntry instances.
IntersectionObserverEntry contains the following properties of Read only:

- `boundingClientRect`: square information of observation target (DOMRectReadOnly)
- `intersectionRect`: Crossed Area Information for Observation Target (DOMRectReadOnly)
- `intersectionRatio`: Percentage of cross-domain observations from `intersectionRect` to `boundingClientRect` (number)
- `isInterseferencing`: Observed target`s state of intersection (Boolean)
- `rootBounds`: Square information for the root element specified (DOMRectReadOnly)
- `target`: Element to be observed
- `time`: information about the time the change occurred (DOMHighResTimeStamp)

```js
const io = new IntersectionObserver((entries, observer) => {
entries.forEach(entry => {
console.log(entry) // entry is 'IntersectionObserverEntry'
})
}, options)

io.observe(element)

```

![image](https://heropy.blog/images/screenshot/intersection-observer/intersection-observer-entry-object.jpg)

Returns the square information (DOMRectReadOnly) of the observation target.
The same value can be obtained using `Element.getBoundingClientRect()` (Reflow occurs in the `getBoundingClientRect` call).

![image](https://heropy.blog/images/screenshot/intersection-observer/intersection-observer-bounding-client-rect.jpg)

Returns square information (DOMRectReadOnly) about the region that intersects (overlaps) the observed target with the root element.

![image](https://heropy.blog/images/screenshot/intersection-observer/intersection-observer-intersection-rect.jpg)

Returns a number between `0.0` and `1.0` for how much (overlapping) the observation crosses the root element.
This represents the ratio of the `intersectionRect` area to the `boundingClientRect` area.

Value (Boolean) indicating whether the observation target enters an intersection with the root element (`true`) or exits an intersection (`false`).

![image](https://heropy.blog/images/screenshot/intersection-observer/intersection-observer-is-intersecting.jpg)

Returns the square information (DOMRectReadOnly) for the root element.
This is changed by option `rootMargin` and returns `null` if a separate root element (option `root`) is not declared.

Returns the observation (Element).

Returns a DOMHighResTimeStamp indicating when a cross-state change occurred based on the time the document was created.

### observer

Reference is made to the corresponding instance in which callback runs.

```js
const io = new IntersectionObserver((entries, observer) => {
console.log(observer)
}, options)

io.observe(element)

```

![image](https://heropy.blog/images/screenshot/intersection-observer/intersection-observer-observer.jpg)

## options

### root

Specifies the element object (root element) to be used instead of the viewport to check the visibility of the target.
Must be an ancestor of the target, and if not specified or `null`, the browser`s viewport is used by default.
The default is `null`.

```js
const io = new IntersectionObserver(callback, {
root: document.getElementById('my-viewport')
})

```

![image](https://heropy.blog/images/screenshot/intersection-observer/intersection-observer-root.jpg)

### rootMargin

The outer margin (Margin) allows you to expand or shrink the Root range.
The margin can be set in four stages, such as `margin` in CSS, and can be expressed as `px` or `%`.
The default value is `0px 0px 0px 0px` and you must enter a unit.

- TOP, RIGHT, BOTTOM, LEFT / e.g. `10px 0px 30px 0px`
- TOP, (LEFT, RIGHT), BOTTOM / e.g. `10px 0px 30px`
- (TOP, BOTTOM), (LEFT, RIGHT) / e.g. `30px 0px`
- (TOP, BOTTOM, LEFT, RIGHT) / e.g. `30px`

```js
const io = new IntersectionObserver(callback, {
rootMargin: '200px 0px'
})

```

![image](https://heropy.blog/images/screenshot/intersection-observer/intersection-observer-root-margin-0.jpg)

### threshold

Shows the percentage of visibility the observer needs to have in order to run.
The default value is Array type `[0]`, but can also be written as a single value of Number type.

- `0`: When the target`s edge pixels cross the Root range (when the target has 0% visibility), the observer is executed.
- `0.3`: The Observer runs when the target has 30% visibility.
- `[0, 0.3, 1]`: The Observer runs when the target has 0%, 30% and 100% visibility.

```js
const io = new IntersectionObserver(callback, {
threshold: 0.3 // or `threshold: [0.3]`
})

```

![image](https://heropy.blog/images/screenshot/intersection-observer/intersection-observer-threshold-0.jpg)

## Methods

### observe()

Start observing the target element.

```js
const io1 = new IntersectionObserver(callback, options)
const io2 = new IntersectionObserver(callback, options)

const div = document.querySelector('div')
const li = document.querySelector('li')
const h2 = document.querySelector('h2')

io1.observe(div) // observe DIV element
io2.observe(li) // observe LI element
io2.observe(h2) // h2 element observation

```

### unobserve()

Stop observing the target element.
You must specify one target element as an argument to stop the observation.
However, it does not perform any action if an argument specifies a target element that the IntersectionObserver instance is not observing.

```js
const io1 = new IntersectionObserver(callback, options)
const io2 = new IntersectionObserver(callback, options)

// ...

io1.observe(div)
io2.observe(li)
io2.observe(h2)

io1.unobserve(h2) // nothing..
io2.unobserve(h2) // Stop observing H2 elements

```

The second argument `observer` in the callback refers to that instance, so you can also write:

```js
const io1 = new IntersectionObserver((entries, observer) => {
entries.forEach(entry => {
// Changes in visibility will cause callbacks to occur throughout the observation.
// Do not run if the observation target's cross state is false (invisible).
if (!entry.isIntersecting) {
return
}
// Run if the observation target's cross-status is true (visible).
// ...

// Process the above runs (1 time) and stop observing
observer.unobserve(entry.target)
})
}, options)

```

### disconnect()

The IntersectionObserver instance stops observing all elements.

```js
const io1 = new IntersectionObserver(callback, options)
const io2 = new IntersectionObserver(callback, options)

// ...

io1.observe(div)
io2.observe(li)
io2.observe(h2)

stop observing all elements (LI, H2) observed by io2.disconnect() // io2

```

### takeRecords()

Returns an array of IntersectionObserverEntry objects.

> You do not need to invoke this method under normal circumstances.

# IE support (polyfill)

The library is officially supported for use by browsers (MSIE) that do not support the IntersectionObserver API.
You just need to import the module without setting it up.

```bash
$ npm i intersection-observer

```

```js
// For IE
import 'intersection-observer'

// Use the same.
const io = new IntersectionObserver(callback, options)

const els = document.querySelectorAll('element')
// For IE
Array.prototype.slice.call(els).forEach(el => {
io.observe(el)
})

```

Alternatively, you can use it globally as follows:

```xml
<script src="https://polyfill.io/v3/polyfill.min.js?features=IntersectionObserver"></script>

```

# Example

## Infinite scroll

![image](https://heropy.blog/images/screenshot/intersection-observer/intersection-observer-infinite-scroll-example-chrome-network-tab-setting.jpg)

# References

https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API
https://github.com/w3c/IntersectionObserver/tree/master/polyfill