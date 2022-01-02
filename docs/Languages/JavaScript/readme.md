# JavaScript

- IIFE(Immediately Invoked Function Expression)
- Strict Mode
- Prototype
- Polyfill
- Transpiler
- minify/uglify

## Inheritance

- Pseudoclassical Inheritance & Prototypal Inheritance

## Closure

- this
- arrow function

## Scope

- function scope & block scope
- TDZ & hoisting

## ES6

- let, const
- Map/Set
- arrow function
- for-of
- class
- promise, async/await(ES7)
- symbol
- rest parameter/default parameter
- es6 module

## Bookmarklet

[How to Create a JavaScript Bookmarklet](http://www.dev-hq.net/posts/1--create-javascript-bookmarklet)

### 動態產生html tags

```js
document.open();
document.write();
document.close();
```

```js
iframe = document.createElement('iframe');
iframe.src = src;
document.appendChild(iframe);
```

[HTML5 Imports: Import HTML Files Into HTML Files](https://www.jotform.com/blog/html5-imports-import-html-files-into-html-files-83467/)
[Blob](https://developer.mozilla.org/zh-TW/docs/Web/API/Blob)

### 動態載入JS

```js
source = document.createElement('script');
source.async = true;
source.src = src;
```

### 與iframe互動

```js
iframe.contentWindow.postMessage(message, targetOrigin);
```

## Browser Support

[caniuse](https://caniuse.com/)

[ECMAScript 5 compatibility table](http://kangax.github.io/)

## Javascript Prototype Chain

[JavaScript's Pseudo Classical Inheritance diagram](https://kenneth-kin-lum.blogspot.com/2012/10/javascripts-pseudo-classical.html?showComment=1484288337339&source=post_page-----54102240a8b4----------------------#c1393503225616140233)
