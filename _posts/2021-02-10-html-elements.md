---
layout: post
title: "HTML Elements at a Glance (Elements)"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/html5.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/html5.png)

# a word of mouth

This post is the result of trying to address as much objective, key, and fragmented information as possible about HTML elements (tags) and properties in one place.
It does not contain all the content of HTML and contains some of the subject matter.
Most accurately, there is a W3C document, but it is not easy to understand because it is too descriptive.
Therefore, in most cases, checking the latest version of MDN documents created with collective intelligence is the best way to achieve both objectivity and understanding.
If this post is different from the MDN document, of course, you can trust the MDN document more.
(Of course, the MDN document has only a small portion of the incorrect content)

And rather than reading this HTML post carefully, Web developers are recommended to quickly understand the overall content through speed reading and to find and use it as needed.
Of course, if you have time and environment to read thoroughly, it would be better to accumulate information in your head gradually.
However, I think HTML perusal is less efficient because there are a few of the top elements that are overwhelmingly active.
Especially HTML learning, CSS or JS learning is another position.
It`s a subjective idea that can help you learn, so just keep that in mind.

I respect and support everyone who always studies and works hard.
Thank you.

# Changes

## July 2019

I deleted some of the contents to deliver a summary of only the more important contents.
(If you`re watching this with an online lecture, skip if you don`t see anything in the current post.)

# a general outline

- Create based on HTML5.
- Must be available in all browsers.(Indicate separately that IE is not supported)
- Excludes elements or attributes that are no longer used.
- It focuses on Semantic content.
- Omit the information as it presupposes that it is not used in a displaying sense (how it appears on the screen).
- Empty tags (Empty Tags) are marked with a `/` as shown in `<TAG />`.
- The required attributes that must be used for this element display `(required)` in the description (any other optional attributes).
- Emphasize/address elements with high frequency, importance, and difficulty of use by personal experience.
- Personal experience omits/simplifies elements of low frequency, importance, and difficulty of use.

# Key Scope

## <html>

Set the scope of HTML documents.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 lang</td><td>The language of the document (<a href="https://en.wikipedia.org/wiki/ISO_639-1_%EC%BD%94%EB%93%9C_%EB%AA%A9
 %EB%A1%9D" target="_blank" rel="noopener">ISO 639-1</a>)</td><td><code>`ko`</code>, <code>`en
 `</code>…
 </td></tr></tbody></table>

 

MDN / W3Schools

## <head>

Set information for HTML documents.

MDN / W3Schools

## <body>

Set the structure of an HTML document.

```css
body { display: block; }

```

MDN / W3Schools

# Metadata

## <title>

Set the title of the document that is shown in the title bar or in the Pages tab of the browser.

MDN / W3Schools

## <base />

Set the reference URL for all relative URLs contained in HTML documents.

- Only one `<base />` element can be included in a document.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th></tr></thead><
 tbody><tr><td>href</td><td>base URL</td><td>URL</td><td></td></tr><tr><td>target</
td><td>Default for elements that use the target attribute like A elements</td><td><code>`_self`</code>, <code>`_blank`</code></td><td
 ><code>`_self`</code></td></tr></tbody></table>

 

MDN / W3Schools

## <link />

Specify the association of external resources and their relationship to the current document.
(Import HTML, CSS, ICON, etc.)

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 rel</td><td>(required) Relationship between current document and external resource (<a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types" target="
 _blank" rel="noopener">Link Types</a>)</td><td><code>`stylesheet`</code>, <code>`icon`</code>…
 </td><td></td></tr><tr><td>href</td><td>URL of external resource</td><td>URL</td><td></
td></tr><tr><td>type</td><td><a href="https://developer.mozilla.org/en/docs/Web/HTTP/Basics_of_HTTP/MIME_types"
 target="_blank" rel="noopener">MIME type</a></td><td><code>`text/css`</code>, <code>`image/x-icon`</code
 >…
 </td><td></td></tr></tbody></table>

 

MDN / W3Schools

## <meta />

Set to represent metadata that cannot be represented by other metadata elements (such as ``link />`` and ``style``).
(Information provided to search engine or browser)

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 charset</td><td><a href="https://www.iana.org/assignments/character-sets/character-sets.xhtml" target="_blank" rel="noopener">character encoding method<
 /a></td><td><code>`UTF-8`</code>, <code>`EUC-KR`</code>…
 </td></tr><tr><td>name</td><td>The name of the metadata (<a href="https://developer.mozilla.org/en/docs/Web/HTML/
 Element/meta#attr-name" target="_blank" rel="noopener">Type of information</a>)</td><td><code>`author`</code>, <code>`description
 `</code>…
 </td></tr><tr><td>http-equiv</td><td><a href="https://developer.mozilla.org/en for changing the way the server/user agent works.
 /docs/Web/HTML/Element/meta#attr-http-equiv" target="_blank" rel="noopener">instruction</a> (providing HTTP response header)</td><td><code>`
 refresh`</code>, <code>`X-UA-Compatible`</code>…
 </td></tr><tr><td>content</td><td><code>`name`</code>, <code>value of `http-equiv`</code></td
 ><td></td></tr></tbody></table>

 

```xml
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no, maximum-scale=1, minimum-scale=1" />
<meta http-equiv="X-UA-Compatible" content="IE=edge" />

```

MDN / W3Schools

## <style>

Set style information (CSS).

<table><thead><tr><th>property</th><th>meaning</th><th>default</th></tr></thead><tbody><tr><td>
 type</td><td><a href="https://developer.mozilla.org/en/docs/Web/HTTP/Basics_of_HTTP/MIME_types" target="_blank" rel="noopener">MIME type</
 a></td><td><code>`text/css`</code></td></tr></tbody></table>

 

MDN / W3Schools

# Content Identification

## <h1>, <h2>, <h3>, <h4>, <h5>, <h6>

Structures the information hierarchy of documents.
(Heading, setting the title of a document or separated area, table of contents of a document)

- The lower the number, the higher the level (important).

```css
h1, h2, h3, h4, h5, h6 { display: block; }

```

MDN / W3Schools

## <header>

Set the header of the document.
(usually including logos, titles, searches, etc.)

```css
header { display: block; }

```

MDN / W3Schools

## <footer>

Set the document`s putter.
(usually including authors, copyrights, relevant documents, etc.)

```css
footer { display: block; }

```

MDN / W3Schools

## <main>

Set the main content of the document.

- IE Unsupported
- Only one `main` element can be included in a document.

```css
main { display: block; }

```

