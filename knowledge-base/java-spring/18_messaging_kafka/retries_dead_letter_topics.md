# Retries and Dead Letter Topics

Kafka consumers should distinguish retryable and non-retryable failures.

Retryable failures are temporary: network timeout, downstream service outage,
short database issue. Non-retryable failures are usually data or contract
problems: invalid payload, unknown enum, missing required business field.

## Blocking Retry With DLT

```java
@Configuration
public class KafkaErrorHandlingConfig {

    @Bean
    DefaultErrorHandler kafkaErrorHandler(KafkaTemplate<Object, Object> kafkaTemplate) {
        DeadLetterPublishingRecoverer recoverer = new DeadLetterPublishingRecoverer(
            kafkaTemplate,
            (record, exception) -> new TopicPartition(record.topic() + ".DLT", record.partition())
        );

        DefaultErrorHandler handler = new DefaultErrorHandler(
            recoverer,
            new FixedBackOff(1_000L, 2L)
        );

        handler.addNotRetryableExceptions(
            IllegalArgumentException.class,
            MessageConversionException.class
        );

        return handler;
    }
}
```

This retries the record and sends it to a dead letter topic after attempts are
exhausted.

## Non-Blocking Retry

Spring Kafka also supports retry topics with `@RetryableTopic`.

```java
@RetryableTopic(
    attempts = "4",
    backoff = @Backoff(delay = 1_000, multiplier = 2),
    dltTopicSuffix = ".DLT"
)
@KafkaListener(topics = "orders.paid.v1", groupId = "invoice-service")
public void handle(OrderPaidEvent event) {
    invoiceService.createInvoice(event);
}
```

Non-blocking retries move failed records to retry topics instead of keeping the
consumer thread blocked for the whole delay.

## DLT Handling

```java
@DltHandler
public void handleDlt(OrderPaidEvent event) {
    log.error("OrderPaidEvent moved to DLT: eventId={}, orderId={}",
        event.eventId(),
        event.orderId()
    );
}
```

Keep the DLT handler close to the retryable listener so the failure path is easy
to inspect and test.

DLT records should be visible to support teams through alerts, dashboards, or a
replay process.

## Key Idea

Retries are not a substitute for understanding failure types. Decide which
errors should retry, which should go to DLT, and how DLT records will be
investigated.
