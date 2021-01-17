---
title: 08 Testing MVC
tags:
  - ExUnit
  - elixir
  - phoenix
---

# 08 Testing MVC
----

- Principles to emphasize:
  - Fast: We're going to make sure our tests run quickly and can run concurrently wherever possible.
  - Isolated: We want to have the right level of isolation in our tests. Tests that are too isolated won't have enough context to be useful. Tests that aren't isolated enough will be difficult to understand and maintain.
  - DRY (Don't Repeat Yourself): We want to eliminate unnecessary repitition in our tests.
  - Repeatable: We want the same test on the same code to alwyas yield the same result.
- Terminology:
  - Unit test: Exercises a function for one layer of your application.
  - Integration test: Focuses on the way different layers of an application fit together.
  - Acceptance test: Tests how multiple actions work together
  - Performance test: Sees how your application performs under load.

## Understanding ExUnit
- Has three main macros
  - *setup* macro specifies some setup code that runs once before each test.
  - *test* macro defines a single isolated test.
  - *assert* macro specifies something we believe to be true about our code.
- `async: true` flag allows the tests in this module to run concurrently with tests defined in other modules.
- Tests in the same module are always executed sequentially, in random order to avoid implicit dependencies between tests.

### Using Mix to Run Tests

### Creating Test Data
- Libraries are like macros. Don't use one when a simple function will do the job.

## Testing Contexts
- Phoenix generates a module in `test/support/data_case.ex` to serve as a foundation for your tests that interact w/ the database.
- The `data_case` handles setup and teardown of the database and integrates with `Ecto.Sandbox` to allow concurrent transactional tests.
- The `using` block serves as a place for defining macros, common imports and aliases.
- We also see a `setup` block for handling transactional tests.
  - A transactional test runs a test and rolls back any changes made during the test.
  - Allows tests to reset the DB to a known state quickly between tests.

### Testing User Accounts
- Most context-related functionality will be tested with integratioon tests as they insert and update records, but not all.
- Error and exception flows are some of the trickiest parts of our application to get right.
- Sometimes you need to apply the same setup code to many different tests.
  - Describe blocks allow us to apply different test setups to a whole block of tests.

### Testing the Multimedia Context

## Using Ecto Sandbox for Test Isolation and Concurrency
- The role of the sandbox is to undo all changes we have done to the DB during our tests.
  - Database transaction rollbacks give us test isolation
- What sets Ecto Sandbox apart from other web frameworks and DB libraries is that it enables concurrent testing within the same DB.
  - You can run multiple tests that interact w/ the DB and they won't affect each other.
- This is a big deal because most devs must make compromises between slow tests that sequentially hit the DB and "fast" tests that stub out DB calls.
- Note that tests do not run concurrently by default.
  - Need to pass the `async: true` option

## Integration Tests
- One of the basic principles for testing is isolation, but that doesn't mean that the most extreme isolation is always the right answer.

### Warming Up with the Page Controller
- Use `Phoenix.ConnTest` to set up the API.
- Sets `@endpoint` module attribute, which is required for `Phoenix.ConnTest`
  - Let's Phoenix know which endpoint to call when you directly call a route in your tests.
- In the example test, we called our controller with `get conn, "/"` rather than calling the index action.
  - This ensures we're testing the router and pipelines because we're using the controller the same way Phoenix does.
- `html_response(conn, 200)` does a few things:
  - Asserts that the conn's response was 200
  - Asserts that the response `content-type` was `text/html`
  - Returns the response body, allowing us to match on the contents
- If our request had been a JSON response, we could have used `json_response`

### Testing Logged-Out Users

### Preparing for Logged-In Users
- We don't want to store anything directly in the session, because we don't want to leak implementation details.
- We choose to test our login mechanism in isolation and build a bypass mechanism for the rest of our test cases.
- Example given here is controversial.
  - Adding code to make implementation more testable.
  - The trade-off could be worth it
    - Improving the contract
    - If a user is in `conn.assigns`, we honor it, no matter how it got there.

### Testing Logged-In Users

### Using Tags
- Tagging allows you to mark specific tests with attributes you can use later.
- You can access these attributes from the test context blocks.
- Tests outside of the describe block will then skip the login requirement.
- `tag` module attribute accepts a keyword list or an atom.
- Passing an atom is a shorthand way to set flag style options
  - eg. `@tag :logged_in` is equivalent to `@tag logged_in: true`
- In this case, the context is raising the `Ecto.NoResultsError`.
  - Instead of letting this error blow up in the user's face, there is a protocol between Plug and Ecto where Plug is told to treat all `Ecto.NoResultsError` as a 404 response status, which we can also refer to as `:not_found`
- We use a function called `assert_error_sent` to test precisely that an error happened but became a 404 when handled by Phoenix.

## Unit-Testing Plugs
- If your code is worth writing, it's worth testing.
- `bypass_through` test helper allows us to do a request that goes through the whole pipeline but bypasses the router dispatch.
  - This approach gives you a connectioned wired up w/ all the transformations your specific tests require, such as fetching the session and adding flash messages
- Test time grows quickly
- If your tests are slow, you won't run them as much
- Hashing passwords is intentionally expensive
  - But we don't need all of that security in the test environment
  - Hashing rounds

## Testing Views and Templates
- Because a template is just a funciton in the view, templates are easy to test because they aren't coupled w/ the controller layer.

## Wrapping Up
- The `Ecto.Sandbox` preserved isolation and allowed concurrent testing.
- The `describe` keyword allowed us to apply setup code to multiple tests at once, saving duplication.
- The goal of integration tests is to test the layers of our application working together.
