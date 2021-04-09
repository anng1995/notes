# Network

## WebSockets

    -Bidirectional, server -> client and client -> server
    -Text or binary data
    -Must implement yor own meaning for the data sent
    -Possible trouble with firewall (use wss for encryption and helps prevent
    firewall/other network layer from blocking because of unfamiliar data
    format

## Server-Sent Events

    -Only Text-based data
    -Limit of maximum open connecitons
    -Transport over simple http instead of custom protocol
    -Unidirectional, server -> client
    -Simple API, auto-reconnects, build-in provision for catching missed events
    -Has Event id

## Http vs Http2

### Similarities

    -Request/Response

### Differences

    -HTTP2 can use a multiplex (single connection for multiple requests)
    -HTTP2 allows server to push data and compress HTTP headers

## TCP vs UDP

## Http2 vs WebSockets

    -Http2 requires request to respondwhile websocket doesn't
    -Http2 is stateless (REST) vs websockets are stateful

## Latency vs Throughput vs Traffic vs Congestion

## Random Notes

    -Each tabs uses a different process
    -Might be better to let proxy like nginx to deal with https and encryption
    due to cpu instensive

## XMLHttpRequest vs Ajax vs Fetch

    -XMLHttpRequest: raw browser object that is wrapped in jQuery (use
    callbacks)
    -Fetch: uses promise vs callbacks
    -Ajax: Async

## Http Headers

    -Content-Type
    -Accept
    -Cache-control
    -Etags
