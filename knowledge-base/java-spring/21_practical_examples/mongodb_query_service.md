# MongoDB Query Service

Build a service that reads order documents from MongoDB and supports filtered
search.

## Requirements

- store order read models in MongoDB;
- search by customer id, status, and created date range;
- return paginated results;
- expose a REST endpoint for querying;
- define useful indexes.

## Document Model

```java
@Document("orders")
public class OrderDocument {
    @Id
    private String id;
    private String customerId;
    private String status;
    private BigDecimal totalAmount;
    private Instant createdAt;
}
```

## Query Design

Use repository methods for simple lookups. Use `MongoTemplate` when dynamic
criteria becomes clearer than many derived method names.

```java
Criteria criteria = Criteria.where("customerId").is(customerId);

if (status != null) {
    criteria = criteria.and("status").is(status);
}

Query query = Query.query(criteria)
    .with(pageable);
```

## Indexes

Create indexes for frequent filters:

```java
@CompoundIndex(name = "idx_customer_status_created",
    def = "{'customerId': 1, 'status': 1, 'createdAt': -1}")
```

## Tests

Use `@DataMongoTest` for query behavior. Use Testcontainers if the query depends
on production MongoDB behavior.

## Review Questions

- Why MongoDB for this read model?
- Which fields should be indexed?
- What happens when filters are optional?
- How does pagination behave for large collections?
