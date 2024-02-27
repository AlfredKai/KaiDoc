# Short URL

https://medium.com/system-design-concepts/tinyurl-system-design-16868b5435de

https://kknews.cc/zh-tw/tech/nlrxkq2.html

https://stackoverflow.com/questions/3538021/why-do-we-use-base64

## Requirements

The total number of unique domains registering redirect URLs is on the order of 10s of thousands
New URL registrations are on the order of 10,000,000/day (100/sec)
Redirect requests are on the order of 10B/day (100,000/sec)
Remind candidates that those are average numbers - during peak traffic (either driven by time, such as 'as people come home from work' or by outside events, such as 'during the super bowl) they may be much higher.
Recent stats (within the current day) should be aggregated and available with a 5 minute lag time
Long look-back stats can be computed daily

## How will shortened URLs be generated? (DB)

ID vs hash, how to create a hash in DB
How do we generate keys?
How to check collision. If we need to query the database to check collision, how can we improve it?
How to store the mappings?

- auto-increment key or timestamp
  - waste charater space
  - security problem
- random
  - collision
  - how to generate random key
- UUID or HASH
  - can't avoid collision
  - collision vs. length of URL is a trade-off
- counter and zookeeper
  - need to be encoded
  - to solve potential security issue in counter based key
    - encoding algorithm can be shuffled based on salt
    - add some random bits
  - example lib: https://hashids.org/

database for storing counter: any CP service is good

## Database Design

Sql/NoSql
Lifespan

## API Design

How server responds to a user (ask cache & API)
What happens when a cache miss
Cache control
Cache Penetration
What if we have space limitations in redis? (cache eviction policies)
CDN can cache 301, 302

## How to implement the redirect servers? (Restful)

301 vs 302
Location header
Get vs Post
Http methods (idempotent)

## How are click stats stored?

Message queue
Large scale questions
If we have a load balancer, how can we do for DBS in a distributed system
How many master/replicas
Sharding?
How to deal with the consistency about keys
What else do you think you can improve this system

Generate a random tiny URL

randomness is good

timestamp issue: collision, waste char

generally, you should avoid hashing to guarantee uniqueueness, as you are trying to map a bigger value set to a smaller value set so you WILL have collisions.
