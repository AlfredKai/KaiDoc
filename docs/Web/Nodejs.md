# Nodejs

[Express web framework (Node.js/JavaScript)](https://developer.mozilla.org/zh-TW/docs/Learn/Server-side/Express_Nodejs)

[10 NodeJs Things You Should Know & Master to be a Pro](https://beforesemicolon.medium.com/10-nodejs-things-you-should-know-master-to-be-a-pro-bdfdb5620c0b)

## Pros and Cons

[The Good and the Bad of Node.js Web App Development](https://www.altexsoft.com/blog/engineering/the-good-and-the-bad-of-node-js-web-app-development/)

## 計時

```js
console.time('timer name')
console.timeEnd('timer name')
```

## /usr/bin/env

[What exactly does “/usr/bin/env node” do at the beginning of node files?](https://stackoverflow.com/questions/33509816/what-exactly-does-usr-bin-env-node-do-at-the-beginning-of-node-files)

## ./bin/www

[What does “./bin/www” do in Express 4.x?](https://stackoverflow.com/questions/23169941/what-does-bin-www-do-in-express-4-x)

## race condition

## Worker Threads

[試玩一下 Nodejs 10.5.0 Worker Threads](https://medium.com/hybrid-maker/%E8%A9%A6%E7%8E%A9%E4%B8%80%E4%B8%8B-nodejs-10-5-0-worker-threads-a6ac7c6dfb8a)

## Event loop

[Event loop](https://en.wikipedia.org/wiki/Event_loop)

[Concurrency model and the event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)

[The Node.js Event Loop, Timers, and process.nextTick()](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)

### "Run-to-completion"

Each message is processed completely before any other message is processed.

This offers some nice properties when reasoning about your program, including the fact that whenever a function runs, it cannot be pre-empted and will run entirely before any other code runs (and can modify data the function manipulates). This differs from C, for instance, where if a function runs in a thread, it may be stopped at any point by the runtime system to run some other code in another thread.

A downside of this model is that if a message takes too long to complete, the web application is unable to process user interactions like click or scroll. The browser mitigates this with the "a script is taking too long to run" dialog. A good practice to follow is to make message processing short and if possible cut down one message into several messages.

## Blocking vs Non-Blocking

[Blocking vs Non-Blocking](https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/)

[Asynchronous I/O](https://en.wikipedia.org/wiki/Asynchronous_I/O)

## Error handling

>In the synchronous if an error is thrown it will need to be caught or the process will crash. In the asynchronous, it is up to the author to decide whether an error should throw as shown.

[error conventions](https://nodejs.org/en/knowledge/errors/what-are-the-error-conventions/)

## Testing

[A testing guide for Express with request and response mocking/stubbing using Jest or sinon](https://codewithhugo.com/express-request-response-mocking/)

## 其他主題

挑戰瀏覽器極限  
[Chrome Experiments](https://experiments.withgoogle.com/collection/chrome)

JS Linux VM  
[jslinux](https://bellard.org/jslinux/)

[The C10K problem](http://www.kegel.com/c10k.html)

DIRTy的好範例  
[Browserling](https://www.browserling.com/)

data intensive real-time applications (dirt)

[Node.js 開發之父：「十個Node.js 的設計錯誤」－ 以及其終極解決辦法](https://m.oursky.com/node-js-%E9%96%8B%E7%99%BC%E4%B9%8B%E7%88%B6-%E5%8D%81%E5%80%8Bnode-js-%E7%9A%84%E8%A8%AD%E8%A8%88%E9%8C%AF%E8%AA%A4-%E4%BB%A5%E5%8F%8A%E5%85%B6%E7%B5%82%E6%A5%B5%E8%A7%A3%E6%B1%BA%E8%BE%A6%E6%B3%95-f0db0afb496e)
