# Blocking Calls and Schedulers

Blocking calls are the main practical risk in WebFlux applications.

WebFlux is designed around non-blocking I/O. If code blocks event-loop threads,
the service can lose the scalability benefits of the reactive stack.

## Blocking Examples

Common blocking operations:

- JDBC queries;
- JPA/Hibernate repositories;
- `Thread.sleep`;
- blocking file I/O;
- synchronous third-party SDK calls;
- calling `block()` on `Mono` or `Flux`.

## Isolating Blocking Work

If blocking work cannot be avoided, isolate it on an appropriate scheduler:

```java
Mono<OrderResponse> findById(String id) {
    return Mono.fromCallable(() -> blockingOrderService.findById(id))
        .subscribeOn(Schedulers.boundedElastic());
}
```

This is a compromise, not a reason to wrap an entire blocking application in
WebFlux.

## Prefer Matching Stacks

If the persistence layer is JDBC/JPA-heavy, Spring MVC is often a clearer fit.
If the stack uses reactive MongoDB, R2DBC, WebClient, and reactive messaging,
WebFlux becomes more coherent.

## Key Idea

WebFlux works best when the full request path is non-blocking. Isolate blocking
work deliberately when it cannot be removed.
