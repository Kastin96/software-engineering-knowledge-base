# DTOs, Entities, and Boundaries

DTOs and entities have different jobs.

An entity models persistence and domain state. A DTO models data crossing a
boundary: HTTP request, HTTP response, Kafka event, or external API payload.

## Request DTO

```java
public record CreateOrderRequest(
    @NotNull Long customerId,
    @NotEmpty List<CreateOrderItemRequest> items
) {
}
```

## Response DTO

```java
public record OrderResponse(
    Long id,
    String status,
    BigDecimal totalAmount
) {
    static OrderResponse from(Order order) {
        return new OrderResponse(
            order.getId(),
            order.getStatus().name(),
            order.getTotalAmount()
        );
    }
}
```

## Why Not Return Entities

Returning JPA entities from controllers can expose:

- internal fields;
- lazy-loading behavior;
- bidirectional relationships;
- persistence-specific structure;
- accidental API changes.

## Mapper Rule

Small mapping can live in static factory methods or simple mapper classes.
Introduce mapping frameworks only when manual mapping becomes repetitive enough
to justify the dependency.

## Key Idea

DTOs protect boundaries. They let the API evolve deliberately instead of leaking
internal persistence design.
