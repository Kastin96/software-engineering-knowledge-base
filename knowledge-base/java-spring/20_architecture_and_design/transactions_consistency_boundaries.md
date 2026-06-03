# Transactions and Consistency Boundaries

A transaction boundary defines which changes must succeed or fail together.

In Spring, transaction boundaries usually belong in application services, not
controllers or repositories.

## Service Transaction

```java
@Transactional
public void cancelOrder(Long orderId) {
    Order order = orderRepository.getRequired(orderId);
    order.cancel();

    inventoryService.releaseReservation(order.getReservationId());
}
```

This is only safe if `inventoryService` participates in the same local
transaction or if the release operation is designed to handle failure.

## Local Transaction

A local database transaction can protect changes inside one database.

It does not automatically protect:

- Kafka publishing;
- HTTP calls to another service;
- writes to another database;
- external payment operations.

## Consistency Decision

For cross-system workflows, decide explicitly:

- should the operation be synchronous?
- can it be eventually consistent?
- do we need an outbox?
- do we need compensation?
- who owns retry and failure handling?

## Key Idea

`@Transactional` is not distributed magic. It protects a local boundary. System
consistency still needs design.
