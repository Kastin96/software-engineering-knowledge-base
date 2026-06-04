# Kafka Cheatsheet

## Core Terms

- topic: stream name;
- partition: ordered subset of a topic;
- offset: record position;
- key: partitioning and ordering input;
- consumer group: consumers sharing work.

## Producer

- Publish explicit event contracts.
- Use meaningful keys.
- Do not publish JPA entities.
- Handle send failures when events are business-critical.

## Consumer

- Keep listeners thin.
- Delegate to application services.
- Expect duplicate delivery.
- Make side effects idempotent.

## Failure Handling

- retry temporary failures;
- route exhausted or invalid records to DLT;
- monitor lag and DLT count;
- document replay/support process.

## Quick Questions

- What is the key?
- What is the consumer group?
- What happens on duplicate delivery?
- What happens when processing fails?
