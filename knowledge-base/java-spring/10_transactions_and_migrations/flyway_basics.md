# Flyway Basics

Flyway manages database migrations through versioned migration files.

It is commonly used in Spring Boot services because it is simple and works well
for SQL-first schema evolution.

## Migration Naming

Typical file names:

```text
V1__create_orders_table.sql
V2__add_order_status_index.sql
V3__add_customer_email_unique_constraint.sql
```

Versioned migrations run in order and are tracked in a schema history table.

## Example Migration

```sql
create table orders (
    id bigserial primary key,
    customer_id bigint not null,
    status varchar(40) not null,
    total numeric(12, 2) not null,
    created_at timestamp not null
);
```

## Spring Boot Integration

When Flyway is on the classpath, Boot can run migrations during startup,
depending on configuration.

Migration location is commonly:

```text
src/main/resources/db/migration
```

## Key Idea

Flyway is a straightforward versioned SQL migration tool. Use it when explicit
SQL migrations fit the team's workflow.
