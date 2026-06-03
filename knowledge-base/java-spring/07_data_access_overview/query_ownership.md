# Query Ownership

Query ownership defines where database query decisions live.

Poor query ownership leads to controllers building filters, services assembling
SQL fragments, or repositories becoming generic dumping grounds.

## Good Ownership

Controllers own HTTP parameters:

```java
GET /api/orders?status=PAID&customerId=10
```

Services own use-case meaning:

```java
orderService.searchOrders(criteria)
```

Repositories own persistence implementation:

```java
orderRepository.search(criteria)
```

## Criteria Object

```java
public record OrderSearchCriteria(
    Long customerId,
    OrderStatus status,
    LocalDate fromDate,
    LocalDate toDate
) {
}
```

A criteria object can prevent long parameter lists and keep query intent
explicit.

## Avoid SQL In Controllers

Controllers should not know table names, joins, column names, or query
construction rules. That creates a tight coupling between the API layer and
database schema.

## Key Idea

HTTP input, application meaning, and database query implementation should be
separate decisions.
