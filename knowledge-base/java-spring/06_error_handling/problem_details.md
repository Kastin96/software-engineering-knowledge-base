# Problem Details

Problem Details is a standard-style response shape for HTTP API errors.

In Spring applications, `ProblemDetail` can be used to represent structured
error responses with fields such as status, title, detail, type, and additional
properties.

## Example

```java
@RestControllerAdvice
class ApiExceptionHandler {
    @ExceptionHandler(OrderNotFoundException.class)
    ProblemDetail handleOrderNotFound(OrderNotFoundException ex) {
        ProblemDetail problem = ProblemDetail.forStatus(HttpStatus.NOT_FOUND);
        problem.setTitle("Order not found");
        problem.setDetail("The requested order does not exist.");
        problem.setProperty("code", "ORDER_NOT_FOUND");
        problem.setProperty("orderId", ex.orderId());
        return problem;
    }
}
```

## Why It Helps

A consistent error schema helps:

- frontend clients parse errors reliably;
- API consumers distinguish error types;
- logs and traces correlate client-visible failures;
- documentation stay aligned with implementation.

## Be Careful With Detail

Client-facing details should be safe. Do not include SQL statements, stack
traces, internal hostnames, secret values, or raw downstream error payloads.

## Custom Error DTO Versus ProblemDetail

A custom `ApiError` type can work well if the organization already has a
standard format. `ProblemDetail` is useful when you want a framework-supported
structured shape.

## Key Idea

Pick one error response format and make it consistent. The exact type matters
less than predictability and safe content.
