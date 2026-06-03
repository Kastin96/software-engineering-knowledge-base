# Common Mistakes

Transaction and migration mistakes can create subtle data corruption or painful
deployment failures.

## Placing Transactions Too Low

Repository-level transactions can fragment a use case. Put transactions around
complete service operations when multiple writes must succeed together.

## Ignoring Self-Invocation

Calling a transactional method from another method in the same class may bypass
Spring proxy behavior.

## Swallowing Rollback Exceptions

Catching and not rethrowing an exception can allow a transaction to commit when
the use case actually failed.

## Changing Isolation Casually

Higher isolation can introduce blocking and performance issues. Use it for
specific concurrency problems.

## Relying On Hibernate Auto-DDL

Automatic schema mutation is not a safe production migration strategy. Use
versioned migrations.

## Editing Applied Migrations

Do not modify migrations already applied to shared environments. Add a new
migration.

## Running Heavy Migrations Blindly

Large table changes should be tested for locking, duration, and rollback plan.

## Key Idea

Transactions and migrations are correctness tools. Treat them as part of
production design, not framework decoration.
