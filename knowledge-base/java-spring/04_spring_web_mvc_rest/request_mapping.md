# Request Mapping

Request mapping connects HTTP methods and paths to controller methods.

Spring MVC provides method-specific annotations that make intent clearer than a
generic `@RequestMapping`.

## Common Mappings

```java
@GetMapping("/{id}")
OrderResponse findById(@PathVariable Long id) {
    return orderService.findById(id);
}

@PostMapping
ResponseEntity<OrderResponse> create(@RequestBody CreateOrderRequest request) {
    OrderResponse created = orderService.create(request);
    return ResponseEntity.status(HttpStatus.CREATED).body(created);
}

@DeleteMapping("/{id}")
ResponseEntity<Void> delete(@PathVariable Long id) {
    orderService.delete(id);
    return ResponseEntity.noContent().build();
}
```

## HTTP Method Intent

Use methods according to resource behavior:

- `GET` reads data;
- `POST` creates a resource or starts a command;
- `PUT` replaces a resource;
- `PATCH` partially updates a resource;
- `DELETE` removes a resource or marks it removed.

## Avoid Verb-Heavy URLs

Prefer resource-oriented paths:

```text
POST /api/orders
```

Instead of:

```text
POST /api/createOrder
```

Commands that do not fit clean CRUD can still be modeled explicitly:

```text
POST /api/orders/{id}/cancel
```

## Key Idea

Request mapping should make the API contract clear before the method body is
read.
