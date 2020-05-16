# System Design and Database Tradeoffs

To understand the tradeoffs within System Design/Database other than write/read
operations, we need to understand some main goals they try to achieve. These
main goals boil down to:

    -Consistency: This focuses on data integrity where data is consistent and
    accruate for all users who accesses it throughout the whole system (cache,
    database, master-slave)
    -Availability: If node(s) of server/database fail, the overall system is
    still function normally and is available to the user that matches the
    business Server Level Agreement (SLA)
    -Maintainability:
    -Scalability:

Some properties such as `ACID` and `CAP` theorem can help decide tradeoffs should
be made. 

### Vertical and Horizontal Scaling

One main consideration once the process load gets really large for a server is
if it should be scaled **vertically** or **horizontally**. 

**Vertical Scaling:** is when more processing power is added to a server or
database. This is simple and doesn't require much added complexity. The
downside is increase processing power of a single server will hit a limit and
be very costly.

**Horizontal Scaling:** is when more servers are added to split the processing
load. Its a lot cheaper than vertical scaling, more resilient and removes a
single point of failure. One main disadvantage is dealign with the added
complexity where applications involve atomic transaction or if a request
requires complex joins that require cross-server communications.

### Master-Slave Architecture

In the Master-Slave Architecture, an addtional database (slave) is create that
is an identical copy to the master database. All write operations are
processed on the master node and eventaully copied to the slave depending on if
it is asynchronous or sychronous. This can increase read/write operation by
decrease load on master. Additionally, this removes a single-point of failure
within the database system. If there are multiple slaves, a single slave can
copy the master node and the rest of the slave can copy from that node. Other
than the `cost` tradeoff for the additional server, consistency tradeoffs
between the master/slaves should be considers. Should reads be blocked while a
write is in action on the master slave? Should we allow temporary data
inconsisteny and allow it to eventually be consistent? Techniques such as 
`Multiple version concurrency control`, SAFA, 2PC, 3PC can be used.

### SQL vs NoSQL

More detail notes of the two can be view in it's own file. The main tradeoff
between choosing between a traditional relational database and a non-relational
database is that `NoSQL` provide better performance and scalability by
sacrificing ACID compliance. `SQL` DB are generally are ACID compliant and good
for reliability/ safe gurantees for transactions. If data are unstructured and
sysyem requires rapid development, used `NoSQL`.

### Message Queues

Message queues can increase a web application or other server application by
becoming more available. This is achieved by decoupling two services. Once a
web server needs to pass off a request, it can put it into a message queue
where the consumer will eventually process the request. Additional, this leads
to a more durable and scalable system. Message queues are typically used for
asynchronous processing.

### Consistency Trade offs

    -Consistency vs Latency
    -Consistency vs Throughput
    -Consistency vs Data Durability
    -Strong consistency vs multi-master

### Sharding Consideration 

    1. Distaster recovery plan
    2. hardware limitation
    3. Storage engine limitation
    4. Hot data vs cold data
    5. Geo-distributed data
    6. Infrastracture limitation
    7. Failover isolation
    8. speed up queries

### Cache

Page Caching

From a resiliency point of view, the real benefit of caching applies to 
caching dynamic content: data that changes fast and that is not unique-per
-request.

You can use in-memory caching techniques to speed up dynamic applications. 
These techniques, as the name suggests, cache data and objects in memory to
 reduce the number of requests to primary storage sources, often databases. 
In-memory caching offerings include caching software such as Redis or 
Memcached,

Lower overall solution cost: Using cached data can help reduce overall 
solution costs, especially for pay-per-request type of services; and also 
save precious temporary results of expensive and complex operations so that 
subsequent calls don’t have to be processed.

It’s important to pick the right cache expiration policy and avoid 
unnecessary cache eviction.

There are two basic caching patterns, and they govern how an application 
writes to and reads from the cache: (1) Cache-aside; and (2) Cache-as-SoR 
(System of Record) — also called inline-cache.

In an inline-cache pattern, the application uses the cache as though it was 
the primary storage 
