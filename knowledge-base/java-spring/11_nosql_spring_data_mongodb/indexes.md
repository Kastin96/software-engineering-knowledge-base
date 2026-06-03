# Indexes

Indexes are critical for MongoDB query performance.

A query pattern without a suitable index can become expensive as the collection
grows.

## Annotation-Based Index

```java
@Document("orders")
public class OrderDocument {
    @Indexed
    private String customerId;

    @Indexed
    private Instant createdAt;
}
```

Annotations document expected query paths, but production index management
should still be coordinated with deployment and database operations.

## Compound Index

```java
@CompoundIndex(
    name = "customer_status_created_idx",
    def = "{ 'customerId': 1, 'status': 1, 'createdAt': -1 }"
)
@Document("orders")
public class OrderDocument {
}
```

Compound indexes should match real filter and sort patterns.

## Unique Index

```java
@Indexed(unique = true)
private String orderNumber;
```

Unique indexes enforce constraints at the database level. Application checks
alone are not enough under concurrency.

## Key Idea

Design indexes from query patterns. Treat index creation as production database
change, not just a Java annotation.
