---
title: 03 Controllers
tags:
  - elixir
  - phoenix
---

# 03 Controllers
----

## Understanding Controllers
- Going to build out *rumbl*
- Handling users

```elixir
connection
|> endpoint()
|> router()
|> browser_pipeline()
|> UserController.action()
```

### The Context
- In Phoenix, it's just a module that groups functions with a shared purpose.
- Encapsulates all business logic for a common purpose.
- A controller exists to work with context functions
  - It parses end user requests, calls context functions, and translates those results into something the end user can understand.
- The context doesn't know about the controller, and the controller doesn't know about the business rules.
  - Each slice of code has an isolated purpose.

### Creating the Project

### A Simple Home Page

### Working with Contexts
- ex. When you call `Logger.debug`, you are accessing the `Logger` context
- Contexts will group related functionality such as posts and comments
  - Encapsulating patterns such as data access and data validation
  - Decouples and isolates our systems into manageable, independent parts.

### Elixir Structs
- A limitation of maps is that they offer protection for bad keys only at runtime, when we effectively access the key.
- Structs allows us to know about such errors as soon as possible, often at compilation time.
- Syntax for structs and maps is nearly identical.
- A struct is a map that has a __struct__ key
  ```elixir
  iex> jose.__struct__
  Rumbl.Accounts.User
  ```

## Building a Controller

- We could create all of the routes needed by a user automatically with the resources macro.

## Coding Views
- The terms _view_ and _template_ are often used synonymously.
- When a controller finishes a task, a view is somehow rendered.
- A _view_ is a module containing rendering functions that convert data into a format the end user will consume, like HTML or JSON.
- Those rendering functions can be defined from templates.
- A _template_ is a function on that module, compiled from a file containing a raw markup language and embedded Elixir code to process substitutions and loops.
- Views are modules responsible for rendering.
- Templates are web pages or fragments that allow both static markup and native code to build response pages, compiled into a function.

## Using Helpers

- I/O list - simply lists of values which allow data to be efficiently used for I/O, such as writing values to a socket.
- The *view* functiom uses Elixir's *quote* to inject a chunk of code into each view.
- Contents of each *quote* are executed for each view.

## Showing a User

### Naming Conventions

- When Phoenix renders templates from a controller, it infers the name of the view module from the name of the controller module.
- The view modules infer their template locations from the view module name.

### Nesting Templates

- A view in Phoenix is just a module
- Templates are just functions.
- Each template in our app becomes a `rener(template_name, assigns)` clause in its respective view.
  - Rendering a template is a combination of pattern matching on the template name and executing the function.

### Layouts

- When we call *render* in our controller, instead of rendering the desired view directly, the controller first renders the layout view, which then renders the actual template in a predefined markup.
- Layouts are regular views with templates.
- `@view_module` and `@view_template` are special assigns.
- When you call *render* in your controller, you're actually rendering with the :layout option set by default.

## Wrapping Up
