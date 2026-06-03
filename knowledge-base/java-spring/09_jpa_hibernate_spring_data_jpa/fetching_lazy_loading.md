# Fetching and Lazy Loading

Fetching controls when related data is loaded from the database.

Lazy loading delays loading until the relationship is accessed. Eager loading
loads related data immediately.

## Lazy Relationship

```java
@ManyToOne(fetch = FetchType.LAZY)
private CustomerEntity customer;
```

Lazy loading can avoid unnecessary data access, but it requires an active
persistence context when the relationship is accessed.

## Common Problem

Accessing a lazy relationship after the transaction/session is closed can fail.

Another common issue is accidentally triggering many extra queries while mapping
entities to DTOs.

## Fetch Join

For known use cases, fetch required associations explicitly:

```java
@Query("""
    select o
    from OrderEntity o
    join fetch o.customer
    where o.id = :id
    """)
Optional<OrderEntity> findByIdWithCustomer(Long id);
```

## Avoid Global Eager Loading

Setting many relationships to eager loading often creates large implicit query
graphs. Prefer use-case-specific fetch plans.

## Key Idea

Fetching is a query design decision. Make required data explicit for each use
case instead of relying on accidental lazy or eager behavior.
