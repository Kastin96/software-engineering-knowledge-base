# API Versioning

API versioning is a compatibility strategy.

The best versioning approach depends on client ownership, deployment cadence,
and how often breaking changes are expected.

## URI Versioning

```text
/api/v1/orders
/api/v2/orders
```

This is explicit and easy to route, document, and test. It is also the most
common approach in many internal and external APIs.

## Header Versioning

```text
GET /api/orders
X-API-Version: 2
```

This keeps URLs stable, but version selection is less visible and can be harder
to test manually.

## Avoid Versioning Too Early

Not every internal service needs versioned endpoints from day one. If the only
consumer is deployed with the service, backward compatibility may be handled at
the deployment level.

For external or long-lived clients, versioning is usually worth planning.

## Breaking Changes

Examples of breaking changes:

- removing a field from a response;
- renaming a field;
- changing field meaning;
- changing validation rules in a way that rejects existing clients;
- changing status code behavior clients rely on.

## Key Idea

Versioning is about client compatibility. Choose an approach based on who
consumes the API and how safely clients can be upgraded.
