# Steps for building a web with Nginx, Gunicorn, Django

    -Web Server: Nginx
    -Application Server: Gunicorn
    -Application: Django

## Domain Name System

ICANN (Main DNS, non-profit) allocate IP address for regional internet
registries in which they allocate for ISP, then the ISP allocate for networks,
and those networks for individuals. Must purchase domain to be able to be found
through DNS 

## Web Server - Nginx

A web server is used to handle requests pertaining to a domain. In addition to
handling the requests for the domain, majority of its tasks are to provide
static content to the users and forward dynamic content to the application to
later be received by back from the application server then sent to the user.

### Nginx

Nginx is a software that will host a static ip address. This static IP address
can be associate with a domain name through a DNS registar.

Nginx is a single threaded software that was design with the awareness of
concurency problems. Nginx is asynchronous, non-blocking, and has event-driven
connection handlign algorithm.

Some properties are:
    -Spawn workers to handle thousands of connections
        -Worker continously check for and process events
        -Process in event for-lopp w/ other connections which are processed 
        asynchronously.
    -Does not have ability to process dynamic content natively or host an
    application like django (although Nginx PLUS can). It forwards it and wait
    for rendered content to be sent back.
    -Primarily works with URIs, translating to file system when necessary. 
    Parse the URI itself and forwards whenever necessary
    -Server block within Nginx itnerprets the hosts being requests
    -Location block responsible for matching URI
    -Static files requests evenetually have to be mapepd to location on 
    the filesysten
    -Nginx requests primarily as URIs instead of filesystems allow nginx to 
    more easily function in both web. mail, and proxy
    -Can be used as a reverse proxy also
    -Handle/terminate SSL, load-balancing, caching

## Applicaiton Server - Gunicorn

Since python cannot handle native http requests, it requires an application
server to convert http requests through the Django **Web Server Gateway
Interface (WSGI - Whiskey)**. Since Application servers are not design to handle static
assets, those are taken care of by the web server. Additionally, since Gunicorn
handles requests synchronously, it makes it easy for it to be DOS intentionally
or accidentally. 

Forwarded requests can generally easily be seen through the URL pattern.

Some task for Gunicorn are:
    -Run pool of worker/processes/threads
    -Translate requests from nginx to be WSGI compatible
    -Translate WSGI repsonses of your app into proper http response
    -Calls python code whe na request comes in
    -Talk to many different web servers

For direct API, still need something like a reverse proxy to protect from
direct access from/to web. Reverse proxy is good for web acceleration such as
caching or compression inbound/ outbound data.
