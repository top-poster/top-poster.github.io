---
layout: post
title: "Flexible Box (CSS) Complete"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/css3.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/css3.png)

Most sites have a vertical layout and use it by scrolling up and down.
When configuring layouts, the elements that you use most are basically displayed as a block concept, which is stacked vertically (from top to bottom) in the view, making vertical configurations relatively easy to make.
However, the situation is slightly different for horizontal (left to right) configurations.

The problem is that the properties of creating horizontal structures were not clear, so in many cases we received help such as `table`, `float`, or `inline-block`.
However, because these methods have a variety of problems (Clear, White space, etc., they can be solved, but..), they are the next best option for horizontal layout configurations, and now we can easily construct layouts with a clear concept (attributes) called Flex (Flexible Box).

> I mentioned easy horizontal configuration above, but Flex can also do easy vertical configuration.

Before we begin, let`s look at a simple topic.
For horizontal configurations using the `float` property, you can create the following styles:

```xml
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="clear-element"></div>

```

```css
.box {
float: left;
}
.clear-element {
clear: both; /* or left */
}

```

Without further explanation, the above method is simple to look at, but also requires the next element of the name class (except `box`), which is very inconvenient to use in practice, and in many cases uses the following method as a non-obvious way.

```xml
<div class="clearfix">
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
</div>

```

```css
''/* The most principled method except for IE nuclear or other methods */
.clearfix::after {
content: "";
clear: both;
display: block;
}
.box {
float: left;
}

```

If you look at the examples, you`ll see the elements that will be horizontal. Apply `float` and apply the `clearfix` that is set in advance to the Container of those elements.

So how do you create a Flexible Box?
It`s as easy as that.

```xml
<div class="box-container">
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
</div>

```

```css
.box-container {
display: flex;
}

```

Flex applies `display: flex;` to the container of elements that will be horizontal.
(You can configure horizontal elements fairly easily and quickly, often without requiring detailed properties.)

# CSS3 Flexible Box

Flex provides an efficient way to align each element, even when its size is unclear or dynamic.

First of all, Flex is divided into two concepts.
The first one is Container and the second one is Items.
As you can see above, Container is a parent element surrounding Items, and Container is essential to align each Item.

The caveat is that the properties that apply to Container and Items are separated.
Properties such as `display`, `flex-flow`, and `justify-content` can be used for Container.
Items can have attributes such as `order`, `flex`, and `align-self`.

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-base.jpg)

## Flex Container

The attributes for Flex Container are as follows:
The concept of main-axis and cross-axis is discussed later.

<table><thead><tr><th>property</th><th>meaning</th></tr></thead><tbody><tr><td>display</td><td>
 Define Flex Container</td></tr><tr><td>flex-flow</td><td><code>`flex-direction`</code> and <code>`flex-wrap`<
 Shorthand property of /code></td></tr><tr><td>flex-direction</td><td>Set main-axis of Flex Items</td></tr>
 <tr><td>flex-wrap</td><td>Set multi-line wrapping (line break) for Flex Items</td></tr><tr><td>justify-content</td><td
 >Set the alignment method of the main-axis</td></tr><tr><td>align-content</td><td>Set the alignment method of the cross-axis (
 2 or more lines)</td></tr><tr><td>align-items</td><td>Set the alignment method of items in the cross-axis (1 line)</td>
 </tr></tbody></table>

 

### display

Define Flex Container with the attribute `display`.
Usually, the display method of elements is used in conjunction with `display: block;`, `display: inline-block` or `display: none;`.
The display method of the same element is defined as Flex (`display: flex` and `display: inline-flex`) rather than Block or Inline.

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 flex</td><td>Defines a Flex container with Block property</td><td></td></tr><tr><td>inline-flex</td><td> Flex with Inline property
 Define Container</td><td></td></tr></tbody></table>

 

The difference between `flex` and `inline-flex` is simple.
Flex Container designated as `display: flex;` has the same tendency (vertical stacking) as the Block element.
Flex Container designated as `display: inline-flex` has the same propensity (horizontal stack) as the Inline (Inline Block) element.

Let`s note that the vertical and horizontal stacking here is not Items but Container.
The difference between the two values does not affect the internal Items.

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-display.jpg)

