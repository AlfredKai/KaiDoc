# TECH INTERVIEW PRO

## Systems Design

### Introduction to Systems Design

- Doesn't always happen, it's more prevalent for senior.
- It's different from interviewing vs actually doing it.
- Get high level overview.
- Try to navigate toward things you know better.
- Research the company and get the sense of what they would ask.
- You may ask to do math(latency, storage size)

### Approach to Systems Design

- Scope it well -> ask more questions
- Outline use cases
- High level design like input/output?
- How components work
- Scaling

### Load Balancing

- load balancer is one of the key component of any web system or backend
- it balances the load
- most simplest one: hash the request and send to the pooled peer server and the application server
- more complicated one: maybe do some health check
- most complicated one: may cache the request and result
- sending or re-routing request is a lot less computation power then your application probably do. That's why we can have just one computer balancing multiple request
- mechanism: Round Robin, least load, randomly
- most case single load balancer is enough but will introduce single failure point. In that case you can have two or three load balancer by using DNS with domain name.
- think of DNS is another load balancer
- the only flaw of using DNS as load balancer is clients will cache the ip
- the flaw of load balancer is when you scale horizantalily(adding more machine) you add more complexity(you have to write more load balancing algorithm), so some people more like verticality scale which is just pay more money and have beast machine
- another scenario is using microservice which each service has its own load balance
- you can add load balancer in any layer

### Caching

- if you don't need to compute the load anymore and that is caching
- usually in a separately server, maybe a huge distributed cache
- memcached, cassandra, redis
- key-value store
- very scalable, very distributed and very fast because all data is in memory
- cache invalidation(for cache data outdated): set invalid when user action, add timeout
- write through cache: write to the cache first then change the database. disadventage is now your write operation is slower
- write back cache: write to the cache and update the database in each interval. disadventage is you may fail to update the database is your cache die
- when hit the limit of cache size: FIFO, LIFO, least recently used, most recently used(?)
- it is just a distributed dictonary similar to you use in your code

### Content Delivery Networks (CDN)

- good for storing static files like: img, html, css, video
- distributed network of servers that will holds your files and it's global
- pull based: CDN will fetch data and cache
- push based: you upload files and push to CDN
- in pull based is very simple to configure just host the files and set up
- in push based you have to manually push the files up and keep it maintained. but it's extremley fast even for first time users
- push based cost money on how much you store, for pull based it cost money based on usage because there's no storage generally
- tag a version in javascript files to keep CDN always fresh
- if your website is static you can just host it on CDN

### Databases and Indexing

- schema design tips:
  - every table should have an id, represent that row uniquely
  - don't store too heavy data
- generlly have an index on the id
- index drawbacks:
  - add space requirement
  - slow down your insertion
- design table always think about what kind of queries I'm going to making on this table

### Redundancy and Replication

- master-master and master-slave two type of replication
- master-slave: master for write and slave for read, not entirly consistent all the time
- master-master: slow down the writes, has consistent issues and more complicated. not suggest that if you can do sharding
- master-slave can easily backup, just choose one slave you would not slow down anything
  
### Database Sharding

- load balancer for webservier, CDN for images and replication for database and data partition is for database writing. Because master-master replication is way too complicated for scaling
- two form: horizontal(sharding), vertical
- vertical partition is very simple, just move different table to different machine
- if you can't slpit out a single table, that is why you horizontal partition
- system design is all about targeting bottlenecks and then choosing certain design such that there are trade off. And then depending on what bottlenecks we have, we choose a system for it
- sharding is you can write to different computers' database and these databases represent a portion of a single table
- if you want to read all data you have to look all of database in different computers
- data partitioning strategy: choose which database to be write
  - key based example: using primary key and mod it to choose which database
  - you can mod 100 even if you only have 3 tables. in that case, when you add another table you only need to re-assign one table to newer table instead of all tables should be re-assign if you mod 3 in 3 tables
  - [Consistent Hashing](https://www.interviewcake.com/concept/java/consistent-hashing) can also minimize the re-assign effort when add new database
  - list partitioning: partition by different attributes, for example, segment by loacation in dating app which users interaction with each other most
- range query would become difficult when using partitioning
- remember you always have cache solution if you don't have to keep your data persist

### SQL vs NoSQL

- NoSQL: usually unstructured, easily to distribute, dynamic schema
- consider it as cache like memcache, redis or cassandra but can keep data persist
- SQL use structured data so we can work on it very easy and join is very efficient
- SQL is ACID compliant
- NoSQL is stardard key-value dictionary, so it desn't have really great way to do like range query
- NoSQL don't follow being transactionable or atomic, it may be delay to wirte to the disk such that data don't consistent
- document based database like MongoDB, data stored in documents which group by collection and every document can have different structure. basically it is still key-value store
- wide column database: you don't need to read all the attributes which you can specify column you want to read
- graph database which is good for graph
- always think of tradoff of storage: how you store the data? schema? how you query the data? need query range? scalability? reliable? acid compliant?
- consistency means that when you generall like wirte to the table that you are be able to read the exactly value you just wrote into it. a lot of NoSQL database don't have consistency
- three different type of consistency:
  - weak consistency: you may not get the data
  - eventually consistency: guarantee to be able to read after some period of time
  - strong consistency: RDBMS

### API design

- API is usually an interface for your client
- make it clean, simple and secure
- it should be very clearly about what your endpoint do
- think the flow of your application like CURD.
- make input/ouput clear, like using id to update somthing instead of using name. And think of how you can get the id
- think what type of input/output should be and you don't want to put more redundant data you don't need to
- use pagination for feed-driven application
- use cursor, an indicator to the next set of result, to gurarantee you don't miss any item
- server-driven API interface: the client doesn't dictate too much, all logic you want to put on server side such you can control logic base on server. for example just tell the server the button is be tapped. it is especially for mobile development which you have to wait for approve by app store
- think the performance
  - how many requests does client need to make
  - does it make a single request that is heavy
  - can you show user something quickly when the first request back so that you can do other background requests
- APIs are user for abstract to the all detail staff so that the client can use whatever you made a lot easier.
- How errors return? For example, json is a great output format. Having error code return 0 when it's success.

## Mobile systems design

- experience is your best preparation
- design patterns
  - how is navigation and communication between portions of the pages going to happen
  - how you break up the dependency of pages
  - use common dependency library which you can register events, register pages, actions, controllers
  - communication pattern
    - don't use notification service it's going to be mass
    - one common technic is publish susbscribe event listener, it can also do well in screen freshness
- how the network handle offline access and caching: use centralize network to abstract all network
- use dependency injection to get the singletons not to persist all the time
- use server-driven API if you can
