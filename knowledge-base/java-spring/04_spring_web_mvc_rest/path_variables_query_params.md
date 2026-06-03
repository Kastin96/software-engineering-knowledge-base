# Path Variables and Query Parameters

Path variables identify a resource. Query parameters modify how a collection or
operation is viewed.

## Path Variables

Use path variables for required resource identity:

```java
@GetMapping("/{orderId}")
OrderResponse findById(@PathVariable Long orderId) {
    return orderService.findById(orderId);
}
```

Example:

```text
GET /api/orders/42
```

The order ID is part of the resource path.

## Query Parameters

Use query parameters for optional filters, paging, sorting, and view options:

```java
@GetMapping
Page<OrderResponse> search(
    @RequestParam(required = false) String status,
    Pageable pageable
) {
    return orderService.search(status, pageable);
}
```

Example:

```text
GET /api/orders?status=PAID&page=0&size=20
```

## Practical Rule

If removing the value changes which resource is addressed, it probably belongs
in the path. If removing the value changes how results are filtered or displayed,
it probably belongs in the query string.

## Key Idea

Use path variables for identity and query parameters for selection, filtering,
and representation options.
