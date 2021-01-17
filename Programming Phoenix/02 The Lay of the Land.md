---
title: 02 The Lay of the Land
tags:
  - elixir
  - phoenix
---

# 02 The Lay of the Land
----

## Simple Functions
- `|>` - pipe operator
- `connection |> phoenix`
  - `connection` is a struct
    - Contains information about the `request`

### Where Are All of the Diagrams?

### The Layers of Phoenix
```elixir
connection
|> endpoint()
|> router()
|> pipelines()
|> controller()
```

### Inside Controllers
- Model-View-Controller (MVC)
- Models access data, views present data, and controllers coordinate between the two.
- The controller is also a pipeline of functions, one that looks like this:

```elixir
connection
|> controller()
|> common_services()
|> action()
```

- In Phoenix, common services are implemented with Plug.
- Plug - a strategy for building web applications and a library with a few simple tools to enable that strategy.
- In Phoenix, all business logic encapsulated in contexts.

## Installing Your Development Environment

### Elixir Needs Erlang
- Erlang provides the base programming virtual machine.
  - Supports our base programming model for concurrency, failover, and distribution.

### Phoenix Needs Elixir
- OTP - the layer for managing concurrent, distributed services.

## Creating a Throwaway Project
- Task - an Elixir script

## Building a Feature
- Routes in Phoenix go in `lib/hello_web/router.ex` by default

### Using Routes and Params

### Pattern Matching in Functions
- When elixir encounters a `=` operator, it means "make the thing on the left match the thing on the right"

#### Atom Keys vs String Keys?
- Controllers: external params have string keys, but internally, we use key: value syntax.
- External data can't safely be converted to atoms
  - Atom table isn't garbage collected
- Explicitly match on string keys, then the app boundaries like controllers and channels will convert them to atom keys.

### Using Assigns in Templates
- `<%= %>`

## Going Deeping: The Request Pipeline
- Each web request is a function call taking a single formatted string--the URL--as an arg.
- Plug library
  - A specification for building apps that connect to the web.
- Each plug consumes and produces a common data structure called **Plug.Conn**
- Think of each individual plug as a func that takes a conn, doess something small, and returns a slightly changed conn.
- Each plug can transform the conn in some small way until you eventually send a response back to the user.
- When you hear request and response, you might think that a request is a plug function call, and a response is the return value.
  - Think of the reponse as just one more action on the conn:
  - `conn |> ... |> render_resposne()`
- Plugs are functions; your web apps are pipelines of plugs.

### Phoenix File Structure
```elixir
...
|-- assets
|-- config
|-- lib
|---- hello
|---- hello_web
|-- test
...
```

- Browser files like JS and CSS go into **assets**
- Phoenix configuration goes into **config**
- Your supervision trees, long running processes, and app business logic go into **lib/hello**
- Web related code--controllers, views, templates--go into **lib/hello_web**
- Tests in **test**

### Elixir Configuration
- Phoenix projects are Elixir apps and as such, have the same structure as other Mix projects.

```elixir
...
|-- lib
|   |-- hello
|   |-- hello_web
|   |   |-- endpoint.ex
|   |   |-- ...
|   |-- hello.ex
|   |-- hello_web.ex
|-- mix.exs
|-- mix.lock
|-- test
...
```

- **.ex** files contain Elixir code which compile to **.beam** files that run on the Erlang VM.
- **.exs** files are Elixir scripts.
  - Are _not_ compiled to **.beam** files
- Compilation happens in memory, each time they are run.
- All Mix projects have a common structure.
- **mix.exs** - contains basic info about the project that supports tasks like compiling files, starting the server, and managing deps.
- **mix.lock** - after compile, will include the specific versions of libs we depend on.
- **lib** directory
  - Support for starting, stopping, and supervising each app in **lib/app/application.ex**

### Environments and Endpoints
- Your app will run in an environment.
  - The environment contains specific config that your web app needs.

```elixir
...
|-- config
|   |-- config.exs
|   |-- dev.exs
|   |-- prod.exs
|   |-- prod.secret.exs
|   |-- test.exs
...
```

- Default supported envs
  - development (**dev.exs**)
  - test (**test.exs**)
  - production (**prod.exs**)
- master config file (**config/config.exs**)
- **prod.secret.exs** - responsible to load secrets and other config values from the environment variables
- Switch between environments via the `MIX_ENV` environment var
- An endpoint is the boundary where the web server hands off the connection to our application code.
- To summaryize:
  - An endpoint is a plug, one that's made up of other plugs.
  - Your app is a series of plugs, beginning w/ an endpoint and ending wtih a controller.
  
  ```elixir
  connection
  |> endpoint()
  |> plug()
  |> plug*()
  ...
  |> router()
  |> HelloWebController()
  ```
  
### The Router Flow
- Every traditional Phoenix app looks like this:

```elixir
connection
|> endpoint()
|> router()
|> pipeline()
|> controller()
```

- The endpoint has functions that happen for every request
- The connection goes through a named pipeline, which has common functions for each major type of request
- The controller invokes the model and renders a template through a view

### Controllers, Views, and Templates
- Two top level files, **hello.ex** and **hello_web.ex**
  - `Hello` module is an empty module which defines the top-level interface and documentation for your application
  - `HelloWeb` module contains some glue code that defines the overall structure to the web-related modules of your app
- Phoenix separates the views from the templates themselves
- The Erlang VM and OTP engine will help the app scale
- The endpoint will filter out static requests and also parse the request into pieces, and trigger the router
- The browser pipeline will honor `Accept` headers, fetch the session, and protect from attacks like cross-site request forgery (CSRF)

## Wrapping Up
- Phoenix is built using Erland and OTP for the service layer, Elixir for the language, and Node.js for packaging static assets
- Elixir build tool `mix` to create a new project and start the server
- Web apps in Phoenix are pipelines of plugs
- Basic flow of traditional apps is endpoint, router, pipeline, controller
- Router distribute requests
- Controllers call services and set up intermediate data for views
