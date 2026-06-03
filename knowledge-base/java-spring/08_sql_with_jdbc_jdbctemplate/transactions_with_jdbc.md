# Transactions with JDBC

`JdbcTemplate` participates in Spring-managed transactions.

The repository does not need to manually open, commit, or roll back
connections. Spring transaction management coordinates the connection used by
JDBC operations inside the transaction boundary.

## Service Transaction

```java
@Service
class OrderService {
    private final OrderJdbcRepository orderRepository;
    private final OrderItemJdbcRepository itemRepository;

    @Transactional
    Long create(CreateOrderCommand command) {
        Long orderId = orderRepository.insert(command.customerId());
        itemRepository.insertItems(orderId, command.items());
        return orderId;
    }
}
```

If `insertItems` fails, the order insert should roll back as part of the same
transaction.

## Repository Without Manual Commit

```java
@Repository
class OrderJdbcRepository {
    Long insert(Long customerId) {
        // use JdbcTemplate
        return 1L;
    }
}
```

Do not manually commit inside repository methods when using Spring transaction
management.

## Transaction Scope

Keep transactions as short as practical. Avoid long external network calls
inside a database transaction unless the use case explicitly requires it.

## Key Idea

Put `@Transactional` around complete service use cases. Let Spring coordinate
JDBC connections and rollback behavior.
