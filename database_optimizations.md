# Database Optimization and Tradeoffs

What does database optimization entail? A database can be optimize by speeding
up write operations or speeding up read operations. Read/write operations can
be optimize within the database itself such as proper **indexing** and 
**normalization**. Other ways would be to physically or logically partition the
database vertically or horizontally,
or use caching. Many of these comes with trade-offs such as increase read
operation speed while decreasing write operation speed. Other trade-offs will
be dealing with data consistency and availability for the read/write
operations.

**Look at the notes for a specific topic to understand why**
**NOTE: What I mean by optimization is increase read/write operations or reduce
load on a single database that will increase overall speed for all users.** 

## Internal Optimization for a Database

This section will cover what a database can do to optimize itself and the
tradeoffs that will need to be considered. Ways that databases can internally
optimize is through:

    -Indexing
    -Normalization/Denormalization

**Indexing:** Increases read operations speed while decreases write operation
speed. Although generally it decreases write operation speed, it can still
increase write operation speed for **update**  operations by quickly finding the 
memory block to update.

**Normalization:** Decreases read operation speed due to complex joins but
increases write operation speed because it only needs to update a single value.
In a **Denormalized** database, there will be redundant data which will require
the DB to update every location of the redundant data to maintain consistency/
data integrity. In **denormalized** db, read operations are a lot faster.

## External Optimization for a Database

    -Database Partitioning
    -Cache
    -Master-Slave Architecture

### Database Partitioning

**NOTE:** There is two types of partitioning, logical and physical. When tables
are partition but remain on the same physical node, its a logic partition or
logic shard. Physical shard is when the logic shard is move to different node.
Database partitioning can increase both read/write operations if the partition
is done/manage correctly. Tradeoffs would be increase cost/complexity.
Depending on the type of complexity
**horizontal/vertical/functional**, they would have to be manage and
restructered to manage change in user load. Physical Partitioning can increase read 
operations if a query require accessing different databases.

Different sharding techniques are key-base, range, and directory based.
Although sharding optimization, it will add a lot of complexity.
Sharding should generally only be consider when the application data exceeds
the single database, volume of read/write is too much to the database even with
replicas for read, or network bandiwth required by the application outpaces the
bandwidth available to a single database and read replicas.

Data Partitioning:
	- How critical the data
	- Location
	- Whether to replicate critical data
	- Replicate data that are relatively static (e.g postal code) for each DB
	- Minimize referential integrity for functional/vertical partition
	- Horizontal Partition should periodically rebalance data/work load
		-Joining data from different shard - increase latency
	-Need to shard usually come downs to large working data or too many writes

### Cache

There are different type of caching, this will mainly focus on **external caching**
such as **memcache/redis** and **content delivery network**

**External Caching:** Caching can be used to optimize read and write time.
Since external cache is much quicker than a DB through RAM/(Possibly latency if
cache is placed closer to user), read/write operations both increases
significantly. The few main problem with using cache is:
    -Data Consistency with cache/DB/users for critical data 
    -Poor cache technique that will cause a lot of cache miss making read/write
    operations longer due to checking cache + latency of going to cache server then
    to DB.
    -Cost of cache/ added complexity
The main problem with cache is dealing with critical data/consistency. Cache is
meant for temporary storage for quick access, not general as a persistent
memory (perhap redis)

**Content Delivery Network:** CDN is a dedicated server that general holds various
static files such as HTML/CSS/Scripts to deliver to users for webpages. Since
CDN servers are generally located closer to users geographically, it reduces
latency time. Although this doesn't necessarily pertain to DB, it reduces loads
on servers/DB that were to hold on them. This leads to more resource focusing
on other tasks.

### Master-Slave Architecture

**Master-Slave Architecture** prevent a single point of failure as well as
increase read/write operations. By having a replica, servers can read non
critical data from the slaves therefor putting less load on the master node.
The master node can be used for reads or mainly used for write operations.
Tradeoff deals with the possible data consistency/cost.
