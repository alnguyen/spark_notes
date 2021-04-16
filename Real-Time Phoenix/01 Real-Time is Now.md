---
title: 01 Real-Time is Now
tags: []
---

# Real-Time is Now

## The Case for Real-Time Systems

- Applications need to be able to reflect the most up-to-date information without requiring a use to take action.

## The Layers of a Real-Time System

- Consists of clients, a real-time communication layer, and back-end servers working together to achieve business objectives.
- Different levels of guarantee in a real-time system
  - Hardware systems that have strict time guarantees are considered to be "hard" real-time
    - ex. airplane control systems need to alwways respond within strict time limits
  - "Soft" real-time are near real-time that can have several seconds of delay.
- Clients
- Single Server
- Server Cluster

### On the Client

- Clients are the entry point too our appplication from the perspective of users.
- Exist to display data and controls to the user, send user requests to the server, and process incoming messages from the server.
- One of the most important functions of a client is to maintain a connection to the server at all times.

### Communication Layer

- Facilitates data exchange between a server and a client.
- Needs to be reliable.
- Persistent connection is one that lasts for many requests or even for as long as the client wants to stay connected.
- WebSockets as a general solution for the communication layer.
- Important that server and client application code is not tied to a particular communication technology.
  - If a new communication layer needs to be implemented in the future, it would be hard to replace due to coupling.

### On the Server

- In a real-time app, a client connects to a single server using the app's communication layer.
- Server keeps connection open for an extended period of time, often as long as the client wants.
- Traditional wweb request uses a short-lived connection.
- One major difference betwween a traditional web requests and real-time requests is statefulness.
  - HTTP web requests are stateless--server doesn't maintain state between requests.
    - Client making HTTP requests must send state, such as cookies, with each request.
  - Real-time server can associate state, such as user or application data, with a specific connection.
    - App will do less work and respond faster.
- A client connects to a single server, but an application has many clients issuing requests.
- In a stateless web-request world, it is possible for each server to exist in near-isolation so that one request doesn't affect another directly.
- Applications that maintain state and behavior across multiple instances are called distributed systems.

## Types of Scalability

- Scalable does not just mean performances.
- Have to consider performance, maintenance, and cost.

### Scalability of Performance

- Most common consideration of scalability.
- Successful scaled performance will have similar, or at least acceptably slower, response times with 1000 client connections as it does 50,000 client connections.
- Many aspects of performance that can affect our real-time application.
- The data store will be a very likely culprit of performance problems as an application grows.

### Scalability of Maintenance

- Maintenance occurs when wwe add new features, debug issues, or ensure uptime of an application over time.
- Maintenance is a hard concern to optimize.
  - We can often be blind to things that will be problematic in the future.
- Leverage programming best practices and clear boundaries in our application.

### Scalability of Cost

- Something that is easy to take for granted.
- Elixir, and more specifically Erlang/OTP apps, can have relatively low costs compared to other languages.

### Tension of Scalabillity

- The different types of scalability exist in tension with each other.

#### Performance vs. Cost

- Can often increase performance by paying for additional server resources--throwing hardware at the problem.
  - Does not address the root cause of the performance problems.
  - Can be more costly to spend dev time to fix.
  - Can be early in development and priority is new feature work.

#### Performance vs. Maintenance

- Writing high-performance code can also mean writing complex and harder-to-maintain code.
- One wway to increase app performance is to reduce or remove code boundaries in code.
  - ex. tightly coupling solutions.
  - Boundaries exist though, to create more understandable and maintainable code.
  - Removing layers could potentially reduce the ability to maintain the code in the future.
- Most apps should focus on maximizing maintenance ability.
  - Allowws new features to be easily added over time.

#### Maintenance vs. Cost

- Maintenance involves people, and people are expensive.
  - Reducing difficulty of maintenance can save developmennt hours and thus reduce cost.
- Can minimize cost by not fixing tech debt but could lead to increasing maintenance costs.

## Achieving Real-Time in Elixir

- Elixir is a functional programming language that enables scalable application development.
- Elixir is a low-ceremony language--emphasis on expressive syntax that conveys meaning of code quickly.
- Elixir builds on top of Erlang/OTP to provide an excellent foundation for soft real-time apps.
- Elixir leverages VM processes, often implemented as `GenServers`.
  - Allows for encapsullation and modeling of the various components of a real-time system.
- Data isolation and error isolation are handled for us, nearly freely, by using separate OTP processes for different elements of our real-time system.

## Building Real-Time Systems

- **Phoenix** is a web framework written in Elixir that drives productive web application development.
  - Phoenix Channels

## Wrapping Up

- You must plan for scalability when building a real-time application.
- Multiple types of scalability to consider
  - Performance, maintenance, and cost.
-
