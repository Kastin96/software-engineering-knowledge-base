# Global Exception Handler

`@ControllerAdvice` centralizes exception handling for controllers.

It allows controller methods to stay focused on the successful path while known
failures are translated in one place.

## Basic Handler

```java
@RestControllerAdvice
class ApiExceptionHandler {
    @ExceptionHandler(OrderNotFoundException.class)
    ResponseEntity<ApiError> handleOrderNotFound(OrderNotFoundException ex) {
        ApiError error = new ApiError(
            "ORDER_NOT_FOUND",
            "Order was not found"
        );

        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
}
```

`@RestControllerAdvice` combines `@ControllerAdvice` and response body
serialization behavior.

## Error DTO

```java
public record ApiError(
    String code,
    String message
) {
}
```

For real APIs, the error object may also include request ID, timestamp, field
errors, or documentation links.

## Multiple Handlers

You can handle application-specific exceptions, validation exceptions, and
framework exceptions in the same advice class or split them by concern.

Keep the response format consistent even if handlers are split.

## Handler Scope

Global handlers should not become business logic. They should map exception
meaning to API response shape.

## Key Idea

Use `@RestControllerAdvice` to centralize HTTP error translation and keep
controllers focused on normal request handling.
