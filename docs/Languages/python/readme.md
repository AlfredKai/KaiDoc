# Python

## Global Interpreter Lock

[GIL](https://en.wikipedia.org/wiki/Global_interpreter_lock)

## main()

可以一目了然的進入點

if **name** == '**main**':
main()

python 沒有進入點，任何.py 都可獨立執行，所以這個寫法可以增加可讀性一眼就看出進入點，另一個優點是當這個.py 被其他 import 的時候就可以防止執行 main()。

## Decorator

[Primer on Python Decorators](https://realpython.com/primer-on-python-decorators/)

## Namespaces and Scope

修改 global 變數需要先宣告，不然一律視為 local variable，但是**讀不需要宣告**。

```python
def change_and_print_global ():
    global animal
    animal = 'wombat'
```

## Resource

[GitHub 神人整理出一份 Python 開源清單](https://buzzorange.com/techorange/2018/12/21/python-list/?fbclid=IwAR3fK0h5CDINJXgyMMch0-KVgjVupWuKsQKnnL0qDgQGKWbpMd9Y5pMLEw0)

[awesome-python-applications](https://github.com/mahmoud/awesome-python-applications)

[An A-Z of useful Python tricks](https://medium.freecodecamp.org/an-a-z-of-useful-python-tricks-b467524ee747)

[Python Complexitipy](https://www.ics.uci.edu/~pattis/ICS-33/lectures/complexitypython.txt)

[Comprehensive Python Cheatsheet](https://github.com/gto76/python-cheatsheet)

[The Hitchhiker’s Guide to Python](https://docs.python-guide.org/)

[Real Python Tutorials](https://realpython.com/)

[The Little Book of Python Anti-Patterns](https://docs.quantifiedcode.com/python-anti-patterns/index.html)
