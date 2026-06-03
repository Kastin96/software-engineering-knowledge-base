# Reactive Error Handling

In WebFlux, errors are part of the reactive pipeline.

Instead of throwing after blocking for a result, reactive code often maps,
switches, or resumes based on pipeline signals.

## Not Found Example

```java
Mono<OrderResponse> findById(String id) {
    return orderRepository.findById(id)
        .map(OrderResponse::from)
        .switchIfEmpty(Mono.error(new OrderNotFoundException(id)));
}
```

## Client Error Mapping

```java
Mono<PaymentResponse> findPayment(String id) {
    return paymentWebClient.get()
        .uri("/payments/{id}", id)
        .retrieve()
        .onStatus(HttpStatusCode::isError,
            response -> Mono.error(new PaymentClientException(id)))
        .bodyToMono(PaymentResponse.class);
}
```

## Global Handling

For annotated controllers, exception handling can still use controller advice
patterns. The important part is to return or propagate errors through the
reactive pipeline rather than hiding them with side effects.

## Avoid Swallowing Errors

`onErrorResume` is useful, but it can hide real failures if used broadly.

```java
.onErrorResume(ex -> Mono.empty())
```

Use it only when an empty fallback is actually correct.

## Key Idea

Reactive error handling should preserve application meaning while staying inside
the pipeline.
