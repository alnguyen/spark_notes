---
title: 12 OTP
tags:
  - elixir
  - otp
  - phoenix
---

# 12 OTP
----
- You've lightly sampled OTP concepts including applications and supervision trees.
- OTP is a way to think about fault-tolerance, concurrency, and distribution.
- It uses a few patterns that allow you to use concurrency to build state without language features that rely on mutability.
- Has rich abstractions for supervision and monitoring.
- Phoenix itself, is an OTP app

## Managing State with Processes
- Functional programs are stateless, but we still need to be able to manage state.
- In Elixir, we use concurrent processes and recursion to handle this task.
- We're creating a `Counter` server that counts up or down
  - Our module implements a `Counter`server as well as functions for interacting with it as a client
  - _client_ serves as the API aand exists only to send messages to the process that does the work
  - It's the _interface_ for our counter
  - The _server_ is a process that recursively loops, processing a message and sending updated state to itself.
  - Our server is the _implementation_

### Building the Counter API
- `pid` process ID for a server process
- `make_ref()`: creates a unique reference for a response to a request
  - Is just a value that's guaranteed to be globally unique
- Message payload is a 3-tuple with an atom designating the command we want to do, followed by our process ID, `pid`, and the globally unique reference.
- The `^` operator means that rather than rebinding the value of `ref`, we match only tuples having that exact `ref`
- We start by defining the client API to interact w/ our counter--`inc/1` and `dec/1` functions.
  - These functions fire off an async message to the counter process w/o waiting for a response.
  - The `val/2` function sends a message to the counter but blocks the caller process while waiting for a response.
- OTP requires a `start_link` function.
  - Its only job is to spawn a process and return `{:ok, pid}` where `pid` identifies the spawned process.
- Our `listen` function doesn't hold any state, but it exploits recursion to manage state.
- The server is wrapped up in the execution of the recursive function.
- We can use Elixir'sa message passing to listen in on the process to find the value of the state at any time.
- When the last thing you do in a function is to call the function itself, the function is _tail recursive_, meaning it optimizes to a loop instead of a function call.
  - That means this loop can run indefinitely.

### Taking Our Counter for a Spin
- We used concurrency and recursion to maintain state.
- We separated the interface from the implementation
- We used different abstractions for asynchronous and synchronous communication with our server.

## Building GenServers for OTP
- The library encapsulating the approach of managing cocurrent state and behavior is called OTP.
  - The abstraction is called a _generic server_, or `GenServer`
- When we want to send async messages such as our `inc` and `dec` messages, we use `GenServer.cast`
  - These functions don't send a return reply.
- When we do want to send synchronous messages that return the state of the server, we use `GenServer.call`
  - Notice the `_from` in the function head
    - You can use an arg leading with an underscore just as you'd use a `_` as a wildcard match.
    - We can explicitly describe the arg while ignoring the contents
- On the server side, the implementation is much the same
  - We use a `handle_cast` for `:inc` and `:dec`, each returning a `:noreply` alongside the new state.
  - We also use `handle_call` to handle `:val` and specify the return value.
- We explicitly tell OTP when to send a reply and when not to send one.
- The `start_link` also changed, starting a `GenServer` by passing in the current module name and the counter.
  - This spawns a new process and invokes the `init` function inside the new proccess
- The first counter was split into client and server code.
  - This segregation remains when we write our `GenServer`.
  - `init`, `handle_call`, and `handle_cast` run in the server.
  - All other functions are part of the client.
- We've gained much by moving to a `GenServer`
  - We no longer need to worry about setting up references for synchronous messages.
  - Those are taken care of for us by `GenServer.call`
  - `GenServer` module is now in control of the `receive` loop, allowing it to provide great features like code upgrading and handling of system messages, which will be useful when we introspect our system with Observer

### Adding Failover
- The benefits of OTP go beyond simply managing concurrent state and behavior.
- It also handles the linking and supervision of processes
- Our supervisor needs to be able to restart each service the right way, according to the policies that are best for the application.
  - ex. If a DB dies, you might want to automatically kill and restart the associated connection pool.
    - _This policy decision should not impact code that uses the database_
- If we replace a simple supervisor process with a supervisor tree, we can build much more robust fault-tolerance and recovery software.
- We need to trust the error reporting to log the errors so that we can fix what's broken, and in the meantime, we can automatically restart services in the last good state.
- With a supervision tree having a configurable policy, you can build _robust self-healing software_ without building _complex self-healing software_
- To specify the children an Elixir app will start, we define a _child spec_
  - You'll specify a single element containing a two-tuple having the module you want to start and the value that will be received on `start_link` by the GenServer.
  - Alternatively, passing only a module name uses a default value of `[]`
- In `opts`, you can see the policy that our app will use if something goes wrong.
  - OTP calls this policy the _supervision strategy_
