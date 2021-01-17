---
title: 01 Introducing Phoenix
tags:
  - elixir
  - phoenix
---

# 01 Introducing Phoenix
----

## Productive
- Phoenix makes programmers productive.
- Web framework
  - A base architecture for your application
  - A database access and management library for connecting to databases
  - A routing layer for connecting web requests to your code
  - A templating language and helpers for you to write HTML
  - Flexible and performant JSON encoding and decoding external APIs
  - Internationalization strategies for taking your application to the world
  - All the breadth and power behind Erlang and Elixir so you can grow

### Productivity vs Maintainability
- Presenting customization introduces complexity
- One side of the line is productivity and the other is maintainability
- Phoenix is opinionated, favoring convention over configuration

### Functional Programming 101: Immutability
- In Elixir, data structures are immutable
- Can not change data structures, only create new ones

## Concurrent

### Types of Concurrency
- For our purposes: conurrency is a web application's ability to process two or more web requests at the same time.
- I/O
- Multi-core
  - Two ways to leverage multi-core concurrency
    - With an OS process per core: If your machine has four cores, you will start four different instances of your web applications.
    - With user space routines: If your machine has four cores, you start a single instance of your web app that is capable of using all cores efficiently.

### Simpler Solutions
- Elixir developers don't need to reach for complex solutions as soon as other developers in other languages

### Performance for Developers
- Elixir's test utilize the computers multiple cores to run concurrently.

### But Concurrency Is Hard
- Most issues w/ concurrency in traditional languages come from in-memory race conditions, caused by mutability.
- In Elixir, our user-space abstraction for concurrency is also called processes, but do not confuse them w/ OS processes.
- Elixir processes are abstractions inside the Erlang VM.

## Beautiful Code
- Elixir supports Lisp-style macros.
  - Like a template for code
- Macros are invaluable for extending the Elixir language.

### Effortlessly Extensible Architecture
- You'll roll up your functions into pipelines, where each function feeds into the next.

## Interactive

### Scaling by Forgetting
- Traditional web servers scale by treating each tiny user interaction as an identical stateless request.

### Processes and Channels
- Connectiosn can be conversations
- Channels
- Isolation and concurrency.
  - Isolation guarantees that if a bug affects one channel, all other channels continue running.
  - Breaking one feature won't bleed into other site functionality.
  - Concurrency means one channel can never block another one, whether code is waiting on the DB or crunching data.
  - UI never bcomes unresponsive because the user started a heavy action.
- Phoenix Channels scale vertically and horizontally.

### Presence and LiveView
- Tools for tracking presence--tracking which users are connected to a cluster of machines.
- Presence doesn't require any external dependencies.
- LiveView
  - "server-side React"
  - Simple:
    - A function renders a web page
    - That function accepts state as an input and returns a web page as output
    - Events can change that state, bit by bit

## Reliable
- By default, Phoenix has set up most of the supervision structure for you.