MDN / W3Schools

## <article>

Set up independently separated or reusable areas.
(magazine/newspaper articles, blogs, etc.)

- Generally, `<h1>`~Identify with `<h6>.
- Write the date and time of creation as `datetime` attribute of `<time>.

```css
article { display: block; }

```

MDN / W3Schools

## <section>

Set the general area of the document.

- Generally, `<h1>`~Identify with `<h6>.

```css
section { display: block; }

```

MDN / W3Schools

## <aside>

Set the separate content of the document.
(Usually set the sidebar, such as advertising or other links)

```css
aside { display: block; }

```

MDN / W3Schools

## <nav>

Set up an area that provides a different page link.
(Navigation, Common Menu (Home, About, Contact), Table of Contents, Index, etc.)

```css
nav { display: block; }

```

MDN / W3Schools

## <address>

Include and use contact information in `body`, `article`, and `footer`.

```css
address { display: block; }

```

MDN / W3Schools

## <div>

Set up content areas that represent essentially nothing.
(Division, used for decoration purposes)

```css
div { display: block; }

```

MDN / W3Schools

# Text Content

## <ol>, <ul>, <li>

Set an ordered list (`<ol>`) or an unordered list (`<ul>`) of each item (`<li`).
(Ordered List, Unordered List, List Item, Define a list that requires an order (`<ol`) or does not require an order (`<ul`))

- <ol> and <ul> are children, and only <li> can be included.
- "`li` shall not be used alone, but shall be included as a child in `ol` or `ul`.
- The order of items in the sorted list (`<ol`) may mean importance.

```css
ol, ul { display: block; }
li { display: list-item; }

```

OL: MDN / W3Schools
UL: MDN / W3Schools
LI: MDN / W3Schools

### <ol>

Set the sorted list.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>feature</th></tr></thead><
 tbody><tr><td>start</td><td>The starting value of the number assigned to the item</td><td>Number</td><td></td></tr>
 <tr><td>type</td><td>Type of number assigned to item</td><td><code>`a`</code>, <code>`A`</code>,
 <code>`i`</code>, <code>`I`</code>, <code>`1`</code></td><td></td></tr></tbody
 ></table>

 

### <li>

Set the item.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>feature</th></tr></thead><
 tbody><tr><td>value</td><td>Set the order of items</td><td>Number</td><td>The order of the following items will be reordered</td>
 </tr></tbody></table>

 

## <dl>, <dt>, <dd>

Set the term (<dt>`) and definition (<d>```) region of pairs (<dl`).
(Description List, Definition Details, Definition Term)

- `<dl>` should contain only `<d>` and `<dt>`.
- Useful when displaying key/value shapes.

```xml
<dl>
<dt>Coffee</dt>
<dd>Coffee is a brewed drink prepared from roasted coffee beans, the seeds of berries from certain Coffea species.</dd>
<dt>Milk</dt>
<dd>Milk is a nutrient-rich, white liquid food produced by the mammary glands of mammals.</dd>
</dl>

```

```css
dl, dt, dd { display: block; }

```

DL: MDN / W3Schools
DT: MDN / W3Schools
DD: MDN / W3Schools

## <p>

Set one paragraph.
(Paragraph)

- Generally, information and communication assistants provide shortcuts that can be passed to the next paragraph (`<p>`).

```css
p { display: block; }

```

MDN / W3Schools

## <hr />

Set up for paragraph separation (by topic).
(Horizontal Rule)

- In most cases, it is marked as a horizontal line (`border`) but should only be used from a semantic perspective.

```css
hr { display: block; }

```

MDN / W3Schools

## <pre>

Set preformatted text.
(Preformatted Text)

- Text can be displayed with spaces and line breaks.
- Displays as Monospace font family by default.

```css
pre { display: block; }

```

MDN / W3Schools

## <blockquote>

Set general quotes.
(Block Quotation)

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 cite</td><td>URL of quoted information</td><td>URL</td></tr></tbody></table>

 

```css
blockquote { display: block; }

```

MDN / W3Schools

# Inline Text

## <a>

Set up hyperlinks that can be linked to different URLs such as different pages, the same page location (`#`, hashtag), files, email addresses, phone numbers, etc.
(Anchor, Export to External)

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th><th>feature</th></tr></thead><tbody><tr><td>download</td><td>means that this element will be used to download resources</td><td>Boolean</td ><td></td></tr><tr><td>href</td><td>Link URL</td><td>URL</td><td></td><td> Can be omitted</td></tr><tr><td>rel</td><td>The relationship between the current document and the link URL (<a href="https://developer.mozilla.org/en-US) /docs/Web/HTML/Link_types" target="_blank" rel="noopener">Link Types</a>)</td><td><code>`license`</code>, <code>`prev `</code>, <code>`next`</code>… </td><td></td><td></td></tr><tr><td>target</td><td>Display (browser tab) location of link URL</td><td><code>`_self`</code>, <code>`_blank`</code></td><td><code>`_self`</code></td><td></td ></tr><tr><td>type</td><td><a href="https://developer.mozilla.org/en/docs/Web/HTTP/Basics_of_HTTP/MIME_types" target in the link URL ="_blank" rel="noopener">MIME type</a></td><td><code>`text/html`</code>… </td><td></td><td></td></tr></tbody></table>

 

```css
a { display: inline; }

```

MDN / W3Schools

## <abbr>

Specify abbreviations.
(Abbrevision, usually using the `title` attribute to provide full letters or descriptions)

```xml
Using <abbr title="HyperText Markup Language">HTML</abbr> is fun and easy!

```

```css
abbr { display: inline; }

```

MDN / W3Schools

## <b>

Sets the range of letters with different styles.
(Bring Attention)

- Does not have any special meaning.
- Use to help with read flow.
- Use as a last resort if other tags are not suitable.
- By default, the letters are thick (Bold).

```css
b { display: inline; }

```

MDN / W3Schools

## <mark>

Used when highlighting to attract your attention.
(Mark Text, the same meaning as using a highlighter to mark a point of interest)

- By default, the character background is yellow, as if using a highlighter.

```css
mark { display: inline; }

```

MDN / W3Schools

## <em>

Show simple emphasis on meaning.
(Emphasis)

- Overlayable.
- The more nested, the stronger the emphasis.
- Pronounced verbally in information and communication aids.
- Marked as italic type by default.

```css
em { display: inline; }

```

MDN / W3Schools

## <strong>

Used to indicate the importance of meaning.
(Strong Importance)

