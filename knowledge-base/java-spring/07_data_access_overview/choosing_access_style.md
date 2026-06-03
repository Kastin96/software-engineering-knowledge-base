# Choosing an Access Style

Spring supports several data access styles. The right choice depends on the
domain model, query complexity, performance requirements, and team experience.

## Common Options

```text
JDBC / JdbcTemplate       -> direct SQL control with less boilerplate than raw JDBC
JPA / Hibernate           -> object-relational mapping and entity lifecycle
Spring Data JPA           -> repository abstraction over JPA
Spring Data MongoDB       -> document database access
R2DBC                     -> reactive relational database access
```

## When JDBC Fits

JDBC or `JdbcTemplate` is a good fit when:

- SQL needs to be explicit;
- queries are reporting-like or join-heavy;
- the domain model does not need ORM lifecycle management;
- performance and query shape need tight control;
- you want predictable database interaction.

## When JPA Fits

JPA can fit when:

- the model is entity-oriented;
- relationships and unit-of-work behavior are useful;
- common CRUD operations dominate;
- the team understands lazy loading, transactions, and dirty checking.

## When MongoDB Fits

MongoDB can fit when:

- data is naturally document-shaped;
- aggregate documents are commonly read together;
- flexible schemas are useful;
- query patterns are known and indexed accordingly.

## Key Idea

Choose data access style based on model and query behavior, not because one
abstraction is considered more modern.
