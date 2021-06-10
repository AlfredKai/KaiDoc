# The Global Interpreter Lock

## Refs

[PyCon 2015 - Python's Infamous GIL by Larry Hastings](https://www.youtube.com/watch?v=KVKufdTphKs)

[Python的GIL是什么鬼，多线程性能究竟如何](http://cenalulu.github.io/python/gil-in-python/)

[What Is the Python Global Interpreter Lock (GIL)?](https://realpython.com/python-gil/)

[CPython may not perform as well as Jython when using threads](https://docs.gunicorn.org/en/latest/design.html#how-many-threads)

## What is GIL

GIL就是一把全域大鎖，上古時代從單核心CPU進步到現在多核心CPU而開始有了thread的需求，要防止race condition最基本的做法就是加鎖，但為什麼是全域大鎖呢？因為這樣不會dead lock，一切都是為了簡單。

GIL並不是python的特性，他是CPython的實做，Jython就沒有GIL。

## Effect of GIL

GIL的作用就是只有擁有這把大鎖的thread可以執行bytecode，甚至[python dict靠著這把大鎖而thread-safe](https://docs.python.org/3/glossary.html#term-global-interpreter-lock)，其影響就是你的multi-thread跟single-thread沒兩樣，舊版本的GIL實作爛到甚至會讓I/O bound task starvation，因為釋放鎖跟取得鎖寫中間沒任何等待機制，導致CPU bound task一釋放馬上又取得，python的multi-thread只剩下似有似無的非同步功能。在後續的版本中GIL有一直在改進，現在除了會在I/O的時候釋放以外，也會在一個固定時間查看是否有其他thread需要而釋放。

## Why Can't We Remove GIL

CPython和其他現代語言不同，他擁有直接對C下指令的C api功能導致他需要直接面對GC問題(現代語言如java的stop the world，擁有檢查並清除當前已經無人使用的物件的pure GC機制)，而CPython處理GC的物件reference count就是靠GIL在保護，這個根本的問題導致要移除GIL極為困難，所以即使在python 3他還是存在。

現代語言不管是GO、Rust都是使用自己的GC，如果需要C模組你必須用單純的C來寫，並在原本的環境呼叫你的C模組各自處理各自的GC，這是目前被認為比較正確的做法。

## Why Don't We Just Use Jython

[ref](https://www.quora.com/Why-dont-more-people-use-Jython)

Yes you can. Jython is suitable for:

1. Django and Flask frameworks.
2. Scrapping libraries like Beutifullsoap, Scrappy, urlib, requests (i.e https request related processing in python)
3. Wherever your python side processing is not very dependent on heavy server side computations like tensorflow, a typical image processing and ML relvance stuffs.

It's not suitable for analytics and statistics operations e.g pandas, sklearn, numpy, scipy depended taks.

## Python GIL

[《 初淺聊聊 Python 的 GIL 及 Thread-safe 》](https://www.facebook.com/yftzeng.tw/posts/10212630968328556)

很喜歡與人討論程式語言的「特色」，因為這可以讓人知道你有多瞭解與喜愛你所用的程式語言。
雖然我已經十多年沒有認真寫過 Python。頂多以前用 Python 寫過簡易的防毒軟體，中間幾次處理資料時用過 Python，或處理一些鎖碎的自動化庶務，但不如各位有寫過規模型的產品或專案。可我對 Python 的瞭解，應該還可以和不少人討論。
- - -
▐ 今天來聊聊 Python GIL
因為近年 Python 的流行，愈來愈多人開始進入 Python 的世界，也發現不少 Pythonist (Python 開發工程師) 不瞭解 GIL (甚至沒聽過)，其實這也沒什麼大不了的，程式能動，好像也能跑出想要的結果。但若僅是如此，我覺得還談不上「瞭解 Python」或「精通 Python」。

## 不懂GIL很難說自己瞭解或精通Python

有人覺得「平時根本用不到 GIL，或反正無感」，但其實我們已經用了，只是你不知道。若不懂 GIL，你無法瞭解或跟人談論 Lock / Process / Thread / Coroutine，遇到稍微難解的問題，你會無法克服；遇到效能問題，你會覺得 Python 就是慢。而往往是這些知識，才能讓我們變成「解決問題的人」，而不是「換掉 Python 的人」。
甚至可能有人會覺得「反正因為 GIL，所以 Python 寫出的程式會是 Thread-safe 的」。其實不然，GIL 並不能保證 Python 寫出來的程式是 Thread-safe。
- - -
▐ 來測試我們對於 Python Thread-safe 的認知
你覺得這行程式碼是 Thread-safe 嗎？
i = i + 1
這行程式碼，在 Python 的世界裡，並不是 Thread-safe，意謂著在 multi-threading 下，這段程式碼的執行結果可能不是你想要的。
以下這些範例也都不是 Thread-safe，
List[i] = List[j]
List[i] = List[i] + 1
List.append(List[-1])
雖然 GIL 可以用於內部確保在 Python VM 中同時僅運行一個 Thread，但通常，Python 要在 Threads 間切換時，只能在 Bytecode instructions 間，而不是 Python 程式碼。
所以，GIL 保證的 Thread-safe 是 Bytecode 而不是 Python Code。Python Code 要編成 Bytecodes 才會在 Python VM 上運行。

## GIL保證的Thread安全是在Bytecode層不是PythonCode層

瞭解 GIL 後，你就可以比較容易利用 Lock / Process / Thread / Coroutine 寫出高效能的程式，去處理高效能網站或巨量資料的應用等。
- - -
▐ 額外：從 Python 小談 Go
Python 與 Go 本身內建 unittest，不像 PHP 我還要額外安裝 PHPUnit 才能有比較好的測試環境，這對我是一大利多。
另外 Python AsyncIO 是 N:1 model，Go 是 M:N model，這先瞭解也會比較好。
還有 Go 可以利用 race 協助偵測 Race Condition 的潛在問題，這點我在 Python 似乎還沒找到比較好的方法，看有無網友能提供建議了。
