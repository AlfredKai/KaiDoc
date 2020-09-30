# Database

## Index

[数据库索引，到底是什么做的？](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ%3D%3D&mid=2651961486&idx=1&sn=b319a87f87797d5d662ab4715666657f)

[Database index](https://en.wikipedia.org/wiki/Database_index)

## Normalization

[資料庫正規化](https://support.microsoft.com/zh-tw/help/283878/description-of-the-database-normalization-basics)

第一正規形式

- 刪除各個資料表中的重複群組。
- 為每一組關聯的資料建立不同的資料表。
- 使用主索引鍵識別每一組關聯的資料。

不要在單一資料表中使用多重欄位儲存類似的資料。

第二正規形式

- 為可套用於多筆記錄的多組值建立不同的資料表。
- 使用外部索引鍵，讓這些資料表產生關聯。

記錄不應依賴資料表主索引鍵之外的索引鍵，但是必要時可使用複合索引鍵。

第三正規形式

- 刪除不依賴索引鍵的欄位。

理論上，正規化值得追求。 但是太多小型資料表可能會降低效能，或超過可開啟的檔案與記憶體容量。
比較可行的方法是只針對變更頻繁的資料運用第三正規形式。 如果保留某些相依的欄位，請將應用程式設計為要求使用者在欄位變更時，驗證所有相關聯的欄位。

## ACID

[ACID compliant](https://zh.wikipedia.org/wiki/ACID)

>In computer science, ACID (Atomicity, Consistency, Isolation, Durability) is a set of properties of database transactions intended to guarantee validity even in the event of errors, power failures, etc. In the context of databases, a sequence of database operations that satisfies the ACID properties (and these can be perceived as a single logical operation on the data) is called a transaction. For example, a transfer of funds from one bank account to another, even involving multiple changes such as debiting one account and crediting another, is a single transaction.

## RDBMS vs NoSQL



## Execution Plan in MySql

[在MySQL使用Explain做SQL SELECT語法效能測試](http://blog.kejyun.com/2012/12/Using-EXPLAIN-SQL-To-Analysis-Efficient-On-MySQL.html)

## TSDB

[Time series database](https://en.wikipedia.org/wiki/Time_series_database)

- 數據結構簡單
- 數據量大
- 寫多於讀：95%-99%的操作都是寫操作
- 和傳統的 RDBMS 和 NoSQL 資料庫不太一樣，比如它不關心範式和事務
- 順序寫：由於是時間序列數據，因此數據多為追加式寫入，而且幾乎都是實時寫入，很少會寫入幾天前的數據
- 很少更新：數據寫入之後，不會更新
- 區塊（bulk）刪除：基本沒有隨機刪除，多數是從一個時間點開始到某一時間點結束的整段數據刪除。比如刪除上個月，或者7天前的數據。很少出現刪除單獨某個指標的數據，或者跳躍時間段的數據

## CAP theorem

[CAP theorem](https://zh.wikipedia.org/wiki/CAP%E5%AE%9A%E7%90%86)

## Partition and Sharding

[Partition (database)](https://en.wikipedia.org/wiki/Partition_(database))

[Shard (database architecture)](https://en.wikipedia.org/wiki/Shard_(database_architecture))

[Understanding Database Sharding](https://www.digitalocean.com/community/tutorials/understanding-database-sharding)

[MySQL CLUSTER](https://www.mysql.com/products/cluster/mysql-cluster-datasheet.pdf)

## Natural vs Artificial Primary Keys

[Natural vs Artificial Primary Keys](https://sqlstudies.com/2016/08/29/natural-vs-artificial-primary-keys/#:~:text=are%20usually%20bigger.-,Artificial,the%20data%20in%20the%20row.)

## Types of Keys in Relational Model(Candidate, Super, Primary, Alternate and Foreign)

[keys in DBMS](https://beginnersbook.com/2015/04/keys-in-dbms/)

- Primary Key – A primary is a column or set of columns in a table that uniquely identifies tuples (rows) in that table.
- Super Key – A super key is a set of one of more columns (attributes) to uniquely identify rows in a table.
- Candidate Key – A super key with no redundant attribute is known as candidate key

## Entity-Relationship Model

[Entity–relationship model](https://en.wikipedia.org/wiki/Entity%E2%80%93relationship_model)

- Conceptual data model
- Logical data model
- Physical data model

## Paging

[Tweeter Cursoring with max_id](https://developer.twitter.com/en/docs/tweets/timelines/guides/working-with-timelines)

## Data Domain

aka reference domain, reference table

[Data Domain](https://en.wikipedia.org/wiki/Data_domain)

## DDL, DML, TCL and DCL

[DDL, DML, TCL and DCL](https://www.geeksforgeeks.org/sql-ddl-dml-tcl-dcl/)

## Resources

[Avoiding Entity Framework Slowdown](https://visualstudiomagazine.com/blogs/tool-tracker/2018/02/avoiding-ef-slowdown.aspx)

[『浅入浅出』MySQL 和 InnoDB](https://draveness.me/mysql-innodb)

[SQL Server 連線基本概念](http://timonshuang-volley.blogspot.com/2010/01/sql-server.html)

[Facts and Fallacies about First Normal Form](https://www.red-gate.com/simple-talk/sql/learn-sql-server/facts-and-fallacies-about-first-normal-form/)

[MySQL Tutorial](https://www.mysqltutorial.org/)

[四個在亞馬遜工作後才知道SQL密技](https://medium.com/@henryfeng/%E5%9B%9B%E5%80%8B%E5%9C%A8%E4%BA%9E%E9%A6%AC%E9%81%9C%E5%B7%A5%E4%BD%9C%E5%BE%8C%E6%89%8D%E7%9F%A5%E9%81%93sql%E5%AF%86%E6%8A%80-e79c9c5912f5)