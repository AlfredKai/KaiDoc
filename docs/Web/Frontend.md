# Frontend

## Package Management

### NPM

從package.json先從這裡開始看

初始

```bash
npm init
```

還原

```bash
npm install
```

## Javascript

- IIFE(Immediately Invoked Function Expression)
- Strict Mode
- Prototype
- Hoisting
- Polyfill
- Transpiler
- Arrow Function
- let, const, var
- CDN
- uglify
- feautify

## Roadmap

- HTML
  - Emmet(寫html)
  - Pug(切版)
- CSS
- JavaScript
- 相容性(jQuery)
- CSS preprocessor，常用的：SCSS/SASS、Less 跟 Stylus)
- mixin, PostCSS Autoprefixer
- browserify(var A = require(‘libraryA’))
- Gulp(用程式碼來管理你的 workflow)
- babel
- webpack
- 在學 React 以前，只希望你記得 React 的這個核心概念就好：只改變 state 就好，UI 就會自動跟著改變。

這個概念就叫做 SPA，全名是 Single Page Application，單頁式應用。與之對應的概念是 MPA，Multiple Page Application。
CSR vs SSR

MVC 就是因為 code 變得越來越亂，所以將職責區分清楚的一種設計模式。SPA 就是因為想增進使用者體驗，而出現的一種在前端利用 Ajax 達成不換頁的方法。SSR 就是因為要解決 SPA 的 SEO 問題而出現的解法。

react: virtual dom為什麼可以搜這麼快

bookmarklet

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

### 判斷存不存在

```js
array.indexOf(x) > -1
```

## Browser Support

[caniuse](https://caniuse.com/)

[ECMAScript 5 compatibility table](http://kangax.github.io/)

## Testing

- Karma

## Javascript Prototype Chain

[JavaScript's Pseudo Classical Inheritance diagram](https://kenneth-kin-lum.blogspot.com/2012/10/javascripts-pseudo-classical.html?showComment=1484288337339&source=post_page-----54102240a8b4----------------------#c1393503225616140233)
