---
title: 02 Connect a Simple WebSocket
tags: []
---

# 02 Connect a Simple WebSocket

- A critical piece of a real-time system is the communication layer that sits between the server and the user.
- In this book, we'll use WebSockets as our communication layer.

## Why WebSockets?

- RFC for the WebSocket protocol emerged wwith the HTML5 spec in 2011.
  - WebSockets gained native support by all major browsers.
- WebSockets allow for efficient two-way data communication over a single TCP connection.
  - Helps to minimize message bandwidth and avoids the overhead of creating frequent connections.
- Has strong support in Elixir with the cowboy web server.
  - Maps very well to the Erlang process model which helps to create robust performance-focused applications.
- Originates with an HTTP request, which means that many standard web tech such as load balancers and proxies can be used with them.
- Able to stay at the edge of our Elixir app.
- Examples of usages:
  - Facebook Messenger
  - Yahoo Finance
  - Multiplayer games such as Slither

## Connecting our First WebSocket

- [Hello Sockets](https://github.com/alnguyen/hello_sockets)

## WebSocket Protocol

- WWe wwill make use of several parts of the WebSocket protocol, but not alll of it.
  - We'lll learn how to establish a connection, keep the connection alive, send and receive data, and keep the WebSocket secure.
- **Using the WebSocket RFC**
  - The RFC can be useful if you find yousrelf doing deep debugging into a WebSocket implementation.
  - Can be useful if you have extremely tight technical requirements that are not met by the standard implementation.
- We'll use Google Chrome's DevTools to walk through an example.

### Establishing the Connection

- Load up `http://localhost:4000`, open up dev tools > network > WS.
  - Find the connection labeled `websocket?token=undefined&vsn=2.0.0`
  - You'll see a feww things that reveal how a WebSocket connects.
    - Request headers, response headers, and an HTTP method (GET)
  - A WebSocket starts its life as a normal web request that becomes "upgraded" to a WebSocket.
  - If you cURL the same request, it receives a 101 HTTP response from the server.
    - Indicates that the connectioon protocol changes from HTTP to a WebSocket.
  - WebSockets operate over a TCP socket using a special data protocol.
  - The same TCP socket that the HTTP connection request went over becomes the data TCP socket after the upgrade.
- To summarize, a WebSocket connection follows this request flow:
  1. Initiate a GET HTTP(S) connection request to the WebSocket endpoint.
  2. Receive a 101 or error from the server.
  3. Upgrade the protocol to WebSocket if 101 is received.
  4. Send/receive frames over the WebSocket connection.

### Sending and Receiving Data
- In DevTools > Messages
  - Shows all messages that are sent to or received from the server.
- A WebSocket is capable of sending messages and receiving messages.
  - Two way data transmission can happen in both directions simultaneously.
  - A connection which is capable of two-way data transmission is called a full-duplex connection.
- WebSockets transmit data through a data framing protocol.
  - Provides security benefits and allows connections to work properly through different networking layers.
- Protocol contains extensions that provide additional functionality.
  - Extensions are requested by the client using the `Sec-WebSocket-Extensions` request header.
  - Server can optionallly use any of the proposed extensions and return the list of active extensions to the client in a response header named `Sec-WebSocketExtensions`.
  - Data frames are not compressed by default.
    - Can be using `permessage-deflate` extension.
    - Allows bandwidth to be reduced at the cost of processing power

### Staying Alive, Keep-Alive

- A disconnected WebSocket is unablle to send or receive data.
- WebSocket protocol specifies Ping and Pong frames
  - Can be used to verify that a connection is still alive.
  - Optional.
  - Phoenix does not use them.
  - Clients send heartbeat-data messages to the Phoenix Server they're connect to every 30 seconds.
    - Phoenix WebSocket process will close a connection if it doesn't receive a ping within a timeout period.
- Predictable heartbeat for connections can be very useful.
  - Connection can be dead but not closed properly.
    - Results in the connection staying active on the server.
- Useful that the client manages the heartbeat rather than the server.
  - The server is aware of the connectivity problem but can not establish a new connection to the client.
  - Whereas, if the client is aware of connectivity problem, it can attempt to reconnect.

### Security

- Use `wss://` URIs to ensure a secure connection.
  - Our example is using `ws://` because it doesn't involve signing a local certificate for SSL
  - You should alwways use `wss` protocol in production to ensure security.
- `Origin` header of every connection request should be checked to ensure that it is coming from a known location.
  - Possible to be spoofed.
- WebSockets do not follow the same rules as standard web requests when it comes to cross-origin resource sharing (CORS)
  - It doesn't use CORS protections at all.
  - Cookies are sent to the server, even if the page initiating the request is on a different domain.
  - These cookies allow access to the server when access should be denied.
  - Cross-Site Request Forgery (CSRF) tokens can be a strategy to solve this problem.
- To prevent CSRF attacks, Phoenix has historically disallowed cookie access wwhen establishing a WebSocket connection.

## Long Polling, a Real-Time Alternative

- WebSocket is not the only real-time communication tech
- It is important for maintenance of aour apps that we do not design it solely around WWebSocket usage.
- One alternative is HTTP long polling.

### What is Long Polling?

- A technique that uses standard HTTP in order to asynchronously send data to a client.
  - Fits the requirement of a real-time communication layer that can send (long poll response) and receive (client request) data from a client.
- Most frequently used predecessor to WebSockets.
- Uses a request flow as follows:
  1. Client initiates an HTTP request to the server.
  2. Server doesn't respond, instead leaves it open. The server will respond when it has new data or too much time elapses.
  3. Server sends a complete response to the client. At this point the client is aware of the real-time data from the server.
  4. Client loops this flow as long as the real-time comm is desired.

- Key component of long polling flow is that the client's connection to the server remains open until new data is received.
- Viable for real-time communication, but there are challenges.

### Should You Use Long Polling?

- Bassed solely on top of HTTP
  - WebSockets uses HTTP only for a small part of its flow.
- Some, but not all, challenges you may face when using long polling
  - Request headers are processed on every long poll request.
    - Can potentially, dramatically increase the number of transmitted bytes which need to be processed by the server.
    - Not optimal for performance.
  - Message latency can be high when a poor network is being used.
    - Dropped packets and slower data transit times can make latency much higher because multiple connections have to complete in order to reestablish the long polling connection.
    - Can affect how real-time the app feels.
- Both problems can affect performance and scalability.
  - WebSockets are not prone to therse issues because the protocol is much lighter than full HTTP requests, requiring less data overhead and network round trips.
- Still, long polling can be useful.
  - Can be load-balanced across multiple servers easily, because the connections are being established often.
    - WebSockets can be tricky to load balance if the connections have a long life.
    - Longer connections provide fewer opportunities to change which server a client is connected to.
  - Can transparently take advantage of protocol advancements such as future versions of HTTP.
- Phoenix ships wwith both a WebSocket and a long polling comm layer out of the box.
- Other real time comm techniques
  - Server-sent events

## WebSockets and Phoenix Channels

- WebSockets map very well to the Erlang/OTP actor model and are leveraged by Channels in Phoenix.
  - We'll be using Phoenix Channels with WebSockets throughout this book.
- Phoenix and Elixir make it easy to have tens of thousands of connections on a single server.
- Each connected Channel and WebSocket in your app has independent memory management and garbage collection because of OTP processes.
  - An advantage of this process based architecture is that WebSocket connections which are not being used often can be stored in a hibernated state.
  - Great for scalability.
- Channels use several levels of processes wwhich provide fault tolerance and reduced memory usage across our app.
  - Important for scaling because it prevents app bottlenecks.

## Wrapping Up

- WebSocket protocol provides a strong real-time comm layer for our real-time apps.
- WebSockets start as normal HTTP requests before being upgraded to TCP sockets for data exchange.
