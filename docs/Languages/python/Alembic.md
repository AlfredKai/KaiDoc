# alembic

[Alembic](https://alembic.sqlalchemy.org/en/latest/index.html)

[使用 Alembic 來進行資料庫版本管理](https://medium.com/@acer1832a/%E4%BD%BF%E7%94%A8-alembic-%E4%BE%86%E9%80%B2%E8%A1%8C%E8%B3%87%E6%96%99%E5%BA%AB%E7%89%88%E6%9C%AC%E7%AE%A1%E7%90%86-32d949f7f2c6)

## Running our First Migration

The process which occurred here included that Alembic first checked if the database had a table called `alembic_version`, and if not, created it.

## Auto Generating Migrations

```bash
alembic revision --autogenerate -m "Added account table"
```

## Relative Migration Identifiers

```bash
alembic upgrade +2

alembic downgrade -1

alembic upgrade head
```
