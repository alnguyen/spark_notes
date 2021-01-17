---
title: 04 Ecto and Changesets
tags:
  - ecto
  - elixir
  - phoenix
---

# 04 Ecto and Changesets
----

## Understanding Ecto

- Ecto is a wrapper that's primarily intended for relational databases.
- Has an encapsulated query language used to build layered queries to the database

## Defining the User Schema and Migration
- Phoenix is built on top of OTP
  - A layer for reliably managing services.
  - Use OTP to start key services in a supervised process so that Ecto and Phoenix can do the right thing in case our repository crashes.

## Using the Repository to Add Data

## Building Forms

### Handling Update Policies
- Conventional persistence frameworks allow one-side-fits-all validations
  - They're forced to work harder to manage change across multiple concerns.
  - Your code has a single update mechanism but multiple update policies.
  - Ecto changesets allow you to separate your update mechanisms.

### Building Resource Routes
- **resources** is a shorthand implementation for a common set of actions that define create, read, update, and delete operations (CRUD) to access _resources_ via simple HTTP verbs.
- If you want to see all the available routes, you can run the `phx.routes` Mix task
- **form_for** provides conveniences like security, UTF-8 encoding, and more.

## Creating Resources
- The controller shouldn't care about persistence details, but neither should the schema.
  - The context between the persistence layer and controller is the perfect place to isolate change policy.
- The Ecto changeset carries validations and stores this information for later use.
  - In addition to that, the changesets also track changes.
