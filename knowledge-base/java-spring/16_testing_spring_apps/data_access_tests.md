# Data Access Tests

Data access tests should verify real persistence behavior.

Mocking repositories usually does not prove SQL, JPA mapping, MongoDB queries,
indexes, constraints, or transaction behavior.

## JPA Example

```java
@DataJpaTest
class OrderRepositoryTest {
    @Autowired
    private OrderRepository orderRepository;

    @Test
    void findsOrdersByCustomerId() {
        orderRepository.save(new OrderEntity("customer-1", "PAID", new BigDecimal("50.00")));

        List<OrderEntity> orders = orderRepository.findByCustomerId("customer-1");

        assertThat(orders).hasSize(1);
        assertThat(orders.get(0).getStatus()).isEqualTo("PAID");
    }
}
```

## What To Test

Test:

- custom queries;
- row/entity/document mapping;
- projections;
- sorting and pagination;
- constraints;
- update behavior;
- database-specific SQL or MongoDB aggregation.

## Database Choice

Embedded databases are convenient, but they may not behave like PostgreSQL,
MySQL, Oracle, or MongoDB. Use the target database through Testcontainers when
query behavior is database-specific.

## Key Idea

Persistence tests should exercise persistence technology, not mocks of it.
