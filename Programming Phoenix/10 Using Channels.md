---
title: 10 Using Channels
tags:
  - channels
  - elixir
  - phoenix
---

# 10 Using Channels
----
- Up to now, our app behaved like this:
  - Browser makes a request and a web server returns a response (Browser -> Server -> Response)
  - Each request is stateless, so it's easy to scale.
- In this chapter, we focus on the problems that Phoenix solves well--the problems that don't let themselves to a request/response flow.
  - Think live chats, Google Maps, kayak.com, and twitter.com
  - A single client on a page connects directly w/ a process on the server called a channel (Page <-> Server)
- Elixir can scale to millions of simultaneous processes that manage millions of concurrent connections
  - A client connects to a channel and then sends and receives messages.

## The Channel
- A Phoenix channel is a conversation.
- The channel sends messages, receives messages, and keeps state.
  - These messages are called _events_, and we put the state in a struct called a _socket_
- A phoenix conversation is about a _topic_
  - It maps onto app concepts like a chat room, a local map, a game, or in our case, the annotations on a video.
- The concept that makes channels so powerful in Elixir is that _each user's conversation on a topic has its own isolated, dedicated process_.
- Whereas request/response interactions are _stateless_, conversations in a long-running process can be _stateful_.
  - This means that for more sophisticated user interactions like interactive pages, you don't have to work so hard to keep track of the conversation by using cookies, DBs, or the like.
  - Each call to a channel simply picks up where the last one left off.
- This only works if your foundation guarantees true isolation and concurrency
  - _true isolation_:one crashing process won't impact other subscribed users.
  - _true concurrency_: lightweight abstractions that won't bleed into one another
- Your channels app will only have to worry about three things on both the client + server:
  - Making and breaking connections
  - Sending messages
  - Receiving messages
- Channel Presence

## Phoenix Clients with ES6
- Remember, each Phoenix conversation is on a topic
- When you _transpile_ a language, you're translating it to a more common form.

## Preparing Our Server for the Channel
- Previously, in the request/response protocol, every request that came in a new `conn` struct would start from scratch and flow through the pipelines and then die.
- In channels, the flow is different.
  - A client establishes a new connection w/ a socket.
  - After the connection is made, that socket will be transformed through the life of the connection.
- When you make a connection, you're creating your initial socket, and that same socket will be transformed with each new received event, through the whole life of the whole conversation.
- Each `socket` macro establishes a _socket mount_ providing all configuration for a single user socker.
- Our socket mount defines the transport layers that will handle the connection between client and server.
- Two default transports that Phoenix supports
  - _longpoll_
  - _websocket_
- `UserSocket` will use a single connection to the server to handle all of your channel processes.
- Regardless of the transport, you operate on a shared socket abstraction, and Phoenix takes care of the rest.
- In our `UserSocket`, we have two simple functions:
  - `connect`: function that decides whether to make a connection.
    - Receices the connection params, the connection socket, and a map of advanced connection information.
  - `id`: function that lets us identify the socket based on some state stored in the socket itself, like the user ID.

## Creating the Channel
- A _channel_ is a conversation on a _topic_.
- Generally, at their most basic level, _topics_ are strings that serve as identifiers..
  - Often taking the form of `topic:subtopic`
  - _topic_ is often a resource name
  - _subtopic_ is often an ID

### Joining a Channel
- Once a socket connection is established, users can join a channel.
- When clients join a channel, they must provide a topic.
- Users can join any number of channels and any number of topics on a channel.
- Transports route events into your `UserSocket`, where they're dispatched into your channels based on topic patterns that you can declare with the `channel` macro.

### Building the Channel Module
- With OTP naming conventions, the ability to join and disconnect connections and send events is sometimes referred to as _callbacks_.
- Remember, sockets will hold all of the stte for a given conversation.
  - Each socket can hold its own state in the `socket.assigns` field
- For channels, _the socket is transformed in a loop rather than a single pipeline_
  - The socket state will remain for the duration of a connection.

## Sending and Receiving Events
- Just as controllers receive requests, channels receive events.
- Each channel module has three ways to receive events.
  - _handle_in_: receives direct channel events
  - _handle_out_: intercepts broadcast events
  - _handle_info_: receives OTP messages

