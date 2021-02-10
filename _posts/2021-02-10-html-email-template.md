---
layout: post
title: "Create HTML Email Template"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/html5.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/html5.png)

We have to comply with the standard when writing HTML and CSS for the web.
Standard is a rule that everyone must follow, so it can serve as a writing guide.
However, there is no standard for email.
You need to consider the possibility of rendering many clients, and depending on the nature of the email (services) you provide, the number of clients you need to consider may increase or decrease.

# HTML Email Template

Considering the variety of rendering possibilities, it is difficult to digest email templates with standard HTML and CSS writing methods, as concepts often used in standard coding such as `div` tags or `margin` properties are not available or depend on `table` and `td` tags in almost every part.

We recommend that you consider sub-compatibility for email templates.
It would be especially helpful if you are familiar with coding using `<table>.

## Email Client

Let`s look at a list of the most common email clients to consider when creating HTML email templates.

### Mobile Email Client

Android 2.3 및 4.0
iPhone 5 iOS 6
iPhone 4S iOS 6
iPhone 3GS iOS 5
iPad 2 iOS 6
BlackBerry OS 4

### Desktop Email Client

Apple Mail 4, 5, 6
Lotus Notes 8.5
Lotus Notes 8
Thunderbird
Windows Live Mail
Outlook 2013 (v15)
Mac 용 Outlook 2011
Outlook 2010 (v14)
Outlook 2007 (v12)
Outlook 2003 (v11)
Outlook 2002 / XP (v10)
Outlook 2000 (v9)

### Webmail clients

AOL Mail (on any browser)
Gmail (on any browser)
Outlook.com (on any browser)
Yahoo! (on any browser)

## Getting ready

### DOCTYPE

Doctype informs email clients of HTML types and allows them to perform HTML quality checks using tools such as W3C Validator.
The XHTML 1.0 Transitional doctype helps email clients validate and render emails in a reliable manner.

Some email clients replace the Doctype they provide with their Doctype.

```xml
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">

</html>

```

### Head

Sets the character encoding scheme (`UTF-8`) to ensure that text and special characters are displayed correctly.
Because Doctype is set to XHTML, you must include a `Content-Type` declaration.

