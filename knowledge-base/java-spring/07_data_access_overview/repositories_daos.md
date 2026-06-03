# Repositories and DAOs

Repositories and DAOs encapsulate persistence operations.

The names are often used loosely. In practice, both can be used to keep database
access out of controllers and application services.

## Repository Example

```java
@Repository
class OrderRepository {
    private final JdbcTemplate jdbcTemplate;

    OrderRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    Optional<OrderRecord> findById(Long id) {
        // query database
        return Optional.empty();
    }
}
```

The repository owns the query and result mapping. The service owns what to do
with the result.

## Keep Repository Methods Intentional

Prefer use-case-friendly methods:

```java
findOpenOrdersForCustomer(customerId)
```

Over vague methods that push query responsibility upward:

```java
findByManyOptionalFilters(...)
```

There are exceptions, especially for search endpoints, but keep ownership clear.

## Avoid Controller Access

Controllers should not inject repositories directly for real use cases. That
skips the application service layer where transactions, permissions, and
business decisions usually belong.

## Key Idea

Repositories and DAOs should make persistence operations explicit and keep query
details near the persistence layer.
