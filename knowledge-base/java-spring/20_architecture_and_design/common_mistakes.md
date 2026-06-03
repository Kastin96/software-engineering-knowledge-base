# Common Mistakes

## Controllers With Business Logic

Controllers should handle HTTP. Complex workflows in controllers are hard to
test and reuse.

## Returning Entities From APIs

JPA entities are not API contracts. Returning them leaks internal design and can
trigger lazy-loading problems.

## Repository-Driven Design

Starting every feature from repositories can make the database shape dominate
the business model.

## Too Many Abstractions Too Early

Interfaces, factories, ports, adapters, and modules are useful only when they
solve a real change or testing problem.

## Hidden Transaction Boundaries

When nobody knows where a transaction begins and ends, consistency bugs become
hard to diagnose.

## Synchronous Calls Everywhere

Calling every dependency synchronously makes latency and availability problems
spread through the system.

## No Ownership Boundaries

If every package depends on every other package, changes become risky even when
the code looks organized.

## Key Idea

Architecture mistakes usually come from unclear boundaries: unclear HTTP
boundaries, unclear transaction boundaries, unclear module boundaries, or
unclear ownership.
