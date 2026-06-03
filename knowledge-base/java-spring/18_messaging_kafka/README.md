# Messaging and Kafka

This section covers Kafka usage in Spring Boot services: publishing events,
consuming records, handling failures, testing, and designing reliable message
flows.

The focus is not Kafka cluster administration. The focus is what a backend Java
developer needs when a Spring service communicates asynchronously with other
systems.

## Topics

- 01\. [Messaging Basics](messaging_basics.md)
- 02\. [Spring Boot Kafka Setup](spring_boot_kafka_setup.md)
- 03\. [Producing Events](producing_events.md)
- 04\. [Consuming Events](consuming_events.md)
- 05\. [Serialization and Contracts](serialization_contracts.md)
- 06\. [Retries and Dead Letter Topics](retries_dead_letter_topics.md)
- 07\. [Offsets, Ordering, and Idempotency](offsets_ordering_idempotency.md)
- 08\. [Transactions and Outbox Pattern](transactions_outbox_pattern.md)
- 09\. [Observability and Operations](observability_operations.md)
- 10\. [Testing Kafka Applications](testing_kafka_applications.md)
- 11\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with messaging basics and Spring Boot setup. Then study producer and
consumer code, because those are the daily entry points. After that, move to
serialization, retries, offsets, idempotency, transactions, observability, and
testing.

## Mini Goal

By the end of this section, you should be able to:

- explain when Kafka is useful and when it is unnecessary;
- configure a Spring Boot service for Kafka;
- publish events with a stable key and meaningful payload;
- consume events with clear error handling;
- distinguish retryable and non-retryable failures;
- explain offset commits, ordering, and consumer groups;
- avoid duplicate side effects with idempotent consumers;
- test Kafka flows with an isolated broker.

## Interview Readiness

You should be able to answer:

- What is the difference between a topic, partition, offset, and consumer group?
- Why does Kafka usually provide at-least-once processing?
- Why does message key matter?
- What is a dead letter topic?
- How do you avoid duplicate processing?
- When would you use the outbox pattern?
- How would you test a Kafka producer or consumer in Spring Boot?
