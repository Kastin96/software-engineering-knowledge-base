# Transaction Role

A transaction defines a unit of work that should commit or roll back together.

In backend services, transaction boundaries usually align with application use
cases: create an order, reserve inventory, mark payment as captured, update a
customer profile.

## Service Boundary

```java
@Service
class OrderService {
    @Transactional
    Long createOrder(CreateOrderCommand command) {
        Long orderId = orderRepository.insert(command.customerId());
        orderItemRepository.insertItems(orderId, command.items());
        return orderId;
    }
}
```

If item insertion fails, the order insert should roll back.

## What Transactions Protect

Transactions protect:

- atomicity of related writes;
- consistency of state transitions;
- rollback on failure;
- isolation from concurrent operations, depending on isolation level;
- reliable persistence behavior inside a use case.

## What Transactions Do Not Solve

Transactions do not automatically solve:

- distributed consistency across multiple services;
- long external calls;
- message delivery guarantees;
- poor locking strategy;
- invalid business rules.

## Key Idea

Transactions are use-case consistency boundaries. Place them where the
application decides what must succeed or fail together.
