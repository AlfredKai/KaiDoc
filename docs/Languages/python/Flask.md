# Flask

## How to Run a Flask Application

[How to Run a Flask Application](https://www.twilio.com/blog/how-run-flask-application)

old school

```py
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run()
```

run

```bash
pip install Flask
python hello.py
```

new school

```py
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"
```

run

```bash
pip install Flask
FLASK_APP=hello.py flask run
```

## Print Console in Flask

```py
import sys

print('This is error output', file=sys.stderr)
print('This is standard output', file=sys.stdout)
```

## Configuration Handling

[Configuration Handling](https://flask.palletsprojects.com/en/1.1.x/config/)

### bw style

#### 1. prepare xxx.env

```env
DB_HOST=XXX
DB_USER=YYY
DB_PASS=ZZZ
```

#### 2. export to env

```bash
export $(cat env/conf.sh | xargs)
```

#### 3. read env to config.py

```py
import os


def _get_env_var(key):
    return os.environ.get(key).strip()

DB_HOST = _get_env_var('DB_HOST')
DB_USER = _get_env_var('DB_USER')
DB_PASS = _get_env_var('DB_PASS')
```

#### 4. app.config.from_object

```py
app.config.from_object('app.config')
```
