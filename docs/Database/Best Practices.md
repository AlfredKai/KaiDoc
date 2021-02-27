# Best Practices

## Natural vs Artificial Primary Keys

[Natural vs Artificial Primary Keys](https://sqlstudies.com/2016/08/29/natural-vs-artificial-primary-keys/#:~:text=are%20usually%20bigger.-,Artificial,the%20data%20in%20the%20row.)

>If you know that the value is unique in the system that you are modeling, it is appropriate to add a unique index or a unique constraint to your table. However, your primary key should generally be some "meaningless" value, such as an auto-incremented number or a GUID. </br> </br> The rationale for this is simple: data entry errors and infrequent changes to things that appear non-changeable do happen. They become much harder to fix on values which are used as primary keys.

## VARCHAR vs TEXT

https://stackoverflow.com/questions/6404628/varchar-vs-text-in-mysql

A long VARCHAR is stored in the same manner as a TEXT/BLOB field in InnoDB (which I assume you're using for transactionality, referential integrity and crash recovery, right?) - that is, externally to the rest of the table on disk (which may require another disk read to retrieve).

>From storage prospective BLOB, TEXT as well as long VARCHAR are handled same way by Innodb. This is why Innodb manual calls it “long columns” rather than BLOBs.

[source](https://www.percona.com/blog/2010/02/09/blob-storage-in-innodb/)

Unless you need to index these columns (in which case VARCHAR is much faster) there is no reason to use VARCHAR over TEXT for long fields - there are some engine specific optimisations in MySQL to tune the data retrieval according to length, and you should use the correct column type to take advantage of these.

## Connection Pool

## Pagination

### 用 limit 跟 offset 還是需要 full scan

用 limit 跟 offset 還是需要 full scan？

對啊，基本知識

:cry_babe:

https://hackernoon.com/please-dont-use-offset-and-limit-for-your-pagination-8ux3u4y

[Tweeter Cursoring with max_id](https://developer.twitter.com/en/docs/tweets/timelines/guides/working-with-timelines)

## Hierarchical database model

[Hierarchical database model](https://en.wikipedia.org/wiki/Hierarchical_database_model)

## Self-Referencing Relationships

[Types of Relationships](http://etutorials.org/SQL/Database+design+for+mere+mortals/Part+II+The+Design+Process/Chapter+10.+Table+Relationships/Types+of+Relationships/)

## Tree

[Storing trees in RDBMS](https://bitworks.software/en/2017-10-20-storing-trees-in-rdbms.html)