### flex-flow

This property sets the main-axis of Flex Items as a short property and also sets the multiline bundle of items (line break).

```undefined
flex-flow: string of main spindle;

```

```css
.flex-container {
flex-flow: row-reverse wrap;
}

```

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 flex-direction</td><td>Set the main-axis of items</td><td><code>`row`</code></td></tr><tr><
td>flex-wrap</td><td>Set multiline wrapping (line breaks) for items</td><td><code>`nowrap`</code></td></tr></tbody
 ></table>

 

Let`s find out the individual properties.

Sets the main-axis of the Items.

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 row</td><td>Display Itmes horizontally (left to right)</td><td><code>`row`</code></td></tr><tr><td>
 row-reverse</td><td>Display items as the opposite axis of <code>`row`</code></td><td></td></tr><tr><td>column<
 /td><td>Display items vertically (top to bottom)</td><td></td></tr><tr><td>column-reverse</td><td>Items <
 code>Display as the opposite axis of `column`</code></td><td></td></tr></tbody></table>

 

```undefined
flex-direction: 주축;

```

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-direction.jpg)

The concepts of main-axis and cross-axis mentioned above are as follows:
The value `row` displays the Items horizontal axis, so the main axis is horizontal and the cross axis is vertical.
Conversely, the value `column` displays the Items as a vertical axis, so the main axis is vertical and the cross axis is horizontal.
That is, the main axis and the cross axis depend on the direction (horizontal, vertical).

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-direction-main-axis.jpg)

There are also concepts called flex-start and flex-end.
This refers to the starting and ending point of the main axis or cross axis.
The starting and ending points also vary depending on the direction.

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-direction-main-start.jpg)

We use `flex-start` and `flex-end` as the values of the attributes to be mentioned later, which means the starting and ending points that fit the direction.

Sets the multiline bundle (line break) of the Items.

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 nowrap</td><td>Do not wrap all Itmes on multiple lines (single line)</td><td><code>`nowrap`</code></td></tr><tr><
td>wrap</td><td>Wrap items into multiple lines</td><td></td></tr><tr><td>wrap-reverse</td><td>Items <code
 >Wrap in multiple lines in reverse direction of `wrap`</code></td><td></td></tr></tbody></table>

 

```undefined
flex-wrap: several lines;

```

By default, Items are displayed only in one line and are not line-switched.
This ignores the specified size (`width` or `height` depending on the main axis) and varies only within a single line.
You must use `wrap` as the value to wrap the Items.

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-wrap.jpg)

### justify-content

Sets how main-axis is sorted.

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 flex-start</td><td>Arrange Items by flex-start</td><td><code>`flex-start`</code></td></tr><tr>
 <td>flex-end</td><td>Arrange Items by flex-end</td><td></td></tr><tr><td>center</td><
td>Center Items</td><td></td></tr><tr><td>space-between</td><td>The start Item is aligned to the start point, and the last Item is aligned to the end point.
 The rest of the Items are evenly arranged between</td><td></td></tr><tr><td>space-around</td><td>Align Items with even margins</td>
td><td></td></tr></tbody></table>

 

```undefined
"justify-content: how to sort;

```

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-justify-content.jpg)

### align-content

Sets how cross-axis aligns.
Note that the `flex-wrap` property allows items to be used only if they have multiple lines (more than two lines) and margins.

> Use the 'align-items' property if the Items are reduced by one.

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 stretch</td><td>Stretch Items to fill the container's cross axis</td><td><code>`stretch`</code></td></tr><tr><td>flex-
 start</td><td>Arrange Items by flex-start</td><td></td></tr><tr><td>flex-end</td><td>Items
 Align by flex-end</td><td></td></tr><tr><td>center</td><td>Center items</td><td><
 /td></tr><tr><td>space-between</td><td>The starting item is aligned to the start point, the last item is aligned to the end point, and the remaining items are evenly aligned between</td><td
 ></td></tr><tr><td>space-around</td><td>Align items including even margins</td><td></td></tr><
 /tbody></table>

 

```undefined
'align-content: how to sort;

