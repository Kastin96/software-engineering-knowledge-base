# N+1 Problem

The N+1 problem happens when one query loads a list of entities and then each
entity triggers an additional query for related data.

## Example Shape

```text
select * from orders limit 20
select * from customers where id = ?
select * from customers where id = ?
select * from customers where id = ?
...
```

One query becomes one plus N additional queries.

## Common Cause

Mapping entities to DTOs can accidentally access lazy relationships:

```java
orders.stream()
    .map(order -> new OrderResponse(
        order.getId(),
        order.getCustomer().getName()
    ))
    .toList();
```

If customers were not fetched with orders, each `getCustomer()` may trigger a
query.

## Fix Options

Possible fixes include:

- fetch joins for specific use cases;
- entity graphs;
- DTO/projection queries;
- batching settings;
- separate optimized queries.

## Use-Case Specific Fetch

```java
@Query("""
    select o
    from OrderEntity o
    join fetch o.customer
    where o.status = :status
    """)
List<OrderEntity> findByStatusWithCustomer(String status);
```

## Key Idea

N+1 is a query design problem. Detect it with logs/tests/profiling and fix it
with use-case-specific fetching.
