# Transactions and Migrations

This section covers transaction management and database schema migrations in
Spring applications.

Transactions protect consistency for a unit of work. Migrations protect schema
evolution across environments. Both are production concerns: they affect
correctness, deployment safety, rollback behavior, and operational reliability.

## Topics

- 01\. [Transaction Role](transaction_role.md)
- 02\. [`@Transactional` Basics](transactional_basics.md)
- 03\. [Proxy Mechanics and Self-Invocation](proxy_mechanics_self_invocation.md)
- 04\. [Rollback Rules](rollback_rules.md)
- 05\. [Propagation](propagation.md)
- 06\. [Isolation](isolation.md)
- 07\. [Read-Only Transactions](read_only_transactions.md)
- 08\. [Locking and Concurrency](locking_concurrency.md)
- 09\. [Migration Role](migration_role.md)
- 10\. [Flyway Basics](flyway_basics.md)
- 11\. [Liquibase Basics](liquibase_basics.md)
- 12\. [Migration Best Practices](migration_best_practices.md)
- 13\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with transaction role and `@Transactional` basics. Then study proxy
behavior, rollback rules, propagation, isolation, read-only transactions, and
locking. After that, review migrations, Flyway, Liquibase, migration practices,
and common mistakes.

## Mini Goal

By the end of this section, you should be able to design a service where:

- transaction boundaries wrap complete use cases;
- rollback behavior is intentional;
- propagation and isolation are not changed casually;
- read-only operations communicate intent;
- concurrent updates are handled deliberately;
- schema changes are versioned through migrations;
- migrations are safe to apply across environments.

## Interview Readiness

You should be able to answer:

- Where should `@Transactional` usually be placed?
- Why can self-invocation break transactional behavior?
- What causes a transaction rollback by default?
- What is transaction propagation?
- What is transaction isolation?
- Why use migrations instead of relying on entity auto-DDL?
- What makes a database migration safe for production?
