# Database

## RDBMS

### Types of Keys

[keys in DBMS](https://beginnersbook.com/2015/04/keys-in-dbms/)

- Primary Key – A primary is a column or set of columns in a table that uniquely identifies tuples (rows) in that table.
- Super Key – A super key is a set of one of more columns (attributes) to uniquely identify rows in a table.
- Candidate Key – A super key with no redundant attribute is known as candidate key
- Alternate Key
- Foreign Key

### Normalization

- First normal form (1NF)
  - Eliminate repeating groups in individual tables
  - Create a separate table for each set of related data
  - Identify each set of related data with a primary key
- Second normal form (2NF)
  - It is in first normal form.
  - It does not have any non-prime attribute that is functionally dependent on any proper subset of any candidate key of the relation. **A non-prime attribute** of a relation is an attribute that is not a part of any candidate key of the relation.
- Third normal form (3NF)
  - The relation R (table) is in second normal form (2NF).
  - Every non-prime attribute of R is non-transitively dependent on every key of R.

### Entity-Relationship Model

[Entity–relationship model](https://en.wikipedia.org/wiki/Entity%E2%80%93relationship_model)

- Conceptual data model
- Logical data model
- Physical data model
- [DDL, DML, TCL and DCL](https://www.geeksforgeeks.org/sql-ddl-dml-tcl-dcl/)

### Data Domain

aka reference domain, reference table

[Data Domain](https://en.wikipedia.org/wiki/Data_domain)

## NoSQL

"Not only SQL"

[SQL vs NoSQL: The Differences](https://www.kshuang.xyz/doku.php/database:sql_vs_nosql)

- 靠著`denormalization`達成可以輕易`scale-out`來增加performance，缺點是無法join且update變得困難

## Time Series Database

[Time series database](https://en.wikipedia.org/wiki/Time_series_database)

- 數據結構簡單
- 數據量大
- 寫多於讀：95%-99%的操作都是寫操作
- 和傳統的 RDBMS 和 NoSQL 資料庫不太一樣，比如它不關心範式和事務
- 順序寫：由於是時間序列數據，因此數據多為追加式寫入，而且幾乎都是實時寫入，很少會寫入幾天前的數據
- 很少更新：數據寫入之後，不會更新
- 區塊（bulk）刪除：基本沒有隨機刪除，多數是從一個時間點開始到某一時間點結束的整段數據刪除。比如刪除上個月，或者7天前的數據。很少出現刪除單獨某個指標的數據，或者跳躍時間段的數據

## ACID / CAP

[ACID compliant](https://zh.wikipedia.org/wiki/ACID)

[CAP theorem](https://zh.wikipedia.org/wiki/CAP%E5%AE%9A%E7%90%86)

- NoSQL可能不滿足ACID
- CAP的C為[Eventual consistency](https://en.wikipedia.org/wiki/Eventual_consistency)

## Index

[数据库索引，到底是什么做的？](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ%3D%3D&mid=2651961486&idx=1&sn=b319a87f87797d5d662ab4715666657f)

[Database index](https://en.wikipedia.org/wiki/Database_index)

### String as Index

[Optimal index size for variable text in MySQL](https://www.xarg.org/2012/07/optimal-index-size-for-variable-text-in-mysql/)

### Execution Plan in MySql

[在MySQL使用Explain做SQL SELECT語法效能測試](http://blog.kejyun.com/2012/12/Using-EXPLAIN-SQL-To-Analysis-Efficient-On-MySQL.html)

### Partition and Sharding

[Partition (database)](https://en.wikipedia.org/wiki/Partition_(database))

[Shard (database architecture)](https://en.wikipedia.org/wiki/Shard_(database_architecture))

[Understanding Database Sharding](https://www.digitalocean.com/community/tutorials/understanding-database-sharding)

[MySQL CLUSTER](https://www.mysql.com/products/cluster/mysql-cluster-datasheet.pdf)

## Monitoring

[PMM](https://www.percona.com/doc/percona-monitoring-and-management/2.x/index.html)

## DAO and Leaky Abstraction

[Data access object](https://en.wikipedia.org/wiki/Data_access_object)

[Leaky abstraction](https://en.wikipedia.org/wiki/Leaky_abstraction)

## Resources

[Avoiding Entity Framework Slowdown](https://visualstudiomagazine.com/blogs/tool-tracker/2018/02/avoiding-ef-slowdown.aspx)

[『浅入浅出』MySQL 和 InnoDB](https://draveness.me/mysql-innodb)

[SQL Server 連線基本概念](http://timonshuang-volley.blogspot.com/2010/01/sql-server.html)

[Facts and Fallacies about First Normal Form](https://www.red-gate.com/simple-talk/sql/learn-sql-server/facts-and-fallacies-about-first-normal-form/)

[MySQL Tutorial](https://www.mysqltutorial.org/)

[四個在亞馬遜工作後才知道SQL密技](https://medium.com/@henryfeng/%E5%9B%9B%E5%80%8B%E5%9C%A8%E4%BA%9E%E9%A6%AC%E9%81%9C%E5%B7%A5%E4%BD%9C%E5%BE%8C%E6%89%8D%E7%9F%A5%E9%81%93sql%E5%AF%86%E6%8A%80-e79c9c5912f5)

[脚踏两只船的困惑 - Memcached与Redis](https://zhuanlan.zhihu.com/p/34069821?fbclid=IwAR1wuQ7B-35x-gm3Tl4XC9VN6TcICBegv5QHMFBdvXhTnzZCKBKgdPmwF5Y)
