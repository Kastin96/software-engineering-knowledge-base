# Pagination, Filtering, and Sorting

Collection endpoints should avoid returning unbounded result sets.

Pagination, filtering, and sorting are part of the API contract, not only a
database concern.

## Basic Spring MVC Shape

```java
@GetMapping
Page<OrderResponse> search(
    @RequestParam(required = false) String status,
    Pageable pageable
) {
    return orderService.search(status, pageable);
}
```

Example request:

```text
GET /api/orders?status=PAID&page=0&size=20&sort=createdAt,desc
```

## Be Careful With Direct Pageable Exposure

`Pageable` is convenient, especially with Spring Data. For public APIs, decide
whether the default Spring query parameter format is acceptable. Some teams
prefer explicit request parameters or a custom response envelope.

## Response Shape

Avoid returning only a list when clients need pagination metadata.

Useful metadata:

- current page;
- page size;
- total elements;
- total pages;
- sort information;
- whether another page exists.

## Filtering

Keep filter names aligned with API fields, not database column names.

```text
GET /api/orders?status=PAID&customerId=10
```

Do not expose internal persistence details unless they are intentionally part of
the API.

## Key Idea

Collection endpoints need explicit limits and predictable query behavior. Design
pagination and filtering as API behavior, then implement the persistence query.
