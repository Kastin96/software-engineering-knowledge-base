# Transaction Boundaries

A transaction boundary defines which operations must succeed or fail together.

In Spring applications, transaction boundaries usually belong in the service
layer because services own use cases.

## Service-Level Transaction

```java
@Service
class OrderService {
    private final OrderRepository orderRepository;
    private final PaymentRepository paymentRepository;

    @Transactional
    OrderResponse markPaid(Long orderId, PaymentId paymentId) {
        Order order = orderRepository.findByIdForUpdate(orderId)
            .orElseThrow(() -> new OrderNotFoundException(orderId));

        order.markPaid(paymentId);
        paymentRepository.save(paymentId, orderId);
        orderRepository.save(order);

        return OrderResponse.from(order);
    }
}
```

The service coordinates multiple persistence operations as one use case.

## Repository-Level Transactions

Repository-level transactions can work for simple operations, but they may
fragment larger use cases. If one service method calls several repositories,
the service should usually control the transaction.

## Read-Only Transactions

```java
@Transactional(readOnly = true)
OrderResponse findById(Long id) {
    return orderRepository.findById(id)
        .map(OrderResponse::from)
        .orElseThrow(() -> new OrderNotFoundException(id));
}
```

Read-only transactions can communicate intent and may help certain persistence
providers optimize behavior.

## Key Idea

Place transactions around complete use cases, not around arbitrary individual
database calls.
