# Persistence Model Boundaries

Persistence models should not automatically become API models.

Database records, JPA entities, MongoDB documents, domain objects, and response
DTOs have different reasons to change.

## Common Model Types

```text
API request DTO       -> client input contract
API response DTO      -> client output contract
domain model          -> business behavior and invariants
persistence entity    -> database mapping
database record       -> raw query result
```

Some small applications collapse these models. That can be acceptable early,
but it creates coupling as the service grows.

## Risk Of Leaking Persistence Models

Returning persistence entities from controllers can expose:

- internal IDs;
- audit columns;
- lazy-loaded relationships;
- fields not intended for clients;
- database schema changes.

## Mapping Is A Boundary Cost

Mapping takes code, but it buys separation:

```java
OrderResponse response = OrderResponse.from(order);
```

For complex mapping, a mapper class can keep conversion consistent.

## Key Idea

Keep persistence shape and API shape separate when they have different
stability requirements.
