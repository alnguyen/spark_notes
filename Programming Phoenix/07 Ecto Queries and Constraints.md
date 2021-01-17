---
title: 07 Ecto Queries and Constraints
tags:
  - ecto
  - elixir
  - phoenix
---

# 07 Ecto Queries and Constraints
----

## Seeding and Associating Categories

### Setting Up Category Seed Data
- Phoenix defines a convention for seeding data.
- `seeds.exs`
- Seed script may be executed multiple times.
  - Need to make sure our seed script won't fail or won't generate duplicate categories
- "Upsert" - Update the data whenever there is a conflict during insert.
  - Ecto allows us to do that via the `:on_conflict` option.
- Sigil
  - eg. `~w` defines a list of words
- Creating a query
  ```elixir
    query = from c in Category, select c.name
    Repo.all query
  ```
  - `from` is a macro that builds a query
  - `c in Category` means we're pulling rows (labeled c) from the Category schema
  - `select: c.name` means we're going to return only the name field
  - `Repo.all` is a repository function that takes a query and returns all rows.
- Ecto's real purpose is to efficiently translate Elixir concepts into a language the DB understands
- Ecto queries are composable, which means you can define the query bit by bit
- Instead of building whole queries at once, we can write it in small steps, piece by piece
  - This allows us to use pipes to build complex queries from simpler ones
- This is possible because Ecto defines something called the `queryable` protocol.
  - `from` receives a `queryable`, and you can use any `queryable` as a base for a new query.
  - A `queryable` is an Elixir protocol.
  - Recall that protocalls like `Enumerable` define APIs for specific language features.
- Remember, views are just modules with pure functions.

## Diving Deeper into Ecto Queries
- `^` (caret) is used for injecting a value or expression for interpolation into an Ecto query
  - It interpolates values into our queries where Ecto can scrub them and safely put them to use
- `Repo.one` doesn't mean "return the first result."
  - It means "one result is expected, so if there's more, fail."
- By relying on Elixir macros, Ecto knows where user-defined variables are located, so it's easier to protect the user from security flaws like SQL-injection attacks.
- Ecto queries do a good part of the query normalization at compile time, so you'll see better performance while leveraging the information in our schemas for casting values at runtime.
- Using our schema definition, Ecto can cast the values properly for us and match up Elixir types with the expected DB types

### The Query API
- Ecto supports a wide range of operators
  - Comparison: `==`, `!=`, `<=`, `>=`, `<`, `>`
  - Boolean: `and`, `or`, `not`
  - Inclusion: `in`
  - Search: `like`, `ilike`
  - Null checks: `is_nil`
  - Aggregates: `count`, `avg`, `sum`, `min`, `max`
  - Date/time intervals: `datetime_add`, `date_add`
  - General: `fragment`, `field`, `type`
- You'll use them from two APIs: keywords syntax and pipe syntax

### Writing Queries with Keywords Syntax
- Expresses different parts of the query by using a keyword list.
  ```elixir
  iex> Repo.one from u in User,
                select: count(u.id),
                where: ilike(u.username, "j%") or
                        ilike(u.username, "c%")
  ```
  - The `u` variable is bound as part of Ecto's `from` macro
  - It represents entries from the `User` schema
  - If you attempt to access or match against an invalid type, Ecto raises an error.
- Bindings are useful when our queries need to join across multiple schemas.
  - Each join in a query gets a specific binding.
- Can name query variables however you like--Ecto doesn't use their names

### Using Queries with the Pipe Syntax
- You've seen different query expressions constructed with key-value pairs.
- You can also build queries by piping through query macros
  ```elixir
  iex> User \
        |> select([u], count(u.id)) \
        |> where([u], ilike(u.username, ^"j%") or
        ilike(u.username, ^"c%")) \
        |> Repo.one()                            
  ```
- Because each query is independent of others, we need to specify the binding manually for each one as part of a list.
- The query syntax you choose depends on your taste and the problems you're trying to solve.
- The former syntax is probably more convenient for pulling together ad-hoc queries and solving one-off problems.
- The latter is probably better for building an application's unique complex layered query API.

### Fragments
- The best abstractions offer an escape hatch, one that exposes the user to one deeper level of abstraction on demand.
- _query fragment_ sends part of a query directly to the database but allows you to construct the query string in a safe way.
  ```elixir
  from u in User,
    where: fragment("lower(username) = ?", ^String.downcase(name))
  ```
