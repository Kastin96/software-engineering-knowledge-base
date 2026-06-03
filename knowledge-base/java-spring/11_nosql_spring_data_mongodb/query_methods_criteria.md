# Query Methods and Criteria

Spring Data MongoDB supports both repository query methods and explicit criteria
queries.

Choose the style based on query complexity and readability.

## Repository Query Method

```java
List<OrderDocument> findByCustomerIdAndStatus(String customerId, String status);
```

This is concise for simple equality filters.

## Criteria Query

```java
Criteria criteria = Criteria.where("customerId").is(customerId);

if (status != null) {
    criteria = criteria.and("status").is(status);
}

Query query = Query.query(criteria)
    .limit(50)
    .with(Sort.by(Sort.Direction.DESC, "createdAt"));

List<OrderDocument> orders = mongoTemplate.find(query, OrderDocument.class);
```

Criteria queries are better when filters are optional or dynamic.

## Field Names

Be deliberate with field names. Query code should match persisted document
fields, not accidental Java property renames.

If field names differ, use mapping annotations and test queries that depend on
those fields.

## Key Idea

Repository methods are good for simple lookups. Criteria queries are better for
dynamic search and explicit MongoDB query behavior.
