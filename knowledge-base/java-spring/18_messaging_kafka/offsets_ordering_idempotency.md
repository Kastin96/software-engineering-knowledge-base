# Offsets, Ordering, and Idempotency

Kafka tracks progress with offsets. A consumer group commits offsets to record
which records were processed.

Most Spring Kafka applications should be designed for at-least-once processing:
a record may be delivered more than once after a crash, rebalance, retry, or
commit failure.

## Ordering

Kafka preserves order within one partition, not across the whole topic.

If order matters per order, account, or customer, use that id as the record key.

```java
kafkaTemplate.send("orders.status-changed.v1", orderId.toString(), event);
```

This keeps events for the same order in the same partition.

## Idempotent Consumer

An idempotent consumer can safely process the same event more than once.

```java
@Service
public class InvoiceService {
    private final ProcessedEventRepository processedEvents;
    private final InvoiceRepository invoices;

    @Transactional
    public void createInvoice(String eventId, Long orderId, BigDecimal amount) {
        if (processedEvents.existsById(eventId)) {
            return;
        }

        invoices.save(new Invoice(orderId, amount));
        processedEvents.save(new ProcessedEvent(eventId, Instant.now()));
    }
}
```

This pattern is common when the consumer writes to a database. The event id is
stored in the same transaction as the side effect.

## Offset Commit Rule

Do not manually commit offsets unless the service has a specific reason and the
team understands the consequences.

With normal listener configuration, let Spring Kafka commit after successful
processing and let the error handler manage failed records.

## Key Idea

Kafka gives ordered logs per partition, not exactly-once business effects.
Idempotency is usually the application-level responsibility.
