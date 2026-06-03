# Document Model and Mapping

Spring Data MongoDB maps Java objects to MongoDB documents.

The document class is a persistence model. It should reflect how data is stored
and queried, not necessarily the public API response shape.

## Basic Document

```java
@Document("orders")
public class OrderDocument {
    @Id
    private String id;

    private String customerId;
    private String status;
    private BigDecimal total;
    private List<OrderItemDocument> items;
    private Instant createdAt;
}
```

`@Document` marks the class as a MongoDB document and can specify the collection
name.

## Embedded Documents

```java
public record OrderItemDocument(
    String productId,
    int quantity,
    BigDecimal price
) {
}
```

Embedding is useful when child data is usually read and updated with the parent
document.

## Document Shape

Document shape should follow access patterns:

- what is loaded together;
- what changes together;
- what needs independent querying;
- how large documents can become;
- which fields need indexes.

## API Boundary

Do not return documents directly from controllers by default. Map them to API
response DTOs so persistence changes do not leak into the HTTP contract.

## Key Idea

MongoDB mapping is not only annotation work. Document shape is a persistence
design decision.
