# HTTP Methods, API Design, REST Architecture

This page will be following the REST architectual style for API design.

## Server Information

Before getting into the details of HTTP protocol, its important to know how
information and actions are done in the backend. Generally, HTTP request are
made to a server through an URI. URI is the identifier of a resource on the
server. A resource can be any object/data/services. Client interacts with them
in with a representation such as a JSON object. The URI can also be a
collection, such as a folder of *orders*.
 For webpages, they are generally created by same entity
 so the endpoint information is known and automatically set to request those
endpoints when a user logs onto their page. They can also be given.
 When a client request a URI, the backend server routes to the URI and
processes it depending on the method that was used and header details. Not all
methods are available and etc, it has to be implemented by the backend.

## How HTTP Works

HTTP is an application layer protocol that allows servers to communicate
between each other, mainly client-sever. HTTP consist of a request (the client)
and a response (the server). HTTP has multiple methods that the client can use
but the main ones are **GET, POST, PUT, PATCH, DELETE**. In addition to the
method that the client sends to the server URI, both request/repsponses contain
a **Header** and **Body** that contains additional information for
optimization/authorization/etc.

**GET:** Request information from the URI. No information from the body is
needed in the request

**POST:** Request URI (generally collection) to process information within the
body (generally used to **create** new resource or possibly update/ask for
resource)

**PUT:** Request server to replace URI with the requests body (used for update)

**PATCH:** Request partial changes to the URI

**DELETE:** Request to delete URI

**HEAD:** Request is similar to the GET request but no body is sent in the
response. Generally use to see if a resource exists

There is an important concept for all HTTP methods and how they interact with
the server. This concept is **idempotent**, meaning, if its idempotent, all
similar request will cause the same result. **GET, PUT, DELETE, HEAD** are
idempotent. The reason why **POST and PUT** are not is because the server
determines what is done with body of those request while the rest either
completely replace, delete, or reads.

### Key Differences Between HTTP Methods

Some key differences that are not mention above is caching. Repsonse headers
must add to the headder *Cache-Control* information to tell the
browser/external cache if its cacheable. For caching, it usually uses the URI
and HTTP method as the key. This leads to some information not being cacheable
such as PUT, DELETE. GET requests are the majority of caching while
occasionally POST and PATCH can be cached.

## REST Architecture

REST architecture is a way to design backend APIs using resources/collections
and following a list of constraints. This is used to decouple the client (web
page or other user services) and server implementation. These constraints are:
    -Client-Server Model:
    -Uniform Interface: (HTTP Methods)
    -Cacheable:
    -Stateless: A request only require information with that request
    -Layer System:

HTTP closely follows the architecture but RESTful API are considerly truly
restful when it reaches level 3
    -Level 0: Define one URI, and all operations are POST requests to this URI
    -Level 1: Create separate URIs for individual resources
    -Level 2: Use HTTP methods to define operations on resources
    -Level 3: Use hypermedia (HATEOAS)

## API Design

For API designs tips:
    -Organize API design around resource, not actions.
    -Avoid API that simply mirror internals. Client should not be expposed to
    internal implementations
    -Consistent naming conventions
    -Simply URI -> collection/item/collection and nothing more complex
    -Avoid 'chatty' web API that requires multiple requests
    -Avoid 'extrenous' requests by adding pagination/filter
    -Use proper status code return
    -Asynchronous should return 202 code and Location header that contains
    where the new URI is located or status

Since API can have different version, there are different types of practice on
how to deal with the versions via adding version number with the uri or some
other method.

Handle exceptions well and send back meaning information on what happen

## Additional

Simple users shouldn't touch database (use cache)
Filter, Pagination, Sorting
Offset pagination is bad, use keyset pagination

Pagination:
    -Don't use exactpage numebr if you don't need it
    -High page numbers
    -Last page number
    -Total number of rows

