# Introduction to HTML

## What is HTML?

HTML stands for HyperText Markup Language:

- A *markup language* is a computer language that defines the structure and presentation of raw text.
- In HTML, the computer can interpret raw text that is wrapped in HTML elements.
- *HyperText* is text displayed on a computer or device that provides access to other text through links, also known as *hyperlinks*. You probably clicked on a couple of hyperlinks on your way to this Codecademy course.

## HTML Anatomy

HTML is composed of *elements*. These elements structure the webpage and define its content. Let’s take a look at how they’re written.

```html
<p>Hello Wrold</p>
```
- **HTML element** (or simply, element) — a unit of content in an HTML document formed by HTML tags and the text or media it contains.
- **HTML Tag** — the element name, surrounded by an opening (<) and closing (>) angle bracket.
- **Opening Tag** — the first HTML tag used to start an HTML element. The tag type is surrounded by opening and closing angle brackets.
- **Content** — The information (text or other elements) contained between the opening and closing tags of an HTML element.
- **Closing tag** — the second HTML tag used to end an HTML element. Closing tags have a forward slash (/) inside of them, directly after the left angle bracket.

## The Body

One of the key HTML elements we use to build a webpage is the *body* element. Only content inside the opening and closing body tags can be displayed to the screen.

```html
<body>
  <p>What's up, doc?</p>
</body>
```

## HTML Structure

HTML is organized as a collection of family tree relationships. When an element is contained inside another element, it is considered the *child* of that element. The child element is said to be *nested* inside of the *parent* element.

```html
<body>
  <p>This paragraph is a child of the body</p>
</body>
```

Since there can be multiple levels of nesting, this analogy can be extended to grandchildren, great-grandchildren, and beyond. The relationship between elements and their ancestor and descendent elements is known as *hierarchy*.

```html
<body>
  <div>
    <h1>Sibling to p, but also grandchild of body</h1>
    <p>Sibling to h1, but also grandchild of body</p>
  </div>
</body>
```

The `<html>` tag is the root element of every page and it should always have `<head>` and `<body>` child elements. The `<head>` and `<body>` elements are siblings to each other and children to the `<html>` parent.

Understanding HTML hierarchy is important because child elements can inherit behavior and styling from their parent element. You’ll learn more about webpage hierarchy when you start digging into CSS.

## Headings

In HTML, there are six different *headings*, or *heading elements*. Headings can be used for a variety of purposes, like titling sections, articles, or other forms of content.

The following is the list of heading elements available in HTML. They are ordered from largest to smallest in size.

1. `<h1>` — used for main headings. All other smaller headings are used for subheadings.
2. `<h2>`
3. `<h3>`
4. `<h4>`
5. `<h5>`
6. `<h6>`

## Divs

`<div>` is short for “division” or a container that divides the page into sections. These sections are very useful for grouping elements in your HTML together.

## Attributes

If we want to expand an element’s tag, we can do so using an *attribute*. Attributes are content added to the opening tag of an element and can be used in several different ways, from providing information to changing styling. Attributes are made up of the following two parts:

- The *name* of the attribute
- The *value* of the attribute

```html
<div id="intro">
  <h1>Introduction</h1>
</div>
```

[Attribute List](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes)  
[Global Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes)

## Displaying Text

If you want to display text in HTML, you can use a *paragraph* or *span*:

- *Paragraphs* (`<p>`) contain a block of plain text.
`<span>` contains short pieces of text or other HTML. They are used to separate small pieces of content that are on the same line as other content.

```html
<div>
  <h1>Technology</h1>
</div>
<div>
  <p><span>Self-driving cars</span> are anticipated to replace up to 2 million jobs over the next two decades.</p>
</div>
```

It’s best to use a <span> element when you want to target a specific piece of content that is *inline*, or on the same line as other text. If you want to divide your content into *blocks*, it’s better to use a <div>.

## Styling Text

You can also style text using HTML tags. The `<em>` tag emphasizes text, while the `<strong>` tag highlights important text.

- The `<em>` tag will generally render as *italic* emphasis.
- The `<strong>` will generally render as **bold** emphasis.

Browsers have built-in style sheets that will generally style these tags.

```html
<p><strong>The Nile River</strong> is the <em>longest</em> river in the world, measuring over 6,850 kilometers long (approximately 4,260 miles).</p>
```

What is a style sheet?

Within the context of web development, a style sheet is a CSS document which specifies the presentation of the content described by associated HTML.

In other words, style sheets style our web pages!

## Line Breaks

The spacing between code in an HTML file doesn’t affect the positioning of elements in the browser. If you are interested in modifying the spacing in the browser, you can use HTML’s *line break* element: `<br>`.

