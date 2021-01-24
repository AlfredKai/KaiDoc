# Python

## PIP

pip 就是 python 的套件管理工具

pip freeze > requirements.txt

可以把目前環境的所有套件都印出到 requirements.txt

pip install -r requirements.txt --upgrade

> ubuntu 上安裝了 python3 之後會與 python2 共存，要確定現在使用的是 python3 還是 python2，尤其在 vscode console 裡面，python3 應該要搭配的是 pip3。

### Configuration

#### Manually Configuration

```config
[global]
extra-index-url = http://devpi.local.bridgewell.com/bridgewell/prod/+simple/
trusted-host = devpi.local.bridgewell.com
```

Here are some common environment

| OS      | path                                                                             |
| ------- | -------------------------------------------------------------------------------- |
| Unix    | `$HOME/.config/pip/pip.conf`                                                     |
| macOS   | `$HOME/Library/Application Support/pip/pip.conf` or `$HOME/.config/pip/pip.conf` |
| Windows | `%APPDATA%\pip\pip.ini`                                                          |

For other environments, check [pip office document](https://pip.pypa.io/en/stable/user_guide/#config-file)

#### One-time Command

```config
$ pip install \
    --extra-index-url http://devpi.local.bridgewell.com/bridgewell/prod/+simple/ \
    --trusted-host devpi.local.bridgewell.com \
    shutong>=1.10.0
```

## main()

可以一目了然的進入點

if **name** == '**main**':
main()

python 沒有進入點，任何.py 都可獨立執行，所以這個寫法可以增加可讀性一眼就看出進入點，另一個優點是當這個.py 被其他 import 的時候就可以防止執行 main()。

## Linq in python

<http://mark-dot-net.blogspot.com/2014/03/python-equivalents-of-linq-methods.html>

## Decorator

```python
def use_logging(level):
    def decorator(func):
        def wrapper(*args, **kwargs):
            if level == "warn":
                logging.warn("%s is running" % func.__name__)
            elif level == "info":
                logging.info("%s is running" % func.__name__)
            return func(*args)
        return wrapper
    return decorator
```

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
