---
layout: post
title: "HTML IMG의 srcset과 sizes 속성(updated)"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/html5.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/html5.png)

# a word of mouth

I would like to summarize some of my thoughts because I often get similar questions regarding this post.
I will summarize it in terms and expressions as easy as possible.
I hope it helps you.

The first property `srcset` means `a set of image sources`.
It is a property that specifies at least two identical images with different sizes of the same proportion.
If you want to specify only one image source, you can use the `src` attribute.

Secondly, I`ve been emphasizing it in the text and in the comments.
The `srcset` property is the property that delegates the image selection to the browser.
Developers don`t need to know what size images will be selected in which environment, and it`s good for their mental health to just pretend they don`t know even if they know.
In many user environments, each browser is always trying to show the best size image to the user.
As a representative example, if a browser brings up a large image once, it doesn`t feel the need to bring up a small image with low resolution, so even if the viewport size changes, it doesn`t update the image.
This is because importing images requires multiple costs, such as network, memory, performance, and time.
For that reason, we used the expression `Same images with different sizes of the same proportions`.

Third, if you want to use different images of different sizes in different proportions,
We recommend using `@media` (media query or media rule) in the CSS, not the `srcset` property of the IMG element.
This is because developers can accurately specify the results to be printed according to the desired resolution.
Other reasons are described as `second`.

Fourth, the `x` and `w` descriptors are units for developers to tell browsers about the size of images.
As I mentioned in the second description, the image load is expensive to pay.
The developer specifies the image size with an `x` or `w` detector so that the browser can know the size of the image without paying.
Therefore, it is important to verify and enter the exact size of the image.

Fifth, Macs often use `scaling` for display resolution.
To put it simply, the display zoom rate is not 100%, but the resolution is 120%, 150%.
Windows has the same functionality, but it`s often not the default option, so I`d like to say less.
All of them can be found in the OS display options.
In particular, Macs often have difficulty solving and understanding problems because they can`t choose options with accurate resolution figures.
Therefore, it is also recommended that you use a program such as RDM (https://github.com/avibrazil/RDM)) on a Mac.
Anyway, if the display is zoomed in/out, the image should be rendered in a zoomed size accordingly.
As a result, the image is blurred in some sections of zoom.
Most images are bitmap (jpg, png, gif, etc.) and are caused by pixel-wise rendering.
However, some browsers arbitrarily adjust the image ratio to solve this phenomenon.
For example, if it`s 141% enlarged, it`s 150%.
That`s why the image you want might not come out of the viewport size.
Other things like the second one can also.