- `:one_for_one` strategy means that if the child dies, only that child will be restarted.
- `:one_for_all`: if all resources depended on some common service, we could kill and restart all child processes if any child dies.
- As with channels, out-of-band messages are handled inside the `handle_info` callback
- When our program crashed, the supervisor identified the crash, and then restarted the process in a known good state.

### Restart Strategies
- First decision you need to make is to tell OTP what should happen if your process crashes.
- If we decide to use anything beyond the module to start and the initial value for the OTP server, we'll need a way to specify those options.
  - That's called a _child spec_, which configures the policy for an OTP restart
- You have some options for defining those options.
  - You can do it within the children definition in `application.ex`
    - You can use the `Supervisor.child_spec` function
    - ex. `restart: :permanent`
    - Having to specify the supervision values every time we list our server would be repetitive and error prone.
    - Can define these values directly in the module
    - ex. `use GenServer, restart: :permanent`
    - Behind the scenes, this code works because `use GenServer` defines a `child_spec(arg)` function, which returns the child specification.
    - Most of the time, the `use` option is enough.
    - When you need more, you can always define your own `child_spec(arg)` function
- Child specifications support the following restart values:
  - `:permanent` - The child is always restarted (Default)
  - `:temporary` - The child is never restarted
  - `:transient` - The child is restarted only if it terminates abnormally, with an exit reason other than `:normal`, `:shutdown`, or `{:shutdown, term}`
- `:permanent` is the default restart strategy and the trailing options are fully optional.
- Say we have a situation in which _mostly dead_ isn't good enough.
  - When a process dies, we want it to really _die_
  - Perhaps restarting the server would cause harm.
- `:temporary` strategy is useful when a restart is unlikely to resolve the problem, or when restarting doesn't make sense based on the flow of the application.
- Sometimes you may want OTP to retry an operation a few times before failing.
  - You can do exactly that with options called `max_restarts` and `max_seconds`.
  - OTP will only restart an application `max_restarts` times in `max_seconds` before failing and reporting the error up the supervision tree.
    - By default, Elixir will allow 3 restarts in 5 seconds.

### Supervision Strategies
- Just as child workers have different restart strategies, supervisors have configurable supervision strategies.
- The most basic and default for new Phoenix apps is `:one_for_one`
  - When a `:one_for_one` supervisor detects a crash, it restarts a worker of the same type without any other consideration.
  - Most of the time, `:one_for_one` is enough but sometimes, processes depend on one another.
    - When such a process dies, more than one process must restart.
- There's more than one strategy provided by Elixir:
  - `:one_for_one` - If a child terminates, a supervisor restarts only that process.
  - `:one_for_all` - If a child terminates, a supervisor terminates all children and then restarts all children.
  - `:rest_for_one` - If a child terminates, a supervisor terminates all child processes defined after the one that dies. Then the supervisor restarts all terminated processes.
- If using maps as child specifications, make sure the `:id` keys are unique.
- If using a module or `{module, arg}` as child, use `Supervisor.child_spec/2` to change the `:id`
- The `GenServer` is the foundation of many different abstractions throughout Elixir and Phoenix

### Using Agents
- _agent_ - a simpler abstraction that has many benefits of a `GenServer`
- You have only five main functions:
  - `start_link` initializes the agent
  - `stop` \stops the agent
  - `update` changes the state of the agent
  - `get` retrieves the agent's current value
  - `get_and_update` performs the last two operations simultaneously.
- To initialize an agent, you pass a function returning the state you want
- To update the agent, you pass a function taking the current state and returning the new state
- Behind the scenes, this agent is an OTP `GenServer`, and plenty of options are available to customize it as needed.
  - One such option is called `:name`

### Registering Processes
- With OTP, we can register a process by name with the `:name` option in `start_link`
- After we register a process by name, we can send message to it using the registered name instead of the pid.
- If a process already exists with the registered name, we can't start the agent.
- Agents are one of many constructs built on top of OTP.

### OTP and Channels
- Supervisors aren't just tiny isolated services.
- Channels are core infrastructure.
- All of our infrastructure is built with a tree of supervisors, where each node of the tree knows how to restart any major service if it fails.
- Channels is an OTP application.
  - Each new channel was a process built to serve a single user in the context of a single conversation on a topic.

## Designing an Information System with OTP

### Planning our Supervision Strategy
- Say we want to access relevant information for a user in real time, across different backends.
  - We're fetching results in parallel
  - A failure likely means the network or one of our third party services failed.
  - We want to spawn processes in parallel and let them do their work, and we'll take as many results as we can within some limited block of time.
- If you find yourself in a position of spinning off some concurrent process w/o the need to supervise the process, you can usually use a _task_ without having to specify any supervision at all.
  - You'll often start several tasks to do high-latency jobs and then wait for them to finish.
  - A parent process starts the asynchronous tasks, which are linked processes.
  - If either of them fails, they are linked so our parent will also fail.
  - You don't want to link processes if you can't address a failure in it, but want the parent to continue still.
    - That means you can't use `Task.async` and `Task.await`
