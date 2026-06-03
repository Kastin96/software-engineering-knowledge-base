# MongoTemplate

`MongoTemplate` provides explicit MongoDB operations with Spring mapping and
exception translation.

It is useful when repository methods are not expressive enough.

## Query Example

```java
Query query = Query.query(
    Criteria.where("customerId").is(customerId)
        .and("status").is(status)
).with(Sort.by(Sort.Direction.DESC, "createdAt"));

List<OrderDocument> orders = mongoTemplate.find(
    query,
    OrderDocument.class
);
```

## Update Example

```java
Query query = Query.query(Criteria.where("_id").is(orderId));
Update update = new Update()
    .set("status", "CANCELLED")
    .set("updatedAt", Instant.now());

UpdateResult result = mongoTemplate.updateFirst(
    query,
    update,
    OrderDocument.class
);
```

## When To Use It

Use `MongoTemplate` for:

- dynamic criteria;
- partial updates;
- aggregation pipelines;
- index operations;
- advanced MongoDB operators;
- custom repository implementations.

## Key Idea

`MongoTemplate` is the explicit MongoDB access tool in Spring Data. Use it when
the query or update shape deserves to be visible.
