# Testing JPA Repositories

JPA repository tests should verify mapping, queries, constraints, and fetch
behavior that matters to the application.

## Data JPA Test

```java
@DataJpaTest
class OrderRepositoryTest {
    @Autowired
    private OrderRepository orderRepository;

    @Test
    void findsOrdersByCustomerId() {
        // persist test data
        // call repository
        // assert result
    }
}
```

`@DataJpaTest` focuses on JPA components rather than starting the entire web
application.

## What To Test

Test:

- custom JPQL queries;
- native SQL queries;
- relationship mappings;
- unique constraints;
- projections;
- sorting and pagination;
- fetch behavior when it affects performance.

## Database Choice

Embedded databases are convenient but may not match production SQL behavior.
For PostgreSQL, MySQL, or Oracle-specific queries, prefer testing against the
target database engine when practical.

## Key Idea

Repository tests should cover persistence behavior, not only method invocation.
