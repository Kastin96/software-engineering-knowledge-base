# API Design Decisions

API design affects clients, support, and future changes.

A good API is explicit about resources, validation, errors, pagination, and
versioning.

## Resource Naming

```text
GET /api/orders/{orderId}
POST /api/orders
PATCH /api/orders/{orderId}/status
```

Use names that match the business language. Avoid exposing database table names
or internal workflow names.

## Pagination

```text
GET /api/orders?customerId=42&page=0&size=20&sort=createdAt,desc
```

Large collections should be paginated. Make default and maximum page sizes
explicit.

## Error Contract

Use one predictable error shape.

```json
{
  "code": "ORDER_NOT_FOUND",
  "message": "Order was not found",
  "details": {
    "orderId": 42
  }
}
```

## Versioning

Version only when needed, but when a breaking change is necessary, make the
migration path clear.

```text
/api/v1/orders
/api/v2/orders
```

## Key Idea

APIs are product surfaces for other developers. Design them as stable contracts,
not as direct projections of internal classes.
