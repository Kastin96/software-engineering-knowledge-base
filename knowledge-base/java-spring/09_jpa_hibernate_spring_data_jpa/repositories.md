# Repositories

Spring Data JPA repositories provide a persistence boundary for JPA entities.

The repository should expose operations that make sense for the application,
not every possible database operation.

## Basic Repository

```java
interface OrderRepository extends JpaRepository<OrderEntity, Long> {
    List<OrderEntity> findByCustomerId(Long customerId);
}
```

Spring Data generates the implementation at runtime.

## Derived Queries

Method names can define simple queries:

```java
List<OrderEntity> findByStatusOrderByCreatedAtDesc(String status);
```

Derived queries are useful for straightforward lookups. If the method name
becomes long or unclear, use JPQL or a custom repository method.

## Optional Lookup

```java
Optional<OrderEntity> findByOrderNumber(String orderNumber);
```

Use `Optional` for lookups where the row may not exist.

## Avoid Over-Generic Repositories

Methods like this often push too much query design into callers:

```java
findByStatusAndCustomerIdAndCreatedAtBetweenAndTotalGreaterThan(...)
```

For search endpoints, consider a criteria object, specification, query method,
or custom repository implementation.

## Key Idea

Repositories should keep persistence access expressive without turning method
names into a query language that no one wants to maintain.
