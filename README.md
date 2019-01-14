# Redis

### **From** [**Redis**](https://redis.io/) **creators…**

> Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs and geospatial indexes with radius queries. Redis has built-in replication, Lua scripting, LRU eviction, transactions and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.



![
](https://cdn-images-1.medium.com/max/600/1*6aNDZOYTCkRGNBMJ9tHQkQ.jpeg)

As shown in above table we can see that NoSQL databases can also be classified too. Each of these categories are for different use cases.

Redis is a key-value database. Each value is mapped with some key. Contrary to SQL databases and NoSQL databases like MongoDB which are using tables or documents to represent a defined schema, these values can vary from each other and doesn't have to follow a particular structure.
## Use cases

### Cache
Being an in-memory database -meaning that it's not written to disk but kept in volatile RAM-  makes Redis a good choice for being **used as cache and as secondary database to support primary database** , However, some persistence measures can be taken to deal with server failures.

### Real-time Applications
Collecting, storing and processing streaming data in large volumes and at high velocity presents architectural challenges. An important first step in delivering real-time data analysis is ensuring that adequate network, compute, storage, and memory resources are available to capture fast data streams. But a company’s software stack must match the performance of its physical infrastructure. Otherwise, businesses will face a massive backlog of data, or worse, missing or incomplete data.

Redis has become a popular choice for such fast data ingest scenarios. A lightweight in-memory database platform, Redis achieves throughput in the **millions of operations per second with sub-millisecond latencies, while drawing on minimal resources.** It also offers simple implementations, enabled by its multiple data structures and functions.

### Message Queue Applications
![
](https://d1.awsstatic.com/product-marketing/Messaging/sqs_seo_queue.1dc710b63346bef869ee34b8a9a76abc014fbfc9.png)
A message queue is a form of asynchronous service-to-service communication used in serverless and microservices architectures. Messages are stored on the queue until they are processed and deleted.


Redis provides atomic operations and can be configured to be highly available. Also Redis can be tuned to match exactly our requirements when dealing with persistence vs performance.

### High Speed Transactions
High speed transactions are the mainstay of the financial industry, and are characterized by very high throughput and extremely low latency requirements. In addition to raw performance, transaction requirements include atomicity, consistency, isolation and durability.

##### Advantages of Using Redis for High Speed Transactions
-   Redis delivers blazing fast performance with its native C, in-memory implementation and optimized data structures that enable extremely fast processing of data
-   Redis supports Lua scripting allowing massive calculations and complex operations right where the data is stored
-   With MULTI/EXEC commands and with Lua scripting, Redis effectively delivers the atomicity and isolation required for transactions.
-   Durability is configurable by using AOF persistence mechanisms for single instance Redis. Redis Enterprise provide multiple consistency mechanisms in clustered highly available Redis, through a combination of the WAIT command, the proxy architecture and cluster manager component which uses algorithms to resolve cluster consistency issues.

##### Redis Uses in Financial Services
-   A binary options PaaS uses Redis to deliver millions of price/currency pairs to subscriber systems over 10 times/second with <1 ms latency
-   A large financial investment firm uses Redis to implement sequential processing of multi-threaded transactions at extremely low latencies, for session storage, login ranking, and distributed locking
-   Xignite, the market data cloud, powering over 1,000 fintech clients, uses Redis for high speed computations, frequently requested data feeds and pub/sub.


## Redis Datatypes 
#### Strings

    SET name "John"                           
    GET name
    
    EXISTS name                            // Check existence of key  
    DEL name                               // Delete key   
    EXPIRE name 5                          // Delete key when expired
    
    SET counter 100  
    INCRBY counter 25                      // Increments by 25  
    DECR counter                           // Decrements by 1  
    
    MSET a 10 b 20                         // Multiple SET  
    MGET a b                               // Multiple GET

#### Lists

Redis lists are implemented using linked-lists rather than arrays. Therefore insertion happens in constant time while searching for key may take more time. Linked-lists implementation is used by Redis it allows to add elements quickly even to long list. However sorted-lists in Redis is recommended if you application would consistently search for elements.

The speed of adding a new element with the  [LPUSH](https://redis.io/commands/lpush)  command to the head of a list with ten elements is the same as adding an element to the head of list with 10 million elements.

What's the downside? Accessing an element  _by index_  is very fast in lists implemented with an Array (constant time indexed access) and not so fast in lists implemented by linked lists (where the operation requires an amount of work proportional to the index of the accessed element).

    LPUSH myList 10                        // Add from left to head  
    RPUSH myList "str"                     // Add from right to tail
    
    LRANGE myList 0 -1                     // Range from 0 to -1 to get all the members of the list
    LTRIM myList 0 1                       // To keep latest elements  
    RPOP myList                            // Pop last element 

#### Hashes

Hash map consists of field-value pairs.
While hashes are handy to represent  _objects_, actually the number of fields you can put inside a hash has no practical limits (other than available memory), so you can use hashes in many different ways inside your application.

The command  [HMSET](https://redis.io/commands/hmset)  sets multiple fields of the hash, while  [HGET](https://redis.io/commands/hget)  retrieves a single field.  [HMGET](https://redis.io/commands/hmget)  is similar to  [HGET](https://redis.io/commands/hget)but returns an array of values:

    HMSET user id 100 name "Smith"        // Setup hash with key "user"  
    HGET user id                          // Only 'id' field returned   
    HGETALL user                         // Get all fields of the hash

#### Sets
Redis Sets are unordered collections of strings. The [SADD](https://redis.io/commands/sadd) command adds new elements to a set. It's also possible to do a number of other operations against sets like testing if a given element already exists, performing the intersection, union or difference between multiple sets, and so forth.

    SADD mySet 10 45 12                  // Add elements to set  
    SMEMBERS mySet                       // Get all members of the set  
    SISMEMBER mySet 10                   // Returns whether value exists

#### Sorted sets

Sorted sets are kind of in-between set and hash data structures, where each element consists of floating point value called ‘score’. This is ordered in term of this score value.

    ZADD children 1992 "Sam"             // Birth-year acts as score  
    ZADD children 1997 "Tom"
    
    ZRANGE children 0 -1                 // Returns all in order  
    ZREVRANGE children 0 -1 withScores   // Reverse order of ZRANGE
    
    ZRANGEBYSCORE children -inf 1995     // Scores until 1993  
    ZREMRANGEBYSCORE children 1990 1995  // Remove from sorted set  
    ZRANK children "Tom"                 // Score rank of 'Tom'

Furthermore apart from these basic data structures, there are some more features such as  **Bitmaps**,  **HyperLogLogs**,  **Pub/Sub** (Message service),  **Scripting** (Lua)
## Redis as cache
As mentioned Redis is known to be used as cache for primary database. Here Redis is inbuilt supported to as  _Least Recently Used (LRU)_ cache or as  _Least Frequently Used (LFU)_ cache from Redis 4.0

Configuration such as  `maxmemory`  could be specified in  `redis.config`  or at run-time using  `redis-server --maxmemory 100mb`. Further configuration include eviction policy by  `--maxmemory-policy allkeys-lru`  and likewise.

![enter image description here](https://cdn-images-1.medium.com/max/800/1*4uXh4XHiGPMsTk8faenZjw.jpeg)

As an example Redis acts between NodeJS server application and MongoDB primary database, where Redis keeps track of recently accessed data. However cache data is preserved for quick access by restricting size of Redis database or validity of data. This caching mechanism is done by Redis as required.

![enter image description here](https://cdn-images-1.medium.com/max/1000/1*-2okR5cAx6wH4gXi7o8T_A.jpeg)

In above NodeJS code in the method  `findBookByTitle`  method we will be searching whether the item is present at Redis cache. If not found we proceed to MongoDB database, and when it is found in the database it is saved into cache for future use.

It should be important to keep in mind to what’s in Redis if the same item is updated in MongoDB primary database. Else we would be retrieving old copy from Redis cache rather than modified value in database.



### Resources

[Redis for High Speed Transactions](https://redislabs.com/solutions/use-cases/redis-high-speed-transactions/)

[ Building a simple message queue using Redis server and Node.js](https://medium.com/@weyoss/building-a-simple-message-queue-using-redis-server-and-node-js-964eda240a2a)

[How to use Redis for real-time stream processing](https://www.infoworld.com/article/3212768/database/how-to-use-redis-for-real-time-stream-processing.html)

[Introduction to redis](https://medium.com/@chathuranga94/introduction-to-redis-348d9ccbfd0d)
