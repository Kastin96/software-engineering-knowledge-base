# Error Handling Role

Error handling defines how failures cross the HTTP boundary.

Application code can fail for many reasons: invalid input, missing resources,
state conflicts, permission problems, downstream service failures, database
errors, timeouts, and programming bugs. A REST API should not expose those
failures as random Java exception messages.

## Responsibilities

The error handling layer should:

- map known failures to appropriate HTTP statuses;
- return a predictable response body;
- include enough client-facing detail to act on the error;
- avoid exposing stack traces or infrastructure internals;
- log diagnostic context for operators;
- keep controller methods focused on normal request flow.

## Controller Without Manual Catching

```java
@GetMapping("/{id}")
OrderResponse findById(@PathVariable Long id) {
    return orderService.findById(id);
}
```

The controller does not catch `OrderNotFoundException`. A global handler maps
that exception to the API error response.

## Boundary Translation

The service can express application meaning:

```java
throw new OrderNotFoundException(orderId);
```

The web layer translates that meaning into HTTP:

```text
404 Not Found
```

## Key Idea

Error handling is a boundary translation concern. Application code raises
meaningful failures; the web layer turns them into stable HTTP responses.
