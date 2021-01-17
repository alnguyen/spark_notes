---
title: 05 Authenticating Users
tags:
  - elixir
  - phoenix
---

# 05 Authenticating Users
----

## Preparing for Authentication 
- *Comeonin* is a specification for password hashing libraries.
- *Pbkdf2* password hashing technique does not require any native dependencies
  - *Comeonin* has other options in their docs
- `:pbkdf2_elixir`, like other dependencies, is an application.
  - Remember, an application is a collection of modules that work together and can be managed as a whole.
- **application** function tells Mix how to start our `:rumbl` application
  - We configure Elixir to start `:logger` and `:runtime_tools` which are part of the standard lib

## Managing Registration Changesets
- `Ecto.Changeset.cast` function converts a raw map of input to a changeset
- Virtual schema fields in Ecto exist only in the struct, not the database.

## Creating Users

## The Anatomy of a Plug
- Two kinds of plugs
  - module plugs
  - function plugs
- A function plug is a single function
- A module plug is a module that provides two functions with some configuration details

### Module Plugs
- Allows you to share a plug across more than one module
- To satisfy the Plug spec, a module plug must have two functions, `init` and `call`
- Remember, a typical plug transforms a connection
- The main work of a module plug happens in `call`
  - The `call` will happen at _runtime_
- Sometimes, you might need Phoenix to do some heavy lifting to transform optins
  - That's the job of the `init` function.
  - Plug uses the result of `init` as the second arg to `call`
- In development, Phoenix calls `init` at runtime, but in production, `init` is called once, at _compile time_
- All plugs take a conn and return a conn
- `conn` is only a `Plug.Conn` struct.

### Plug.Conn Fields
- This structure has various fields that web apps need to understand about web requests and responses.
- Request fields contain information about the inbound request.
  - They're parsed by the adapter for the web server you're using.
- Cowboy is the default web server that Phoenix uses
- These fields contain strings, except where otherwise specified:
  - _host_: The requested host.
  - _method_: The request method
  - _path_info_: The path, split into a List of segments
  - _req_headers_: A list of request headers.
  - _scheme_: The request protocol as an atom
- Fetchable fields
  - A fetchable field is empty until you explicitly request it.
  - These fields require a little time to process, so they're left out of the connection by default until you want to explicitly fetch them
  - _cookies_: These are the request cookies with the response cookies
  - _params_: These are the request params. SOme plugs help to parse these from the query string, or from the req body
- Some fields that are used to process web requests and keep information about the plug pipeline:
  - _assigns_: User-defined map that contains anything you want to put in it.
  - _halted_: Sometimes a connection must be halted, such as a failed authorization.  The halting plug sets this flag.
  - _secret_key_base_: For everything related to encryption
- Some fields for the response:
  - _resp_body_: Initially an empty string, the ressponse body will contain the HTTP response string when it's available.
  - _resp_cookies_: Has the outbound cookies for the response
  - _resp_headers_: These headers follow the HTTP spec and contain info such as the response type and caching rules
  - _status_: Response code
- Plug supports some private fields reserved for the adapter and frameworks:
  - _adapter_: Information about the underlying web server is stored here.
  - _private_: Has a map for the private use of frameworks
- Initially, a `conn` will come in almost blank and is filled out progressively by different plugs in the pipeline.

## Writing an Authentication Plug

### Restricting Access
- Like endpoints and routers, controllers have their own plug pipeline.
- Plug pipelines explicitly check for `halted: true` between every plug invocation, so the halting concern is neatly solved by Plug.

### Logging In
- `configure_session(renew: true)` protects us from session fixation attacks.
  - Tells Plug to send the session cookie back to the client with a different identifier.

## Implementing Login and Logout

## Presenting User Account Links

## Wrapping Up
- We used our existing **Accounts** context to look up session users.
- We added the **pbkdf2_elixir** dependency to our project for password hashing.
- We built our own authentication layer.
- We built the associated changesets to handle validation of passwords.
- We implemented a module plug that loads the user from the session and made it part of our browser pipeline.
- We implemented a function plug and used it alongside some specific actions in our controller pipeline.
