# asyncio

一直以來python在web方面都被nodejs屌打，為什麼multi-thread會被single-thread屌打？當然是因為GIL，有了GIL你沒了multi-thread的好處還加上要context switch，當然被屌打。

nodejs怎麼達成single-thread非同步的？靠的是non-blocking I/O，在nodejs裡面有一個東西叫event loop，當nodejs執行遇到需要I/O的時候，nodejs的main thread會把任務交給kernel處理然後自己去做別的事情，當執行完一些callbacks再回來檢查I/O任務完成沒，而kernel，當然就是multi-thread。這樣做有兩個好處，第一因為是single-thread效能極佳，第二因為執行以callback為單位，不會有race condition(其實硬要搞還是搞得出來，就不討論了)，缺點是你有需要消耗大量CPU資源的callback時會直接卡死event loop。

情況一直到python 3.5有了改善，python迎來了與nodejs相同機制的event loop，比較新的python框架如fastapi靠著async性能屌打flask，並號稱某些情況甚至不輸GO。自此python終於可以與nodejs平起平坐。

lane  7 hours ago
async/await 在 C# 是 task (比 thread 還要小)
task 是由 task engine 執行, task engine 一般而言是 threadpool.
所以嚴格來說介於 eventloop , thread 之間

## Refs

[Nodejs Event Loop](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)

[FastAPI、Flask、Golang性能测试](https://studygolang.com/articles/25842?fbclid=IwAR1FdYhfrujtowf50npJRrQNN8BA5-hDGf-2yJfog7LCZRk1KgP3qNTiF6A)

[Async IO in Python: A Complete Walkthrough](https://realpython.com/async-io-python/)

[理解 Python asyncio](https://lotabout.me/2017/understand-python-asyncio/)

[async await 學習筆記](https://medium.com/%E9%AB%92%E6%A1%B6%E5%AD%90/aysnc-await-%E6%95%99%E5%AD%B8%E7%AD%86%E8%A8%98-debabdb9db0e)