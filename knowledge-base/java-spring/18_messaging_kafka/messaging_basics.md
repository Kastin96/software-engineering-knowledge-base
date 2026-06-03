# Messaging Basics

Messaging is useful when services should communicate without a direct synchronous
HTTP call.

Kafka is commonly used for event-driven integration, audit/event streams,
asynchronous processing, and decoupling producers from consumers.

## Core Terms

- topic: named stream of records;
- partition: ordered subset of a topic;
- offset: position of a record inside a partition;
- producer: writes records to a topic;
- consumer: reads records from a topic;
- consumer group: set of consumers sharing work for a topic;
- key: value used for partitioning and ordering within a partition.

## When Kafka Fits

Kafka fits when:

- events must be consumed by multiple systems;
- consumers can process asynchronously;
- replaying historical records is useful;
- throughput matters;
- ordering is required per entity, such as per order or account.

Kafka is usually not the right first choice when the caller needs an immediate
response or the workflow is a simple request/response operation.

## Event Shape

Good events describe something that happened.

```text
OrderPaid
OrderShipmentRequested
CustomerEmailChanged
PaymentFailed
```

Avoid command-like names unless the topic is intentionally used as a command
queue.

## Key Idea

Kafka moves work from direct calls to event streams. That gives flexibility, but
it also introduces delivery, ordering, retry, and duplicate-processing concerns.
