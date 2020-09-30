# Python

## PIP

pip就是python的套件管理工具

    pip freeze > requirements.txt

可以把目前環境的所有套件都印出到requirements.txt

    pip install -r requirements.txt --upgrade

>ubuntu上安裝了python3之後會與python2共存，要確定現在使用的是python3還是python2，尤其在vscode console裡面，python3應該要搭配的是pip3。

### Configuration

#### Manually Configuration

```config
[global]
extra-index-url = http://devpi.local.bridgewell.com/bridgewell/prod/+simple/
trusted-host = devpi.local.bridgewell.com
```

Here are some common environment

|OS|path|
|---|---|
|Unix|`$HOME/.config/pip/pip.conf`|
|macOS|`$HOME/Library/Application Support/pip/pip.conf` or `$HOME/.config/pip/pip.conf`|
|Windows| `%APPDATA%\pip\pip.ini`|

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

    if __name__ == '__main__':
      main()

python沒有進入點，任何.py都可獨立執行，所以這個寫法可以增加可讀性一眼就看出進入點，另一個優點是當這個.py被其他import的時候就可以防止執行main()。

## 好用套件

- SqlAlchemy: ORM in python
- logging: for log
- pywinrm: remote windows
- devpi: nuget in python

## Llint

**媽的潔癖症**  
[Linting Python in VS Code](https://code.visualstudio.com/docs/python/linting)  
[Flake8 Rules](https://lintlyci.github.io/Flake8Rules/)

- flake8-docstrings：public interfaces 需加上 docstrings。
- flake8-import-order：import statement 需要照 stdlib、3rd party、local packages 的順序分組。組間以空行隔開。
- flake8-quotes：統一單行使用單引號、多行使用雙引號。
- pep8-naming：命名符合 PEP8 規範。

公司.flake8

```flake8
[flake8]
max-line-length = 99
import-order-style = pep8
ignore = D100,D101,D102,D103,D104,D400,D401,E402,N806
exclude =
    .git,
    __pycache__,
    protobuf
```

## cryptography錯誤

    Failed building wheel for cryptography

請安裝，我不知道這三小  
<https://cryptography.io/en/latest/installation/#building-cryptography-on-linux>

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

## *args  **kwargs

不定長度參數與關鍵字參數

## r prefix

`r``''` or `r``""`

## 兜字串

<https://pyformat.info/>

## Lists, Tuples, Dictionaries, Sets

|List|Tuple|Dictionary|Set|
|-|-|-|-|
|[] or list()|()|{}|set()|

## Tuple unpacking

```python
row, col = cell
```

## List Slice Notation

```python
a[start:stop]  # items start through stop-1
a[start:]      # items start through the rest of the array
a[:stop]       # items from the beginning through stop-1
a[:]           # a copy of the whole array
```

## False

|||
|-|-|
|boolean        |False  |
|null           |None   |
|zero integer   |0      |
|zero float     |0.0    |
|empty string   |'' |
|empty list     |[] |
|empty tuple    |() |
|empty dict     |{} |
|empty set      |set()|

## zip(), range()

回傳為`iterable object`並不是整個實體，所以需搭配`for`或直接`list(), dict()`使用。

## Comprehensions

list:

```python
[ expression for item in iterable ]
[ expression for item in iterable if condition ]
```

dict:

```python
{ key_expression : value_expression for expression in iterable }
```

## Namespaces and Scope

修改global變數需要先宣告，不然一律視為local variable，但是**讀不需要宣告**(乾您娘超雷)。

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