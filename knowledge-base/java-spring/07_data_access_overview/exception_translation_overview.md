# Exception Translation Overview

Spring can translate many persistence exceptions into `DataAccessException`
types.

This gives application code a framework-level abstraction over vendor-specific
database exceptions.

## Repository Boundary

`@Repository` is not only a stereotype annotation. It also participates in
exception translation for persistence components.

```java
@Repository
class CustomerJdbcRepository {
    private final JdbcTemplate jdbcTemplate;

    CustomerJdbcRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }
}
```

## Application Translation

Even Spring's translated exceptions are usually too technical for API clients.

For example, a duplicate key error may become an application exception:

```java
try {
    customerRepository.insert(customer);
} catch (DuplicateKeyException ex) {
    throw new DuplicateCustomerEmailException(customer.email(), ex);
}
```

The API handler can map that to `409 Conflict`.

## Key Idea

Translate database-specific failures into application meaning before they cross
the service boundary.
