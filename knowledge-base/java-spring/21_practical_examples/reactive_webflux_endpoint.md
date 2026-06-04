# Reactive WebFlux Endpoint

Build a WebFlux endpoint that returns a customer's recent activity.

## Requirements

- expose `GET /api/customers/{id}/activity`;
- call a reactive repository;
- call a reactive external client;
- combine the results without blocking;
- return `404` when the customer does not exist;
- test the service with `StepVerifier`;
- test the controller with `WebTestClient`.

## Service Shape

```java
public Mono<CustomerActivityResponse> findActivity(String customerId) {
    Mono<CustomerDocument> customer = customerRepository.findById(customerId)
        .switchIfEmpty(Mono.error(new CustomerNotFoundException(customerId)));

    Flux<ActivityEvent> events = activityClient.findRecentEvents(customerId);

    return customer.zipWith(events.collectList())
        .map(tuple -> CustomerActivityResponse.from(tuple.getT1(), tuple.getT2()));
}
```

## Rules

- Do not call `block()` inside the reactive pipeline.
- Use `Mono` for zero-or-one values.
- Use `Flux` for streams or collections.
- Keep blocking repositories out of WebFlux flows unless they are isolated on a
  bounded scheduler with a clear reason.

## Tests

```java
StepVerifier.create(service.findActivity("customer-1"))
    .expectNextMatches(response -> response.customerId().equals("customer-1"))
    .verifyComplete();
```

Use `WebTestClient` for route, status, and JSON response assertions.

## Review Questions

- Which dependencies are reactive?
- Where is error mapping handled?
- Is anything blocking inside the flow?
- What happens when the external client fails?
