---
layout: post
title: "Resize Observer - Observing Element Size Changes"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/javascript.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/javascript.png)

The Resize Observer observes the size changes of the set elements.
You can use it without a variety of problems, such as infinite callback loops or circular dependencies, that can occur when you control the size change.

<iframe height="488" style="width:100%" scrolling="no" title="ResizeObserver example(textarea)" src="https://codepen.io/heropark/embed/ExaYWBa?height=488&amp;theme-id=default&amp;default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen=""><br>See the Pen <a href="https://codepen.io/heropark/pen/ExaYWBa" target="_blank" rel="noopener">ResizeObserver example(textarea)</a> by park young woong<br>(<a href="https://codepen.io/heropark" target="_blank" rel="noopener">@heropark</a>) on <a href="https://codepen.io" target="_blank" rel="noopener">CodePen</a>.<br></iframe>
# Polyfill

Since ResizeObserver is a standardized draft (ED, Editor`s Draft), it is highly recommended to use Polyfill because the use of native ResizeObserver may vary depending on the browser version.

This post uses @juggle/resize-observer.

> Specifications for ResizeObserver are subject to change because it is not an advisory stage (REC).
Be aware of major major conflicts, as major changes to standard specifications may result in changes to this library (@juggle/resize-observer).

## Installation and Basic usage

```bash
$ npm i @juggle/resize-observer

```

```js
import ResizeObserver from '@juggle/resize-observer'

const ro = new ResizeObserver(callback)
ro.observe(element)

```

## Tested Browsers

### Desktop

| ![chrome](<https://github.com/alrra/browser-logos/raw/master/src/chrome/chrome_64x64.png>) | ![safari](<https://github.com/alrra/browser-logos/raw/master/src/safari/safari_64x64.png>) | ![ff](<https://github.com/alrra/browser-logos/raw/master/src/firefox/firefox_64x64.png>) | ![opera](<https://github.com/alrra/browser-logos/raw/master/src/opera/opera_64x64.png>) | ![edge](<https://github.com/alrra/browser-logos/raw/master/src/edge/edge_64x64.png>) | ![edge](<https://github.com/alrra/browser-logos/raw/master/src/archive/edge_12-18/edge_12-18_64x64.png>) | ![IE](<https://github.com/alrra/browser-logos/raw/master/src/archive/internet-explorer_9-11/internet-explorer_9-11_64x64.png>) |
| ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| Chrome | Safari | Firefox | Opera | Edge | Edge 12-18 | IE11 |

IE 9-10 (with polyfills)\*\* |

### Mobile

| ![chrome](<https://github.com/alrra/browser-logos/raw/master/src/chrome/chrome_64x64.png>) | ![safari](<https://github.com/alrra/browser-logos/raw/master/src/safari/safari_64x64.png>) | ![ff](<https://github.com/alrra/browser-logos/raw/master/src/firefox/firefox_64x64.png>) | ![opera](<https://github.com/alrra/browser-logos/raw/master/src/opera/opera_64x64.png>) | ![opera mini](<https://github.com/alrra/browser-logos/raw/master/src/opera-mini/opera-mini_64x64.png>) | ![edge](<https://github.com/alrra/browser-logos/raw/master/src/archive/edge_12-18/edge_12-18_64x64.png>) | ![samsung internet](<https://github.com/alrra/browser-logos/raw/master/src/samsung-internet/samsung-internet_64x64.png>) |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| Chrome | Safari | Firefox | Opera | Opera Mini | Edge | Samsung Internet |

# ResizeObserver

Initialize the Observer with an instance created by `newResizeObserver` and specify the Element to observe.
The constructor has one argument (`callback`).

```js
// Initialize Observer
const ro = new ResizeObserver(callback)

// Register what to observe (element)
ro.observe(element)

```

## callback

When an observation is registered or the size changes, the observer executes a callback.
Callback has two arguments (`entries` and `observer`

```js
const ro = new ResizeObserver((entries, observer) => {})

ro.observe(element)

```

### entries

`entries` is an array of ResizeObserverEntry instances.
ResizeObserverEntry contains the following properties:

- `contentRect` (legacy): Rectangular information of observation target (DOMRectReadOnly)
- `target` (legacy): Element to be observed
- `contentBoxSize`: Size of `content-box` (content) for observation
- `borderBoxSize`: 관찰 대상의 `border-box`(content + padding + border) 크기

```js
const ro = new ResizeObserver((entries, observer) => {
entries.forEach(entry => {
console.log(entry)
})
})

ro.observe(element)

```

![image](https://heropy.blog/images/screenshot/resize-observer/resize-observer-entry-object.jpg)

### observer

Reference is made to the corresponding instance in which callback runs.

![image](https://heropy.blog/images/screenshot/resize-observer/resize-observer-object.jpg)

## Methods

### observe()

Start observing the target element.

```js
const ro1 = new ResizeObserver(callback1)
const ro2 = new ResizeObserver(callback2)

const div = document.querySelector('div')
const button = document.querySelector('button')
const textarea = document.querySelector('textarea')

ro1.observe(div) // observe DIV element
ro2.observe(button) //BUTTON element observation
ro2.observe(textarea) // TEXTAREA 요소 관찰

```

### unobserve()

Stop observing the target element.

```js
const ro1 = new ResizeObserver(callback1)
const ro2 = new ResizeObserver(callback2)

// ...

ro1.observe(div)
ro2.observe(button)
ro2.observe(textarea)

ro1.unobserve(button) // Nothing..
ro2.unobserve(button) // Stop observing BUTTON elements

```

### disconnect()

Stop observing all elements that the ResizeObserver instance observes.

```js
const ro1 = new ResizeObserver(callback1)
const ro2 = new ResizeObserver(callback2)

// ...

ro1.observe(div)
ro2.observe(button)
ro2.observe(textarea)

ro2.disconnect() // Stop observing all elements observed by 'ro2' (BUTTON, TEXTAREA)

```

# Example

<iframe height="516" style="width:100%" scrolling="no" title="Resize Observer example - Text overflow" src="https://codepen.io/heropark/embed/MWYgoVv?height=516&amp;theme-id=default&amp;default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen=""><br>See the Pen <a href="https://codepen.io/heropark/pen/MWYgoVv" target="_blank" rel="noopener">Resize Observer example - Text overflow</a> by park young woong<br>(<a href="https://codepen.io/heropark" target="_blank" rel="noopener">@heropark</a>) on <a href="https://codepen.io" target="_blank" rel="noopener">CodePen</a>.<br></iframe>
# References

https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver
https://github.com/juggle/resize-observer