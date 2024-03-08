# SignalR, WebSocket, HTTP polling

## Table of contents

- [SignalR, WebSocket, HTTP polling](#signalr-websocket-http-polling)
  - [Table of contents](#table-of-contents)
  - [SignalR](#signalr)
  - [WebSocket](#websocket)
  - [Server-sent events](#server-sent-events)
  - [HTTP polling](#http-polling)

## SignalR

**SignalR** is an open-source library that simplifies adding real-time web functionality to apps

SignalR supports the following techniques for handling real-time communication (in order of graceful fallback):

- WebSockets
- Server-sent events
- HTTP long polling

SignalR automatically chooses the best transport method that is within the capabilities of the server and client.

## WebSocket

**WebSocket** is a protocol that provides simultaneous two-way communication channels over a single TCP connection.

WebSocket is designed to work over HTTP ports 443 and 80 thus making it compatible with HTTP. To achieve compatibility, the WebSocket handshake uses the HTTP Upgrade header to change from the HTTP protocol to the WebSocket protocol.

The WebSocket protocol enables _full-duplex_ interaction between a web browser (or other client application) and a web server with lower overhead than _half-duplex_ alternatives such as _HTTP polling_, facilitating real-time data transfer from and to the server.

A **full-duplex** system allows communication in both directions, and, unlike **half-duplex**, allows this to happen simultaneously.

## Server-sent events

**Server-sent events** or **SSE** is a server push technology enabling a client to receive automatic updates from a server via an HTTP connection, and describes how servers can initiate data transmission towards clients once an initial client connection has been established.

SSE are commonly used to send message updates or continuous data streams to a browser client and designed to enhance native, cross-browser streaming through a JavaScript [↑ `EventSource`](https://developer.mozilla.org/en-US/docs/Web/API/EventSource) interface, through which a client requests a particular URL in order to receive an event stream.

An `EventSource` instance opens a persistent connection to an HTTP server, which sends events in `text/event-stream` media type format. The connection remains open until closed by calling `EventSource.close()`.

Unlike WebSockets, server-sent events are unidirectional; that is, data messages are delivered in one direction, from the server to the client, such as a user's web browser. That makes them an excellent choice when there's no need to send data from the client to the server in message form. For example, `EventSource` is a useful approach for handling things like social media status updates, news feeds, or delivering data into a client-side storage mechanism like [↑ IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) or [↑ web storage](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API).

The EventSource API is standardized as part of HTML5.

## HTTP polling

**HTTP polling** is a mechanism where the client keeps requesting the resource at regular intervals. If the resource is available, the server sends the resource as part of the response. If the resource is not available, the server returns an empty response back to the client.

**HTTP long polling** is a variation of HTTP polling where the connection continues and the client waits for a long time and the server only sends the response back either when the response is ready or a timeout occurs. In this scenario, the response is never empty, and unnecessary network calls are not made.