Additionally, set for title, viewport of the device.
XHTML must close the empty tag (`/`).(`<link/>, `img/`, `br/`)

```xml
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<title>HTML Email Template</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
</head>

```

### Body

Some email clients remove `<body>`.
Therefore, create a `table` with a container that will be used instead of `<body>` on the outermost side.
Add `id="bodyTable" for Style application.

With the width of the email template, 600px is the safest maximum width horizontal, optimized for email clients with varying resolutions.
It is not recommended to exceed the practical upper limit of 800px.

```xml
<body>
<!-- OUTERMOST CONTAINER TABLE -->
<table border="0" cellpadding="0" cellspacing="0" width="100%" id="bodyTable">
<tr>
<td>

<!-- 600px - 800px CONTENTS CONTAINER TABLE -->
<table border="0" cellpadding="0" cellspacing="0" width="600">
<tr>
<td>

</td>
</tr>
</table>



</td>
</tr>
</table>


</body>

```

### Style

We recommend inline creation, but for convenience, some styles can be written and included as `style` or moved to inline creation later.
If NPM is available, it is easier to write with a CSS file connected to `link` or `style`, and a library such as online-mail can be easily merged in-line later.

```xml
<style type="text/css">
/* GENERAL STYLE RESETS */
body, #bodyTable { width:100% !important; height:100% !important; margin:0; padding:0; }
#bodyTable { padding: 20px 0 30px 0; background-color: #ffffff; }
img, a img { border:0; outline:none; text-decoration:none; }
.imageFix { display:block; }
table, td { border-collapse:collapse; }
</style>

```

You can add some rules to a client convention to correct some of the disadvantages:

> The following rules must be included separately prior to use in case of future inline mergers:

```xml
<style type="text/css">
/* CLIENT-SPECIFIC RESETS */
/* Allow Outlook.com (Hotmail) full width and appropriate line height */
.ReadMsgBody{ width: 100%; }
.ExternalClass{ width: 100%; }
.ExternalClass, .ExternalClass p, .ExternalClass span, .ExternalClass font, .ExternalClass td, .ExternalClass div { line-height: 100%; }
/* Remove spacing around tables Outlook adds */ in Outlook 2007 or later
table, td { mso-table-lspace: 0pt; mso-table-rspace: 0pt; }
/* Modify how Internet Explorer renders resized images */
img { -ms-interpolation-mode: bicubic; }
/* Modify Webkit and Windows-based clients to automatically resize text */
body, table, td, p, a, li, blockquote { -ms-text-size-adjust: 100%; -webkit-text-size-adjust: 100%; }
</style>

```

## To create

### table, tr, td

To provide the structure of your email normally, you must write `table` instead of `div`.
This method, commonly referred to as `TABLE coding`, uses a lot of overlapping tables, so it is recommended that you style it after the label-up of the table structure is completed.

> It is difficult to modify, so it is better to work after the layout is confirmed.

It is better to avoid other tags related to `table` such as `the body`, `tbody`, and `colgroup`.
The lightest and safest method is to use only the three tags: `table`, `tr`, and `td`.

### HTML Properties and Inline Styles

Some email clients do not remove `<head>` or properly support `<style>.
Therefore, it is recommended that you structure the table with HTML properties.

In particular, it is recommended that `<table>` be initialized as follows:

```undefined
<table border="0" cellpadding="0" cellspacing="0" width="100%"></table>



```

<table><thead><tr><th>property</th><th>value</th><th>meaning</th></tr></thead><tbody><tr><td>
 border</td><td><code>`1`</code><br><code>`0`</code></td><td>Presence of line</td></tr><tr
>
<td>cellpadding</td><td>Pixels</td><td>Inner margin of cell (td)</td></tr><tr><td>cellspacing</td><td>Pixels
 </td><td>Width between cells</td></tr><tr><td>width</td><td>Pixels<br><code>`%`</code></
td><td>Width of table</td></tr></tbody></table>


 

<table><thead><tr><th>property</th><th>value</th><th>meaning</th></tr></thead><tbody><tr><td>
 align</td><td><code>`left`</code><br><code>`right`</code><br><code>`center`</code><br><code>
 `justify`</code><br><code>`char`</code></td><td>Align the contents of the cell horizontally</td></tr><tr><td>valign</
td><td><code>`top`</code><br><code>`middle`</code><br><code>`bottom`</code><br><code>`baseline`
 </code></td><td>Vertical alignment of the cell contents</td></tr><tr><td>bgcolor</td><td>HEX Colors</td><td>
 Color(ex&gt; <code>`#ffffff`</code>)</td></tr><tr><td>width</td><td>Pixels<br><code>`%`</
 code></td><td>Horizontal width of cell</td></tr><tr><td>height</td><td>Pixels<br><code>`%`</code><
 /td><td>cell's vertical width</td></tr></tbody></table>


 

Inline style refers to creating a style with HTML attributes, such as `<td style="color: #ff0000;">".
This is useful and recommended if you do not support `<style> properly.

> Because inline styles have high style priorities, they are often overwritten when mixed with '<style>' or external CSS files.

### Nested Table

In many cases, the `colspan` and `rowspan` attributes are not supported.
Therefore, you should avoid merging (Merge) cells as follows:

![image](https://heropy.blog/images/screenshot/html_email_template_table_colspan.jpg)

```undefined
<table border="0" cellpadding="0" cellspacing="0" width="100%">
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td colspan="2"></td>
</tr>
<tr>
<td colspan="3"></td>
</tr>
</table>



```

You can nest tables to create the same effect as merged.
It`s more complex, but it`s safely rendered on almost every email client.

![image](https://heropy.blog/images/screenshot/html_email_template_table_nested.jpg)

```xml
<table border="" cellpadding="0" cellspacing="0" width="100%">
<tr>
<td>
<table border="" cellpadding="0" cellspacing="0" width="100%">
<tr>
<td></td>
<td></td>
<td></td>
</tr>
</table>


</td>
</tr>
<tr>
<td>
<table border="" cellpadding="0" cellspacing="0" width="100%">
<tr>
<td></td>
<td></td>
</tr>
</table>


</td>
</tr>
<tr>
<td>
<table border="" cellpadding="0" cellspacing="0" width="100%">
<tr>
<td></td>
</tr>
</table>


</td>
</tr>
</table>



```

### Color

For compatibility, use Hexadecimal Colors, which write like `#ffffff`.
Colors such as RGB, RGBA, and HSV are not supported by all email clients.

> Be careful not to use abbreviations such as '#fff'.

Use HTML `bgcolor` properties rather than CSS `background` properties.

```xml
<td bgcolor="#ff0000"></td>

```

### Single Class

Do not write multiple values of the `class` property.
You must create one single value.

```xml
<!-- MULTIPLE VALUES -->
<td class="table-data description bold"></td>

<!-- SINGLE VALUE -->
<td class="description"></td>

```

### CSS Properties

Do not use CSS shortening properties as follows.

```css
td {
font: 16px / 1.4 Arial, sans-serif;
}

```

Use individual properties.

```css
td {
font-size: 16px;
line-height: 1.4;
font-family: Arial, sans-serif;
}

```

### Image

You can use the image in HTML email, but there are a few caveats.

- Use absolute path
- Keep capacity below 250kb
- Enter width/length width (width, height)
- Enter alternate text (alt)

Please enter `width`, `height`, and `alt` properties for some email clients that remove images.

```xml
<img src="http://via.placeholder.com/200x100" alt="Some image" width="200" height="100">

```

In many cases, `<table>` is not suitable for reactive layouts, so it is written with a fixed layout, and some email clients are modified to display the width of the device regardless of the width specified.
In this case, you can fix the problem by inserting images of the same width as the specified width.

This is simply used to maintain the width of the specified table, so it is not displayed on the screen as follows:

```undefined
<td style="font-size: 0; line-height: 0; height: 0;" height="0">
<img alt="" src="http://via.placeholder.com/600x1" style="display: block;" width="600" height="0"/>
</td>

```

The following email layout was identified by Desktop Gmail (Chrome):

![image](https://heropy.blog/images/screenshot/html_email_template_desktop.jpg)

The layout identified by Mobile Gmail (Android) has changed.
If the result is not intended, there may be problems such as changing the text line or arbitrarily adjusting the width of each cell.

![image](https://heropy.blog/images/screenshot/html_email_template_mobile.jpg)

When you insert a fixed image, it can be displayed the same as the layout you saw on Desktop.
However, you should consider the overall screen reduction.

> If it was based on 600px, the overall screen reduction would not be a problem.

![image](https://heropy.blog/images/screenshot/html_email_template_mobile_use_to_fixed_image.jpg)

### Margins

`margin` of CSS is not available.
Instead, you can create margins with the width of the cell and the `padding`.

When using `padding`, you must complete the top, bottom, left, and right values.

```xml
<td style="padding: 0 0 30px 0"></td>

```

It may be a little uncomfortable, but in fact, the safest way is to use individual attributes as follows:

```xml
<td style="padding-top: 0; padding-right: 0; padding-bottom: 30px; padding-left: 0;"></td>

```

If you use SCSS as a CSS Pre-processor, you can write it conveniently using the nested properties.

```undefined
td {
padding: {
top: 0;
right: 0;
bottom: 30px;
left: 0;
}
}

```

When utilizing external margins other than internal margins, add empty cells to the margin location (to be used as margins).
Empty cells are blank characters (`)

```xml
<!-- HORIZONTAL MARGIN 30px -->
<td width="30" style="font-size: 0; line-height: 0;">
```

### Text

It is safer to use it with tags that contain styles such as `font`, `b`, `i`, and `u`.

```xml
<style type="text/css">
.bold {
font-weight: bold;
}
</style>
<td>
HTML <b class="bold">email</b> template
</td>

```

Combining style with `<font>` is the best way to ensure that the link`s primary color, blue, never appears.

```xml
<td>
<a href="https://google.com" target="_blank" style="color: #ff0000;"><font color="#ff0000">GOOGLE</font></a>
</td>

```

But `p`, `h1`, `h2`... Do not use the same paragraph, title tag.
This is rendered without style consistency across email clients and is very difficult to modify.

> In most cases, you can write '<td>.

### Conditional Comments

The keyword `mso` (Microsoft Outlook) can be used for conditional annotations.

```xml
<td>
<!--[if mso]>
OUTLOOK CONTENTS
<![endif]-->
<!--[if !mso]>
NON-OUTLOOK CONTENTS
<![endif]-->
<!--[if (gte mso 9)|(IE)]>
GREATER THAN EQUAL OUTLOOK 9 or INTERNET EXPLORER
<![endif]-->
</td>

```

- Outlook 2000: Version 9
- Outlook 2002: Version 10
- Outlook 2003: Version 11
- Outlook 2007: Version 12
- Outlook 2010: Version 14
- Outlook 2013: Version 15

<table><thead><tr><th style="text-align:center">symbol</th><th style="text-align:center">meaning</th><th style="text- align:center">example</th><th style="text-align:center">example interpretation</th></tr></thead><tbody><tr><td style="text-align :center"><code>`!`</code></td><td style="text-align:center">negative<br>(not)</td><td style="text-align: center"><code>`&lt;!--[if !IE]&gt;&lt;![endif]--&gt;`</code></td><td style="text-align:center"> Without IE browser</td></tr><tr><td style="text-align:center"><code>`lt`</code></td><td style="text-align :center">Small, less than<br>(less than)</td><td style="text-align:center"><code>`&lt;!--[if lt IE 9]&gt;&lt;! [endif]--&gt;`</code></td><td style="text-align:center">Under IE9</td></tr><tr><td style="text-align: center"><code>`lte`</code></td><td style="text-align:center">Less than or equal to, less than <br>(less than equal)</td><td style= "text-align:center"><code>`&lt;!--[if lte IE 8]&gt;&lt;![endif]--&gt;`</code></td><td style="text -align:center">IE8 or less</td></tr><tr><td style="text-align:center"><code>`gt`</code></td><td st yle="text-align:center">greater than <br>(greater than)</td><td style="text-align:center"><code>`&lt;!--[if gt IE 6 ]&gt;&lt;![endif]--&gt;`</code></td><td style="text-align:center">IE6 exceeded</td></tr><tr><td style ="text-align:center"><code>`gte`</code></td><td style="text-align:center">greater than equal<br>(greater than equal)</td><td style="text-align:center"><code>`&lt;!--[if gte IE 7]&gt;&lt;![endif]--&gt;`</code></td> <td style="text-align:center">IE7 or later</td></tr><tr><td style="text-align:center"><code>`&amp;`</code></td><td style="text-align:center">and<br>(and)</td><td style="text-align:center"><code>`&lt;!--[if (gt IE 6) &amp; (lte IE 9)]&gt;&lt;![endif]--&gt;`</code></td><td style="text-align:center">Over IE6 ~ Below IE9</td></ tr><tr><td style="text-align:center"><code>`|`</code></td><td style="text-align:center"> or <br>(or) </td><td style="text-align:center">-</td><td style="text-align:center">-</td></tr></tbody></table>


 

### Bulletproof Buttons

In many cases, an image button has been used for tables.
However, the Image button creates a critical issue where the link does not work when the email client removes the use of the image.
In this case, there is a solution through Microsoft Vector Markup Language (VML).

```xml
<td>
<!--[if mso]>
<v:roundrect xmlns:v="urn:schemas-microsoft-com:vml" xmlns:w="urn:schemas-microsoft-com:office:word" href="https://google.com" style="height: 40px; v-text-anchor: middle; width:200px;
<w:anchorlock/>
<center style="color:#147e94;font-family:sans-serif;font-size:13px;font-weight:bold;">Button</center>
</v:roundrect>
<![endif]-->
<a href="https://google.com"
style="background-color: #2bcae3; border: 1px solid #1caeba; border-radius: 20px; color: #147e94; display: inline-block; font-family: sans-serif; font-size: 13px; font-weight
</td>

```

There is an easier way to consider sub-compatibility.
However, the entire area of the button cannot be set to a link range.

```xml
<td bgcolor="#2bcae3" align="center" width="200" style="border: 1px solid #1caeba; border-radius: 20px; -webkit-text-size-adjust: none;">
<a href="https://google.com" style="color: #147e94; font-family: sans-serif; font-size: 13px; font-weight: bold; line-height: 40px; text-decoration: none;"><font color="#147e94">Button</
</td>

```

![image](https://heropy.blog/images/screenshot/html_email_template_bulletproof_button.jpg)

## Verification

Examine the document you worked on in the W3C Validator.
If you are not familiar with XHTML, you should especially look at the test results.

# References

https://templates.mailchimp.com/development/html/
https://webdesign.tutsplus.com/tutorials/what-you-should-know-about-html-email--webdesign-12908
https://www.campaignmonitor.com/css/text-fonts/font-face/
https://litmus.com/community/learning/13-foundations-email-coding-101