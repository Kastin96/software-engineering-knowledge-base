# HTTP Status Mapping

HTTP status mapping should be consistent across the API.

The same kind of failure should not return `400` in one controller and `409` in
another.

## Common Mapping

```text
400 Bad Request      -> invalid request shape or invalid simple parameter
401 Unauthorized     -> missing or invalid authentication
403 Forbidden        -> authenticated but not allowed
404 Not Found        -> resource does not exist
409 Conflict         -> request conflicts with current resource state
422 Unprocessable    -> valid shape, semantically invalid request
500 Internal Error   -> unexpected server-side failure
503 Unavailable      -> dependency or service temporarily unavailable
```

Many teams use `400` for most client-side validation failures and `409` for
state conflicts. The exact mapping is less important than consistency and clear
documentation.

## Not Found

```java
@ExceptionHandler(OrderNotFoundException.class)
ResponseEntity<ApiError> handle(OrderNotFoundException ex) {
    return ResponseEntity.status(HttpStatus.NOT_FOUND)
        .body(new ApiError("ORDER_NOT_FOUND", "Order was not found"));
}
```

## Conflict

```java
@ExceptionHandler(OrderCancellationNotAllowedException.class)
ResponseEntity<ApiError> handle(OrderCancellationNotAllowedException ex) {
    return ResponseEntity.status(HttpStatus.CONFLICT)
        .body(new ApiError("ORDER_CANNOT_BE_CANCELLED", "Order cannot be cancelled"));
}
```

## Key Idea

Status codes should communicate API semantics, not the Java exception hierarchy.
