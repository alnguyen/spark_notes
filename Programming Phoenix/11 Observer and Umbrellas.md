---
title: 11 Observer and Umbrellas
tags:
  - ecto
  - phoenix
---

# 11 Observer and Umbrellas
----
- Both channels and the MVC components support UIs and communicate directly w/ the business backend.
- It would be nice to be able to deal w/ the web and backend pieces of our system independently.
- We'll refactor and extract the web-centered and backend pieces of our application into their own projects called child applications.
  - We'll be able to test, develop, and deploy each child app independently.
- _umbrella_project_: a loose confederation of parts in a project.
- _child_application_: each app under an umbrella
- Easier to build an umbrella from scratch; harder to refactor existing apps into child apps under an umbrella.
- `Observer` is a tool that ships with Erlang to offer a visualization of exactly what's happening.

## Introspecting Applications with Observer
- An application in Elixir is a runtime concern with these responsibilities:
  - Applications package our code.
  - Supervisors can start and stop applications as a unit.
    - An app may have a supervision tree, which defines exactly which services to start when the application starts, and which services to shutdown when the app shuts down.
  - Applications provide unified configurations.
    - Each application has its own environment, which is a key-value store to host application configuration.
- `:observer.start()` opens up a GUI
- Observer is a great tool for understanding all running processes for your app.
- Initial tab gives you general information about Erlang and also statistics about your system and your app
- Processes tab lists all running processes in your system, providing a tremendous amount of visibility into the system.
  - Remember, in Elixir, almost all state exists in your processes.
  - Includes the Message Queue (MsgQ) for each process.
    - If a process has a very large message queue, it is likely that it is a bottleneck in your system.
- Applications tab is where you can see all of the apps that run on your system as well as each app's supervision tree.
  - Inspecting our supervision trees is a great way to analyze how complex our systems are.
- Observers allows us to trigger failures.
  - Right click a process in the tree and send it a kill signal, which will cause it to terminate.
  - Since our services are supervised, the supervisor will notice the failure and start a new instance of the same service in its place.
- By breaking our existing application into two smaller ones, the main benefit is better boundaries.
  - Can keep web and backend as two distinct applications.
    - They'll only use each other if they have explicit dependencies between them.
    - This may be a small benefit for some teams, but for others, it may make a drastic difference.
- Umbrella projects provide an alternative to maintaining code across repositories.
  - Instead of breaking apps into multiple distinct source-code repos, which adds overhead, the apps in an umbrella are managed and versioned together, under the same repo.

## Using Umbrellas
- Each umbrella project has a parent directory that defines:
  - The shared configuration of the project
  - The dependencies for that project
  - The apps directory with child applications

### Choosing an Approach
- Main goal of an umbrella application is to give us the freedom to work with distinct pieces of the application independently, while still allowing convenient common averarching tasks.
- When possible, it's best to _let Phoenix generators automate as much configuration as possible_.
  - Hand rolling configuration per app works but it's also more error prone
- `mix new --umbrella`
- `mix phx.new --umbrella`

### Creating a Skeleton

### Understanding Umbrella Configuration
- `:in_umbrella` flag in the `*_web/mix.exs` file.
  - It's set in the dependency tuple.
  - Now you can use the application as a dependency of `*_web` and Elixir will automatically start it _before_ starting the `:*_web` server
- At the end of the day, Elixir simply configures the project to use the configuration, dependencies, and build paths from the parent application.
- _all children applications share the same configuration and the same dependencies_
  - You can't have two different apps in the same umbrella that depend on two different Phoenix versions.
- So while umbrella projects do provide some isolation between children, all children still run on the same VM instance, sharing configuration and dependencies.

## Extracting Rumbl and RumblWeb

### Copying the `rumbl` Source Tree

### Copinyg the Web Source Files

### Moving Assets

## Wrapping Up
- We took time to break the project into bite-sized pieces.
- We used umbrellas, an Elixir construct that allows us to develop and test projects in isolation but integrate them into a whole.
- We used Observer to understand the importance behind applicatioons
- We extracted `rumbl` and `rumbl_web` into their own child umbrella project.
- We learned to identify configuration changes, including dependencies, supervision trees, and application configuration.
