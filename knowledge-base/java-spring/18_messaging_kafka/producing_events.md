# Producing Events

Use `KafkaTemplate` when a Spring service needs to publish Kafka records.

The producer should publish a stable event, not a JPA entity or internal DTO
that accidentally exposes persistence details.

## Event Model

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

`eventId` helps with tracing and idempotency. `occurredAt` records business time,
not only broker time.

## Producer Example

```java
@Service
public class OrderEventPublisher {
    private static final String TOPIC = "orders.paid.v1";

    private final KafkaTemplate<String, OrderPaidEvent> kafkaTemplate;

    public OrderEventPublisher(KafkaTemplate<String, OrderPaidEvent> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public CompletableFuture<SendResult<String, OrderPaidEvent>> publish(Order order) {
        OrderPaidEvent event = new OrderPaidEvent(
            UUID.randomUUID().toString(),
            order.getId(),
            order.getCustomerId(),
            order.getTotalAmount(),
            Instant.now()
        );

        return kafkaTemplate.send(TOPIC, order.getId().toString(), event);
    }
}
```

Use the entity id as the key when ordering by that entity matters. Records with
the same key go to the same partition, so Kafka preserves their order within
that partition.

## Handling Send Result

```java
publisher.publish(order)
    .whenComplete((result, ex) -> {
        if (ex != null) {
            log.error("Failed to publish order event: orderId={}", order.getId(), ex);
            return;
        }

        RecordMetadata metadata = result.getRecordMetadata();
        log.info("Order event published: topic={}, partition={}, offset={}",
            metadata.topic(),
            metadata.partition(),
            metadata.offset()
        );
    });
```

Do not ignore send failures if the event is required for the business flow.

## Key Idea

Producer code should make the event contract explicit: topic, key, payload,
business timestamp, and failure behavior.
