# SignalR, WebSocket, HTTP polling

## SignalR

**SignalR** is an open-source library that simplifies adding real-time web functionality to apps

SignalR supports the following techniques for handling real-time communication (in order of graceful fallback):

* [WebSockets](#websocket)
* Server-sent events
* [HTTP long polling](#http-polling)

SignalR automatically chooses the best transport method that is within the capabilities of the server and client.

## WebSocket

**WebSocket** is a protocol that provides simultaneous two-way communication channels over a single TCP connection.

WebSocket is designed to work over HTTP ports 443 and 80 thus making it compatible with HTTP. To achieve compatibility, the WebSocket handshake uses the HTTP Upgrade header to change from the HTTP protocol to the WebSocket protocol.

The WebSocket protocol enables _full-duplex_ interaction between a web browser (or other client application) and a web server with lower overhead than _half-duplex_ alternatives such as _HTTP polling_, facilitating real-time data transfer from and to the server.

A **full-duplex** system allows communication in both directions, and, unlike **half-duplex**, allows this to happen simultaneously.

## HTTP polling

**HTTP polling** is a mechanism where the client keeps requesting the resource at regular intervals. If the resource is available, the server sends the resource as part of the response. If the resource is not available, the server returns an empty response back to the client.

**HTTP long polling** is a variation of HTTP polling where the connection continues and the client waits for a long time and the server only sends the response back either when the response is ready or a timeout occurs. In this scenario, the response is never empty, and unnecessary network calls are not made.
