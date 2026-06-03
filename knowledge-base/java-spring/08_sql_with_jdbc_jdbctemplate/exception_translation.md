# Exception Translation

`JdbcTemplate` translates many `SQLException` failures into Spring
`DataAccessException` types.

This avoids scattering vendor-specific SQL exception handling throughout the
application.

## Duplicate Key Example

```java
try {
    jdbcTemplate.update(
        "insert into customers (email, name) values (?, ?)",
        email,
        name
    );
} catch (DuplicateKeyException ex) {
    throw new DuplicateCustomerEmailException(email, ex);
}
```

The repository or service translates a technical persistence failure into
application meaning.

## Do Not Expose DataAccessException To Clients

`DataAccessException` is useful inside the application, but it is still a
technical exception. API handlers should return stable application-level errors,
not raw Spring exception names.

## Common Translated Exceptions

Examples include:

- duplicate key;
- bad SQL grammar;
- empty result;
- data integrity violation;
- transient connection or resource issues.

## Key Idea

Use Spring exception translation as an internal abstraction, then translate
important failures into application-specific exceptions.
