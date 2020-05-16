# Web Security and the HTTP Protocol

## Web Security Vunerbilities

Some of the major vunerabilities in web technologies today are:
    -Cross-Site Scripting (XSS)
    -Cross-Site Request Forgery (CSRF)
    -Authentication

Although there are other vunernabilities like `SQL Injections`,
`Click-Jacking`, and `DNS Poisoning`, XSS/CSRF are the main concerns with HTTP
protocol.

## Cross-Site Scripting

XSS is a malicious attack due to poor sanitation that allows attacked websites
to inject client-side scripts into web pages viewed by other users.

## Cross Site Request Forgery

CSRF is a malicious attack that allows a website to do unwanted actions to a
different web page that the user is logged into. Certain conditions must be
applied for CSRF to occur. These conditions are:

    1.Web browsers behavior use cookies and http authentications
    2.Knowledge by the attack of valid web application URLs
    3.Application session management rely only on information known by browser
    4.Existence of HTML tags whose presence cause imeediate access to http
        e.g image tags

### Preventing CSRF

One way to prevet CSRF is to not use cookies a a form of authentication. Since
http requests cookies will automatically be sent by the browser, using other
forms of authentication will prvent CSRF. 

Another way using HTTP headers is by checking the Referrer within the HTTP
header. Once a http request is created by a malicious HTML tag, it will send a
request and the browser will attach a referrer. Same orgin should be checked.
The probelm with this is that browser/proxies can also not send referrers. It
can also easily be faked.

The best prevention for CSRF attacks is through the web developer. They should
create a hidden token within the HTML form that will be sent with the request.
This token will randomly be generated and checked by the server.

 Cookies also set CSRF token which through the same-origin policy, cross-origin
reads are not allowed. With each form request sent by the client, the cookie is
also sent with it and input the CSRF token into the hidden field to be double
checked. There is also a `double submit cookie` pattern

### Prevent XSS Attacks

Other than sanitation, there are a few ways to protect webpages from XSS
attacks within a webpage that is included within the HTTP protocol. These
headers are:
    -Content Security Policy
    -Setting Cookies to httponly

*Content Security Policy:* is a HTTP header that allows web administrators to
control resources the user agent is allowed to load for a given page. CSP
mostly involves specifying server origin and scritps endpoints, essentially
whitelisting to allow scripts and certain access depending on orgin.

*Cookies:* While defining cookies, when servers uses Set-Cookie header, they
can define that it can be accessed only by HTTP. This prevent scripts from
accessing cookies. Also use *Secure* in Set-Cookie header attribute

### HTTP CORS Header 

    -Tells browser at one origin, access to selected resource from a different
    origin

## Other Security in HTTP

*HTTP Strit Transport Security*: Tell browser that the site only works via
HTTPS
