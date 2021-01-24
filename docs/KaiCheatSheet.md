# Kai Cheat Sheet

## Docker Mariah DB

```bash
docker run --name KaiDB -d -e MYSQL_ROOT_PASSWORD=123456 -e TEST_DB_HOST=kai -p 3306:3306 mariadb:10.2.15
```
