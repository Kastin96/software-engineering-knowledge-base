# MongoRepository

`MongoRepository` is the Spring Data repository abstraction for MongoDB.

It is useful for common CRUD operations and straightforward query methods.

## Basic Repository

```java
interface OrderRepository extends MongoRepository<OrderDocument, String> {
    List<OrderDocument> findByCustomerId(String customerId);
    Optional<OrderDocument> findByOrderNumber(String orderNumber);
}
```

Spring Data creates the implementation at runtime.

## Derived Queries

Derived query methods work well for simple lookup patterns:

```java
List<OrderDocument> findByStatusOrderByCreatedAtDesc(String status);
```

If the method name grows too long or the query requires advanced operators, use
`@Query`, `MongoTemplate`, or a custom repository implementation.

## Custom Query

```java
@Query("{ 'customerId': ?0, 'status': ?1 }")
List<OrderDocument> findCustomerOrders(String customerId, String status);
```

Use custom query strings carefully. They are production query logic and should
be tested.

## Key Idea

Use `MongoRepository` for simple and expressive access patterns. Move complex
queries or updates to `MongoTemplate` or custom repository code.
