# Request Body and Response Body

`@RequestBody` tells Spring to deserialize the HTTP request body into a Java
type. Return values from `@RestController` methods are serialized into the HTTP
response body.

## Request Body

```java
@PostMapping
ResponseEntity<OrderResponse> create(@RequestBody CreateOrderRequest request) {
    OrderResponse created = orderService.create(request);
    return ResponseEntity.status(HttpStatus.CREATED).body(created);
}
```

In real APIs, request bodies are usually DTOs or command objects, not entities.

```java
public record CreateOrderRequest(
    Long customerId,
    List<CreateOrderItemRequest> items
) {
}
```

## Response Body

```java
public record OrderResponse(
    Long id,
    String status,
    BigDecimal total
) {
}
```

The response should represent the public API contract, not necessarily the full
internal model.

## Serialization Boundary

Spring usually uses Jackson for JSON serialization and deserialization. Be
careful with types that are hard to serialize predictably, lazy-loaded
relationships, bidirectional object graphs, and fields that should not be
publicly exposed.

## Key Idea

Request and response bodies are API contracts. Treat them as boundary types, not
as accidental dumps of internal objects.
