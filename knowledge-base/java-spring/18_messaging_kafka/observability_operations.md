# Observability and Operations

Kafka problems often appear as lag, retry growth, DLT records, or producer send
failures.

A Spring service should expose enough signals to understand whether it is
publishing and consuming correctly.

## What To Monitor

Monitor:

- consumer lag per group and topic;
- records consumed per second;
- records produced per second;
- consumer errors and retry attempts;
- DLT record count;
- producer send failures;
- processing duration;
- rebalance frequency;
- broker connection errors.

## Useful Log Context

```java
log.info("Kafka event consumed: topic={}, partition={}, offset={}, key={}",
    record.topic(),
    record.partition(),
    record.offset(),
    record.key()
);
```

Do not log the full payload by default. Log ids and metadata that help find the
event.

## Listener With Record Metadata

```java
@KafkaListener(topics = "orders.paid.v1", groupId = "invoice-service")
public void handle(ConsumerRecord<String, OrderPaidEvent> record) {
    OrderPaidEvent event = record.value();

    log.info("Processing order event: eventId={}, topic={}, partition={}, offset={}",
        event.eventId(),
        record.topic(),
        record.partition(),
        record.offset()
    );

    invoiceService.createInvoice(event);
}
```

## Operational Questions

During an incident, ask:

- Is the producer publishing records?
- Is the consumer group assigned partitions?
- Is lag growing?
- Are records failing repeatedly?
- Are DLT records being created?
- Did a schema or payload change break consumers?

## Key Idea

Kafka observability should connect broker-level signals with application-level
business processing.
