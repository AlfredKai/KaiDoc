# Advanced Database Design For a Relational DB w/ MySQL

- naming conventions is important because you don't want to change that naming convention after you have a few people created already because then it's pure mess

## Database Design Essentials

1. **Understand the database's purpose** (OLTP/OLAP?)
2. **Get the right tool** (MySQL Workbench)
3. **Gather the requirements for the database** (biz, tech, ops)
4. **Standardize the naming convention**
5. **Enforce relationships** (data integrity)
6. **Use the appropriate data types** (use a variety of data types)
7. **Be conscientious** when modeling the data (do what's right!)
8. **Include indexes** when modeling
9. **Document your work** & keep track of changes (GitHub repo)

## Common Database Design Mistakes

1. Poor design/planning
2. Ignoring normalization
3. Poor naming standards
4. Lack of documentation
5. One table to hold all domain values
6. Using identity columns are your ONLY key
7. Not using SQL facilities to protect data integrity
8. Trying to build generic objects (common reference table)
9. Lack of testing
10. Poor deployment workflow (lack of environments)

## Database Purpose

- what is the purpose of the database? transactional(OLTP) or anlytical database(OLAP)?
- OLTP: optimize for the speed of transactions
- OLAP: optimize for the speed of aggregate queries
- The OLTP disk access tends to be random disk access. (thus may get benefit from storage grids). On the other hand, OLAP operations tend to be long and resource intensive thus more contention risk. Furthermore, the OLAP disk access tends to be sequential. To avoid OLAP blocking the OLTP it's advisable to **separate OLTP & OLAP processing**, i.e. using a dedicated server only for OLAP operations or to defer OLAP operations to nightly/less intensive period.

## Best Practices

### Naming Convetions

- Michal: I think that the scrum is a little enemy for database design, because in database design things should not really change so fast because it's very cumbersome.
- table or column names always always in singular, so you don't spend time thinking of if it is user or users.

### Which Columns to Index

How to choose which columns for index:

- The primary and foreign **keys** columns are candidates for index.
- Choose columns which **not frequently updated**, since index will degrade performance of DML (update, insert, delete) operations (due to index table maintenance).
- Choose columns which are used in **order by / group by** clauses since indexes will order the data to make these statements faster.
- If you use **range** in where clause (e.g. between) use this range columns when making clustered index.
- Use **high selective** / high cardinality columns.
- **Order is matter** when creating composite indexes: create highly selective indexes by using the most restrictive columns first.

Avoid over indexing:

- Remove **unused index**
- For **small table** your don't need index
- If more than 10 percent of table must be examined you might better use full table scan instead of index

### When to Use a View

Consider to use views in these cases:

- To limit columns/tables that a user can see due to **security/privacy** concerns. You might restrict the view query to read only and limit the rows return.
- To **hide complex join**, to represent a combination of tables.
- To give **meaningful names**.
- To **absorb changes**. When the data model in database changes, the application can still use the same query if we modify the view definition.

### Use Artifical Keys

- artifical key is a little bit better than natural since you don't need to worry about the number could be changed

### Use Integrity Checks As Much As You Can

**Domain tables (reference tables)** are effective for enforcing integrity. They work well when there are many values to be checked against, or the values to be checked are frequently changing.

A common misconception is to think that the **application** should check integrity. The issue here is that a central database might be accessed by many applications. Also, you generally want to protect the data where it is: in the database.

If the possible values are limited or in a range, then a check constraint may be preferable. Let’s say that messages are defined as either Incoming or Outgoing, in which case there is no need for a foreign key. But, for something like valid currencies, while these may seem static, they actually change from time-to-time. Countries join a currency union and currencies change.

Applications should also perform integrity checks, but **don’t rely only on the application for integrity checking.** Defining integrity rules on the database ensures that those rules will never be violated. In this way, the data satisfies the defined integrity rules at all times.

### Don't Have All Columns NULLable

Within the database columns definitions good data domains, ranges and values should be analyzed, evaluated and prototyped for the business application.

Having good default values, a limited scope of values and always a value are best for performance and application logic. NULLable columns are only good when data is unknown or doesn’t have a value yet.

Someone’s death date data is the classic example of a NULLable column because it is unknown unless they are already dead. Make sure your database design represents data that is known and only uses a minimum of NULLable columns.

### Make Sure You Understand Database Normalization

Your OLTP database should be well normalized - using at least third normal form and maybe even up to fifth normal form is the starting critical evaluation criteria.

The reason the database design normalization processes have been endorsed forever is because they are effective for identifying all the insert, update and delete data anomalies and support the integrity of the application data.