# Migration Role

Database migrations version schema changes.

They allow the database structure to evolve alongside application code in a
controlled and repeatable way.

## Why Migrations Matter

Migrations help with:

- creating tables and indexes;
- adding or changing columns;
- adding constraints;
- backfilling data;
- keeping environments consistent;
- reviewing schema changes before deployment.

## Avoid Relying On Auto-DDL In Production

JPA/Hibernate can generate schema changes, but production schema evolution
should usually be explicit.

```yaml
spring:
  jpa:
    hibernate:
      ddl-auto: validate
```

`validate` can check mapping compatibility without letting Hibernate mutate the
schema automatically.

## Migration Ownership

Schema migrations should be treated as production code. They affect deployment,
rollback, performance, and data integrity.

## Key Idea

Migrations make database change intentional, reviewable, and repeatable.
