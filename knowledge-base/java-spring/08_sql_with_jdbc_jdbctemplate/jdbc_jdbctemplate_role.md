# JDBC and JdbcTemplate Role

JDBC is the standard Java API for relational database access.

Raw JDBC gives direct control but requires repetitive code for connections,
statements, result sets, exception handling, and resource cleanup.

`JdbcTemplate` keeps SQL explicit while handling much of that boilerplate.

## Raw JDBC Cost

With raw JDBC, every query tends to involve:

- getting a connection;
- preparing a statement;
- binding parameters;
- executing the query;
- iterating a result set;
- mapping rows;
- closing resources;
- handling SQL exceptions.

`JdbcTemplate` handles the repetitive resource management and lets repository
code focus on SQL and mapping.

## When JdbcTemplate Fits

`JdbcTemplate` is a good fit when:

- SQL should stay visible;
- query shape matters;
- reporting or join-heavy queries are common;
- ORM lifecycle behavior is unnecessary;
- the team wants predictable database interaction.

## Repository Example

```java
@Repository
class OrderJdbcRepository {
    private final JdbcTemplate jdbcTemplate;

    OrderJdbcRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }
}
```

## Key Idea

`JdbcTemplate` is a pragmatic middle ground: explicit SQL with Spring-managed
resource handling and exception translation.