Sixth, the "x" and "w" descriptors are used as needed, not in units of better or worse.
As far as I know, `w` descriptors are relatively more up-to-date, and there is much more information related to `x` detectors that have been used a lot before.
As the iPhone moved from 3 to 4, it had to support 3 and 4 at the same time, so the image needed for iOS development began to be demanded in a specific multiples size.
(e.g., I had to manage both image.png and image@2x.png at the same time.
Since then, many mobile developments have used drainage methods, making `x` descriptors useful.
And there are times when it`s requested through API.
Therefore, there seems to be some differences between the use of `x` and `w` descriptors, and this is what I think.
`If you don`t have to write `x` in general web development cases and just provide various images with `w` descriptors, wouldn`t the browser have the right to select images and use the images it needs?` However, if the development requirement is an `x` detector, it must be written!’
Because many user environments do not require the same drainage image, developers cannot prepare images that correspond to all environments, and I think `w` descriptors have become more important.
And `w` means `Width`, so is there any `h` descriptor that means `Height`?
Although we`ve actually discussed the introduction of `h` descriptors, it has been discarded as a number of issues, one of which is the complexity of use and awareness.
In conclusion, if the developer clearly specifies the width of the image using a `w` detector, the browser will take care of the response to the large number of user environments, so you can think less.

That`s my opinion on the questions that I often get through comments and e-mails.
It is a very subjective answer based on various information, so no unconditional trust is allowed.
If you have any other opinions or information than above, please comment or e-mail.
Thank you.

# Changes

## April 2020

- To avoid confusion with the `px` unit, the `unit` word of `x` and `w` descriptor was changed to `Discriptor`.
- Added `Measure My Device Screen` link to X descriptor part.
- I corrected some contents and typos.

# a general outline

To support images on the reactive web, the CSS Media Rule (`@media`), commonly referred to as `media query`, uses the `background-image` property, which requires consideration of many environments, from the size of the Viewport to the resolution of the user`s screen.
However, with HTML IMG `srcset` and `sizes`, we can easily pass most of the considerations to the user agent by simply setting the size of the image.

# Before the test...

There is something to check first for the test.
Because images can be cached and not refreshed (if using Chrome browser) check `disable cache` in developer tool (F12)!

![image](https://heropy.blog/images/screenshot/html-img-srcset-and-sizes/chrome_network_disable_cache.jpg)

And the images used in the example are attached at the bottom of the post.

# Simple example

```xml
<img
srcset="images/heropy_small.png 400w,
images/heropy_medium.png 700w,
images/heropy_large.png 1000w"
sizes="(max-width: 500px) 444px,
(max-width: 800px) 777px,
1222px"
src="images/heropy.png"
alt="HEROPY" />

```

The example above can be understood as follows:
The `srcset` property specifies the path of the images to be used and the source size of the images.
The `sizes` property specifies the comma-separated media condition (optional) and the image size to be optimized accordingly.

```xml
<img
srcset="path source size,
Path source size,
Path Source Size"
sizes= (media condition) optimized output size,
(Media Conditions) Optimized Output Size,
Default Output Size"
src="path"
alt="Alternative Text" />

```

It`s not a normal IMG method, so it`s a little unfamiliar.
But you don`t have to think hard!
Let`s find out step by step.

> The 'src' property works in an environment where 'srcset' is not available!

# srcset

`srcset` specifies the original size of the images to be presented (used) in the browser.

It`s simple to use.

Prepare at least two images for each size and write them in the `srcset` attribute.
Note, however, that the size of the image requires you to enter the `w` or `x` descriptors, not the `px` units, and enter the smaller images in order.

## W descriptor

The `w` descriptor means the original size (width and width) of the image.
For example, the `w` value of the `400x300` (px) size image is `400w`.

> The browser (User agent) calculates the optimized pixel density of each image through the specified 'w' detector.

Let`s look at the next example.

```xml
<img
srcset="images/heropy_small.png 400w,
images/heropy_medium.png 700w,
images/heropy_large.png 1000w"
src="images/heropy.png"
alt="HEROPY" />

```

As a result of the above example,
`heropy_small.png` (400px) is used when the viewport width is less than 400px.
`heropy_medium.png` (700px) is used when the viewport width is 401 to 700px.
When the viewport width is 701px or greater, it is called `heropy_large.png` (1000px) is used.

> The viewport width in all examples below means the horizontal width of the viewport.

![image](https://heropy.blog/images/screenshot/html-img-srcset-and-sizes/example_1.jpg)

We only entered 3 images and their size into `srcset`, but the browser selects and outputs images optimized for the current viewport width among each image.
It is similar to the following CSS media conditions:

```css
.some-image {
width: 400px;
height: 400px;
background-image: url("images/heropy_small.png");
background-repeat: no-repeat;
}
@media (min-width: 401px) {
.some-image {
width: 700px;
height: 700px;
background-image: url("images/heropy_medium.png");
}
}
@media (min-width: 701px) {
.some-image {
width: 1000px;
height: 1000px;
background-image: url("images/heropy_large.png");
}
}

```

To maintain a fixed image size, you can add the `width` attribute.
(It`s a different concept from `sizes` attributes!)

```xml
<img
srcset="images/heropy_small.png 400w,
images/heropy_medium.png 700w,
images/heropy_large.png 1000w"
width="400"
src="images/heropy.png"
alt="HEROPY" />

```

![image](https://heropy.blog/images/screenshot/html-img-srcset-and-sizes/example_2.jpg)

This is similar to the following:

```css
.some-image {
width: 400px;
height: 400px;
background-image: url("images/heropy_small.png");
background-repeat: no-repeat;
background-size: cover;
}
@media (min-width: 401px) {
.some-image {
background-image: url("images/heropy_medium.png");
}
}
@media (min-width: 701px) {
.some-image {
background-image: url("images/heropy_large.png");
}
}

```

## X descriptor

The `x` descriptor refers to the ratio intention of the image.
The example used in the above `w` descriptor can be modified as follows:

```xml
<img
srcset="images/heropy_small.png 1x,
images/heropy_medium.png 1.75x,
images/heropy_large.png 2.5x"
src="images/heropy.png"
alt="HEROPY" />

```

The `x` detector is selected for optimization to match the pixel ratio of the device.
You can view the measurements on the current screen at mydevice.io.

Typically, it is recommended to provide an integer value.

> Using a 'w' detector does not require the use of an 'x' detector.
In many cases, the use of a 'w' detector is recommended.

# sizes

`sizes` specifies the media condition and the `optimization output size` of the corresponding image.

Let`s look at the next example.

```xml
<img
srcset="images/heropy_small.png 400w,
images/heropy_medium.png 700w,
images/heropy_large.png 1000w"
sizes="(min-width: 1000px) 700px"
src="images/heropy.png"
alt="HEROPY" />

```

As a result of the above example,
`heropy_small.png` (400px) is used when the viewport width is less than 400px.
`heropy_medium.png` (700px) is used when the viewport width is 401 to 700px.
When the viewport is 701~999px wide, it is called heropy_large.png` (1000px) is used.
`heropy_medium.png` (700px) is used when the viewport width is greater than 1000px.

![image](https://heropy.blog/images/screenshot/html-img-srcset-and-sizes/example_3.jpg)

In `sizes=` (min-width: 1000px) 700px`, `min-width: 1000px` means `when the width of the viewport is more than 1000px`, and `700px` means that the image will be optimized to 700px under that condition.
If so, the best image to be used in the `srcset` list for outputting images at 700px is `heropy_medium.png`, and as a result `heropy_medium.png` was used when the viewport width (landscape) was over 1000px.

Let`s look at the next example.
I skipped the media condition this time.

```xml
<img
srcset="images/heropy_small.png 400w,
images/heropy_medium.png 700w,
images/heropy_large.png 1000w"
sizes="500px"
src="images/heropy.png"
alt="HEROPY" />

```

As a result of the above example,
Regardless of the width of the viewport (`don`t care?!`) only `heropy_medium.png` is used.
Also, `heropy_medium.png` has a size of `500px` (originally a 700px image).

![image](https://heropy.blog/images/screenshot/html-img-srcset-and-sizes/example_4.jpg)

I`m not sure yet why only one image is used regardless of the width of the viewport, but let`s compare it to the following example.
There is no `sizes` but `width` this time.

```xml
<img
srcset="images/heropy_small.png 400w,
images/heropy_medium.png 700w,
images/heropy_large.png 1000w"
width="500"
src="images/heropy.png"
alt="HEROPY" />

```

As a result of the above example,
`heropy_small.png` is used when the viewport width is less than 400px.
`heropy_medium.png` is used when the viewport width is 401 to 700px.
`heropy_large.png` is used when the viewport width is 701px or higher.
The image used depends on the width of the viewport, but the size is fixed at `500px`.

![image](https://heropy.blog/images/screenshot/html-img-srcset-and-sizes/example_5.jpg)

Does that make sense?!
While `width` only specifies the `output size` of the image, `sizes` also specifies the `output size` + `optimal size` of the image.
Therefore, the first example of `sizes="500px" is the image optimized for `500px` and the image size is also set to `500px`.

> When writing 'sizes' and 'width', 'width' takes precedence.

If the concept is understood, you can also create several conditions:
Be aware of the separation using commas (`,`).

```xml
<img
srcset="images/heropy_small.png 400w,
images/heropy_medium.png 700w,
images/heropy_large.png 1000w"
sizes="(min-width: 701px) 1000px,
(min-width: 401px) 700px,
400px"
src="images/heropy.png"
alt="HEROPY" />

```

# Browser Support

`srcset` and `sizes` do not support IE.

https://caniuse.com/#feat=srcset

![image](https://heropy.blog/images/screenshot/html-img-srcset-and-sizes/browser_support.jpg)

## Polyfill

https://github.com/aFarkas/respimage

# Sample images

![image](https://heropy.blog/images/screenshot/html-img-srcset-and-sizes/heropy.png)

# References

https://developer.mozilla.org/ko/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images
https://stackoverflow.com/questions/40890825/explain-w-in-srcset-of-image
https://github.com/ResponsiveImagesCG/picture-element/issues/85
https://stackoverflow.com/questions/26928828/html5-srcset-mixing-x-and-w-syntax