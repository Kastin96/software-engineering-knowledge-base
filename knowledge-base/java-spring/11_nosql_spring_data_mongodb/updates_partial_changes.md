# Updates and Partial Changes

MongoDB supports partial document updates.

Spring Data exposes these through `MongoTemplate` and update operators.

## Partial Update

```java
Query query = Query.query(Criteria.where("_id").is(orderId));

Update update = new Update()
    .set("status", "SHIPPED")
    .set("updatedAt", Instant.now());

UpdateResult result = mongoTemplate.updateFirst(
    query,
    update,
    OrderDocument.class
);
```

Check the update result when missing documents or state conditions matter.

## Conditional Update

```java
Query query = Query.query(
    Criteria.where("_id").is(orderId)
        .and("status").is("PAID")
);

Update update = new Update().set("status", "CANCELLED");

UpdateResult result = mongoTemplate.updateFirst(query, update, OrderDocument.class);
```

The current state is part of the update condition. If no document is modified,
the service can translate that into not found or conflict behavior.

## Replace Versus Patch

Saving a full document can overwrite fields if the object is stale or incomplete.
Use partial updates when only specific fields should change.

## Key Idea

Partial updates are useful, but they should include state conditions and result
checks when application correctness depends on them.
