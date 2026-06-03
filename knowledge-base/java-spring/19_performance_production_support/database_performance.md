# Database Performance

Database behavior is one of the most common causes of slow Spring services.

Start with query count and query plans. Application code can look clean while
generating inefficient SQL.

## N+1 Problem

```java
List<Order> orders = orderRepository.findByStatus("PAID");

for (Order order : orders) {
    log.info("Customer name: {}", order.getCustomer().getName());
}
```

If `customer` is lazy-loaded, this can produce one query for orders and one
extra query per order.

## Fetch Join

```java
@Query("""
    select o
    from Order o
    join fetch o.customer
    where o.status = :status
    """)
List<Order> findByStatusWithCustomer(String status);
```

Use fetch joins carefully. Fetching too much data can be as bad as fetching too
little.

## Pagination

```java
Page<OrderSummary> findByCustomerId(Long customerId, Pageable pageable);
```

Do not return unbounded lists from production endpoints. Paginate large
collections and make sorting explicit.

## Indexes

If a query filters or sorts by a column frequently, check whether an index is
needed.

```sql
create index idx_orders_customer_id_created_at
on orders (customer_id, created_at);
```

The right index depends on the query shape. Always verify with an execution
plan.

## Key Idea

Spring Data makes data access convenient, but it does not remove the need to
understand SQL, indexes, and generated query behavior.
