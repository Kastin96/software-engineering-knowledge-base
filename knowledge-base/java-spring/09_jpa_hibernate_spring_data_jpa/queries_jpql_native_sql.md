# Queries with JPQL and Native SQL

Spring Data JPA supports derived queries, JPQL, and native SQL.

Each option has a place.

## Derived Query

```java
List<OrderEntity> findByCustomerIdAndStatus(Long customerId, String status);
```

Good for simple lookups.

## JPQL Query

```java
@Query("""
    select o
    from OrderEntity o
    where o.customerId = :customerId
      and o.status = :status
    order by o.createdAt desc
    """)
List<OrderEntity> findCustomerOrders(Long customerId, String status);
```

JPQL queries entity fields, not table columns.

## Native SQL

```java
@Query(
    value = """
        select *
        from orders
        where customer_id = :customerId
        order by created_at desc
        """,
    nativeQuery = true
)
List<OrderEntity> findCustomerOrdersNative(Long customerId);
```

Native SQL is useful for database-specific features, complex queries, or
performance-sensitive paths.

## Key Idea

Use derived queries for simple cases, JPQL for entity-oriented queries, and
native SQL when database-specific control matters.
