# Kafka Event Flow

Build an order payment event flow.

## Requirements

- publish `OrderPaidEvent` when an order is paid;
- use order id as Kafka key;
- consume the event in an invoice service;
- make invoice creation idempotent;
- configure retries and DLT;
- test the flow with a real Kafka broker.

## Event

```java
public record OrderPaidEvent(
    String eventId,
    Long orderId,
    Long customerId,
    BigDecimal amount,
    Instant occurredAt
) {
}
```

## Producer Rule

Publish a stable event contract, not a JPA entity.

```java
kafkaTemplate.send("orders.paid.v1", order.getId().toString(), event);
```

## Consumer Rule

Keep listener logic thin:

```text
listener -> invoice use case -> repositories
```

## Idempotency

Store processed event ids in the same transaction as the side effect.

```text
if eventId was processed -> return
create invoice
store eventId
```

## Tests

Use Testcontainers to verify:

- event is published to the expected topic;
- consumer creates invoice;
- duplicate event does not create duplicate invoice;
- invalid event goes to DLT or configured failure path.

## Review Questions

- Why is order id used as the key?
- What happens if the consumer crashes after processing?
- How are DLT records investigated?
- Is event publishing consistent with database state?
