# Transactions and Consistency

MongoDB supports multi-document transactions, and Spring Data MongoDB provides
managed transaction support.

Transactions can be useful, but MongoDB data models should usually try to keep
strongly consistent data inside a single aggregate document when possible.

## Single Document Atomicity

Updates to a single MongoDB document are atomic.

If related data belongs together and is usually updated together, embedding can
avoid the need for multi-document transactions.

## Multi-Document Transaction

```java
@Transactional
void createOrderAndReserveInventory(CreateOrderCommand command) {
    orderRepository.save(command.toOrderDocument());
    inventoryRepository.reserve(command.items());
}
```

This requires MongoDB transaction support and appropriate Spring transaction
configuration.

## Use Carefully

Transactions add operational and performance considerations. If the service
needs many cross-document transactions, reconsider document boundaries and data
modeling.

## Consistency Alternatives

Alternatives include:

- embedding;
- idempotent updates;
- outbox/event-driven workflows;
- optimistic state checks;
- compensating actions.

## Key Idea

Use MongoDB transactions when the use case truly needs them. Prefer document
models that keep common consistency boundaries within one document.
