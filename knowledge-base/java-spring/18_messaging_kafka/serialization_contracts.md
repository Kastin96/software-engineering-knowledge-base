# Serialization and Contracts

Kafka records are integration contracts. Changing them carelessly breaks
consumers.

Spring Kafka supports JSON serialization, String payloads, byte arrays, and
custom serializers. Many production systems use Avro, Protobuf, or JSON Schema
with a schema registry, but JSON is still common in Spring Boot services.

## JSON Event Contract

```java
public record CustomerEmailChangedEvent(
    String eventId,
    Long customerId,
    String oldEmail,
    String newEmail,
    Instant occurredAt
) {
}
```

Use stable field names. Avoid exposing internal class names, package names, or
JPA relationships as part of the event contract.

## Compatibility Rules

Usually safe:

- adding optional fields;
- adding fields with defaults;
- keeping old fields until consumers migrate.

Risky or breaking:

- renaming fields;
- changing field meaning;
- changing numeric/string types;
- removing fields used by consumers;
- changing topic key semantics.

## Trusted Packages

```yaml
spring:
  kafka:
    consumer:
      properties:
        spring.json.trusted.packages: com.example.customers.events
```

Do not set trusted packages to `*` in production unless the risk is understood
and controlled.

## Versioning

For important contracts, prefer explicit versioning:

```text
customers.email-changed.v1
customers.email-changed.v2
```

Versioning gives consumers time to migrate instead of forcing a synchronized
release across services.

## Key Idea

The event payload is not an internal DTO. It is a public contract between
systems and should evolve deliberately.
