# Kafka Exercises

## Exercise 1: Publish Event

Publish `OrderPaidEvent` when payment succeeds.

Acceptance criteria:

- event contract is explicit;
- topic name is versioned;
- order id is used as the key;
- send failure is not silently ignored.

## Exercise 2: Consume Event

Consume `OrderPaidEvent` and create an invoice.

Acceptance criteria:

- listener delegates to an application service;
- invoice creation is idempotent;
- duplicate event test exists.

## Exercise 3: Retry and DLT

Configure retries and a dead letter topic.

Acceptance criteria:

- retryable and non-retryable errors are separated;
- DLT records are logged with event id;
- support workflow for DLT records is described.
