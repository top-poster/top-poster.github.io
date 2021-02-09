---
layout: post
title: "Create 3D images with PixiJS and Depth map"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/pixi.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/pixi.png)

Use the PixiJS library and Depth map to create a 3D image.
The image sources used in this post are indicated at the bottom of each image.
The key code is Red Stapler`s Create 3D Photo from Image JavaScript Tutorial.
We`ve created and deployed an NPM module (pixi-3d-photo) that makes it easier for you to apply an example:D

```bash
$ npm i pixi-3d-photo

```

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/github.png)

# PixiJS란?

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/pixijs-homepage.jpg)

PixiJS is a high-speed HTML5 2D rendering library using WebGL.
Canvas or WebGL APIs can be used similarly to Flash APIs.

## WebGL

WebGL uses JavaScript to render graphics in a browser without the need for a separate plug-in.
Easily a web version of OpenGL, using the actual OpenGL ES 2.0-based API.
Supported by most modern browsers, GPU acceleration allows you to achieve a variety of image processing and effects.
Note that you should check your browser options for GPU acceleration.

For Chrome,
The `Advanced` setting allows you to control that option (using hardware acceleration if possible).

```undefined
chrome://settings

```

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/hardware-acceleration.jpg)

You can check WebGL entries in the Graphics Feature Status.

```undefined
chrome://gpu

```

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/chrome-gpu.jpg)

# What is Depth map?

`Depth map` refers to an image with information about the distance of an object representation at a given point in time.
Typically expressed in gray scale, the brighter the color, the closer the distance, and the darker the distance the longer the distance is.

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/cubic.jpg)

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/cubic.map.jpg)

# Example

## Pikachu

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/sample.gif)

Let`s make a 3D Pikachu image using PixiJS.
The necessary supplies are Pikachu original image and Depth map.
The size and depth information of the two images must match to work properly.

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/pikachu.jpg)

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/pikachu.map.jpg)

The code is as follows:

```css
your_project
├-- pikachu.jpg
├-- pikachu.map.jpg
└-- index.html

```

```xml
<!-- index.html -->

<script src="https://cdnjs.cloudflare.com/ajax/libs/pixi.js/5.1.3/pixi.min.js"></script>

<div id="pikachu"></div>

<script>
// Source image path and Depth map path.
let src = 'pikachu.jpg'
let srcMap = 'pikachu.map.jpg'
// The size of the image to be output.
let width = 768
let height = 432
// HTML elements.
const $target = document.querySelector('#pikachu');
$target.style.width = `${width}px`;
$target.style.height = `${height}px`;

// Define the PixiJS app as the image size to be output.
let app = new PIXI.Application({
width,
height
});
$target.appendChild(app.view);

// Define the original image (Sprite object) that will be rendered on the screen.
let img = new PIXI.Sprite.from(src);
img.width = width;
img.height = height;

// Define the Depth map (Sprite object) to be rendered on the screen.
let depthMap = new PIXI.Sprite.from(srcMap);
depthMap.width = width;
depthMap.height = height;

// register filters that can change the position information of the image using the specified Depth map.
let displacementFilter = new PIXI.filters.DisplacementFilter(depthMap);

// register each image on the app stage.
app.stage.addChild(img);
app.stage.addChild(depthMap);
// Register filter information in App Stage.
app.stage.filters = [displacementFilter];

// Move the mouse within an HTML element.
// modify filter information with that information to achieve the desired effect.
$target.onmousemove = function(e) {
displacementFilter.scale.x = (width / 2 - e.clientX) / 20;
displacementFilter.scale.y = (height / 2 - e.clientY) / 20;
};
</script>

```

## Car

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/car-sample.gif)

The necessary supplies are also the original image and the Depth map.
This time, I will make it simpler with the module I distributed.

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/car.jpg)

![image](https://heropy.blog/images/screenshot/generate-3d-photo-by-pixijs/car.map.jpg)

```xml
<div id="car"></div>

```

```js
import { generate3dPhoto } from 'pixi-3d-photo'

generate3dPhoto({
el: '#car',
src: 'car.jpg',
map: 'car.map.jpg'
scale: 1.3
})

```

A simple project is really convenient for Parcel Bundler!
Skip the detailed usage.
(Parcel - Easy, Fast, Powerful Web App Bundler)

```bash
$ parcel index.html

```

# References

https://redstapler.co/3d-photo-from-image-javascript-tutorial/
https://www.oculus.com/blog/introducing-new-features-for-3d-photos-on-facebook/?locale=en_US
http://pixijs.download/release/docs/index.html