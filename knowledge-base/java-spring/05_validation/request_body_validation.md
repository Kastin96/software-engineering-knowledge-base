# Request Body Validation

Use `@Valid` on a request body to trigger Bean Validation for the deserialized
DTO.

```java
@PostMapping
ResponseEntity<OrderResponse> create(@Valid @RequestBody CreateOrderRequest request) {
    OrderResponse created = orderService.create(request);
    return ResponseEntity.status(HttpStatus.CREATED).body(created);
}
```

## Request DTO

```java
public record CreateOrderRequest(
    @NotNull Long customerId,
    @NotEmpty List<@Valid CreateOrderItemRequest> items
) {
}

public record CreateOrderItemRequest(
    @NotNull Long productId,
    @Positive int quantity
) {
}
```

The controller validates the request contract before calling the service.

## Keep DTOs Focused

Request DTOs should describe client input. They should not expose server-owned
fields:

- generated IDs;
- calculated totals;
- audit timestamps;
- internal status transitions.

## Failure Handling

Validation failures should be handled by a global error handler, not by each
controller method. The validation section defines the constraints; the error
handling section defines how failures become API responses.

## Key Idea

`@Valid @RequestBody` keeps invalid request payloads from entering application
services.
