# Status Codes and ResponseEntity

HTTP status codes are part of the API contract.

Spring lets a controller return plain objects for simple `200 OK` responses, or
`ResponseEntity` when status, headers, or empty responses need to be explicit.

## Plain Response

```java
@GetMapping("/{id}")
OrderResponse findById(@PathVariable Long id) {
    return orderService.findById(id);
}
```

This is appropriate for a normal successful read.

## Explicit Status

```java
@PostMapping
ResponseEntity<OrderResponse> create(@RequestBody CreateOrderRequest request) {
    OrderResponse created = orderService.create(request);
    return ResponseEntity.status(HttpStatus.CREATED).body(created);
}
```

`201 Created` communicates that a new resource was created.

## Empty Response

```java
@DeleteMapping("/{id}")
ResponseEntity<Void> delete(@PathVariable Long id) {
    orderService.delete(id);
    return ResponseEntity.noContent().build();
}
```

`204 No Content` is a common response for successful deletion without a body.

## Common Status Codes

- `200 OK` for successful reads or updates with a response body;
- `201 Created` for successful creation;
- `204 No Content` for successful actions without a response body;
- `400 Bad Request` for invalid input shape;
- `401 Unauthorized` for missing or invalid authentication;
- `403 Forbidden` for authenticated users without permission;
- `404 Not Found` for missing resources;
- `409 Conflict` for state or uniqueness conflicts.

## Key Idea

Use `ResponseEntity` when the response needs explicit HTTP semantics. Otherwise,
plain return values keep controller methods concise.