### Taking Our Channels for a Trial Run
- _handle_info_ is basically a loop.
  - Each time, it returns the socket as the last tuple element for all callbacks
  - This allows us to maintain state
- This is the primary difference between channels and controllers
  - Controllers process a request.
  - Channels hold a conversation

### Annotating Videos

### Adding Annotation on the Server
- `handle_in`: This function will handle all incoming messages to a channel, pushed directly from the remote client.
- `broadcast!` simply broadcasts events to all the clients on this topic.
  - Sends an event to all users on the current topic.
  - Takes three args: the socket, the name of the event, and a payload, which is an abritrary map.
  - Within the body of our callback, we can send as many messages as we'd like.
- Behind the scenes, `broadcast!` uses Phoenix's Publish and Subscribe (PubSub) system to send the message to all processes listening on the given topic.
  - PubSub is distributed out of the box; if there are multiple machines running Phoenix, they will all receive the message as long as they are connected via distributed Erlang.
- **Note**: Forwarding a raw message payload without inspection is a big security risk.
  - Broadcasting events delivers the payload to all clients on this topic.
  - If we don't properly structure the payload from the remote client before forwarding the message along as a broadcast, we're effectively allowing a client to broadcast arbitrary payloads across our channel.
  - For security, we want to control the payload as closely as we can.

## Socket Authentication
- For request/response-type apps, session-based authentication makes sense.
- For channels, _token authentication_ works better because the connection is a long-duration connection.
  - We assign a unique token to each user.
  - Tokens allow for a secure authentication mechanism that doesn't rely on any specific transport.
- You should not be able to access your session cookies in a channel
  - Insecure over WebSockets because of cross-domain attacks.
  - Cookies would couple channel code to the WebSocket transport, eliminating future transport layers.
- `Phoenix.Token`

## Persisting Annotations
- Allowing access to every possible type of data associated with a record could/would probably lead to a tedious, bloated API and conflate the purpose of your contexts.
- `render_many` function collects the render results for all elements in the enumerable passed to it.
- `render_one`function provides conveniences such as handling possible `nil` results.

## Handling Disconnects
- Any stateful conversation between a client and server must handle data that gets out of sync.
  - This can happen w/ unexpected disconnects, or a broadcast that isn't received while a client is away.
- We can't assume network reliability when designing real-time systems.
- We can track a `last_seen_id` on the client and bump this value every time we see a new annotation.

## Tracking Presence on a Channel
- Channel Presence solves the problem of tracking users.
  - Based on the standard Elixir library.
  - Built on OTP, it's self-healing.

### Generating Presence Files
- `Presence` is an OTP application so we need to add it to our supervision tree.

### Tracking Presence in Channels
- There is an important distinction between the _user_ and a _session_.
  - A user is a unique entity within the presence.
  - A user can have multiple sessions, such as a single user with open browser tabs or multiple devices.
- `RumblWeb.Presence.track` does a majority of the work.
  - Accepts our socket, a key to track, and a map of metadata.
  - The key is a _unique user identity_.
- Presence will maintain this data for the life of the user.
- When we track presence, we're asking Phoenix to track broadcast messages to our socket's topic about users coming and going.

### Adding Presence to Templates
- The JS Presence API takes care of all the housekeeping of synchronizing user info as users come and go.

### Using Channel Presence in JavaScript

### Decorating Entries with Application Data
- `Phoenix.Presence` provides a `fetch` callback to solve the problem of stale information across our cluster.
- As users join and leave the app across your cluster, Phoenix batches these events together to optimize performance and network chatter.
- `fetch` callback will fetch the data for _a batch of presences_, not just a single presence.
- `fetch` callback implementation is optional.
- `:metas` information contains data necessary for tracking presence data over a client.

## Wrapping Up
- You learned to build simple client/server APIs w/ Phoenix channels.
- You learned to connect to a server-side channel through an ES6 client.
- We built a server-side channel w/ both long-polling and WebSocket support.
- We built a simple API to let users join a channel.
- We processed inbound messages from OTP with `handle_info` and channels with `handle_in`
- We sent broadcast messages with `broadcast!`
- We authenticated users with `Phoenix.Token`
- We persisted annotations with Ecto and exposed those new features through our Multimedia context.
- We used Channel Presence to track the list of users on a video channel.
