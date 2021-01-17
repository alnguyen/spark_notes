---
title: 13 Testing Channels and OTP
tags:
  - channels
  - elixir
  - otp
  - phoenix
  - testing
---

# 13 Testing Channels and OTP
----
- We want to isolate our tests from _external request_ code

## Testing the Information System

### Testing Our Cache
```elixir
defp assert_shutdown(pid) do
  ref = Process.monitor(pid)
  Process.unlink(pid)
  Process.exit(pid, :kill)

  assert_receive {:DOWN, ^ref, :process, ^pid, :killed}
end
```
- `assert_shutdown` serves as a custom set of assertions to verify a server shuts down cleanly.
  - Starts a monitor and then unlinks the process.
  - Remove the link, otherwise killing the server would make our test process also crash.
  - Kill the process and make sure we get a `:DOWN` message on the monitor

```elixir
defp eventually(func) do
  if func.() do
    true
  else
    Process.sleep(10)
    eventually(func)
  end
end
```
- Helper to prevent tests from having to sleep for long periods while the test waits on an expected result.
  - Ideally, we want to have our tests react only to messages.
  - When it's not enough, we'll execute some `func` until it eventually returns `true`

### Testing the InfoSys
- Because the system interacts w/ an external interface, we have some decisions to make
- How do we isolate our test code from the internet requests?
  - We solve this problem by defining a stub called `TestBackend`
  - The module acts like our Wolfram backend, returning a response in the format that we expect.
  - Since we don't use the URL query string to do actual work, we can use this string to identify specific types of results we want our test backend to fetch.

#### What's the Difference Between a Stub and a Mock?
- Both are testing fixtures that replace real-world implementations.
- _stub_ replaces real-world libraries with simpler, predictable behavior
  - A programmer can bypass code that would otherwise be difficult to test
  - Stub has nothing to say about whether a test passes or fails.
  - ex. a `http_send` stub might always return a fixed JSON response.
  - A stub is just a simple scaffold implementation standing in for a more conplex real-world implementation
- _mock_ is similar, but it has a greater role.
  - It replaces real-world behavior just as a stub does, but it does so by allowing a programmer to specify expectations and results, playing back those results at runtime.
  - Will fail a test if the test code doesn't receive the expected function calls.
  - ex. A programmer might create a mock for `http_send` that expects the `test` argument, returning the value `:ok`. If the test code doesn't call the mock first with the value `test` and next with the value `test2`, it'll fail.
  - A mock is an implementation that records expected behavior at definition time and plays it back at runtime, enforcing those expectations.

### Incorporating Timeouts in Our Tests
- `@tag :capture_log` captures the log in the test so that our tests aren't printing out a bunch of noisy log messages when the exception fires
- Since we have code that has an external interface, it's best to test that part in isolation.

## Isolating Wolfram
- Our code makes an HTTP request to an external API, which isn't something we want to perform within our test.
- In the Elixir community, we want to avoid mocking whenever possible.
- Most mocking libraries, including dynamic stubbing libraries, end up changing global behavior
  - ex. by replacing a function in the HTTP client library to return some particular results
  - _These function replacements are global_, so a change in one place would change all code running at the same time.
  - _Tests written in this way can no longer run concurrently_
- The better strategy is to identify code that's difficult to test live, and to build a configurable, replaceable testing implementation rather than a dynamic mock.
- We'll make our HTTP service pluggable.
  - Our development and production code wil use our simple `:httpc` client, and our testing code can instead use a stub that we'll call as part of our tests.
- Our goal isn't to test the Wolfram service, but make sure we can parse the data Wolfram provides.

#### At What Level Should We Apply Our Stubs/Mocks?
- No single strategy works for every case.
- It depends on your team's confidence and the code being tested.
  - ex. If communicatioon w/ the endpoint requires passing headers and handling different responses, you might want to make sure that all of those params are sent correctly.
- One possible solution is the Bypass project.
  - Allows us to create a mock HTTP server that our code can access during tests w/o resorting to dynamic mocking techniques that introduce global changes and complicate the testing stack.

## Adding Tests to Channels
- Remember that underneath, channels are also OTP servers.
- Phoenix includes the `Phoenix.ChannelTest` module
  - Will simplify your testing experience
  - You can make several types of common assertions
    - ex. assert that your app pushes messages to a client, replies to a message, or sends broadcasts.
- `ExUnit.CasteTemplate` establishes this file as a test case.
- `using` block starts an inline macro
- 1quote` specifies the template for the code that we want to inject.
- `use Phoenix.ChannelTest` establishes `Phoenix.ChannelTest` as the foundation for our test file.
- The result is a file that prepares your tests for the features you're most likely to use in your channel tests.

## Authenticating a Test Socket
- `connect` helper simulates a `UserSocket` connection

## Communicating with a Test Channel
- We use `connect` to start a simulated socket connection
- We did not pass `async: true` flag to our `ChannelCase`
  - In Ecto's `Sandbox` mode, every process has its own cnonection.
  - That's not a problem in apps that limit DB access to a single process.
  - The test case starts a transaction, modifies the DB

## Wrapping Up
- We tested our OTP layer for our InfoSys OTP app
- We split out an independent caching layer for performance
- We built a specific backend rather than a dynamic stub or mock to keep our tests isolated, as our unit and integration tests should be
- We tested our sockets authentication code
- We used the Phoenix testing support to test our channels
- We did not cover user acceptance testing
- We did not cover performance testing
-
