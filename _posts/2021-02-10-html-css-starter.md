---
layout: post
title: "HTML recommended for beginners, CSS first step"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/html5.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/html5.png)

# a general outline

The understanding and learning of HTML, CSS, and JS that make up the Web is not just about learning technology, but also helps you understand the past, present and future of the Web, mobile and IT as a whole.
And in practice, we can develop the ability to see Product from a more structural perspective.
In particular, it can be of great help in the areas of planning, design, and technology sales that collaborate closely with product development.
For this reason, many companies are demanding development experience from non-publicists.

Before learning HTML, CSS, I organized as much knowledge as I could.

> If you think it's a little difficult, you can skip it!

## HTML, CSS, and JavaScript

![image](https://heropy.blog/css/images/vendor_icons/html5.png)

Hyper Text Markup Language (HTML) is a static language that defines and represents titles, paragraphs, tables, images, videos, and more on a page.
Don`t try to make the screen look pretty with HTML! (I can`t do that, but...)
You should focus only on creating a completely robust structure.

![image](https://heropy.blog/css/images/vendor_icons/css3.png)

Cascading Style Sheets (CSSs) are static languages that shape the content structure by specifying how the markup language (HTML, XML, etc.) is actually displayed (color, size, font, layout, etc.).
I can only focus on making pretty things.
You can focus on the right size, beautiful color, and position you want.

![image](https://heropy.blog/css/images/vendor_icons/javascript.png)

JavaScript (JS) is a programming language that dynamically decorates pages, such as changing and moving content, and is responsible for the dynamic processing of the web.
JS can use HTML and CSS to handle their work (structure, visual representation, etc.), but they are not as good as they are, so they should focus only on dynamic processing (performance).

For efficient work such as framing, grooming, and interior design, we need to be careful not to replace each language`s role in producing web pages, and to optimize language (code) structurally and technically to ensure that each role is performed properly.
(Many beginners wonder why these three simple roles are divided into files and folders and written more complexly, as mentioned, but they are structurally and technically optimized for larger and more complex web pages and are essential for maintenance, scalability, productivity and more. Understanding and getting used to this process itself is very important when learning. But you don`t have to think too hard yet!)

The following is a visual representation of the roles played by HTML, CSS, and JS.
It will help you understand the role of each language.

HTML-CSS-JS

![image](https://heropy.blog/images/screenshot/html-css-starter/html-css-js-site-screenshot.jpg)

### Web Standard

Web Standard means `standard technology or rules used on the web` and includes technologies from W3C`s recommendations.
Based on these standard technologies, web browsers (chrome, IE, safari, etc.) are created, and browsers that run slightly differently, such as differences in interpreting standard technologies (such as Google, MS, Apple, etc.), and new technologies (depending on standardization steps).
It`s almost gone now, but technologies like ActiveX and Flash are not standard.

> In most cases, it is easy to think of technology as a standard that corresponds to the "recommendation" (REC) of the standardization phase.

### Cross-browsing

![image](https://heropy.blog/images/screenshot/html-css-starter/browsers.jpg)

> From the right, Google Chrome, MS Edge, Mozilla Firefox, Opera, Eastsoft Swing, Naver Whale, MS IE, Apple Safari

Cross browsing refers to techniques, methods, etc. that allow multiple browsers that run slightly differently, as described above, to give the same user experience (the same screen, the same behavior, etc.).
While most browsers are designed to comply with web standards as much as possible, Microsoft Internet Explorer (MSIE) browsers are not easy to standardize in many ways because they are designed to be Web standards and so on.
Therefore, what works in IE is also called cross-browsing.(In most cases, if IE does not have a problem, other browsers are OK!)

> In the spring of 2016, Naver launched a campaign called, "Let's break up with the old version of Explorer, which threatens the security of my PC.

### Web Accessibility

Web accessibility refers to how to create web content that is equally accessible to all users, including those who have limited physical and environmental conditions, such as the elderly and the disabled.
Simple ways to put subtitles in an image for the deaf, make the web available on a keyboard (or vice versa) for those who cannot use the mouse, and provide alternative text to the image are all web accessibility.
Please refer to the link below for detailed documentation such as instructions for accessibility and production techniques.

Web Accessibility Laboratory
Korean Web Content Accessibility Guidelines 2.1
Web content creation techniques

> The power of the web lies in its universality. It is essential that everyone can access it regardless of the disorder.
Sir Tim Berners-Lee - The founder of the Web

For people with disabilities, Web content should be accessible through the following information and communication aids:
These assistive devices can be purchased by individuals or leased from government agencies or welfare facilities for the disabled.

![image](https://heropy.blog/images/screenshot/html-css-starter/assistiv_technology.jpg)

- One-handed keyboard: a keyboard designed to be typed with one-handed for people with partial paralysis
- LARGE KEYBOARD: Make the keyboard`s input buttons large, suitable for those with hand tremors or upper extremity functions, and special keyboards with key guards to prevent typos.
- n-Abler Joystick: Joystick Mouth for those whose arms are not moving smoothly and making it difficult to use a regular mouse
- SmartNav: A special mouse that can be controlled by people who have difficulty using the upper and lower extremities through head movements (using infrared and oblique stickers).
- Sound notification: Deaf radio signals that inform sound information by light and vibration

![image](https://heropy.blog/images/screenshot/html-css-starter/web_access_mark.jpg)

As a system to certify and mark quality of excellent sites that comply with web accessibility standards for the disabled and the elderly, they can be reviewed and certified through web accessibility certification institutions designated by the Ministry of Science and ICT under Article 32-2 of the Framework Act.
After a written examination and technical examination, you have to pay more than 1 million won depending on the number of pages.

> Realistically, it is not easy to acquire from individual or small business sites.

## Editor for Web Development

Unlike web app designs that depend on limited tools such as Photoshop and Sketch3, web development has little dependence on tools.
It doesn`t matter if you even use a notepad, but I recommend choosing a good editor for development performance.
It briefly introduces cross-platform (Windows, macOS) editors that are popular in practice among web development (front-end, Node.js) editors for HTML, CSS, and JS.

> The following are all good editors.
I wrote it from a subjective point of view, so just refer to it and try writing it once and choose the appropriate editor for you.

### Sublime Text

![image](https://heropy.blog/images/screenshot/html-css-starter/logo_sublime_text.jpg)

https://www.sublimetext.com/

It is relatively light and has less performance degradation.
I don`t use it much, so I skip the evaluation.
It`s free.

### Atom

![image](https://heropy.blog/images/screenshot/html-css-starter/logo_atom.jpg)

https://atom.io/

Text editor created by GitHub.
It has enough expansion function and is popular in foreign countries.
(Github was acquired by Microsoft in 2018 and what is Atom`s future?)
There are a lot of regrets about using it in Windows.
macOS is frequently used when working with documents.
It`s free.

### Brackets

![image](https://heropy.blog/images/screenshot/html-css-starter/logo_brackets.jpg)

http://brackets.io/

Text editor created by Adobe.
It`s not a Creative Cloud product line, but it`s open-source.
Although it is specialized in viewing visual output, such as the built-in Live Preview feature (also Adobe!), it is regrettable that the expansion feature or performance optimization is especially optimal (also Adobe..)
It`s free.

### VS Code

![image](https://heropy.blog/images/screenshot/html-css-starter/logo_vs_code.jpg)

https://code.visualstudio.com/

Visual Studio Code (VSCode) is a text editor created by Microsoft (MS).
You can start light and have a lot of scalability.
In the 2018 Stack Overflow survey, the editor`s popularity also topped the list.
It`s free.

### WebStorm

![image](https://heropy.blog/images/screenshot/html-css-starter/logo_webstorm.jpg)

https://www.jetbrains.com/webstorm/

One of the Integrated Development Environment (IDE) programs created by JetBrain.
You can start most projects right away, with little or no additional scaling required.
It`s a recommended program that`s so comfortable that it`s hard to transfer to another editor if you use it once, but the trap is that it`s paid.
However, if you get a student license, you can use it for free and there is a 30-day trial, so I recommend you try it if you have time.

> The subscription program has a personal license of $59 per year. I don't think it's expensive.

## Installing and Setting Up the VS Code

I chose VS Code as a free program. (Company uses WebStorm)
Let`s prepare an editor for web development, from installation to shortcut usage to extension!

### Installation

Install the Stable version for your operating system (Windows, macOS, Linux).
Ignore the separate settings and press Next/Next to proceed.

![image](https://heropy.blog/images/screenshot/html-css-starter/vs_code_download.jpg)

![image](https://heropy.blog/images/screenshot/html-css-starter/vs_code_interface.jpg)

### Extensions

![image](https://heropy.blog/images/screenshot/html-css-starter/vs_code_extensions_icon.jpg)

Other editors are the same, but VS Code provides extensions, and you can download (install) a variety of extensions from the original version and use them after connecting (most of them can search for extensions on the editor itself).
Some extensions may already be supported depending on the version of the editor, so I recommend you to check and install the features provided.

Almost all menus in the VS Code are changed to Korean.

Modify the code creation style (beautifully) for code readability.
I highly recommend it to beginners.

- `Preferences>
 Keyboard Shortcut` 선택
- Search for `HookieQR.beauty` (not `HookieQR.beautyFile`!)
- Select `Key Bind`
- Register your own shortcut

> "Alt + Ctrl + L" (Windows) / "Alt + Cmd + L" (macOS) is recommended.

Select `Go Live` at the bottom status bar
Or
Right-click on the HTML screen>
 Select `Open with Live Server`

> Depending on the version of VS Code, it might already be installed.

When you modify the tag name, the open and closed tags are modified in pairs.
You can reduce the hassle of having to modify each one.

You won`t need the following extensions right away.
Take a look later.

Terminal
Live Sass Compiler
Turbo Console log
Better Comments
Highlight Matching Tag

GitLens
REST Client

### A good shortcut to know.

You can view or change shortcuts under Preferences>
 Keyboard Shortcut.

> The key bindings can be retrieved using '''.
Because closing '" requires accurate search, it is recommended to search without closing '"" as shown in 'cmd + p'

<table><thead><tr><th>Windows shortcut</th><th>macOS shortcut</th><th>Description</th></tr></thead><tbody><tr><td>“Ctrl + B”</td><td>“Cmd + B”</td><td>Open/close sidebar</td></tr><tr><td>“Ctrl + P” </td><td>“Cmd + P”</td><td>Fast open (search for files or symbols)</td></tr><tr><td>“Ctrl + Shift + P”</td><td>“Cmd + Shift + P”</td><td>Show all commands (access all commands in editor)</td></tr><tr><td>“Ctrl + F”</td><td>“Cmd + F”</td><td>Find (search)</td></tr><tr><td>“Ctrl + H”</td><td>“Cmd + Opt(Alt) + F”</td><td>Find(Search)/Replace(Replace)</td></tr><tr><td>“Alt + Up”</td><td> “Alt + Up”</td><td>Move up line</td></tr><tr><td>“Alt + Down”</td><td>“Alt + Down”</td> <td>Move down line</td></tr><tr><td>“Shift + Alt + UpArrow”</td><td>“Shift + Alt + UpArrow”</td><td>Up Copy line</td></tr><tr><td>“Shift + Alt + DownArrow”</td><td>“Shift + Alt + DownArrow”</td><td>Copy line below</td>
</tr><tr><td>“Tab”</td><td>“Tab”</td><td>Indentation</td></tr><tr><td>“Shift + Tab”</td><td>“Shift + Tab”</td><td>Outdent</td></tr><tr><td>“Ctrl + PageUp”</td><td>“ Cmd + Shift + ["</td><td>Open previous editor (switch to left pane)</td></tr><tr><td>“Ctrl + PageDow n”</td><td>“Cmd + Shift + ]”</td><td>Open next editor (switch to right pane)</td></tr><tr><td>“Ctrl + \ ”</td><td>“Cmd + \”</td><td>Editor split (backslash)</td></tr><tr><td>“Ctrl + number”</td><td>“Cmd + number”</td><td><code>`number`</code>Focus on second split editor group</td></tr><tr><td>“Ctrl + W” </td><td>“Cmd + W”</td><td>Close editor</td></tr></tbody></table>

 

- Select code to wrap
- Run All Commands / "Ctrl + Shift + P" (Windows), "Cmd + Shift + P" (macOs)
- Enter "Emmet: Wrap with Abbrevision" or select from the list ("Enter")
- Emmett grammar such as `div` and `span` (ex: `).wrapper`, `span.bold`)
- Complete ("Enter")

## Images used by the Web

### Bitmap and Vector Images

Images (graphics) are largely separated by bitmaps and vectors.

Bitmap is a collection of information created by each pixel, also known as a Raster image.
Rendering on the screen in pixels. (Rendering: allows computers to draw pictures on the screen so we can see them.)
Most of the images we usually use are in bitmap format.
You can edit it with tools such as Paint and Photoshop.

Vector is the result of mathematical forms of information.
It has complete information such as the point, line, position (coordinate), color, etc. of the image and renders it on the screen.
Therefore, you need to do more operations, but instead you can freely render the resolution, which affects the resolution (pixel), as opposed to the bitmap image.
Simply put, the image doesn`t break even if you zoom in or out.
There is also no capacity change due to image zoom because it has only mathematical information.
You can edit it with tools like illustrations.

![image](https://heropy.blog/images/screenshot/html-css-starter/difference_bitmap_vector.jpg)

<table><thead><tr><th>image type</th><th>advantages</th><th>disadvantages</th></tr></thead><tbody><tr><td
>
Bitmap</td><td>Exquisite and natural expression of various colors</td><td>Scaling up/down when zooming, quality deterioration</td></tr><tr><td>Vector</td>
td><td>Free to zoom in/out, no change in size</td><td>Difficulty expressing sophisticated images (such as portraits and landscape photos)</td></tr></tbody></table>

 

> Sketch 3 can be seen as a vector-based UI production tool rather than editing images.

### JPG(JPEG)

Created for compression of the Joint Photographic coding Experts Group (JPG) Full-color and Gray-scale, it is widely used in photography and art.

- Loss Compression
- Excellent representation color scheme (24 bits, approximately 16 million colors) for high resolution displays
- Easily adjust image quality and capacity
- Most popular image format

> High compression rate (low capacity)!

### PNG

Portable Network Graphics (PNG) was developed as an alternate format for Gif.

- Non-loss compression
- 8-bit (256 color) / 24-bit (approximately 16 million color) color image processing
- Alpha Channel Support (Transparent)
- W3C Recommended Format

> Transparency enabled!

### GIF

Graphics Interchange Format (GIF) can store information such as images and strings within an image file.

- Non-loss compression
- Multiple images can be placed in a single file (gifs, animations)
- Supports only 8-bit colors (not suitable for expressing various colors)

> Video-like images! (animation)

### WEBP

It is an image format developed by Google that can replace JPG, PNG, and GIF.

- Supports complete loss/non-loss compression
- Support for animations such as GIF
- Alpha Channel support (both lost and non-loss)

> Perfect format! But the support browser...

Support Browser for Webp

### SVG

Scalable Vector Graphics (SVG) is a format for representing vector graphics based on the Markup Language (HTML/XML).

- Freedom from the effects of resolution
- CSS enables styling
- JavaScript로 Event Handling 가능
- Can be used as a code or file

> If you are not familiar with vector images, it may be a little tricky to handle.

## Special Character Terminology Cleanup

<table><thead><tr><th>symbol</th><th>English (pronunciation)</th><th>Korean</th></tr></thead><tbody><tr> <td>`</td><td>Grave</td><td>-</td></tr><tr><td>~</td><td>Tilde</td><td>Tilde</td></tr><tr><td>!</td><td>Exclamation mark</td><td>Exclamation mark</td></tr><tr><td>@</td><td>At (at) sign</td><td>Golf</td></tr><tr><td>#</td><td>Number (number) sign, Sharp (sharp)</td><td>Sharp, well well</td></tr><tr><td>$</td><td>Dollar (dollar) sign </td><td>Dollar</td></tr><tr><td>%</td><td>Percent(percent) sign</td><td>percent</td></tr>
<tr><td>^</td><td>Caret</td><td>-</td></tr><tr><td>&amp;</td><td> Ampersand</td><td>-</td></tr><tr><td>*</td><td>Asterisk</td><td>asterisk</td>td></tr><tr><td>-</td><td>Hyphen, Dash</td><td>Minus</td></tr><tr><td>
_</td><td>Underscore, Low dash</td><td>Underscore</td></tr><tr><td>=</td><td>
Equals sign</td><td>Icor</td></tr><tr><td>“</td><td>Quotation mark</td><td> Double quotes</td></tr><tr><td>'</td><td>Apostrophe</td><td>Single quotes</td></tr><tr><td>
:</td><td>Colon</td><td>dot</td></tr><tr><td>;</t d><td>Semicolon</td><td>Furry dot</td></tr><tr><td>,</td><td>Comma</td><td>comma</td></tr><tr><td>.</td><td>Period, Dot</td><td>dot, period</td></ tr><tr><td>?</td><td>Question mark</td><td>Question mark</td></tr><tr><td>/</td><td>Slash</td><td>-</td></tr><tr><td>|</td><td>Vertical bar</td><td>- </td></tr><tr><td>\</td><td>Backslash</td><td>-</td></tr><tr><td>( )</td><td>Parenthesis</td><td>(small) parentheses</td></tr><tr><td>{}</td><td>Brace (Brace)</td><td>Brace</td></tr><tr><td>[]</td><td>Bracket</td><td>Bracket</td> </tr><tr><td>&lt;&gt;</td><td>Angle Bracket</td><td>Angle brackets</td></tr></tbody></ table>
 

HTML Entity List

## Open Source License

> Open source is the disclosure of the source code or design required in the process of developing a product for anyone to access and access.

Open source is usually a free copyright and I don`t think it`s a problem to use it for free, but in fact, there are many kinds of open source licenses available for personal use, but there are commercial restrictions or in some cases you may have to pay for them.

Realistically, you can`t write all your own code from beginning to end, so in many cases you rely on open source. There will be no problem for personal use, but if you use it without thinking at work (commercially), you may have to get fired or compensate for the damage.
Of course, copying a few lines of code floating on the Internet won`t be such a serious problem, but always be careful.
If you found a good open source while working at the company, find the license reflexively first!

You don`t have to study all the licenses, but I`m going to introduce some licenses that make you happy just by looking at them.

### Apache License

A license created by the Apache Software Foundation for application to its own software.
Personal/commercial use, distribution, modification, and patent application are possible.

### MIT License

A license developed by the Massachusetts Institute of Technology (MIT) for software students.
You just need to keep the indication that you are using this license for your personal source, and it is popular because there are no restrictions on the rest of your use.

### BSD License

Berkeley Software Distribution (BSD) is a license developed by the University of California, Berkeley.
Just keep the license mark the same as MIT.

### Beerware

It`s a license that requires you to buy beer to an open source developer.
Of course, if we can meet!

More open source licenses are available at OpenSource.org.

# HTML

## Really easy HTML grammar

### Basic Form

Tags have their own meanings and have the following forms:

```xml
<TAG></TAG>
<TAG>CONTENT</TAG>

```

```xml
The rabbit and the turtle.
Once upon a time, there lived a rabbit and a turtle...</p>

<!--> can be understood as follows: -->
The Rabbit and the Turtle.
Once upon a time, there lived a rabbit and a turtle...</paragraph>

```

Tags also have a tag structure that opens and closes, which is a pair.
(sometimes called an end structure that starts and ends)
This structure creates a range of tags.

```xml
The rabbit and the turtle.

<!--> can be understood as follows: -->
The Rabbit and the Turtle. The Rabbit and the Turtle.

```

What the introducer should be aware of is that closing tags have a `/` (slash) in front of the tag name.

### Attributes and Value

`Properties` can be used to extend the functionality of tags (elements).

```xml
'<TAG Properties="Value"></TAG>

```

```xml
<img src=>./my_photo.jpg" alt="My Profile Picture" />
<div class="name">홍길동</div>

<!--> can be understood as follows: -->
<Image insertion source location="./my_photo.jpg" Alternate Text="My Profile Picture" />
<Unmeaningful split tag alias ="name">Hong Gil-dong</Unmeaningful split>

```

<img />` is the tag used to insert the image.
However, because the tag alone does not tell which image to insert, use the attribute `src`(source) to specify the path of the image to insert. The attribute `alt` (alternative) specifies the text that will be displayed instead of the image when it fails to output the image.
`<div></div>` can be wrapped up in a meaningless tag (Wrap anything.
On top of this, `Hong Gil-dong` was grouped together, but `name` was added because we don`t know what it means.

> <img />' looks a little weird. If no tag is closed, it is called an empty tag. Explain again.

### Parent and Child Elements

When tag A is used as the content of tag B, tag B is called the parent element of tag A and tag A is called the child element of tag B.

```xml
<PARENT>
<CHILD></CHILD>
</PARENT>

```

```xml
<section class="fruits">
<h1>List of fruits</h1>
<ul>
<li>Apples</li>
<li>Strawberry</li>
<li>Banana</li>
<li>Orange</li>
</ul>
</section>

<!--> can be understood as follows: -->
<SECTION AREA TAGNAMINATION="fruits"SECTION AREA TAGNAME
<Topic 1>List of fruits</Topic 1>
<List of Unordered>
<Item>Apple</Item>
<Item>Strawberry</Item>
<Item>Banana</Item>
<Item>Orange</Item>
</List without order>
</Section Area>

```

There are <h1></h1>, <ul></ul>, <li></l> and <li></l> in <ul></l> and <l></l> in <ul></l> (content></l>).
(I`ll skip the closing tag.)
In this structure, `section` is the parent element of `h1` and `ul`.
In addition, `ul` is the parent element of `li`.
On the contrary, `h1` and `ul` are the child elements of `section`.
In addition, `li` is the child element of `ul`.

where `ul` is the child element of `section` and the parent element of `li`.
Like this, the elements of parents and children are relative concepts.
(A little furthermore, <section> is the ancestor of <li>, whereas <li> is the descendant of <section>).

Just as we define titles like grandfathers, mothers, uncles, and brothers through basic family tree (or vice versa), the structure of HTML defines titles with the above concept and is used important when dealing with HTML with CSS and JS through selector later.

### Empty Tag

HTML has tags that don`t have a closing concept.
It has the following forms:

```xml
Empty tag without '!--' -->
<TAG>

<!-- Empty tag with '/' -->
<TAG/>
<TAG />

```

HTML5 can use both of the above forms, which may be required to use `/` depending on the XHTML version, the Lint environment, or the framework setting.

> There is an argument that some form should be used, which is not really important. Use the form you want, but don't mix it.

## Extents of HTML documents

HTML files such as `index.html` can be described as HTML documents.
Let`s find (meaning) tags that represent the range of HTML documents.

```xml
<!DOCTYPE html>
<html>
<head>
Information in a Document
</head>
<body>
Structure of a documentation
</body>
</html>

```

```xml
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="author" content="홍길동">
<meta name="description" content="Our site is the best!">
<title>My Site</title>
<link rel="stylesheet" href="./css/main.css">
<script src="./js/main.js"></script>
</head>
<body>
<section>
<h1></h1>
<div>
<ul>
<li></li>
<li></li>
</ul>
</div>
</section>
</body>
</html>

```

### HTML (Full Range)

<html> specifies the full range of HTML documents.
It tells you where the HTML document that the web browser should interpret begins and ends.

### HEAD (Information Scope)

Specifies the range of information in HTML documents that the Web browser should interpret.
The information we`re talking about is the title of the web page, how the web page is encoded in characters, the location of the external file you want to link to, and the default setting value for structuring the web page.
In other words, it can be described as `preferences for configuring the screen`.

### BODY (Structure Scope)

Specifies the structure range of HTML documents that the Web browser should interpret.
Structure refers to the form or layout of content (content) that users can see on the screen, and everything they see, such as logos, headers, putters, navigation, menus, buttons, input windows, pop-ups, advertisements, etc., is structural.
The structure is created only within the BODY range.

### DOCTYPE (DTD, versioning)

Document Type Definition (DOCTYPE) defines the document format in the markup language.
This tells the web browser what HTML version of the document we`re going to provide should be structured in an interpretation manner.
(HTML has largely 1, 2, 3, 4, X-, 5 versions)

The current standard mode is HTML5.

```xml
<!-- HTML 5 -->
<!DOCTYPE html>

<!-- XHTML 1.0 Transitional -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

```

View more documentary information

> It's easy to think that the Windows operating system is similar to 95, 98, ME, XP, Vista, 7, 8, 10 versions.

## Information in HTML documents

Tags used in the <head></head> contain information about HTML documents.

### TITLE (Title of Web Page)

Defines the title of the HTML document.
It appears as a name on each Site tab of the Web browser.

![image](https://heropy.blog/images/screenshot/html-css-starter/browser_tabs.jpg)

```xml
<head>
<title>Naver</title>
</head>

```

### META (information on web pages)

Provides information about HTML documents (web pages), such as how they are displayed, author (owner), content, keywords, etc., to the search engine or browser.
The empty tag.Empty tag.

```xml
<head>
<meta charset="UTF-8">
<meta name="author" content="홍길동">
<meta name="description" content="Our site is the best!">
</head>

<!--> can be understood as follows: -->
<Document Information Scope>
= "UTF-8" =
<Information information type="Site Maker" information value="Hong Gil-dong">
<Information Type="Site Description" Information Value="Our Site Is the Best!">
</Information Scope of Document>

```

The following properties are available for `<meta>`.
Each tag has its own attributes and values that it can use.
You must be able to distinguish which properties are available and which are not.
You do not need to memorize all properties and values immediately because some properties are not used well.
(There are some attributes that can be used by any tag just because it`s a global attribute, but you don`t have to check it now!)

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 <code>`charset`</code></td><td>Character encoding method</td><td><code>`UTF-8`</code>, <code>`EUC-KR`</
 code> etc...</td></tr><tr><td><code>`name`</code></td><td>Type of information (meta data) to provide to search engines, etc.<
 /td><td><code>`author`</code>, <code>`description`</code>, <code>`keywords`</code>, <code>`viewport`</code>, etc.
 ..</td></tr><tr><td><code>`content`</code></td><td><code>`name`</code> or <code>`http-
 equiv`</code> provide the value of the attribute</td><td></td></tr></tbody></table>

 

### LINK (CSS Recall)

Use to link external documents.
Specifically, it is used to recall and link CSS documents (the ``xxx.css`` file) created outside HTML.
The empty tag.Empty tag.

```xml
<head>
<link rel="stylesheet" href="./css/main.css">
<link rel="icon" href="./favicon.png">
</head>

<!--> can be understood as follows: -->
<Document Information Scope>
= "CSS" Document Path="./css/main.css">
="Site Representative Icon" Document Path="./favicon.png">
</Information Scope of Document>

```

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 <code>`rel`</code></td><td>(required) Specify the relationship between the current document and the external document</td><td><code>`stylesheet`</code>, <code
>
`icon`</code> etc..</td></tr><tr><td><code>`href`</code></td><td>Specify the location of the external document</td>
td><td>path</td></tr></tbody></table>

 

### STYLE (Create CSS)

Use to create a CSS inside an HTML document rather than creating it from an external document.

```xml
<style>
img {
width: 100px;
height: 200px;
}
p {
font-size: 20px;
font-weight: bold;
}
</style>

<!--> can be understood as follows: -->
<Style Definition>
<!-- CSS Code -->
</Style Definition>

```

### SCRIPT (Bring Up or Create JS)

In HTML documents, a CSS can be created by recalling a CSS to `link>` or within `style></style>.
JS can use both of these approaches with `<script></script>>.

```xml
"<!-- Load -->
<script src="./js/main.js"></script>

<!-- Create -->
<script>
function windowOnClickHandler(event) {
console.log(event);
}
window.addEventListener('click', windowOnClickHandler);
</script>

<!--> can be understood as follows: -->

<!-- Load -->
<JavaScript Document Path="./js/main.js"></JavaScript>

<!-- Create -->
<JavaScript>
<!-- JS Code -->
</JavaScript>

```

## Structure of HTML documents

Tags used within <body></body> represent the structure of HTML documents.

### DIV (Writing Tag)

`div` in `<div></div>` stands for `division`, which means `division` and `defines a part or section of a document`.
Because it doesn`t have a clear meaning, in many cases, it`s simply used to bind a particular range.
Usually, you apply CSS or JS to the parts that are tied up like this.

```xml
<body>
<div>
<p></p>
</div>
<div>
<div>
<h1></h1>
<p></p>
</div>
</div>
</body>

<!--> can be understood as follows: -->
<body>
<Bind 1>
<p></p>
</Bind 1>
<Bind 2>
<Bind 2-1>
<h1></h1>
<p></p>
</Bind 2-1>
</Tied 2>
</body>

```

> "DIV means nothing. Because it doesn't mean anything."

### IMG (Tag to Insert Image)

<img> is used to insert images into HTML.
(There are two main ways to insert an image into a web page: `IMG` in HTML and `background` in CSS)

```xml
<body>
<img src="./kitty.png" alt="냥이">
</body>

<!--> can be understood as follows: -->
<body>
<image path="./kitty.png> substitute text="cat">
</body>

```

<table><thead><tr><th>property</th><th>meaning</th><th>value</th></tr></thead><tbody><tr><td>
 <code>`src`</code></td><td>(required) URL of the image</td><td>URL</td></tr><tr><td><code>`alt
 `</code></td><td>(required) Specify an alternate text for the image</td><td></td></tr></tbody></table>

 

In the table above, the attributes `src`, `alt` are the attributes (required attributes) that must be included when using `img`.
If the `alt` property is missing by writing `<img src=>./kitty.png"> this is against the web standard.

## To check web standards

We can test whether the HTML document we wrote meets the standard.
Access the W3C validator, upload the HTML document you created, and start testing!
You can determine whether it is a basic standard.
Especially if you`re a beginner, it`s better to check often until you get used to it.

![image](https://heropy.blog/images/screenshot/html-css-starter/markup%20_validation_result_image_kytty.jpg)

This is the result of writing `="/kitty.png" above.
There are many of these rules that you need to follow while creating HTML documents.

> The test pass does not ultimately determine compliance with web standards/web accessibility. This test is actually more like a basic grammar test.

Alternatively, you can check with a site (page) address.
The following is the result of testing `naver.com`.

![image](https://heropy.blog/images/screenshot/html-css-starter/markup%20_validation_result_naver_com.jpg)

## HTML Examples

The following image is part of the GitHub main page (the image used in this example is what the old main page looked like)
Let`s code part of this page in HTML.

> Share the completed page (https://heropcode.github.io/GitHub-Responsive/). This is a clone page on the GitHub main page.

![image](https://heropy.blog/images/screenshot/html-css-starter/guthub_clone_page.jpg)

In order not to increase the difficulty level in the introductory stage (so that the code does not get too long), let`s organize only a part of the contents of the next header on the screen above.

![image](https://heropy.blog/images/screenshot/html-css-starter/guthub_clone_page_header_structure.jpg)

Create a project directory (folder) named `example1` in a familiar place, such as the desktop. (Please feel free to name it however you want.)
Now run the VS Code and select `File/Open` to locate and open the directory you created.
Create a file called `index.html` in it (set the name and extension of `file/new`>
 `Save`>
 name)
Compare the following code with the above structure and code it.

```xml
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>GitHub</title>
</head>
<body>
<div class="header">
<div class="container">
<div class="container-left">
<div class="logo">
<img src="https://heropcode.github.io/GitHub-Responsive/img/logo.svg" alt="GitHub Logo">
</div>
<div class="menu">
<div class="menu-item">Personal</div>
<div class="menu-item">Open Source</div>
<div class="menu-item">Business</div>
<div class="menu-item">Explore</div>
</div>
</div>
</div>
</div>
</body>
</html>

```

Currently, if you print this structure to a browser, only four TEXTs in one image and menu appear as follows (right-click>
 `Open with Live Server` on the HTML screen).

> If you do not have the 'Open with Live Server' menu, you must install the Live Server with the VS Code extension.

![image](https://heropy.blog/images/screenshot/html-css-starter/html_example_result.jpg)

It looks incomplete, but HTML is done with the role.
CSS is now required for pretty decoration (style).

# CSS

## CSS Grammar Can't Be Easier

CSS grammar is easier than HTML.
As shown in the example below, there are selectors, attributes, values, and understanding what they mean is sufficient for the basic grammar!

```css
div {
font-size: 20px;
color: red;
}

/* This is understood as follows: */
Selectors {
Properties: Value;
Properties: Value;
}

```

### Role of the selector

The selector is a sign that selects a specific element of HTML to apply style (CSS) to HTML.
"CSS, `color: red;` will be applied to that title (HTML, `<h1></h1`)!" and "CSS, `color: blue;` will be applied to this text (HTML, `<p><p></p>`)`!"

The above contents are organized into code as follows:

```xml
<div>
<h1>Title...</h1>
<p>The body</p>...</p>
</div>

```

```css
h1 {
color: red;
}
p {
color: blue;
}

```

### Properties and Value

Use to specify the same style of attributes and values as the letter color, width, and margin.

```css
div {
Color: red; /* letter color is red */
font-size: 20px; /* character size is 20px */
width: 300px; /* width is 300px */
margin: 20px; /* outer margin is 20px */
padding: 10px 20px; /* inner margin is 10px up and down, 20px */ left and right
Position: absolute; /* location based on parent element, hmm.. What does this mean? */
}

```

It is important to know a lot of properties and values.
If you want to increase the font size but don`t know what role `font-size` plays, there`s no way to adjust the font size.
Also, if you want to specify a location (coordinate), but you don`t know the `position` property, or if you don`t know what `absolute` means by using the value of the property, there`s no way.

Let`s assume that English is the only grammar consisting of a subject and a verb, like `I like`. Grammar may be very easy, but there will be many words such as `You`, `She` and `They` in the subject, and words such as `love`, `work` and `eat` in the verb, right?
Just as you can make rich and wonderful sentences as you know many words, you can make wonderful styles as you know many attributes and values!

## CSS Declaration Method

Now, let`s find out how I can apply the CSS code I wrote to HTML.

### Create Your Own on Tags (Inline)

This method is written directly to the HTML tag and therefore does not require a selector.

```xml
<divstyle="color:red;">Create directly in tag1</div> <!--red -->
<divstyle="color:red;">Create directly in tag2</div> <!--red -->
<divstyle="color:red;">Create directly in tag3</div> <!--red -->
<divstyle="color:red;">Create directly in tag4</div> <!--red -->

```

### Include in HTML (built-in)

You need a selector because you only create CSS separately.
CSS code is included in HTML`s `<style></style>`.

```xml
<head>
<style>
div {
color: red;
}
</style>
</head>
<body>
Include in <div>HTML1</div> <!--red -->
Include in <div>HTML2</div> <!--red -->
Include in <div>HTML3</div> <!--red -->
</body>

```

### Load from outside HTML

You can completely disconnect the CSS code.
You can use a single separate CSS file by importing several HTML files.

```xml
<!-- HTML 1 -->
<head>
<link rel="stylesheet" href="/css/main.css">
</head>
<body>
<Import externally to HTML1</div> <!--red -->
</body>

```

```xml
<!-- HTML 2 -->
<head>
<link rel="stylesheet" href="/css/main.css">
</head>
<body>
<Import externally to HTML2</div> <!--red -->
</body>

```

```css
/* main.css */
div {
color: red;
}

```

## Selectors

As described above, the selector is a sign that selects a specific element of HTML.
There are many kinds, but I`ll look at only two of them.

### Find with Tag

Enter the name of the tag that you want to apply (to find) to the part where you enter the selector.

```css
The letter h1 is red!*/
h1 {
color: red;
}
The letter "p" is blue!*/
p {
color: blue;
}

```

```xml
<h1>Title 1</h1> <!--red-->
<h1> Title2</h1> <!--red-->
<p>Body1</p> <!--blue-->
<p>Body2</p> <!--blue-->

```

What should I do if I want to add color only to Title 1 and Body 1?
I don`t think it`s going to be easy with a tag selector.

### Find by Class

There is something called a `class selector` to find the element you want more clearly.
Let`s take an example.

```css
*class="title" is red!*/
.title {
color: red;
}
/*class="main-text" is blue!*/
.main-text {
color: blue;
}

```

```xml
<h1 class="title">제목1</h1> <!--red-->
<h1>Subject2</h1>
<p class="main-text">본문1</p> <!--blue-->
<p>Body2</p>

```

Enter the desired nickname in the HTML attribute called `class`.
The title is `title` and the body is nicknamed `main-text`.
You can now find elements based on this nickname in the CSS.
However, it is important to note that before the selector, "."It`s about getting on.
The `.` represents the class and the `.title` of the CSS is the same as the `class="title" of HTML.
The selector `title` without `.` is recognized as a completely different meaning since it means tag `title`.
As such, it is easy to miss HTML and CSS by using special symbols such as `.`. Therefore, meticulous selection creation is important.

## Property

Now let`s find out the properties.
You can specify visible styles such as size, margin, and color.

### Size

Specifies the horizontal width of the element.
Units use `px` (pixels).

```css
div {
width: 300px;
element width: width value;
}

```

Specifies the vertical width of the element.

```css
div {
height: 150px;
element length width: width value;
}

```

Specifies the letter size of the element content (Text).

```css
div {
font-size: 16px;
letter size: size value;
}

```

### Margins

Specifies the outer margin of the element.
The outer margin is used to create a margin (distance, space) between the element and the element.

```css
div {
margin: 20px;
element outer margin: margin value;
}

```

The above code means that `margin` will designate a margin of `20px` for all elements up, above, below, left, and right.
To refine, you can also specify one direction at a time as shown below.
The code above and the code below mean the same thing.

```css
div {
margin-top: 20px;
margin-right: 20px;
margin-bottom: 20px;
margin-left: 20px;
element outside margin-up: margin value;
element outer margin-right: margin value;
element outer margin-bottom: margin value;
element outer margin-left: margin value;
}

```

Specifies the internal margin of the element.
Internal margin means the margin surrounding the child element.

```css
div {
padding: 20px;
within element margin: margin;
}

```

You can specify one direction at a time, such as `margin`

```css
div {
padding-top: 20px;
padding-right: 20px;
padding-bottom: 20px;
padding-left: 20px;
in-element margin-top: margin value;
in-element margin-right: margin value;
in-element margin - bottom: margin;
in-element margin-left: margin;
}

```

### Color

Specifies the character color of the element content (Text).
So many beginners make mistakes with `font-color` and `text-color`, but there is no such attribute.

```css
div {
color: red;
letter color: red;
}

```

Specifies the background color of the element.
`color` can only specify the color of letters; `background-color` is required to change the color of elements.

```css
div {
background-color: red;
element background: red;
}

```

## CSS Example

Let`s apply CSS following the example we wrote at the end of HTML.
Create an additional directory named `css` inside the `example1` directory (right-click>
 select a new folder in the Sidebar area)
Create the `main.css` file inside the `css` directory you created.

The following are the directories and file structures in this example:

```bash
/example1
├- /css
│ └- main.css
└- index.html

```

The `main.css` file you created (although you have not yet written CSS content) needs to be associated with the existing `index.html` file before it can be used.
Use `link` to connect a CSS file (the `main.css` you just created) that exists outside HTML.

```xml
<head>
[!-- -->
<link rel="stylesheet" href="./css/main.css">
</head>

```

Once connected, write CSS as follows.

```css
body {
/* Each browser has a value of margin and padding set by default on the BODY element. */
/* Each browser may have a different value for the BODY element, so we reset it and use it consistently. */
/* 0 does not use units. */
margin: 0;
padding: 0;
}
.header {
The /* screen is rendered with the following values, which can be omitted for many reasons (which requires a lot of explanation): */
/* width: 100%; */
/* height: 75px; */
background-color: white;
Border-bottom: 1px solid light gray; /* element borderline-below: 1px thick solid light gray; - The bottom of the header shows a gray line. */
}
.container {
/* height: 75px; */
width: 980px;
margin: auto; /* element outer margin: margin automatic; - This property and value are used as properties that center the container horizontally. */
}
.container-left {
/* width: 370px; */
/* height: 75px; */
/* float: left; */
padding-top: 20px;
padding-bottom: 20px;
}
.logo {
margin-right: 5px;
float: left; /* horizontal alignment: used to horizontally align the - logo and menu in order from the left. The exact meaning of this property is not horizontal alignment, but it was translated for easy understanding. */
}
.logo img { /* the child (behind) element of logo, img tag - Spacing from selector means child (behind) element. */
display: block; /* element characteristics: shape-oriented; - used to eliminate unnecessary margins at the bottom of the image. */
}
.menu {
used to horizontally align float: left; /* logo and menu. */
}
.menu-item {
font-size: 16px;
padding: 8px 10px; /* padding-top: 8px; padding-bottom: 8px; padding-left: 10px; padding-right: 10px; 과 같습니다. */
float: left; /* used to horizontally align each menu-item. */
Line-height: 1; /* Line height, similar concept to between lines. The default value is normal, which is about 1.2 times larger. If left intact, the .menu-item will be about 35px high, so you can modify it to 1x and work with 32px the same size as the .logo. */
}

/* float: left; is required to use and finish. */
/* float: enter class="clearfix" to the parent element of that HTML element using left; to turn off what happens with the CSS float property. */
.clearfix::after {
content: "";
display: block;
clear: both;
}

```

You will get used to it quickly if you practice by learning changes one by one by one by one by one, by alternating coding from `header` and coding from `menu-item` or `logo`.

This is important, if you used `float: left`, be sure to enter `class="clearfix" in the parent element of the element you used and see the change. (Exact usage and meaning are considerable, so I won`t explain it here.)
The final HTML is as follows: Take a closer look at what has changed from before.

```xml
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>GitHub</title>
Added <!-- link element! -->
<link rel="stylesheet" href="./css/main.css">
</head>
<body>
<div class="header">
<!-- Clearfix class value added! -->
The <!-- class attribute can be used simultaneously by separating multiple classes by spacing. -->
<!--> The elements below are like £div class="container" class="clearfix"> -->
<div class="container clearfix">
<!-- Clearfix class value added! -->
<div class="container-left clearfix">
<div class="logo">
<img src="https://heropcode.github.io/GitHub-Responsive/img/logo.svg" alt="GitHub Logo">
</div>
<!-- Clearfix class value added! -->
<div class="menu clearfix">
<div class="menu-item">Personal</div>
<div class="menu-item">Open Source</div>
<div class="menu-item">Business</div>
<div class="menu-item">Explore</div>
</div>
</div>
</div>
</div>
</body>
</html>

```

The following results should be obtained:

![image](https://heropy.blog/images/screenshot/html-css-starter/html_example_result_final.jpg)

# From now on...

As I wrote it, there are many parts that I omitted or translated to help beginners understand.
However, if you came all the way here because the beginning was half done, I think you succeeded enough.
Many non-public beginners are frustrated with unfamiliar learning environments, but HTML/CSS is not really that difficult.
You need to be confident!

Well, then, the entrance is over and I`ll briefly summarize how to learn from now on.

When searching for the content of HTML, CSS, JavaScript (ECMAScript, ES) you want to learn, search `MDN` together, such as `html div tag mdn` or `css background mdn`.
MDN Web documents are the official website of Mozilla (Firefox Web browser), and most documents support multiple descriptions, examples, and Hangul and are highly reliable.
If W3C`s web standard document is a textbook, it would be easy to understand MDN as if it were a guidebook.

Alternatively, the W3Schools website is also recommended. Unlike MDN, it is a site for profit (banner advertising, etc.) but it is well-organized for beginners to access. However, information is less reliable than MDN.

Additionally, speaking of very subjective quick learning,

HTML has a large number of tag types.
Don`t study too closely right now, just quickly go through what tags are and what roles they play.
And look for it when you need it!

CSS is very important for web app designers, publishers, and front-end developers.
There are many good UI frameworks, but CSS learning is essential to make the style you want.
Look at the role of each property and the values (default) you have.
In particular, please focus on studying `position`, `float`, and `flex` properties, and understand only the concept of `grid`.

JavaScript has a very wide range of learning.
There are many terms and skills that require background knowledge, so you can get tired quickly if you don`t study with ease.
There are many things that are not needed in practice right now, so it is recommended that you lightly look at the overall picture to draw a big picture, and optionally focus on what you need in practice.
You can`t handle all the possible exceptions in theory, so write your own code, write your own code, create your own practice, sub-projects, examples, and more!

Thank you for reading it to the end and I hope this post will help many beginners.

## Fast Campus Online Lecture[FO]

Fast Campus online lecture materials are fragmented and organized.
Please refer to the individual lecture materials separately from the online lecture.
The PPT materials that you can find in your personal course materials can be expanded by pressing [ESC

### [FO] Introduction to HTML/CSS

HTML recommended for beginners, CSS first step

### [FO] HTML

HTML Elements at a Glance (Elements)

### [FO] CSS

CSS Overview
CSS Properties
CSS3 Properties
Flexible Box (CSS) Complete
CSS Grid Complete Guide

### [FO] SCSS

Sass (SCSS) completely conquered!

### [FO] Markdown

Total Cleanup for MarkDown Usage

### [FO] VueJS

Vue Todo App Example
Vue Movie App Example
Test Vue Component Unit with Jest and Vue Test Utilities (VTU)

# References

https://namu.wiki/w/%EC%98%A4%ED%94%88%20%EC%86%8C%EC%8A%A4
http://www.bloter.net/archives/209318
https://opensource.org/licenses/
https://www.freeformatter.com/html-entities.html
https://wit.nts-corp.com/2013/10/16/280