- We always want to start processes inside supervision trees for cleanup and discoverability.
  - Each process we start should obey its explicit start and shutdown rules, so we'll clean up effectively and be able to view those supervised processes through tools like Observer
  - Therefore, we'll start our tasks through `Task.Supervisor.async_nolink` instead of the typical `Task.async`

### Building a Cache Server Without Bottlenecks
- A shared service to write things to memory: `:ets`
  - Erlang Term Storage
  - An in-memory storage solution that's included w/ OTP that allows you to store and retrieve any valid Erlang or Elixir data
- Since the cache is in-memory, itis not shared between different Phoenix nodes.
  - Every time the node starts, the cache starts empty.
- ETS tables are owned by a single process, and the table's existence lives and dies with that of its owner.
  - We don't have to worry about state values or cleanup when a process stops, either naturally or via a crash.
- `:set` is a type of ETS table that acts as a key-value store
  - `:named_table` option allow us to locate this table by its name
  - `:public` lets processes other than the owner read and write values
- If we don't remove old values from our cache, our memory footprint will just grow.
- `:interval` will define the amount of time between sweeps
- `:timer` will hold the `pid` for a timer, and `table` will hold our ETS table.

### Using Tasks to Fetch Data
- `Task.Supervisor.async_nolink` to spawn off the new task.
  - Spawns off a task in a new process, calling the function we specify
  - `async_nolink` is used to spawn the task isolated from the caller so we don't have to worry about a crash or unexpected error
    - If a result doesn't come back from one of the tasks, we just discart the result and the supervisor will kill it.

## Building the Wolfram Info System
- Since all of our backends will have the same contract, this is a perfect use case for a backend _behaviour_
  - A _behaviour_ is a contract, a common API across modules.
  - We have seen OTP behaviours, such as GenServer and Supervisor, as well as behaviours from libraries like Plug
    - Remember, each plug implements two functions, `init/1` and `call/2`
- We define two functions.  We don't actually declare a function.
  - Instead we use _typespecs_, which specify not just the name of our functions but also the types of arguments and return values.
- You should not check in private credentials under version control.
  - Phoenix points you in the right direction with `config/prod.secret.exs`
  - That file references environment variables that are securely set on the productionn server, meaning you can establish sensitive configuration in your local development environment without checking secret values into version control.
- We specify our `compute` function as an implementation of a behaviour with the `@impl true` notation
  - That model attribute is not required but it makes our intentions clear.
    - Users of our modules can immediately tell which functions implement our behaviour and which ones don't
- `:https` ships with Erlang's standard library
  - Does straight HTTP request
- We can wait for each task to complete with `Task.await`
- `flush()` helper is used from IEx to see any messages we've received
  - Can just return `:ok` if the message isn't in our inbox yet

### Monitoring Processes
- `{:DOWN, ...}`
  - Internally, the Task lib sets up a monitor from the caller process to the Task.
  - If we wanted to, we could use `Process.monitor` to detect backend crashes while we're waiting on results.
  - The `:DOWN` message informs us that our process died.
  - We won't be monitoring our backends directly w/ `Process.monitor` because the Task module calls it for us

### Working with Task Tools
- Task _yielding_
  - While `Task.await` would crash the caller should a given task time out, `Task.yield` blocks the caller, returning the result, an error, or nil, depending on whether a reply is received.
- `Task.yield_many` gives us the ability to wait on _all tasks_, taking no more than a given time for total execution.
- Theoretically, a task could complete between when we ask for the `yield_many` and when we actually process the results.
  - In this case, we still want to make sure to kill the task.
- We were able to add complexity such as isolated failures and timeouts to the combiend information system service w/o changing the policies for individual backends.
  - Because each backend is simply synchronous code running inside a new task process, we can leverage everything in OTP to make our system resilient without changing the business code.

### Caching Results
- We used `:timer.tc` to measure the execution time in microseconds to run the given module, function, and args.
  - We saw the call time decrease on subsequent queries

## Integrating OTP Services with Channels

## Wrapping Up
- You built a solid understanding of how OTP uses concurrency and message passing to safely encapsulate state without implicit state, or instance or global variables.
- You created a new child app under our umbrella
- You built a counter that demos how some OTP behaviours work.
- You looked at several OTP supervision and restart strategies
- You saw examples of a full OTP service as `GenServer`
- You learned how tasks wrap behavior and agents encapsulate state
- You implemented an ETS backed cache, with GenServer powered cache expiration
- You implemented an information system abstract fronted with concrete backends
- Yoou learned to fetch WolframAlpha results from an HTTP service and share them with our channels
