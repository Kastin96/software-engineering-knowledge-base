# WebFlux Exercises

## Exercise 1: Reactive Lookup

Build `GET /api/customers/{id}/activity` with WebFlux.

Acceptance criteria:

- service returns `Mono<CustomerActivityResponse>`;
- missing customer becomes `404`;
- no `block()` appears in the reactive flow;
- service test uses `StepVerifier`;
- controller test uses `WebTestClient`.

## Exercise 2: Merge Two Reactive Sources

Combine customer profile and recent events.

Acceptance criteria:

- independent calls are combined reactively;
- failure behavior is explicit;
- test covers success and one failure case.

## Exercise 3: Blocking Boundary

Call a blocking adapter from a reactive flow safely.

Acceptance criteria:

- blocking work is isolated;
- reason for blocking integration is documented;
- test verifies the returned publisher behavior.
