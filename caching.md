# Caching

Takes advantage of the locatlity of reference principle:
    -recently requested data is likely to be requested again.

Two types of Caching:
    -Private cache, where data is heald locally on computer thats running the
    application/ or service
    -Shared cache among multiple processes/machines
        -Two Main Disadvantage:
            -Cache is slower to access because it is no longer held locally
            -The requirement to implement a separate cache service adds
            complexity to the solution

Cache doesn't need to hold all the values for multivalued objects e.g:
    -Store bank customer name/address/etc
    -Don't store bank balance

-----Seed Cache-----

Noncritical/does not require auditing dynamic cache may help. Directly change
on chace (write-back)

Application must not be dependent on cache, if cache service is down, go to
database.

Do a local cache + shared cache
Failover cache, 

## Application Server Cache

Cache directly on a requesting layer node enables the local storage of 
response data. Cache can be located both in memory and on node's local disk
(which is faster than goign to network storage). Each node can have it's own 
cache but if load balancer randomly distributes requests across the nodes, the
same request can go to different node, thus increasing cache miss therefore
increase overall time due to the search in cache then search in storage.

To overcome this hurdle, two types of cache can be used :
    -Global Caches
    -Distributed Caches

## Content Distribution Network

CDN are cache for sites serving large amount of static media on the edge
servers that are close to end users to minimize latency

In a typical CDN setup:
    1st. request will first ask the CDN for a peice of static media
    2nd. CDN will serve taht content if it has it locally available
    3rd. If it is available, CDN will query the back-end servers for the file,
    cache it locally, and serve it to the requesting user

Future transition by servin the static media off a separate subdomain using a 
lightweight HTTP server like NGINX, and cut-over the DNS from your servers to
a CDN later.

## Cache Invalidation

*Cache Invalidation:* Making cache consistent from the database 

Three main schema that are used:
    -Write-Through cache: Data written into cache the ninto database at the same
    time. Write-through snsures that nothing willg et lost in crash, power failure
    but write operation must be done twice before returning. -> higher latency
    for write oeprations
    -Write-around cache: Only write to permanent memory
    -Write-back cache: Written to cache alone but written to memory after a
    specified interval. Lower latency and high throughput but speed comes
    with a data loss in case of crash.

## Extra Notes

WHY NOT CACHE
Additional latency if miss external cache
Additional cost for external cache
Application complexity - your application needs to handle more complexity
External Caching ruins database caching - not utilizing database cache

## Conditions for Caching 

*Data Contention:* multiple processes or instances competing for access to the
same index or data block at the same time

Most effective caching when a client repeatedly reads the same data,
especially if all the following conditions apply to the original data store:
    -It remais relatively static
    -It's slow compare to the speed of the cache
    -It's subject to high level of contention
    -It's far away when network latency can cause access to be slow

## Cache Policies

    -Least Recently Used
    -Least Frequently Used
    -Most Recently Used
    -First in First out

## Managing Concurrency in Cache

    -Optimistic
    -Pessimistic

## System Design Caching

Least Frequently Used

--Memcached allows you yo take memory from parts fo your system where you have
more than you need and make it accessible to areas where you have less than you
need

-- Biggest problem for cache is concurrency due to most policies every access
is a wrtie to some shared state. Traditionally guard the cache with a single
lock. This thens to limit benefit. When contention becomes a bottleneck,
classic step has been to update only per entry metadata and use rando sampling
or FIFO-based eviction policy. --> Great read performance, poor write. Another
would be to keep a log and replayed in aysnchronous batches (Similar to
databases)A

-- For searching, perhap use a search cluster, each node builds and maintains
an index over its lcoal document fcollection. All nodes in the clsuter
contribute to prcoessing of a query
    Querry Process involves:
        -Query to search node
        -computing partial result ranking in all nodes
        -Merging partial ranking to obtain a global top-k result set
        -computing snippets for the top-k documents
        -Generating the final result page