```

The value `stretch` is automatically increased to populate the cross axis if the width (attribute `width` or `height`) corresponds to the cross axis is the value `auto` (default).

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-align-content.jpg)

### align-items

Sets how items are sorted on the cross-axis.
Use a lot when Items is reduced by one.

Note that if the Items are multiple lines (more than two lines) through `flex-wrap`, the `align-content` property takes precedence.
Therefore, to use `align-items`, the `align-content` property must be set to the default value (`stretch`).

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 stretch</td><td>Stretch Items to fill the container's cross axis</td><td><code>`stretch`</code></td></tr><tr><td>flex-
 start</td><td>Arrange Items by flex-start of each line</td><td></td></tr><tr><td>flex-end</td><
td>Align Items by flex-end</td><td></td></tr><tr><td>center</td><td>Center Items</td>
td><td></td></tr><tr><td>baseline</td><td>Sort Items to character baseline</td><td></td></tr></tbody></table>

 

```undefined
'align-items: how to sort;

```

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-align-items.jpg)

## Flex Items

The attributes for Flex Items are as follows:

<table><thead><tr><th>property</th><th>meaning</th></tr></thead><tbody><tr><td>order</td><td>
 Set the order of the Flex Item</td></tr><tr><td>flex</td><td><code>`flex-grow`</code>, <code>`flex-shrink`<
 /code>, <code>Abbreviated property of `flex-basis`</code></td></tr><tr><td>flex-grow</td><td>Flex item increase width ratio
 Set</td></tr><tr><td>flex-shrink</td><td>Set the reduction width ratio of the Flex Item</td></tr><tr><td>flex-basis
 </td><td>Set default width (before space distribution) of Flex Item</td></tr><tr><td>align-self</td><td>in cross-axis
 Set the item sorting method</td></tr></tbody></table>

 

### order

Sets the order of the items.
Specify a number in the Item and the larger the number, the more the order is pushed.
Negative numbers are allowed.

> This is useful because you can change the order regardless of the HTML structure.

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 Number</td><td>Set the order of items</td><td><code>`0`</code></td></tr></tbody></table>

 

```undefined
"order: order;

```

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-order.jpg)

### flex

Short property that sets the width (increase, decrease, default) of the item.

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 flex-grow</td><td>Set the percentage of the item's increase width</td><td><code>`0`</code></td></tr><tr><td>flex-
 shrink</td><td>Set item's reduction width ratio</td><td><code>`1`</code></td></tr><tr><td>flex-basis<
 /td><td>Set default width (before space allocation) of items</td><td><code>`auto`</code></td></tr></tbody></table>

 

```undefined
'flex: increase width decrease width default width;

```

```css
.item {
flex: 11 20px; /* increase width reduction width default width */
flex: 11; /* increase width decrease width */
flex: 120px; /* increase width default width (flex-basis applies using units) */
}

```

Individual properties other than `flex-grow` can be omitted.
If you write `flex: 1;`, it is the same as `flex-grow: 1;`.
So we skipped the rest of the properties, so the default values would be `flex-shrink: 1;` and `flex-basis: auto;`, right?
In other words, `flex: 1;` or `flex: 11;` is the same as `flex: 11 auto;`, but it is not.

The default value of `flex-basis` is `auto`, but if the value is omitted from the short property `flex`, `0` is applied.
In other words, `flex: 1;` or `flex: 11;` becomes `flex: 11;`.
If you don`t remember this part, you might get the wrong result, so be careful!

Sets the ratio of the increase width of the item.
If the number is large, it has more widths.
If the item is not variable width or the value is `0`, it will not work.

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 Number</td><td>Set the percentage of the item's increasing width</td><td><code>`0`</code></td></tr></tbody></table>

 

```undefined
flex-grow: width of increase;

```

You can have the width of the total increase width of all items (`flex-grow`) by the ratio of the increase width of each item.
For example, if there are three items and the width of the increase is `1`, `2`, and `1`, respectively,
The first item is 25% (1/4) of the total width.
The second item is 50% (2/4) of the total width.
The third item will have 25% (1/4) of the total width.

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-grow.jpg)

