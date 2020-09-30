# Learn CSS

## Intro to CSS
  
The basic structure of every web page, HTML, is very plain on its own. The beautiful websites that you see across the internet are styled with a variety of tools, including CSS.

CSS, or Cascading Style Sheets, is a language that web developers use to style the HTML content on a web page. If you’re interested in modifying colors, font types, font sizes, shadows, images, element positioning, and more, CSS is the tool for the job!

## Inline Styles

Although CSS is a different language than HTML, it’s possible to write CSS code directly within HTML code using *inline styles*.

```html
<p style="color: red;">I'm learning to code!</p>
```

If you’d like to add *more* than one style with inline styles, simply keep adding to the `style` attribute. Make sure to end the styles with a semicolon (`;`).

```html
<p style="color: red; font-size: 20px;">I'm learning to code!</p>
```

## The `<style>` Tag

HTML allows you to write CSS code in its own dedicated section with the `<style>` element. CSS can be written between opening and closing `<style>` tags. To use the `<style>` element, it must be placed inside of the `<head>` element.

```html
<head>
  <style>
    p {
      color: red;
      font-size: 20px;
    }
  </style>
</head>
```

## The .css file

You can create a CSS file by using the .css file name extension, like so: style.css

With a CSS file, you can write all the CSS code needed to style a page without sacrificing the readability and maintainability of your HTML file.

## Linking the CSS File

Use the `<link>` element to link HTML and CSS files together. The `<link>` element must be placed within the head of the HTML file. It is a self-closing tag and requires the following three attributes:

1. `href` — like the anchor element, the value of this attribute must be the address, or path, to the CSS file.
2. `type` — this attribute describes the type of document that you are linking to (in this case, a CSS file). The value of this attribute should be set to `text/css`.
3. `rel` — this attribute describes the relationship between the HTML file and the CSS file. Because you are linking to a stylesheet, the value should be set to `stylesheet`.

```html
<link href="https://www.codecademy.com/stylesheets/style.css" type="text/css" rel="stylesheet">
```

```html
<link href="./style.css" type="text/css" rel="stylesheet">
```

## Tag Name

CSS can select HTML elements by using an element’s tag name. A tag name is the word (or character) between HTML angle brackets.

```html
p {

}
```

In the example above, all paragraph elements will be selected using a CSS *selector*. The selector in the example above is `p`. Note that the CSS selector matches the HTML tag for that element, but without the angle brackets.

In addition, two curly braces follow immediately after the selector (an opening and closing brace, respectively). Any CSS properties will go inside of the curly braces to style the selected elements.

## Class Name

CSS is not limited to selecting elements by tag name. HTML elements can have more than just a tag name; they can also have *attributes*. One common attribute is the `class` attribute. It’s also possible to select an element by its `class` attribute.

```html
<p class="brand">Sole Shoe Company</p>
```

```css
.brand {

}
```

To select an HTML element by its class using CSS, a period (`.`) must be prepended to the class’s name. In the example above case, the class is `brand`, so the CSS selector for it is `.brand`.

## Multiple Classes

It’s possible to add more than one class name to an HTML element’s `class` attribute.

```css
.green {
  color: green;
}

.bold {
  font-weight: bold;
}
```

```html
<h1 class="green bold"> ... </h1>
```

We can add multiple classes to an HTML element’s `class` attribute by separating them with a space. This enables us to mix and match CSS classes to create many unique styles without writing a custom class for every style combination needed.

## ID Name

If an HTML element needs to be styled uniquely (no matter what classes are applied to the element), we can add an ID to the element. Then, CSS can select HTML elements by their `id` attribute. To select an `id` element, CSS prepends the `id` name with a hashtag (`#`).

```html
<h1 id="large-title"> ... </h1>
```

```css
#large-title {

}
```

## Classes and IDs

CSS can select HTML elements by their tag, class, and ID. CSS classes and IDs have different purposes, which can affect which one you use to style HTML elements.

CSS classes are meant to be reused over many elements. By writing CSS classes, you can style elements in a variety of ways by mixing classes on HTML elements.

While classes are meant to be used many times, an ID is meant to style only one element. As we’ll learn in the next exercise, IDs override the styles of tags and classes. Since IDs override class and tag styles, they should be used sparingly and only on elements that need to always appear the same.

## Specificity

Specificity is the order by which the browser decides which CSS styles will be displayed. A best practice in CSS is to style elements while using the lowest degree of specificity, so that if an element needs a new style, it is easy to override.

IDs are the most specific selector in CSS, followed by classes, and finally, tags.

To make styles easy to edit, it’s best to style with a tag selector, if possible. If not, add a class selector. If that is not specific enough, then consider using an ID selector.

## Chaining Selectors

When writing CSS rules, it’s possible to require an HTML element to have two or more CSS selectors at the same time.

This is done by combining multiple selectors, which we will refer to as chaining. For instance, if there was a `.special` class for `h1` elements, the CSS would look like:

```css
h1.special {

}
```

## Nested Elements

```html
<ul class='main-list'>
  <li> ... </li>
  <li> ... </li>
  <li> ... </li>
</ul>
```

The nested <li> elements are selected with the following CSS(note the space in the selector):

```css
.main-list li {

}
```

Selecting elements in this way can make our selectors even more specific by making sure they appear in the context we expect.

We can target deeply nested child elements. While we can target arbitrarily nested elements with the *descendant selector*, in practice we should aim to keep our selectors short. As a very general rule of thumb, if our descendant selectors start to become greater than 3 levels deep we may want to consider ways to more specifically target the element in question. For example, perhaps we can throw a class on the targeted element instead of using a deeply nested selector.

## Chaining and Specificity

A chained or qualified selector has a higher specificity than a class selector but a lower specificity than the id selector

Adding more than one tag, class, or ID to a CSS selector increases the specificity of the CSS selector.

The order of specificity from highest to lowest is:

1. id selector (`#main`)
2. descendant selector *and* chained or qualified selector (`.main p` and `p.main`)

[Specificity Calculator](https://specificity.keegan.st/)

## Important

There is one thing that is even more specific than IDs: `!important`. `!important` can be applied to specific attributes instead of full rules. It will override any style no matter how specific it is. As a result, it should almost never be used. Once `!important` is used, it is very hard to override.

## Multiple Selectors

In order to make CSS more concise, it’s possible to add CSS styles to multiple CSS selectors all at once. This prevents writing repetitive code.

```css
h1, 
.menu {
  font-family: Georgia;
}
```

## Review CSS Selectors

Throughout this lesson, you learned how to select HTML elements with CSS and apply styles to them. Let’s review what you learned:

- CSS can change the look of HTML elements. In order to do this, CSS must select HTML elements, then apply styles to them.
- CSS caㄒn select HTML elements by tag, class, or ID.
- Multiple CSS classes can be applied to one HTML element.
- Classes can be reusable, while IDs can only be used once.
- IDs are more specific than classes, and classes are more specific than tags. That means IDs will override any styles from a class, and classes will override any styles from a tag selector.
- Multiple selectors can be chained together to select an element. This raises the specificity, but can be necessary.
- Nested elements can be selected by separating selectors with a space.
- The !important flag will override any style, however it should almost never be used, as it is extremely difficult to override.
- Multiple unrelated selectors can receive the same styles by separating the selector names with commas.