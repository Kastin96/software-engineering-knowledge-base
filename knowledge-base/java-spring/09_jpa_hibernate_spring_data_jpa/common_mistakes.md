# Common Mistakes

JPA mistakes often come from forgetting that ORM code still executes SQL.

## Returning Entities From Controllers

This leaks persistence shape, risks lazy loading during serialization, and
couples API responses to database mapping.

## Making Every Relationship Bidirectional

Bidirectional relationships are harder to maintain. Add them only when both
sides are genuinely needed.

## Using Eager Fetching As A Shortcut

Global eager fetching often creates large implicit query graphs. Prefer
use-case-specific fetch plans.

## Ignoring N+1

N+1 issues may not show up with small local datasets. Check SQL logs and test
realistic query paths.

## Overusing Derived Query Names

Very long repository method names become hard to read and maintain. Switch to
JPQL, native SQL, specifications, or custom repositories when needed.

## Treating Entity Annotations As Migrations

Entity annotations do not replace schema migrations. Production schema changes
should be versioned and reviewed.

## Key Idea

JPA removes boilerplate, not persistence complexity. Understand the SQL and
lifecycle behavior behind the repository call.
