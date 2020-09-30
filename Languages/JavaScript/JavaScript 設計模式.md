# JavaScript 設計模式

## 第一章：介紹

本書討論的四種模式：

- 設計模式(Design patterns)
- 編碼模式(Coding patterns)
- 反模式(Antipatterns)

四人幫的**設計模式**主要是用在強型別語言，利用了以class為基礎的繼承方式來解決某些問題，使用鬆散型別的動態語言JS來實踐這些模式沒甚麼道理，可能有更簡單的選擇。

**編碼模式**是指JS特有的模式，用語言獨有的功能實踐，是本書重點。

**反模式**不是bug，他指的是糟糕的實踐方式。

## 第二章： 精要

在`var`宣告中連續賦值，a是區域變數，b卻是全域變數。

```js
//antipattern
funciton foo() {
    var a = b = 0;
    // ...
}
```

cache length模式，當myarray是HTMLCollection時效果更好，因為這類物件每次存取集合長度時都是直接查詢DOM。

```js
for (var i = 0, max = myarray.length; i < max; i++) {
    // do something with myarray[i]
}
//更好的var單一模式(可是code更醜)
function looper() {
    var i = 0,
        max,
        myarray = [];
    for (i = 0, max = myarray.length; i < max; i++) {
        // do something with myarray[i]
    }
}
```

for-in pattern

```js
for (const key in object) {
  if (object.hasOwnProperty(key)) {
    const element = object[key];
    
  }
}
```