<iframe height="500" scrolling="no" title="flex-grow" src="//codepen.io/heropark/embed/zMLbPw/?height=265&amp;theme-id=0&amp;default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="" style="width:100%">See the Pen <a href="https://codepen.io/heropark/pen/zMLbPw/" target="_blank" rel="noopener">flex-grow</a> by park young woong (<a href="https://codepen.io/heropark" target="_blank" rel="noopener">@heropark</a>) on <a href="https://codepen.io" target="_blank" rel="noopener">CodePen</a>.<br></iframe>

Sets the ratio of width for which the Item decreases.
Larger numbers reduce more widths.
If the item is not variable width or the value is `0`, it will not work.

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 Number</td><td>Set item's reduction width percentage</td><td><code>`1`</code></td></tr></tbody></table>

 

```undefined
'flex-shrink: reduced width;

```

The reduction width (`flex-shrink`) is difficult to calculate because it is affected by the width of the element.
The width of the affected element means that the width is designated as `width`, `height`, and `flex-basis`.
If the width of the Container decreases and affects the width of the Items, the width of the Item decreases to the ratio of the width of the reduced distance from the point where it begins to affect.

For example, when the width of the Container decreases and the actual distance reduced is `90px` from the point where it begins to affect the width of the Item.
If two items have the same width of elements and the specified width of reduction is `2` and `1`, respectively,
Decreased width is 2:1 ratio.
The first item is reduced in width by `60px`, which is 2/3 of `90px`.
The second item is reduced in width by `30px`, 1/3 of `90px`.

Another example is when the width of the Container is reduced and the actual distance reduced is `90px` from the point where the width of the Item is reduced.
There are two items with different widths of elements, and the width of elements is `200` and `100`, respectively.
If the specified width of reduction is `2` and `1`, respectively,
The ratio of `200 x 2 = 400` and `100 x 1 = 100` is 4:1.
The first item`s width is reduced by `72px`, which is 4/5 of `90px`.
The second item is reduced in width by `18px`, which is 1/5 of `90px`.

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-shrink.jpg)

<iframe height="500" scrolling="no" title="flex-shrink" src="//codepen.io/heropark/embed/oeWLVm/?height=265&amp;theme-id=0&amp;default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="" style="width:100%">See the Pen <a href="https://codepen.io/heropark/pen/oeWLVm/" target="_blank" rel="noopener">flex-shrink</a> by park young woong (<a href="https://codepen.io/heropark" target="_blank" rel="noopener">@heropark</a>) on <a href="https://codepen.io" target="_blank" rel="noopener">CodePen</a>.<br></iframe>

I think the utilization is a little lower because the calculation is difficult.
Let`s just understand the principles and move on.

Sets the default width (before space allocation) of the item.
If the value is `auto`, the width of the item can be set with properties such as `width`, `height`.
However, if you are given a unit value, you cannot set it.

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 auto</td><td>Same width as variable item</td><td><code>`auto`</code></td></tr><tr><td>unit</td><
td>Specify in units such as px, em, cm, etc.</td><td></td></tr></tbody></table>

 

```undefined
flex-basis: basic width;

```

If `flex-basis` is omitted within a short-term property, as explained in `flex` property, the value becomes `0`.

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-basis.jpg)

### align-self

Sets how individual items are sorted on the cross-axis.

`align-items` sets the sort method for all items in the Container.
You can use `align-self` if you want to change the sorting method for only some items as needed.
This property takes precedence over the `align-items` property.

<table><thead><tr><th>value</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 auto</td><td>Inherits the <code>`align-items`</code> property of the container</td><td><code>`auto`</code></td></tr
 ><tr><td>stretch</td><td>Stretch Item to fill the cross axis of container</td><td></td></tr><tr><td>flex-start</td><td>Arrange Items by flex-start</td><td></td></tr><tr><td>flex-end</td><td>Item
 Align to the flex-end of each line</td><td></td></tr><tr><td>center</td><td>center the item</td><
td></td></tr><tr><td>baseline</td><td>Align item to character baseline</td><td></td></tr></tbody><
 /table>
 

```undefined
'align-self: how to sort;

```

![image](https://heropy.blog/images/screenshot/css-flexible-box/flex-align-self.jpg)

# References

https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Flexible_Box_Layout/Flexbox%EC%9D%98_%EA%B8%B0%EB%B3%B8_%EA%B0%9C%EB%85%90