The line break element is unique because it is only composed of a starting tag. You can use it anywhere within your HTML code and a line break will be shown in the browser.

 Both `<br>` and `<br/>` are valid syntaxes, choose what you want.

## Unordered Lists

In HTML, you can use an *unordered list* tag (`<ul>`) to create a list of items in no particular order. An unordered list outlines individual *list items* with a bullet point.

The `<ul>` element should not hold raw text and won’t automatically format raw text into an unordered list of items. Individual list items must be added to the unordered list using the `<li>` tag. The `<li>` or list item tag is used to describe an item in a list.

```html
<ul>
  <li>Limes</li>
  <li>Tortillas</li>
  <li>Chicken</li>
</ul>
```

The `<li>` element can be a direct child of either the `<ul>`or the `<ol>` elements but it should never exist outside of either of these parent elements.

## Ordered Lists

*Ordered lists* (`<ol>`) are like unordered lists, except that each list item is numbered. They are useful when you need to list different steps in a process or rank items for first to last.

The `<li>` element can contain any element which is valid within the `<body>` tag.

## Images

The `<img>` tag allows you to add an image to a web page. Most elements require both opening and closing tags, but the `<img>` tag is a *self-closing* tag. Note that the end of the `<img>` tag has a forward slash /. Self-closing tags may include or omit the final slash — both will render properly.

The important distinction between self-closing tags and all other tags is that self-closing tags represent void elements. Void elements like img and br cannot contain any content. All other tags may (but are not required to) contain content.

```html
<img src="image-location.jpg" />
```

The `<img>` tag has a required *attribute* called `src`. The `src` attribute must be set to the image’s *source*, or the location of the image. In this case, the value of `src` must be the *uniform resource locator* (URL) of the image. A URL is the web address or local address where a file is stored.

## Image Alts

The `alt` attribute, which means alternative text, brings meaning to the images on our sites. The `alt` attribute can be added to the image tag just like the `src` attribute. The value of `alt` should be a description of the image.

```html
<img src="#" alt="A field of yellow sunflowers" />
```

The `alt` attribute also serves the following purposes:

- If an image fails to load on a web page, a user can mouse over the area originally intended for the image and read a brief description of the image. This is made possible by the description you provide in the `alt` attribute.
- Visually impaired users often browse the web with the aid of screen reading software. When you include the `alt` attribute, the screen reading software can read the image’s description out loud to the visually impaired user.
- The `alt` attribute also plays a role in Search Engine Optimization (SEO), because search engines cannot “see” the images on websites as they crawl the internet. Having descriptive `alt` attributes can improve the ranking of your site.

## Videos

In addition to images, HTML also supports displaying videos. Like the `<img>` tag, the `<video>` tag requires a src attribute with a link to the video source. Unlike the `<img>` tag however, the `<video>` element requires an opening and a closing tag.

```html
<video src="myVideo.mp4" width="320" height="240" controls>
  Video not supported
</video>
```

In this example, the video source (`src`) is `myVideo.mp4` The source can be a video file that is hosted alongside your webpage, or a URL that points to a video file hosted on another webpage.

After the `src` attribute, the `width` and `height` attributes are used to set the size of the video displayed in the browser. The `controls` attribute instructs the browser to include basic video controls: pause, play and skip.

The text, “Video not supported”, between the opening and closing video tags will only be displayed if the browser is unable to load the video.

 The controls attribute is an example of a “Boolean attribute”. Boolean attributes have true/false conditions. Including a boolean attribute within an element signifies the condition is true (or on) while omitting it signifies the condition is false (or off).

## Review

Congratulations on completing the first lesson of HTML! You are well on your way to becoming a skilled web developer.

Let’s review what you’ve learned so far:

1. **HTML** stands for **H**yper**T**ext **M**arkup **L**anguage and is used to create the structure and content of a webpage.
2. Most HTML elements contain opening and closing tags with raw text or other HTML tags between them.
3. HTML elements can be nested inside other elements. The enclosed element is the child of the enclosing parent element.
4. Any visible content should be placed within the opening and closing `<body>` tags .
5. Headings and sub-headings, `<h1>` to `<h6>` tags, are used to enlarge text.
6. `<p>`, `<span>` and `<div>` tags specify text or blocks.
7. The `<em>` and `<strong>` tags are used to emphasize text.
8. Line breaks are created with the `<br>` tag.
9. Ordered lists (`<ol>`) are numbered and unordered lists (`<ul>`) are bulleted.
10. Images (`<img>`) and videos (`<video>`) can be added by linking to an existing source.

# HTML Document Standards