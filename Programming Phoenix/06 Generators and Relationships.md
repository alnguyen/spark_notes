---
title: 06 Generators and Relationships
tags:
  - elixir
  - phoenix
---

# 06 Generators and Relationships
----

## Using Generators

### Adding Videos and Annotations

### Generating Web Interfaces
- Two mix tasks to bootstrap web interfaces
  - `phx.gen.html`
  - `phx.gen.json`
- When you're organizing code, think contexts first.
```elixir
$ mix phx.gen.html Multimedia Video videos user_id:references:users \
url:string title:string description:text
```
- Name of the context: Multimedia
- Name of the module that defines the schema: Video
- Plural form of the schema name: videos
- Each field, with some type information

### Examining the Generated Context, Controller, and View

## Building Relationships

## Managing Related Data
- Each controller is also a plug.
- To call a controller, Phoenix invokes the default action function at the end of the controller pipeline.
- `__MODULE__` directive, which expands to the current module, in atom form.

## In-context Relationships
- Sometimes, new resources should go into existing contexts.
- Sometimes, we'll define relationships within the same context.
- **Should I Create Another Context?**
  - The fact two resources are related in the DB does not imply they belong in the same context.
  - In cases you are unsure how to group your resources, prefer distinct contexts per resource and refactor later if necessary.
  - When in doubt, put your new resource in its own context.

### Schema and Context Generators
- Up to this point, we've used `ecto.gen.migration` and `mix phx.gen.html`
- The migration generator has a specific concern and generates only migration files
- The html and json generators generate migrations, schemas, contexts, as well as controllers, views, and templates.
- Context and schema generators fit between the HTML generator and the migration.
- `mix help GENERATOR_NAME` for more information about any generrator
- Examples
  - `mix phx.gen.html Multimedia Category categories name:string`
    - Generates controller, view, and template on the front end
    - Generates a `Multimedia` context, a `Multimedia.Category` schema, and a migration.
    - This generator (and the json gen) are typicalled used when we want to define all conveniences to expose a resource over the web interface
  - `mix phx.gen.context Multimedia Category categories name:string`
    - Generates a `Multimedia` context, a `Multimedia.Category` schema and the associated migration.
    - Useful for generating a resource with all of its context functions _without_ exposing that resource via the web interface.
    - *Note*: If the context already exists, which is the case for `Multimedia`, the generator will inject the new category functions into the existing context.
  - `mix phx.gen.schema Multimedia.Category categories name:string`
    - Generates a schema with a migration
    - Useful for creating a resource when you want to define the context functions yourself.
  - `mix ecto.gen.migration create_categories`
    - Generates a new empty migration
    - Useful when the schema and context are already laid out, and all you need is to update the database

### Generating Category Migrations

## Wrapping Up
- We used contexts throughout to craft an easy-to-maintain API layer for our app
- We converted a private plug into a public function and shared it with our controllers and routers
- You learned how to migrate and roll back changes to the DB
- We defined relationships between `User` and `Video` schemas and used functioons from Ecto to build and retrieve associated data.
- We discussed the main generators Phoenix provides to scaffold our apps.
