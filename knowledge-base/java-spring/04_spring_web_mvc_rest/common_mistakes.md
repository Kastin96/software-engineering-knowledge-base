# Common Mistakes

REST controller issues often come from mixing HTTP concerns, business behavior,
and persistence details in the same class.

## Business Logic In Controllers

Controllers should coordinate HTTP input and output. Complex decisions belong in
services or domain code.

## Exposing Entities Directly

Returning JPA or persistence entities can leak internal fields, trigger lazy
loading problems, and couple the API to database structure.

## Ignoring Status Codes

Returning `200 OK` for every successful operation makes the API less precise.
Creation, deletion, conflicts, and missing resources should be represented
intentionally.

## Unbounded Collection Endpoints

Endpoints that return all records become production problems as data grows.
Add pagination before the endpoint becomes expensive.

## Mixing API And Database Naming

API filters and fields should match client-facing concepts. Database column
names and persistence details should not leak unless deliberately exposed.

## Versioning Without A Compatibility Need

Versioning every endpoint too early adds maintenance overhead. Not versioning
long-lived external APIs creates upgrade risk. Match the decision to client
ownership.

## Key Idea

REST controllers should keep the HTTP contract clear while delegating behavior
to the application layer.
