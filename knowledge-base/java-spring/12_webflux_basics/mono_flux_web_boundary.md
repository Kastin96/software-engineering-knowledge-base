# `Mono` and `Flux` at the Web Boundary

WebFlux uses reactive types at the HTTP boundary.

In Reactor:

- `Mono<T>` represents zero or one value;
- `Flux<T>` represents zero to many values.

## Single Response

```java
@GetMapping("/{id}")
Mono<OrderResponse> findById(@PathVariable String id) {
    return orderService.findById(id);
}
```

The endpoint returns one order or completes with an error.

## Collection Response

```java
@GetMapping
Flux<OrderResponse> findByCustomer(@RequestParam String customerId) {
    return orderService.findByCustomer(customerId);
}
```

The endpoint can stream or collect multiple values depending on content type and
client behavior.

## Do Not Subscribe In Controllers

Do not call `subscribe()` in controller methods:

```java
orderService.findById(id).subscribe();
```

The framework subscribes to the returned publisher. Controller code should
return the reactive pipeline.

## Key Idea

At the WebFlux boundary, return the reactive pipeline. Do not block or subscribe
manually inside the controller.
