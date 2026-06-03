# Testing JDBC Repositories

JDBC repositories should be tested against a real database engine or a close
test equivalent when SQL behavior matters.

Mocking `JdbcTemplate` often tests implementation details instead of SQL
behavior.

## What To Test

Repository tests should verify:

- SQL returns expected rows;
- row mapping is correct;
- inserts and updates affect expected data;
- constraints behave as expected;
- query filters and sorting are correct;
- transaction-related assumptions are valid.

## Test Slice

Spring Boot supports focused data access tests, depending on configuration and
dependencies.

For JDBC repositories, tests often use:

- a test profile;
- schema setup scripts or migrations;
- an embedded database for simple cases;
- Testcontainers for database-specific behavior.

## Example Focus

```java
@Test
void findsOrdersByCustomerId() {
    // insert test rows
    // call repository
    // assert mapped records and ordering
}
```

The exact setup matters less than testing the real SQL path.

## Embedded Database Caveat

An embedded database may not match PostgreSQL, MySQL, or Oracle behavior. For
database-specific SQL, use the target database in tests when possible.

## Key Idea

Test JDBC repositories through SQL and database behavior, not by mocking every
`JdbcTemplate` call.
