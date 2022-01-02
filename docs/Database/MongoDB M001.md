# MongoDB M001

## Chapter 1: What is MongoDB?

MongoDB is an noSQL **document** database.

Document - a way to organize and store data as a set of field-value pairs.

Field - a unique identifier for a datapoint.

Value - data related to a given identifier.

Collection - an organized store of documents in MongoDB, usually with common fields between documents. There can be many collections per database and many documents per collection.

## Chapter 2: Importing, Exporting and Querying Data

MongoDB stores data in BSON, internally and over the network.

| JSON                           | BSON                                                                       |
| ------------------------------ | -------------------------------------------------------------------------- |
| UTF-8 String                   | Binary                                                                     |
| String, Boolean, Number, Array | String, Boolean, Number(Integer, Long, Float,...), Array, Date, Raw Binary |
| Readability: Human and Machine | Readability: Machine Only                                                  |
| mongoimport/mongoexport        | mongoredtore/mongodump                                                     |

[JSON and BSON](https://www.mongodb.com/json-and-bson)

Find Command

- Use `show dbs` and `show collections` for available namespaces.
- `find()` returns a cursor with documents that match the find query.
- `count()` returns the number of documents that match the find query.
- `pretty()` format the documents in the cursor.

## Chapter 3: Creating and Manipulating Documents

`_id` unique identifier for a document in a collection.
`_id` required in every MongoDB document.
`ObjectId()` is the default value for the `_id` field unless otherwise specified.

Identical documents can exist in the same collection as long as their `_id` values are different.
MongoDB has schema validation functionality allows you to enforce document structure.

Insert multiple documents by using an array:
`db.collection.insert([{<doc1>}, {<doc2>}])`

Use `{"ordered": false}` to disable the default ordered insert.

Collections and databases are created when they are being used:
`use tools` followed by `db.tractors.insert({<tractor doc>})`
create the `tools.tractors` namespace.

`{"$inc": {"pop": 10, "<field2>": <increment value>, ...}}`
increments field value by a specified amount.

`{"$set": {"pop": 17630, "<field2>": <new value>, ...}}`
sets field value to a new specified value.

`{ $push: { <field1>: <value1>, ...}}`
adds an element to an array field.

`db.<collection>.drop()` deleted the given collection.
`deleteOne(), deleteMany()` deletes documents that match a given query.
After these commands is issued the **data is GONE.**

## Chapter 4: Advanced CRUD Operations

Query operators provide additional ways to locate data within the database.

Comparison operators specifically allow us to find data within a certain range.
`{ <field>: { <operator>: <value> } }`

`$eq` is used as the default operator when an operator is not specified.

Logic operators allow us to be more granular in our search for data.

Syntax
`{ "$<operator>": [{ <clause1> }, {<clause2>}, ...]}`
Syntax for `$not`:
`{ "$not": {<clause>}}`

`$and` is used as the default operator when an operator is not specified.

Allows for more complex queries and for comparing fields within a document.

The `$` can be used to access the field value.

Syntax for comparison operators using aggregation:
`{ <operator>: { <field>, <value> } }`

`{<array field> : { "$size": <number>}}`
Returns a cursor with all documents where the specified array field is exactly the given length.

`{<array field>: { "$all": <array>}}`
Returns a cursor with all documents in which the specified array field contains all the given elements regardless of their order in the array.

**Querying an array field using**
An array returns only exact array matches.
A single element will return all documents where the specified array field contains this given element.

`db.<collection>.find({ <query> }, { **<projection>**})`
Specifies which fields should or should not be included in the result cursor.
Do **not** combine `1s` and `0s` in a projection.

- Except for `{ "_id: 0", <field>: 1}`
  
`{<field> : { "$elemMatch": { <field>: <value> }}}`
Matches documents that contain an array field with at least one element that matches the specified query criteria.

**or**

Projects only the array elements with at least one element that matches the specified criteria.

MQL uses dot-notation to specify the address of nested elements in a document

To use dot-notation in arrays specify the position of the element in the array.

`db.collection.find({"field 1.other field.also a field": "value"})`

## Chapter 5: Indexing and Aggregation Pipeline

### Aggregation Framework

- Agg Framework > MQL
- Pipeline stages in order
- Syntax

- Agg Framework
  - `$group`
  - compute
  - reshape
- MQL
  - filter
  - update

### Cursor Methods

Cursor is not apply to the data stored in the database, it is instead applied to the result set that lives in the cursor.

- sort()
- limit()
- pretty()
- count()

MongoDB assumes that when you use `sort()` and `limit()`, you always mean to sort first, regardless of the order in which you type these methods.

`cursor.limit().sort()` == `cursor.sort().limit()`

### Indexes

- Single field index
- Compound Index

### Data modeling with MongoDB

Data that is **used** together should be **stored** together.
Evolving application implies an evolving data model.

"MongoDB is built for quick data model changes sand evolution"

### Upsert

Everything in MQL that is used to locate a document in a collection can also be used to **modify** this document.

`db.collection.updateOne({<query to locate>}, {<**update**>})`
 Upsert is a hybrid of update and insert, it should only be used when it is needed.

 `db.collection.updateOne({<query>}, {<update>}, {**"upsert": true**})`

`Upsert:true`

**Be mindful**

Is `{<update>}` enough to create a new document?

Will the document have the same or similar form to other documents in the collection?

## Chapter 6: Next Steps

### Atlas Data Explorer

- Performance Advisory
- Aggregation Builder
- Anti-Pattern Advisory
- Advanced Text Search

### Atlas Products

- Billing is at organization level
- Projects are within organization
- Teams assigned to projects
- Clusters have unique names

### MongoDB Compass

- Connection
- Documents
- Schema
- Indexes

### MongoDB Compass Part 2

- Explain
- Validation
- Schema

### Why MongoDB?

At this point you have learned enough to start having a meaningful conversation about why developers, DBAs, and companies choose MongoDB as their database.

This can also help you decide whether MongoDB is the right choice for you.

To help you learn more about specific use cases, we put together case studies and articles about some of our customers and community members.

We also added a link to the MongoDB Developer Hub and Community Forums where you can ask questions, learn new things, and connect with others who are diving into MongoDB content and products.

Take a look and feel free to browse for more information.

- [MongoDB Developer Hub](https://developer.mongodb.com/)
- [MongoDB Community Forums](https://developer.mongodb.com/community/forums/)
- [Case study: Bosch Leads Charge into Internet of Things](https://www.mongodb.com/customers/bosch)
- [Case study: Forbes](https://www.mongodb.com/blog/post/forbes-cloud-migration-helps-worlds-biggest-media-brand-continue-standard-digital-innovation)
- [Case study: SEGA](https://www.mongodb.com/blog/post/sega-hardlight-migrates-to-mongodb-atlas-simplify-ops-improve-experience-mobile-gamers)
- [How the Financial Sector Uses MongoDB](https://www.mongodb.com/industries/financial-services)
