# Asynchronous Programming

## Callback Hell

## Error Handling

## Promise

```js
new Promise( /* executor */ function(resolve, reject) { ... } );
```

**在 Promise 實作中，`executor` 函式在傳入參數 `resolve` 與 `reject` 後會立刻執行（`executor` 函式會在 Promise 建構式回傳 Promise 物件前被執行）。**

A `Promise` is in one of these states:

- *pending*: initial state, neither fulfilled nor rejected.
- *fulfilled*: meaning that the operation completed successfully.
- *rejected*: meaning that the operation failed.

A pending promise can either be *fulfilled* with a value, or *rejected* with a reason (error). When either of these options happens, the associated handlers queued up by a promise's `then` method are called. (If the promise has already been fulfilled or rejected when a corresponding handler is attached, the handler will be called, so there is no race condition between an asynchronous operation completing and its handlers being attached.)

The arguments to `then` are optional, and `catch(failureCallback)` is short for `then(null, failureCallback)`.

## Async Function

```js
async function name([param[, param[, ...param]]]) {
   statements
}
```

The **async function** declaration defines an **asynchronous function** — a function that returns an `AsyncFunction` object. Asynchronous functions operate in a separate order than the rest of the code via the `event loop`, returning an implicit `Promise` as its result. But the syntax and structure of code using async functions looks like standard synchronous functions.

An `async` function can contain an `await` expression that pauses the execution of the async function to wait for the passed `Promise`'s resolution, then resumes the `async` function's execution and evaluates as the resolved value.

**The `await` keyword is only valid inside `async` functions.** If you use it outside of an `async` function's body, you will get a `SyntaxError`.

Most async functions can also be written as regular functions using Promises. However, `async` functions are less tricky when it comes to error handling.

### Error-First Callbacks

## Middleware Pattern

[ASP.NET Core Middleware](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/middleware/index?view=aspnetcore-2.2)
