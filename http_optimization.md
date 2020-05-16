# Server Optimization via HTTP

HTTP Optimization will cover ways HTTP can be used to improve speed for user.

## Cache

There are two types of cache that HTTP helps to increase overall speed/improve
experience for user. These two different type of headers in HTTP are:

    -Set-Cookies
    -Cache-Control

Cache control headers tell browsers what content can be stored and if a content
is still fresh or stale.
Cookies are used for maintaining session and logged in state for users while
also storing information for users personalization.

### Cache Control

**NOTE: Web browser cache primary key typically consists of the request method
and target URI. This is the reason why some HTTP method like PUT is not
cacheable because PUT will always replace the current URI with request body**

While servers reply to HTTP requests, mainly the GET method, the server can
state if the content is cacheable. If it is cacheable, server will add an
attribute of `max-age=<seconds>` to the `Cache-Control` header. Adding a
`must-revalidate` attribute tells the browser the cache must be verified and
cannot use stale/expired cache. 
Cache can be used for many users or for a specific user. By using `private` or
`public` attribute, it tells if the cache must not be stored by a shared cache
such as an external cache. `no-store` is used to tell request/response should
not be stored anywhere. By using `no-cache`, cache will send a request to
origin server for validation before sending copy.

So how does a browser revalidate? They revalid either by using `ETags` or
`Last-Modified` with a HTTP conditional. Request attaches **Etags**  with 
a `If-None-Match`. Server sends 304 status code (No change) if still valid.
Modify uses `If-Modified-Since` .

One issue with caching is that `Content-Type` of the HTTP response can vary
such as XML or JSON. With the `Vary` HTTP response header that can be sent with
the content response, it tells the cache system that the request must also
match the vary header field or else it should not use the cached item. An
example would be `Vary: User-Agent` because cache for mobile/web can be
different.

**Additional Information: Because a lot of conditional request were made for
content that are generally staic, therefore increasing network load/latency,
Facebook developed the idea of hashing the content and using that as the URI of
the content. By doing this, if the content ever change, the URI location would
also change due to a new value. With the technique and with the help of
different browser company implementing, instead of sending a conditional
request, the URI is just checked. If it doesn't exist, the content has change.
Any additional conditional requests for servers that implemented this generally
sends a 304 (Not modified)**

### Cookies

Server response attaches a cookie to the response with the `Set-Cookie` header.
Depending on the type of cookie (session/Permanent) the cookie also contains
the `Expire=` time. For session cookies, they do not include `Expire` or
`Max-Age` to indicate that they are session cookies. Cookies are sent with each
request. Additional information such as `Secure` and `HttpOnly` are used with
`Set-Cookie` for increase security. `Secure` onlly how the cookie to be sent
the server if its encrypted request over HTTPS. `HttpOnly` prevent XSS attacks
by making it inaccessible to scripts. Another protection for cookies is the
`SameSite=Strict` attribute attached to Set-Cookies. This states that cookies
should not be sent by cross-origin requests. This helps protect against CSRF
attacks. Another protection through Set-Cookies is `CSRF=(string)`, this helps
protect against CSRF.

Domain and Path directives define scope of the cookie.

## Filters/Pagination

When using a single API that exposes a large collection, it can lead to
extranenous load on the server when user only needs a small subset. With
*pagination*, we can use parameters for the HTTP method such as limit and offset
to notify server the range to send. Eg `(/orders?limit=25&offset=50)`. Upper
limit should be set to protect from DDOS attacks. Additional, *filters* can be
similiarly used to find a specific subset the user is looking for.
Eg `(/orders?minCost=50)`

## Range Request 

Clients can askfor a partial response through a `Range` Header in their
request (If server accept range). Server sends `Accept-Range` header. Request
can also send `If-Range` conditional. When a range is sent by the server,
`Content-Range` is sent, which indicates the range of bytes sent out of the
total bytes available.
