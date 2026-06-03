# DTO Boundaries

DTOs protect the API contract from internal implementation details.

Persistence entities, domain objects, and external API responses often have
different reasons to change. A stable REST API should not expose those internal
shapes by accident.

## Avoid Returning Entities Directly

```java
@GetMapping("/{id}")
Order findById(@PathVariable Long id) {
    return orderRepository.findById(id).orElseThrow();
}
```

This couples the public API to persistence structure.

Prefer a response DTO:

```java
@GetMapping("/{id}")
OrderResponse findById(@PathVariable Long id) {
    return orderService.findById(id);
}
```

```java
public record OrderResponse(
    Long id,
    String status,
    BigDecimal total
) {
}
```

## Separate Request And Response DTOs

Create and update requests often differ from responses:

```java
public record CreateOrderRequest(Long customerId, List<CreateOrderItemRequest> items) {
}

public record OrderResponse(Long id, String status, BigDecimal total) {
}
```

The client should not send server-owned fields such as `id`, `createdAt`, or
calculated totals.

## Mapping Location

Mapping can live in a dedicated mapper, service, or small private controller
method depending on complexity. Avoid spreading the same mapping logic across
many endpoints.

## Key Idea

DTOs are not ceremony. They keep API contracts stable and prevent persistence
or domain internals from leaking through HTTP.