- Using a fragment allows us to construct a fragment of SQL for the query but safely interpolate the `String.downcase(name)` code using a prepared statement.
- When everything else fails and fragments aren't enough, you can always run direct SQL w/ `Ecto.Adapters.SQL.query`:
  ```elixir
  Ecto.Adapters.SQL.query(Repo, "SELECT power($1, $2)", [2, 10])
  ```

### Querying Relationships
- When working with relationships, Ecto associations are explicit, and we used `Repo.preload` to fetch associated dataa.
- We don't always need to preload associations as a separate step:
  ```elixir
  Repo.one(from v in Video, limit: 1, preload: [:user])
  ```
- Ecto also allows us to join on associations inside queries
  ```elixir
  Repo.all from v in Video,
    join: u in assoc(v, :user),
    join: c in assoc(v, :category),
    where: c.name == "Comedy",
    select: {u, v}
  ```
  - This returns users and videos side by side as long as the video belongs to the Comedy category
  - We use a tuple in select, but we could also return each entry in a list or even a map

## Constraints
- Sometimes, application layer constraints may not be good enough to ensure consistent data.
  - We'll need to rely on database constraints.
- In Phoenix, we use constraints to manage change in a way that combines the protections of the DB w/ Ecto's gentle guiding hand to report errors w/o crashing.
- Terminology:
  - _constraint_ - An explicit database constraint. This might be a uniqueness constraint on an index, or an integrity constraint between primary and foreign keys.
  - _constraint error_ - The `Ecto.ConstraintError`. This happens when Ecto identifies a constraint problem, such as trying to insert a record w/o specifying a required key.
  - _changeset constraint_ - A constraint annotation added to the changeset that allows Ecto to convert constraint errors into changeset error messages.
  - _changeset error messages_ - Beautiful error messages for the consumption of humans.
- Relational databases deal w/ relationships between tables.

### Validating Unique Data
- By default, Ecto infers the constraint name for us, but it can also be given with the `:name` option.
- Calling `unique_constraint` won't perform any validations on the spot.
  - It stores all the relevant info in the changeset.
  - When it's time, the repository can convert those constraints into a human-readable error.

### Validating Foreign Keys
- `assoc_constraint` converts foreign-key constraint errors into human-readable error messages
- As with `unique_constraint`, when we set up `assoc_constraint`, we no longer get `Ecto.ConstraintError`. Instead, they're converted into changeset error messages.
- Each changeset encapsulates the whole change policy, including allowed fields, detecting change, validations, and messaging the user.

### On Delete
- We can't delete related records because it could leave orphaned records.
  - We could solve this in several ways:
    - changeset constraints
      - `Repo.delete` also accepts a changeset, and you can use `foreign_key_constraint` to ensure that no associated records exist when a related record is deleted.
      - The `foreign_key_constraint` function is like the `assoc_constraint`, except it doesn't inflect the foreign key from the relationship.
      - We could also add `no_assoc_constraint` to lift up the foreign key name and setting a good error message.
    - You could configure the DB references to either cascade the deletions or simply make the `foreign_key_id` column `NULL` on delete.
      - The `references` function accepts the `:on_delete` option, with one of the following:
        - `:nothing` - The default
        - `:delete_all` - When the referenced table record is deleted, all current table records are deleted
        - `:nilify_all` - When a referenced record is deleted, the `foreign_key_id` of all associated records are set to `NULL`
    - Set up `:on_delete` when configuring `has_many`or `belongs_to` relationships in your schema, moving the logic effectively to the application domain.
      - Only recommended when you can't perform one of the preceding operations.
      - _The work best suited to the database must be done in the database_

### Let It Crash
- If a constraint is violated when our app is the one responsible for setting up relationships between two tables, it can only be a bug in our app or a data-integrity issue.
  - In such cases, _the user can do nothing to fix the error_, so crashing is the best option.
- Elixir was designed to handle failures, and Phoenix allows us to convert them into nice status pages.
- We recommend setting up a notification system that aggregates and emails errors coming from your application.
- The `*_constraint` changeset functions are useful when the constraint being mapped is triggered by external data, often as part of the user request.
- _Using changeset constraints only makes sense if the error message can be something the user can take action on_

## Wrapping Up
- We learned how to seed data and how to use the `:on_conflict` optioon to manage data conflicts.
- We used Ecto's query API, which is independent of the repository API, to do some basic queries.
- We used two forms of queries, a keyword list-based syntax and a pipe-based syntax.
- We used fragments to pass SQL commands through the query API unchanged.
- We explored the different ways Ecto queries work with relationships, beyond data preloading.
- We wrote constraint-style validations for unique indexes and foreign-key violations.
- We learned how to choose between letting constraint errors go and when to report them to the user.
