---
title: 09 Watching Videos
tags:
  - ecto
  - elixir
  - phoenix
---

# 09 Watching Videos
----

## Watching Videos
- We used a regular expression to extract the video ID from the URL
  - Uses patterns to match specific patterns within strings.
  - `named_captures` extracts the id field given our url name in the example

## Adding JavaScript
- We'll use webpack to build, transform, and minify JS and CSS code.
- Processing assets in this way makes your page load more efficiently.
- We put everything in `assets/static` that doesn't need to be transformed by webpack
  - The build tool will copy those static assets just as they are to `priv/static`
  - They'll be served by `Plug.Static` in our endpoint
- Phoenix configures webpack to use ECMAScript 6 (ES6) to provide the necessary import statements
- webpack wraps the contents for each JS file in `assets/js` in a function and collects them into `priv/static/js/app.js`
  - This is what is loaded by browsers
  - Since each file is wrapped in a func, it won't be automatically executed by browsers unless you explicitly import it in your app.js file
  - `app.js` acts like a manifest in this way
- `assets/vendor` js files is the exception to this rule.
  - It will automatically be concatenated to your `priv/static/app.js` bundle and executed when your page loads.
- `webpack` comes with a CLI
  - `$ webpack` just compiles the assets into static files and copies the results to `priv/static` before existing
  - `$ webpack --watch` will have webpack monitor the files and automatically recompile them as they change
  - `$ webpack --mode production` generally does everything you need to prepare your JS and CSS for production, such as building and minifying them.
- Phoenix usually does the first two commands for you

## Creating Slugs
- A _slug_ is a unique URL-friendly identifier
- `alter` macro changes the schema for both _up_ and _down_ migrations.
- At this point, you've learned all the concepts behind changesets, and the benefits are becoming clearer:
  - Because Ecto separates changesets from the definition of a given record, we can have a separate change policy for each type of change. We could easily add a JSON API that creates videos, including the `slug` field, for example.
  - Changesets filter and cast the incoming data, making sure sensitive fields like a user role cannot be set externally, while conveniently casting them to the type defined in the schema.
  - Changesets can validate data--for example, the length or the format of a field--on the fly, but validations that depend on data integrity are left to the database in the shape of constraints.
  - Changesets make our code easy to understand and implement because they can compose easily, allowing us to specify each part of a change with a function.

### Extending Phoenix with Protocols
- `RumblWeb.Router` generates the `Routes.watch_path`
  - It's available because of the `Routes` alias in `lib/rumbl_web.ex`
- `Phoenix.Param` protocol
  - By default, this protocol extracts the `id` from a struct, if one exists
  - Because it is an Elixir protocol, we can customize it for any data type in the language.
- Elixir protocols can be implemented for any data structure, anywhere, any time.
- URI struct is part of Elixir's standard lib, and all route functions accept it as their first argument.
- Primary keys in Ecto have a default type of `:id`.
  - For now, we can consider `:id` to be an `:integer`

### Extending Schemas with Ecto Types
- Sometimes we might want to associate some behavior to our `id` fields.
  - A custom type allows us to do that.
- `Ecto.Type` behavioour expects us to define four functions:
  - _type_: Returns the underlying Ecto type. In this case we're building on top of `:id`
  - _cast_: Called when external data is passed into Ecto. It's invoked when values in queries are interpolated or also by `cast` function in changesets
  - _dump_: Invoked when data is sent to the database.
  - _load_: Invoked when data is loaded from the database
- By design, the `cast` functioon often processes end-user input
- `dump` and `load` handle the struct-to-database conversion.
- `cast` does the dirty work of sanitizing our input.
- For our example
  - Successful casts return integers.
  - `dump` and `load` return `:ok` tuples with integers or `:error`
- Because Ecto automatically defines the `id` field for us, we can customize the primary key w/ the `@primary_key` module attribute.
  - We tacked on the `autogenerate: true` option because our DB autogenerates `id` values

## Wrapping Up
- You learned to use webpack to support development-time reloading and minimization for production code.
- We used generators to create an Ecto migration
- We used changesets to create slugs
- We used protocols to seamlessly build URLs from those new slugs.
