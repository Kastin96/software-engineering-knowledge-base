# Projections and DTO Queries

JPA queries do not always need to load full entities.

For read-only API responses, reports, lists, or dashboards, projections can be
more appropriate than entity loading.

## Interface Projection

```java
public interface OrderSummaryProjection {
    Long getId();
    String getStatus();
    BigDecimal getTotal();
}
```

```java
List<OrderSummaryProjection> findByCustomerId(Long customerId);
```

## DTO Projection

```java
public record OrderSummary(
    Long id,
    String status,
    BigDecimal total
) {
}
```

```java
@Query("""
    select new com.example.orders.OrderSummary(o.id, o.status, o.total)
    from OrderEntity o
    where o.customerId = :customerId
    """)
List<OrderSummary> findSummariesByCustomerId(Long customerId);
```

## Why Use Projections

Projections can:

- avoid loading full entities;
- reduce accidental lazy loading;
- match API response shape;
- make read models explicit.

## Do Not Use Projections For Updates

If the use case changes entity state, load the entity that owns that state.
Projection queries are best for read-only views.

## Key Idea

Use entities for state changes and projections for read models that do not need
full entity lifecycle behavior.
