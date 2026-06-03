# Transactions and Outbox Pattern

The hard part of messaging is not publishing a record. The hard part is keeping
database state and published events consistent.

## The Problem

This code has a consistency risk:

```java
orderRepository.save(order);
kafkaTemplate.send("orders.paid.v1", order.getId().toString(), event);
```

If the database write succeeds and Kafka publishing fails, the system state and
event stream diverge.

## Outbox Pattern

The outbox pattern stores the business state and the event in the same database
transaction. A separate publisher later sends outbox rows to Kafka.

```java
@Transactional
public void pay(Long orderId) {
    Order order = orderRepository.getRequired(orderId);
    order.markPaid();

    outboxRepository.save(OutboxEvent.orderPaid(order));
}
```

Then an outbox publisher reads pending events and publishes them.

```java
@Scheduled(fixedDelayString = "${outbox.publisher.delay:1000}")
public void publishPendingEvents() throws Exception {
    List<OutboxEvent> events = outboxRepository.findNextBatch();

    for (OutboxEvent event : events) {
        kafkaTemplate.send(event.topic(), event.aggregateId(), event.payload())
            .get(10, TimeUnit.SECONDS);
        event.markPublished();
    }
}
```

Production implementations need locking, batching, retries, and duplicate-safe
publishing. The important rule is that an outbox row should be marked published
only after Kafka accepts the record.

## Kafka Transactions

Spring Kafka supports Kafka transactions, but they do not automatically solve
all database plus Kafka consistency problems. Use them when the team has a clear
transactional messaging requirement and understands producer configuration,
consumer isolation, and failure behavior.

## Key Idea

For typical service plus database workflows, the outbox pattern is often easier
to reason about than trying to make one method update the database and publish
to Kafka perfectly.
