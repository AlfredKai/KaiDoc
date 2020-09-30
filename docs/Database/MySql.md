# MySQL

## Change Account

```sql
mysql> SET PASSWORD FOR '目標使用者'@'主機' = PASSWORD('密碼');
mysql> flush privileges;

mysql> use mysql;
mysql> UPDATE user SET Password=PASSWORD("密碼") WHERE User='目標使用者';
mysql> flush privileges;
```

把textcol複製到intcol並轉成int，遇到空值設成-1

```sql
    UPDATE `kaitest` SET intcol = CAST(CASE textcol WHEN '' THEN '-1' ELSE textcol END AS INT)
```

如果沒有就新增column且設為NOT NULL並加上default，新增位置在cid之後，**MariaDB才適用**

```sql
    ALTER TABLE `testdata` ADD COLUMN IF NOT EXISTS `smallrish` VARCHAR(5) NOT NULL DEFAULT "@Q@" AFTER cid
```

## explain