- By default, the letters are thick (Bold).

```css
strong { display: inline; }

```

MDN / W3Schools

## <i>

It is used if it is not suitable for expression such as `em`, `strong`, `mark`, `cite`, and `dfn`.
(Use to distinguish between ordinary letters (such as icons or special symbols)

- Marked as italic type by default.

```css
i { display: inline; }

```

MDN / W3Schools

## <dfn>

Use to define terms.
(Definition)

```css
dfn { display: inline; }

```

MDN / W3Schools

## <cite>

Set a reference to the creation.
(titles such as books, papers, movies, TV shows, songs, games, etc.)

- Marked as italic type by default.

```xml
<cite>The Scream</cite> by Edward Munch. Painted in 1893.

```

```css
cite { display: inline; }

```

MDN / W3Schools

## <q>

Set short quotes.
(Inline Quotation)

- Use `<blockquote>` to set long quotes.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 cite</td><td>URL of quoted information</td><td>URL</td></tr></tbody></table>

 

```css
q { display: inline; }

```

MDN / W3Schools

## <u>

Set the underlined letter.
(Underline)

- Used as a purely decorative element.
- Be careful not to use it in a position that may be confused with `<a`.
- Use is not recommended if `<spanstyle="text-decoration:underline;">` is available.

```css
u { display: inline; }

```

MDN / W3Schools

## <code>

Set the computer code range.
(Inline Code)

`<code>document.getElementById(`id-value`)</code> is a piece of computer code.`

- By default, it appears as a Monospaced font family.

```css
code { display: inline; }

```

MDN / W3Schools

## <kbd>

Set the text range that represents user input on the text input device (keyboard).
(Keyboard Input)

```xml
<p><kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>K</kbd></p>, <kbd>ESC</kbd>

```

```css
kbd { display: inline; }

```

MDN / W3Schools

## <sup>, <sub>

Set up `<sup>` and `<sub>` above and below.
(Superscripted text, Subscript text)

```xml
X<sup>4</sup> + Y<sup>2</sup>, H<sub>2</sub>O

```

```css
sup, sub { display: inline; }

```

SUP: MDN / W3Schools
SUB: MDN / W3Schools

## <time>

For the purpose of indicating the date or time.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 datetime</td><td><a href="https://www.w3.org/TR/html51/infrastructure.html#dates-and-times" target="_blank" rel="noopener">valid date
 Character</a></td><td>Date</td></tr></tbody></table>

 

- IE Unsupported

```xml
<p>The Cure will be celebrating their 40th anniversary on <time datetime="2018-07-07">July 7</time> in London's Hyde Park.</p>

```

```css
time { display: inline; }

```

MDN / W3Schools

## <span>

Set up content areas that represent essentially nothing.

```css
span { display: inline; }

```

MDN / W3Schools

## <br />

Set line break.

```css
br { display: inline; }

```

MDN / W3Schools

# crystal

## <del>

Specifies the range of deleted (changed) text.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 cite</td><td>The URI of the resource describing the change</td><td>URI</td></tr><tr><td>datetime</td><td><a where the change occurred
 href="https://www.w3.org/TR/html51/infrastructure.html#dates-and-times" target="_blank" rel="noopener">valid date characters</a></td>
 <td>Date</td></tr></tbody></table>

 

```css
del { display: inline; }

```

MDN / W3Schools

## <ins>

Specifies the range of newly added (changed) text.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 cite</td><td>The URI of the resource describing the change</td><td>URI</td></tr><tr><td>datetime</td><td><a where the change occurred
 href="https://www.w3.org/TR/html51/infrastructure.html#dates-and-times" target="_blank" rel="noopener">valid date characters</a></td>
 <td>Date</td></tr></tbody></table>

 

```css
ins { display: inline; }

```

MDN / W3Schools

# Multimedia

## <img />

Insert image.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 src</td><td>(required) Image URL</td><td>URL</td><td></td></tr><tr><td>alt</td><td>
 (Required) Alternative text of image</td><td></td></tr><tr><td>width</td><td>width of image</td><td></td
 ></tr><tr><td>height</td><td>height of the image</td><td></td></tr><tr><td>srcset</td><
td>Define a list of image URLs and original sizes to be presented to the browser</td><td><code>`w`</code>, <code>`x`</code></td></tr
 ><tr><td>sizes</td><td>Defines a list of media conditions and image optimization sizes for those conditions</td><td></td></tr></tbody></
 table>
 

```xml
<!-- srcset, sizes -->
<!-- Browser selects and uses the best image for different display resolutions -->
<img srcset="./small.jpg 320w,
./medium.jpg 640w,
./large.jpg 1024w"
sizes="(max-width: 480px) 300px,
(max-width: 800px) 600px,
900px"
src="./small.jpg"
alt="The image" />
<img srcset="./image.jpg,
./image-1.5x.jpg 1.5x,
./image-2x.jpg 2x"
src="./image.jpg"
alt="The image" />

```

```css
img { display: inline; }

```

MDN / W3Schools

- srcset and sizes properties of HTML IMG
- Responsive images for `srcset` and `sizes`

## <audio>

Insert sound content (MP3).

- If `autoplay` is specified, `preload` is ignored.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th></tr></thead><
 tbody><tr><td>autoplay</td><td>Play as soon as it's ready</td><td>Boolean</td><td></td></tr><tr><
td>controls</td><td>Show control menu</td><td>Boolean</td><td></td></tr><tr><td>loop</td
 ><td>Play from the beginning again when playback ends</td><td>Boolean</td><td></td></tr><tr><td>preload</td><td
 >Specify whether to load the file when the page loads (provided a hint)</td><td><code>`none`</code>: do not load,<br><code>`metadata`</code
 >: Load metadata only,<br><code>`auto`</code>: Load entire file</td><td><code>`metadata`</code></td></tr><
 tr><td>src</td><td>Content URL</td><td>URL</td><td></td></tr><tr><td>muted</td><
td>Mute or not</td><td>Boolean</td><td></td></tr></tbody></table>

 

```css
audio { display: inline; }

```

MDN / W3Schools

## <video>

Insert video content (MP4).

- If `autoplay` is specified, `preload` is ignored.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th></tr></thead><tbody><tr><td>autoplay</td><td>Play as soon as it's ready</td><td>Boolean</td><td></td></tr><tr><td>controls</td><td>Show control menu</td><td>Boolean</td><td></td></tr><tr><td>loop</td ><td>Play from the beginning again when playback ends</td><td>Boolean</td><td></td></tr><tr><td>muted</td><td >Mute or not</td><td>Boolean</td><td></td></tr><tr><td>poster</td><td>Video thumbnail image URL</td ><td>URL</td><td></td></tr><tr><td>preload</td><td>Specify whether to load the file when the page loads (provided a hint)</td><td><code>`none`</code>: do not load,<br><code>`metadata`</code>: load metadata only,<br><code>`auto`</code>: Load full file</td><td><code>`metadata`</code></td></tr><tr><td>src</td><td>Content URL</td><td>URL</td><td></td></tr><tr><td>width</td><td>Video width</td><td></td><td></td></tr><tr><td>height</td><td>Video vertical width</td><td></td><td></td></tr></tbody></table>

 

```css
video { display: inline; }

```

MDN / W3Schools

## <figure>, <figcaption>

<Figure> sets the area of an image, illustration, or chart.
`figcaptions` are included in `figuration` to display descriptions such as images or illustrations.

```xml
<figure>
<img src="milk.jpg" alt="A milk">
<figcaption>Milk is a nutrient-rich, white liquid food produced by the mammary glands of mammals.</figcaption>
</figure>

```

```css
figure { display: block; }
figcation { display: inline; }

```

FIGURE: MDN / W3Schools
FIGCAPTION: MDN / W3Schools

# Built-in content

## <iframe>

Insert another HTML page into the current page.
(displays nested browser context (frame)

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th></tr></thead><
 tbody><tr><td>name</td><td>name of frame</td><td></td><td></td></tr><tr><td>src</
td><td>URL of document to include</td><td>URL</td><td></td></tr><tr><td>width</td><td> width of frame
 Width</td><td></td><td></td></tr><tr><td>height</td><td>Height of frame</td><td></
td><td></td></tr><tr><td>allowfullscreen</td><td>Enable full screen mode</td><td>Boolean</td><td>
 </td></tr><tr><td>frameborder</td><td>Use frame borders</td><td><code>`0`</code>, <code>`1`
 </code></td><td><code>`1`</code></td></tr><tr><td>sandbox</td><td>Insert read-only for security
 </td><td>Boolean or<br><code>`allow-form`</code>: form can be submitted,<br><code>`allow-scripts`</code>: script execution
 Yes ,<br><code>`allow-same-origin`</code>: resources from the same origin (domain) are available</td><td></td></tr></tbody></
 table>
 

```xml
<iframe width="1280" height="720" src="https://www.youtube.com/embed/Q9yn1DpZkHQ" frameborder="0" allowfullscreen></iframe>

```

```css
iframe { display: inline; }

```

MDN / W3Schools

## <canvas>

Landering graphics or animations using the Canvas API or WebGL API.

<table><thead><tr><th>property</th><th>meaning</th></tr></thead><tbody><tr><td>width</td><td>
 Width of canvas</td></tr><tr><td>height</td><td>Vertical width of canvas</td></tr></tbody></table>

 

```css
canvas { display: inline; }

```

MDN / W3Schools

# Script

## <script>

Include the script code in the document or refer to it (external script).

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>feature</th></tr></thead><
 tbody><tr><td>async</td><td>whether the script executes asynchronously</td><td>Boolean</td><td><code>`src`<
 /code> attribute required</td></tr><tr><td>defer</td><td>whether it works after parsing (parsing) the document</td><td>Boolean</td
 ><td><code>`src`</code> attribute required</td></tr><tr><td>src</td><td>External script URL to refer to</td><td>
 URL</td><td>Included script code is ignored</td></tr><tr><td>type</td><td><a href="https://developer.mozilla.
 org/en/docs/Web/HTTP/Basics_of_HTTP/MIME_types" target="_blank" rel="noopener">MIME type</a></td><td><code>`text/javascript`</code>
 (Default)</td><td></td></tr></tbody></table>

 

```css
script { display: none; }

```

MDN / W3Schools

## <noscript>

Defines HTML to insert if script is not supported.

```xml
<noscript>
<p>Your browser does not support JavaScript!</p>
</noscript>

```

```css
noscript { display: inline; }

```

MDN / W3Schools

# Table Content

```xml
<table>
<caption>Fruits</caption>
<colgroup>
<col span="2" style="background-color: yellowgreen;">
<col style="background-color: tomato;">
</colgroup>
<thead>
<tr>
<th>ID</th>
<th>Name</th>
<th>Price</th>
</tr>
</thead>
<tbody>
<tr>
<td>F123A</td>
<td>Apple</td>
<td>$22</td>
</tr>
<tr>
<td>F098B</td>
<td>Banana</td>
<td>$19</td>
</tr>
</tbody>
</table>


```

## <table>, <tr>, <th>, <td>

Create rows (line / <tr>) and columns (kan, cell) / <th>, and <td> of data table (<table>.
(Table Row, Table Header, Table Data)

```css
table { display: table; }
tr { display: table-row; }
th, td { display: table-cell; }

```

TABLE: MDN / W3Schools
TR: MDN / W3Schools
TH: MDN / W3Schools
TD: MDN / W3Schools

### <th>

Specify `headlet space`

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th></tr></thead><
 tbody><tr><td>abbr</td><td>Brief description of columns</td><td></td><td></td></tr><tr><td>headers<
 /td><td>One or more other related header fields <code>`id`</code> attribute values</td><td></td><td></td></tr><tr><
td>colspan</td><td>Number of columns to expand (merge)</td><td></td><td><code>`1`</code></td></tr><
 tr><td>rowspan</td><td>Number of rows to expand (merge)</td><td></td><td><code>`1`</code></td><
 /tr><tr><td>scope</td><td>specify whose'header space' you are</td><td><code>`col`</code>: own column<br
 ><code>`colgroup`</code>: all columns<br><code>`row`</code>: own rows<br><code>`rowgroup`</code>: all rows<br>
 <code>`auto`</code></td><td><code>`auto`</code></td></tr></tbody></table>

 

### <td>

Specify `General spaces`

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th></tr></thead><
 tbody><tr><td>headers</td><td>one or more other related header fields <code>`id`</code> attribute values</td><td></td><td></
td></tr><tr><td>colspan</td><td>Number of columns to expand (merge)</td><td></td><td><code>`1`</code
 ></td></tr><tr><td>rowspan</td><td>Number of rows to expand (merge)</td><td></td><td><code>`1
 `</code></td></tr></tbody></table>

 

## <caption>

Set the title of the table.

- Must be created immediately after the opening table tag.
- Only one <caption> per <table>.

```css
caption { display: table-caption; }

```

MDN / W3Schools

## <colgroup>, <col />

The columns that commonly define the columns in the table (`<col>`) and their set (`<colgroup`).
(Column, Column Group)

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th></tr></thead><
 tbody><tr><td>span</td><td>Number of consecutive columns</td><td>Number</td><td><code>`1`</code></
td></tr></tbody></table>

 

```css
colgroup { display: table-column-group; }
col { display: table-column; }

```

COLGROUP: MDN / W3Schools
COL: MDN / W3Schools

## <thead>, <tbody>, <tfoot>

Specify the header of the table (`<head>, body (`<tbody>, and footer.

- By default, it does not affect the layout of the table.

```css
thead { display: table-header-group; }
tbody { display: table-row-group; }
tfoot { display: table-footer-group; }

```

THEAD: MDN / W3Schools
TBODY: MDN / W3Schools
TFOOT: MDN / W3Schools

# form

## <form>

Define a range of forms for submitting information to a Web server.

- Cannot include `form` as a child element with a different `form`.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th></tr></thead><tbody><tr><td>action</td><td>URL of the web page that will process the transmitted information</td><td>URL</td><td></td></tr><tr><td>autocomplete</td><td>whether to use autocomplete with values previously entered by the user</td><td><code>`on`</code>, <code>` off`</code></td><td><code>`on`</code></td></tr><tr><td>method</td><td>Send to server <a href="https://www.w3.org/Protocols/rfc2616/rfc2616.html" target="_blank" rel="noopener">HTTP</a> method</td><td><code>`GET `</code>, <code>`POST`</code></td><td><code>`GET`</code></td></tr><tr><td>name</td><td>unique form name</td><td></td><td></td></tr><tr><td>novalidate</td><td>when sent to server Specifies not to validate form data</td><td></td><td></td></tr><tr><td>target</td><td>Send to server and respond Specify how to receive</td><td><code>`_self`</code>, <code>`_blank`</code></td><td><code>`_self`</code></td></tr></tbody></table>

 

```css
form { display: block; }

```

MDN / W3Schools

## <input />

The data form to be entered by the user.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th><th>feature</th></tr></thead><tbody><tr><td>autocomplete</td><td>whether to use autocomplete with values previously entered by the user</td><td><code>` on`</code>, <code>`off`</code></td><td><code>`on`</code></td><td></td></tr><tr><td>autofocus</td><td>Autofocus when page loads</td><td>Boolean</td><td></td><td> Must be unique within the document Ham</td></tr><tr><td>checked</td><td>Indicate that the form is selected</td><td>Boolean</td><td></td> <td><code>`type`</code> only when the attribute value is <code>`radio`</code>, <code>`checkbox`</code></td></tr><tr ><td>disabled</td><td>Disable form</td><td>Boolean</td><td></td><td></td></tr><tr ><td>form</td><td><code><code>`id`</code> attribute value of `&lt;form&gt;`</code></td><td></td><td></td><td>Only if they are not descendants of the <code>`&lt;form&gt;`</code></td></tr><tr><td>list</td><td >Refer to <code>`&lt;datalist&gt;`</code>'s <code>`id`</code> attribute value</td><td></td><td></td><td> </td></tr><tr><td>max</td><td>Maximum value specified</td><td>Number</td><td></td><td ><code>`type`</code> Only when the property value is <code>`number`</code>,<br><code>`min` </code>Only values greater than the attribute are allowed</td></tr><tr><td>min</td><td>Minimum value specified</td><td>Number</td> <td></td><td><code>`type`</code> Only when the property value is <code>`number`</code>,<br><code>`max`</code> Allow only values less than the attribute</td></tr><tr><td>maxlength</td><td>Maximum number of characters that can be entered</td><td>Number</td><td> <code>`524288`</code></td><td><code>`type`</code> attribute value is <code>`text`</code>, <code>`email`</code >, <code>`password`</code>, <code>`tel`</code>, <code>`url`</code> only</td></tr><tr><td >multiple</td><td>whether more than one value can be entered</td><td>Boolean</td><td></td><td><code>`type`< Only when the /code> property value is <code>`email`</code>, <code>`file`</code>, <br><code>`email`</code> <code>` Separated by ,`</code></td></tr><tr><td>name</td><td>Name of form</td><td></td><td></td ><td></td></tr><tr><td>placeholder</td><td>Hint of value to be entered by user</td><td></td><td></td ><td><code>`type`</code> property value is <code>`text`</code>, <code>`search`</code>, <code>`tel`</code>, <code>`url`</code>, <code>`email`</code> only</td></tr><tr><td>readonly</td><td>Unmodifiable read Dedicated</td><td>Boolean</td><td ></td><td></td></tr><tr><td>step</td><td>Effective incremental interval</td><td>Number</td> <td><code>`1`</code></td><td><code>`type`</code> property value is <code>`number`</code>, <code>`range` </code> only</td></tr><tr><td>src</td><td>URL of the image</td><td>URL</td><td></td ><td><code>`type`</code> only when the attribute value is <code>`image`</code></td></tr><tr><td>alt</td><td>Alternate text for image</td><td></td><td></td><td><code>`type`</code> attribute value is <code>`image`</code> <td></tr><tr><td>type</td><td>Type of data to be input</td><td>Separately organized</td><td><code>` text`</code></td></tr><tr><td>value</td><td>initial value of form</td><td></td><td></td> <td></td></tr></tbody></table>

 

### Value of data type (Values)

A list of values that can be entered in the `type` property.

```xml
<input type="button" />
<input type="checkbox" />
<input type="file" />
<input type="text" />

```

<table><thead><tr><th>value</th><th>data type</th><th>characteristic</th></tr></thead><tbody><tr><td >button</td><td>normal button</td><td><code>`&lt;button&gt;`</code> use like</td></tr><tr><td>checkbox</td><td>Checkbox</td><td></td></tr><tr><td>color</td><td>color</td><td>IE not supported</td ></tr><tr><td>email</td><td>email</td><td></td></tr><tr><td>file</td><td>file </td><td></td></tr><tr><td>hidden</td><td>Form invisible but to be sent</td><td><code>`value`</code> Specify value as attribute</td></tr><tr><td>image</td><td>Submit image button</td><td><code>`&lt;img /&gt;`</ Use like code></td></tr><tr><td>number</td><td>number</td><td></td></tr><tr><td>password</td><td>secret</td><td>covered form</td></tr><tr><td>radio</td><td>radio button</td><td>like < code>`name`</code> Only one can be selected within the attribute group</td></tr><tr><td>range</td><td>Range control</td><td><code>` min`</code>, <code>`max`</code>, <code>`step`</code>, <code>`value`</code>(default) attribute use</td></ tr><tr><td>reset</td><td>reset</td><td>all forms in scope for that <code>`&lt;form&gt;`</code></td></tr> <tr><td>search</td><td>search</td><td></td></tr><tr><td>submit</td><td>submit button</td> <td> Per <code>`&lt;form&gt;`</code> Unique form within scope</td></tr><tr><td>tel</td><td>phone number</td><td> </td></tr><tr><td>text</td><td>plain text</td><td></td></tr><tr><td>url</td> <td>Absolute URL</td><td></td></tr></tbody></table>

 

```css
input { display: inline-block; }

```

MDN / W3Schools

## <label>

The title of the labelable element (Captions

- Refer to labeling elements as `for` attributes or include them as content.
- Labeling elements are `button`, `input`, `progress`, `select`, `textarea`

<table><thead><tr><th>property</th><th>meaning</th></tr></thead><tbody><tr><td>for</td><td>
 Value of the <code>`id`</code> attribute of the labelable element to be referenced</td></tr></tbody></table>

 

```xml
"<!-- See Labelable Elements -->
<input type="checkbox" id="user-agreement" />
="user-agreement">Do you agree?</label>

<!-- Include labelable elements -->
><label><input type="checkbox" />Do you agree?</label>

```

```css
label { display: inline; }

```

MDN / W3Schools

## <button>

Specify which buttons are selectable.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>feature</th></tr></thead><
 tbody><tr><td>autofocus</td><td>autofocus when page loads</td><td>Boolean</td><td>must be unique within document</td
 ></tr><tr><td>disabled</td><td>disable button</td><td>Boolean</td><td></td></tr><tr
 ><td>form</td><td><code><code>`id`</code> attribute value of `&lt;form&gt;`</code></td><td></td><
td>Only if it is not a descendant of the <code>`&lt;form&gt;`</code></td></tr><tr><td>name</td><td>
 Name of button</td><td></td><td></td></tr><tr><td>type</td><td>Type of button</td><td><
 code>`button`</code>, <code>`reset`</code>, <code>`submit`</code></td><td></td></tr></tbody>
 </table>

 

```css
button { display: inline-block; }

```

MDN / W3Schools

## <textarea>

Multiple lines of plain text form.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th><th>feature</th></tr></thead><tbody><tr><td>autocomplete</td><td>whether to use autocomplete with values previously entered by the user</td><td><code>` on`</code>, <code>`off`</code></td><td><code>`on`</code></td><td></td></tr><tr><td>autofocus</td><td>Autofocus when page loads</td><td>Boolean</td><td></td><td> Must be unique within the document Ham</td></tr><tr><td>disabled</td><td>Disable form</td><td>Boolean</td><td></td><td ></td></tr><tr><td>form</td><td><code>`&lt;form&gt;`</code>'s <code>`id`</code> attribute value</td><td></td><td></td><td>only if they are not descendants of the <code>`&lt;form&gt;`</code></td></tr><tr ><td>maxlength</td><td>Maximum number of characters that can be entered</td><td>Number</td><td>Infinite</td><td></td></tr ><tr><td>name</td><td>Name of form</td><td></td><td></td><td></td></tr><tr> <td>placeholder</td><td>Hint of user input</td><td></td><td></td><td></td></tr><tr> <td>readonly</td><td>Unmodifiable read-only</td><td>Boolean</td><td></td><td></td></tr><tr><td>rows</td><td>Number of lines in the form</td><td>Number</td><td><code>`2`</code></td><td></td></tr></tbod y></table>

 

```css
textarea { display: inline-block; }

```

MDN / W3Schools

## <fieldset>, <legend>

Group (`<fieldset`) forms for the same purpose to specify a title (`<legend`).

```xml
<form>
<fieldset>
<legend>Coffee Size</legend>
<label>
<input type="radio" name="size" value="tall" />
Tall
</label>
<label>
<input type="radio" name="size" value="grande" />
Grande
</label>
<label>
<input type="radio" name="size" value="venti" />
Venti
</label>
</fieldset>
</form>

```

```css
fieldset, legend { display: block; }

```

FIELDSET: MDN / W3Schools
LEGEND: MDN / W3Schools

### <fieldset>

Group forms for the same purpose.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 disabled</td><td>Disable all form elements in group</td><td>Boolean</td><td></td></tr><tr><td>form</
td><td>Value of the <code>`id`</code> attribute of one or more <code>`&lt;form&gt;`</code> the group will belong to</td><td></td></tr
 ><tr><td>name</td><td>name of group</td><td></td></tr></tbody></table>

 

## <select>, <datalist>, <optgroup>, <option>

Select menu (`<select>`) or `<datalist>` of option (`option`, `optgroup`) are provided.

```xml
<select>
<optgroup label="Coffee">
<option>Americano</option>
<option>Caffe Mocha</option>
<option label="Cappuccino" value="Cappuccino"></option>
</optgroup>
<optgroup label="Latte" disabled>
<option>Caffe Latte</option>
<option>Vanilla Latte</option>
</optgroup>
<optgroup label="Smoothie">
<option>Plain</option>
<option>Strawberry</option>
<option>Banana</option>
<option>Mango</option>
</optgroup>
</select>

```

```css
select { display: inline-block; }
datalist { display: none; }
optgroup, option { display: block; }

```

SELECT: MDN / W3Schools
DATALIST: MDN / W3Schools
OPTGROUP: MDN / W3Schools
OPTION: MDN / W3Schools

### <select>

The menu that selects the option.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>default</th></tr></thead><
 tbody><tr><td>autocomplete</td><td>whether to use autocomplete with values previously entered by the user</td><td><code>`on`</code>, <
 code>`off`</code></td><td><code>`on`</code></td><td></td></tr><tr><td>disabled</
td><td>Disable selection menu</td><td>Boolean</td><td></td></tr><tr><td>form</td><td>select
 One or more <code>`&lt;form&gt;`</code> <code>`id`</code> attribute values</td><td></td><td></td><
 /tr><tr><td>multiple</td><td>Multiple selection</td><td>Boolean</td><td></td></tr><tr><
td>name</td><td>Name of selection menu</td><td></td><td></td></tr><tr><td>size</td><td>
 Number of rows visible at one time</td><td>Number</td><td><code>`0`</code>(Same as <code>`1`</code>
 )</td></tr></tbody></table>

 

### <datalist>

Used to provide autocomplete functionality by specifying predefined options in `<input>.

- Bind the `list` property of `<input>.
- Specify defined options, including `<option>.

```xml
<input type="text" list="fruits">

<datalist id="fruits">
<option>Apple</option>
<option>Orange</option>
<option>Banana</option>
<option>Mango</option>
<option>Fineapple</option>
</datalist>

```

### <optgroup>

Group `<option>`.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 label</td><td>(required)Name of option group</td><td></td></tr><tr><td>disabled</td><td>Disable option group</td>
td><td>Boolean</td></tr></tbody></table>

 

### <option>

The option to be used in the selection menu (<select>`) or in the autocomplete (<datalist>.

- Available as an optional empty tag.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>property</th></tr></thead><
 tbody><tr><td>disabled</td><td>disable option</td><td>Boolean</td><td></td></tr><tr><td
 >label</td><td>Title of option to be displayed</td><td></td><td>Show embedded text if omitted</td></tr><tr><td>selected
 </td><td>Indicate that the option is selected</td><td>Boolean</td><td></td></tr><tr><td>value</td><
td>Value to be submitted in the form</td><td></td><td>If omitted, use embedded text as value</td></tr></tbody></table>

 

## <progress>

Shows the progress of the job completion.

<table><thead><tr><th>property</th><th>meaning</th><th>value</th><th>feature</th></tr></thead><
 tbody><tr><td>max</td><td>Total amount of operations</td><td>Number</td><td></td></tr><tr><td
 >value</td><td>The progress of the operation</td><td>Number</td><td><code>`max`</code> If you omit the attribute <code>`
 Must be a number between 0`</code>~<code>`1`</code></td></tr></tbody></table>

 

```xml
<progress value="70" max="100">70 %</progress>

```

```css
progress { display: inline-block; }

```

MDN / W3Schools

# Global Attributes

Properties common to all HTML elements.

## class

Specify an alias for the space-separated elements.
Select or access elements through the CSS or JavaScript element selector (such as GetElementsByClassName, QuerySelectorAll).

MDN / W3Schools

## id

Define a unique identifier (identifier, ID) in the document.
Select or access elements through the CSS or JavaScript element selector (such as GetElementsByClassName, QuerySelectorAll).

MDN / W3Schools

## style

Declare the CSS to be applied to the element.

MDN / W3Schools

## title

Specifies the information (description) of the element.

MDN / W3Schools

## lang

Specifies the language of the element (ISO 639-1).

```xml
<p lang="en">This paragraph is English</p>
£Plang="ko" is Korean.</p>
<p lang="fr">Ce paragraphe est défini en français.</p>

```

MDN / W3Schools

## data-*

Specify custom data properties.
Used to store data (information) available in JavaScript in HTML.

```xml
<!-- data-custom-data-attributes -->
<div id="me" data-my-name="Heropy" data-my-age="851">Heropy</div>

```

```js
// dataset.customDataAttributes
const $me = document.getElementById('me');
console.log($me.dataset.myName); // "Heropy"
console.log($me.dataset.myAge); // "851"

```

MDN / W3Schools

## draggable

Specifies whether the element is capable of using the Drag and Drop API.

```xml
<div draggable="true">The element to drag.</div>

```

MDN / W3Schools

## hidden

Hide element.

```xml
<form id="hidden-form" action="/form-action" hidden>
The hidden forms. -->
</form>
<button form="hidden-form" type="submit">전송</button>

```

MDN / W3Schools

## tabindex

Use the `Tab` key to specify the order in which elements are sequentially focused.

- Interactive content is ordered by default by tabs in order of code.
- Specify `tabindex="0" for non-interactive content to use tab order like interactive content.
- `tabindex="-1" allows focus but excludes from tab order.
- Positive values above `tabindex=1" are not recommended because they interfere with logical flow.

```xml
<h1 tabindex="0">Sign In</h1>
<label>Username: <input type="text"></label>
<label>Password: <input type="password"></label>
<label>PS: <input type="text" tabindex="-1"></label>
<input type="submit" value="Sign In">

```

MDN / W3Schools

Using the tabindex attribute

# skipped elements

## <template>

Retains content that is not rendered.

- Rendering using JavaScript.
- Use for content that is used repeatedly.
- IE Unsupported

MDN / W3Schools

## <map>, <area>

Define image maps (`<map`) and clickable areas (`<area`).
(Use in conjunction with `<img />`)

MAP: MDN / W3Schools
AREA: MDN / W3Schools

## <picture>

Insert image.
(Replaceable by `srcset` and `sizes` of `<img />`)

MDN / W3Schools

## <source>

Specify multiple media resources such as `audio`, `video`, and `picture` where browser can select.

MDN / W3Schools

## <track>

Specify subtitles, caption files, etc. to be displayed when media such as `audio` and `video` are playing.

MDN / W3Schools

## <embed>

Insert an external application or an interactive plug-in.

MDN / W3Schools

## <object>

Insert multimedia, nested browser context (frames), plug-ins, etc.

MDN / W3Schools

## <param>

Define the parameters of `<object>`.

MDN / W3Schools

# omitted properties

<table><thead><tr><th>use tag</th><th>property</th><th>meaning</th><th>value</th><th>feature</th> </tr></thead><tbody><tr><td><code>`&lt;link /&gt;`</code>,<br><code>`&lt;a&gt;`</code></td><td>hreflang</td><td>Alternate language for current page (<a href="https://en.wikipedia.org/wiki/ISO_639-1_%EC%BD%94%EB%93 %9C_%EB%AA%A9%EB%A1%9D" target="_blank" rel="noopener">ISO 639-1</a>)</td><td><code>`ko`</ code>, <code>`en`</code>… </td><td><a href="https://moz.com/learn/seo/hreflang-tag" target="_blank" rel="noopener">If you have multiple versions of the page in different languages or regions </a></td></tr><tr><td><code>`&lt;ol&gt;`</code></td><td>reversed</td><td> in reverse order Settings</td><td></td><td>IE not supported</td></tr><tr><td><code>`&lt;link&gt;`</code>,<br>< code>`&lt;img /&gt;`</code>,<br><code>`&lt;video&gt;`</code>,<br><code>`&lt;script&gt;`</code></td><td>crossorigin</td><td>import is <a href="https://developer.mozilla.org/en/docs/Web/HTTP/Access_control_CORS" target="_blank" rel="noopener Whether it should be done using ">CORS</a></td><td><code>`anonymous`</code>,<br><code>`use-credentials`</code></td ><td></td></tr><tr><td><code>`&lt;img /&gt;`</code></td><td>ismap</td><td>server side Whether to send the coordinates to the server by specifying as an image map and clicking <a href="https://en.wikipedia.org/wiki/Query_string" target="_blank" rel="noopener">Queries</a> </td><td>Boolean</td><td><code>`&lt;img /&gt;`</code> <code> with valid <code>`href`</code> attribute >`&lt;a&gt;`</code> only allowed for sub-elements</td></tr><tr><td><code>`&lt;img /&gt;`</code></td><td>usemap</td><td>Specify as client-side image map</td><td><code>`&lt;map&gt;`</code> in <code>`#`</code> > + <code>`name`</code> attribute value</td><td><code>`&lt;a&gt;`</code>, <code>`&lt;button&gt;`</code> Not available for elements</td></tr><tr><td><code>`&lt;form&gt;`</code></td><td>accept-charset</td><td>server <a href="https://www.iana.org/assignments/character-sets/character-sets.xhtml" target="_blank" rel="noopener">character encoding method</a></td ><td><code>`UTF-8`</code>, <code>`EUC-KR`</code>… </td><td><code>`UNKNOWN`</code></td></tr><tr><td><code>`&lt;form&gt;`</code></td><td >enctype</td><td><code>`method`</code> If the property is <code>`POST`</code>, <a href="https://developer of the content sent to the server .mozilla.org/en/docs/Web/HTTP/Basics_of_HTTP/MIME_types" target="_blank" rel="noopener">MIME type</a></td><td></td><td></td></tr><tr><td><code>`&lt;input /&gt;`</code></td><td>accept</td><td>Type of file the server will receive</td ><td>file extension(<code>`.jpg`</code>, <code>`.png`</code>..),<br><a href="https://developer.mozilla. org/en/docs/Web/HTTP/Basics_of_HTTP/MIME_types" target="_blank" rel="noopener">MIME type</a>,<br><code>`audio/*`</code>,<br ><code>`video/*`</code>,<br><code>`image/*`</code></td><td><code>`type="file"`</code> </td></tr><tr><td><code>`&lt;input /&gt;`</code></td><td>width</td><td>width of image</td>td><td>Number</td><td><code>`type="image"`</code></td></tr><tr><td><code>`&lt; input /&gt;`</code></td><td>height</td><td>horizontal width of the image</td><td>Number</td><td><code>` type="image"`</code></td></tr><tr><td><code>`&lt;i nput /&gt;`</code>,<br><code>`&lt;button&gt;`</code></td><td>formaction</td><td>where to send form data when submitting a form </td><td>URL</td><td><code>`type="submit"`</code>,<br><code>`type="image"`</code>,<br ><code>Overrides properties of `form`</code></td></tr><tr><td><code>`&lt;input /&gt;`</code>,<br><code >`&lt;button&gt;`</code></td><td>formenctype</td><td>Specify how the form data will be encoded before sending it to the server</td><td>-</td> of <td><code>`type="submit"`</code>,<br><code>`type="image"`</code>,<br><code>`form`</code> Over attribute</td></tr><tr><td><code>`&lt;input /&gt;`</code>,<br><code>`&lt;button&gt;`</code></td><td>formmethod</td><td>How to send form data</td><td><code>`GET`</code>, <code>`POST`</code></td ><td><code>`type="submit"`</code>,<br><code>`type="image"`</code>,<br><code>`form`</code> Over the properties of</td></tr><tr><td><code>`&lt;input /&gt;`</code>,<br><code>`&lt;button&gt;`</code> </td><td>formnovalidate</td><td>Specify not to validate form data</td><td>Boolean</td><td><code>`type="submit Properties of "`</code>,<br><code>`type="image"`</code>,<br><code>`form`</code> Take precedence over</td></tr><tr><td><code>`&lt;input /&gt;`</code>,<br><code>`&lt;button&gt;`</code></td><td>formtarget</td><td></td><td><code>`_self`</code>, <code>`_blank`</code></td><td><code >`type="submit"`</code>,<br><code>`type="image"`</code>,<br>Overrides properties of <code>`form`</code></code></code>td></tr><tr><td><code>`&lt;input /&gt;,`</code><br><code>`&lt;textarea&gt;`</code></td><td >minlength</td><td>Minimum number of characters that can be entered</td><td>Number</td><td><code>`type="text"`</code>,<br> <code>`type="email"`</code>,<br><code>`type="password"`</code>,<br><code>`type="tel"`</code> ,<br><code>`type="url"`</code></td></tr><tr><td><code>`&lt;input /&gt;`</code></td ><td>pattern</td><td>Regular expression that checks the value of the form</td><td>RegExp</td><td><code>`type="text"`</code>,<br><code>`type="search"`</code>,<br><code>`type="tel"`</code>,<br><code>`type=" url"`</code>,<br><code>`type="email"`</code></td></tr><tr><td><code>`&lt;input /&gt;` </code>,<br><code>`&lt;textarea&gt;`</code>,<br><code>`&lt;select&gt;`</code></td><td>required</td> <td>Required</td? ><td>Boolean</td><td></td></tr><tr><td><code>`&lt;input /&gt;`</code></td><td >size</td><td>Horizontal width of form</td><td>Number (Number, <code>`20`</code>)</td><td>Average character width
 

# omitted global properties

## accesskey

Provides keyboard shortcut hints for elements.

- It is not recommended for use on general purpose websites for the following reasons:
Conflict with browser keyboard shortcuts or functionality of assistive devices
Use non-existent keys on a particular keyboard
Specify keys that do not have logical relationships, such as numbers
User`s mistake of not knowing the existence of `accesskey`
- Conflict with browser keyboard shortcuts or functionality of assistive devices
- Use non-existent keys on a particular keyboard
- Specify keys that do not have logical relationships, such as numbers
- User`s mistake of not knowing the existence of `accesskey`

```xml
<a href="https://google.com" accesskey="G">Press 'Alt' + 'G' key on Chrome!</a>

```

MDN / W3Schools

## contenteditable

Specifies whether to edit the user of the element.

```xml
<style>
p::before { content: "["; }
p::after { content: "]"; }
</style>

<blockquote contenteditable="true">
<p>Edit this content to add your own bracket.</p>
</blockquote>

```

MDN / W3Schools

# References

https://developer.mozilla.org/ko/docs/Web/HTML/Element
https://webclub.tistory.com/523