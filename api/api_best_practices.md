# API Best Practices

## REST

Set Content-Type to applicaiton/json for response header
Use nouns not verbs for resources
Handle errors gracesfully, HTTP response codes that indicate what kind of error
occcurred.

Error codes need to have a message accompanied with them so that the
maintainers have enough information to troubleshoot the issue. But the attackers
cant use the error content to carry out atttacks like stealing information or
brinign dow nthe system.

ALlow filtering, sorting, and pagination.

Sort by: http://example.com/articles?sort=+author,-datepublished

- = ascending

* = decending

  Twitter, to get more information from a tweet via api, have to have
  tweets?ids=211&tweet.fields=public_metrics

## Cache data to improve performanceo

Express cache: apicache

versioning api /v1

## Common API Mistakes

- Be Stingy with Data for API because if an API provides that data,
  the API will basically always need to provide that data.

Always look at business needs and determine what absolute minimum amout of data
can be provided to satify those requirements.

- Create Domain Objects for APIs
- Forward compatible attribute naming.

  - New fields can be added/ignored by server but changing type or removing a
    field will break the client and should be avoided.

- Use positive, "happy" names: use shown: false instead of hidden: true

- HTTP RFC, uppercase letters for first letter of words: Content-Type
- Be liberal with what you accept from others, this meants taht in case of HTTP
  headers, you normalize each incoming header into a consistent format
  regardless of casing.
- Type coercing such as an api expecting a number but get a "640".

- Test all error conditions
- Write a middleware which converts any caught exceptions as a json response.
  - Try to upload a file which is too large
  - Send a payload with an incorrect type
  - Send malformed json requests
