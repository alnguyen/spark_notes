---
title: 14 What's Next
tags:
  - LiveView
  - elixir
  - phoenix
---

# 14 What's Next
----

## Other Interesting Features

### Supporting Internationalization with Gettext
- Gettext is an internationalization (i18n) and localization (110n) system
- Can automatically extract translations from your source code, reducing the burder on the dev
- Programmers often organize translations into namespaces called domains
  - Phoenix places Ecto messages in the errors domain by default

## Intercepting on Phoenix Channels
- When you broadcast a message, Phoenix sends it to the Publish and Subscribe (PubSub) system, which then broadcasts it directly to all user sockets.
  - This approach is called _fastlaning_ because it completely bypasses the channel, allowing us to encode the message once.
- Phoenix Channels also provide a feature called *intercept*, which allows channels to intercept a broadcast message before it's sent to the user.
- For each event that we specify in *intercept*, we must define a `handle_out` clause to handle the intercepted event.
- You can intercept an event and choose not to push it at all, in case you want to make sure that some clients don't receive specific events.
- Example
  - Imagine you have 10,000 users watchinga video at the same time.
  - Instead of using *intercept*, you could write code that includes a `:video_user_id` in the message, letting the client determine editability.
    - For this implementation, Phoenix would encode the broadcast once and send the message to all sockets
  - With the *intercept* implementation, Phoenix would send the message to the first 10,000 channel processes, one for each client.
    - While processing the *intercept*, each channel would independently modify the intercepted message and push it to the socket to be encoded and sent.
    - The cost of *intercept* is 10,000 extra messages, one per channel, as well as encoding those messages 9,999 times--again, once per channel--compared to the one-time encoding of the impl w/o intercept.
- Example
  - If you have old clients that haven't migrated to new code.
  - You could use *intercept* to retrofit the `new_annotation` broadcast into the old one.
    - In these cases, *intercept* would be an ideal solution.

### Understanding Phoenix Live Reload
- Phoenix Live Reload is composed of
  - A dependency called `file_system` that watches the filesystem for changes
  - A channel that receives events from the `file_system` app and converts them into broadcasts
  - A plug that injects the live-reload iframe on every request and serves the iframe content for web requests

### Phoenix PubSub Adapter
- By default, PubSub uses distributed Erlang to ensure broadcasts work across multiple nodes.
- PubSub is extensible--it supports multiple adapters
  - eg. Redis Adapter

### Phoenix Clients for Other Platforms
- Phoenix Channels are transport agnostic.

## Phoenix LiveView
- A library for building interactive, rich applications, that are bidirectional.
- Conceptually, LiveView:
  - Represents a web page as a function over web state
  - Establishes messages and callbacks to change that state
  - Allows browser events such as mouse clicks, form submits, and key presses to send events

### Establishing a Static LiveView
- For the simplest of examples, all of our code can live in two places: the router and the live view.
- `render/1` is a pure function that takes `socket.assigns` as its lone arg
- Every LiveView page is a simple pure function
  - Debugging is much easier than you might find in alternatives.
- LiveView is a Phoenix Channels implementation so the live view for each end user will run in its own process.
- When `router.ex` has a `live` route, it will call the `mount/2` function on that live view.
  - The `mount` function's job is to establish the initial state of the live view.
- This function is analogous to an `init` function in an OTP GenServer.
- `~L"""` sigil
  - Does everything necessary to render a LiveView
- It starts a process and will loop over messages, called `render/1` each time there's a new event.
- We can define fields in `socket.assigns` and access those directly to do substitutions in LiveView
  - Whenever that state changes, Phoenix LiveView will use channels to make sure that the changes to our state make it to the client.
- Initial lifecycle for a LiveView:
  ```elixir
  live(url, LiveView)
  |> mount
  |> render
  ```

### Processing Events in a Clock
- This simple LiveView (clock example) is a GenServer

### Handling Links in a Counter
- `phx-click`
  - This attribute signals the JavaScript code on the client to send a Phoenix Channels message to the client.
- We must init every `assigns` field so our `mount` function establishes the initial values
- Pipeline for an arbitrary event
  ```elixir
  handle_event(event, data, socket)
  |> render
  ```
- Remember, channels is built on OTP

### Implementing Autocomplete Forms
- LiveView will only send down the parts of the page that need to change

### Validating Forms
- `phx-change` fires on each form change
- `phx-submit` fires on submit
- We can render a template directly
- We can also render other live views
- `error_tag` fields that will show messages for a changeset when errors are present.
  - This code is not LiveView specific

### Learning More
- Live views can render other live views
  - Like this: `live_render(@socket, DemoWeb.ImageLive)`
- When LiveView sends down new content for a page, it sends down _only changes since the last render_. If there are no changes, nothing is sent.
- LiveView can handle other kinds of events too, including keystroke events for both key up and key down.
- It works seamlessly with Phoenix PubSub. Therefore, you can push changes down to the page at any time, like we did with channels.

## Phoenix PubSub 2.0
- Phoenix will no longer start the Phoenix PubSub as part of the endpoint.
  - You will need to explicitly start Phoenix PubSub in your supervision tree.

## Phoenix and Telemetry Integration
- Developers have a unified API for dispatching metrics and instrumentation.
- Provides a mechanism for collecting built-in VM metrics and a shared vocab for consuming and reporting those metrics.
- StatsD is just an example for a metric aggregation tool.

## Good Luck!
