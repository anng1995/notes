# Introduction to Architecting Systems for Scale

## Load Balancing

An ideal system increases capcity linearly
On the failure side of an ideal system, the system isn't disrupted by the loss
of a server. It should simply decrease the system's capacity. AKA redundancy

Vertical scalability is usually an undesirable property for a large system
because there is inevitably a point whre it becomes cheaper to add capacity in
the form of additional machines rather than additional resources on one machine.

*Load Balancing:* the process of spreading requests across multiple resources
according to some metric.

Load balancing needs to be balanced at every stage to achieve full scalability
and redundancy.

` A moderately large system may balance load at three layer:
    -User to web server
    -Web servers to internal platform layer
    -Internal playform layer to database`

*HAProxy:* hybrid software load-balancer

## Caching

Caching will enable you to make vastly better use of the resource you already have

Caching is important earleir in the development process than load-balancing.
Starting with a consistent caching strategy will save time later.

With early caching, we can ensure that we don't:
    -optimize access patterns which can't be replicated with our caching mechanism
    -Access patterns where performance become unimportant after addition of caching

(Hard to add caching for an optimized database where caching strategy can't be 
applied to your access patterns because data model is generally inconsistent
between "cassandra" and your cache)

### Application vs Database Caching

Two primary caching:
    -Application caching
    -Database caching

Database caching: When database is on, dfault configuration will provide some
degree of caching and performance for generic usecase

In-memory caches:
    -Most potetnt in terms of raw performances are in memory (Memcached/redis)

## Content Distribution Networks

CDN: cache for static media

Application servers are typically optimzed for serving dynamic pages

CDN will put less strain on your server but more strain on business expenses

Typical setup for CDN:
    1. CDN will as kfor a piece of static media
    2. CDN will server that content if it has it locally available
    (HTTP headers are used for configuring how the DN caches a given piece of content
    3. If not available, CDN will query your servers for the file and then
    cache it locally and serve it to the requesting user. (read-through cache)

If site isn't big enough to merit its own CDN, ease future transition using 
light weight HTTP server like Nginx

For caching, it easy to introduce more errors if you have multipel codepaths 
writing to your database and cache which is usually goign to happen if you don't
write the application with caching strategy in mind.

Invalidation becomes more meaningfully challenging with fuzzy queries or modification
to unknown number of elements -- e.g deleting all objects created more than a week ago.

These scenarios, you want to cosnider relying fullyo n database caching, adding 
aggressive expirations to the cached data, or reworking your applicaitons logic to
avoid the issue (e.g insteadof DELETE FROM a WHERE ..., retrieve all the items
and invalidate the corresponding cache rows and then delete the rows by their 
primary key explicitly).

## Message Queues

Message queues allow your web applications to quickly publish messages to the
queue and have other consumers processes perform the processing outside the scope
and timeline of the client request

The work divided off-line and in-line between consumer/web application depends
on the interface you are exposign to your users. Genreally you'll either:
    1.Tell user that youll merely be scheduling a task and perform it offline
    usually with a polling mechanism to update the interface once the task is complete
    2.Perform enough work in-line to make it appear to the user that the task
    has completed, and tie up hanging ends afterwards.

Message queues have another benefit, which is that they allow you to create a 
separate machine pool for perofring off-line processing rather tahn burdening your
web applicaiton servers. This allows you to target increases in resources to your
current performance or throughput bottleneck rather than uniformly increasing 
resources across the bottleneck and non-bottleneck systems.

## Scheduling periodic tasks

Large system require daily or hourly task but there are no widely accept soultuion
that support redundancy. Use `cron`

It can be used to public messages to consumers. Only responsible for scheduling.

## Map-Reduce

Makes it possible to perform data and/or processing intensive operation in a 
reasonable amount of time. e.g use it for calculating suggest users in a social graph

For small system, adhoc queries on a SQL works until quanity of data stored or
write-load requires sharding your database

