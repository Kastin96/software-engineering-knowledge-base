# Common Mistakes

## Treating Kafka Like HTTP

Kafka is asynchronous. Do not build flows that secretly depend on an immediate
response unless the design explicitly supports request/reply.

## No Idempotency

Consumers can see the same record more than once. If processing creates a side
effect, make it duplicate-safe.

## Bad Message Keys

Random keys spread load but destroy per-entity ordering. Use a meaningful key
when ordering matters.

## Logging Full Payloads

Kafka payloads can contain customer data, tokens, or business-sensitive fields.
Log metadata and ids instead of full events.

## Infinite Retry Without Visibility

Retrying forever can hide data problems and block partitions. Failed records
need alerts, DLT handling, or a clear replay process.

## Breaking Event Contracts

Renaming fields or changing semantics without a migration plan breaks consumers.
Treat events as external contracts.

## Managing Topics Casually

Partitions, retention, compaction, and replication are production decisions.
Do not treat them as random local defaults.

## Ignoring Consumer Lag

If lag grows continuously, the service is falling behind. This can be caused by
slow processing, downstream failures, rebalances, or insufficient partitions.

## Key Idea

Kafka reliability comes from design choices around keys, contracts, retries,
idempotency, and observability. The annotation is the easy part.
