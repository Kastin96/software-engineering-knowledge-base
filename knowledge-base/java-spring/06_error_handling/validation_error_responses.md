# Validation Error Responses

Validation failures should return enough detail for clients to fix the request.

A useful validation error response identifies which fields failed and why.

## Example Shape

```json
{
  "code": "VALIDATION_FAILED",
  "message": "Request validation failed",
  "fields": [
    {
      "name": "email",
      "message": "must not be blank"
    },
    {
      "name": "items[0].quantity",
      "message": "must be greater than 0"
    }
  ]
}
```

## Spring Handler Example

```java
@ExceptionHandler(MethodArgumentNotValidException.class)
ResponseEntity<ValidationErrorResponse> handleValidation(
    MethodArgumentNotValidException ex
) {
    List<FieldErrorResponse> fields = ex.getBindingResult()
        .getFieldErrors()
        .stream()
        .map(error -> new FieldErrorResponse(
            error.getField(),
            error.getDefaultMessage()
        ))
        .toList();

    return ResponseEntity.badRequest().body(
        new ValidationErrorResponse("VALIDATION_FAILED", fields)
    );
}
```

## Avoid Raw Framework Output

Raw validation exceptions are often too verbose and inconsistent for API
clients. Translate them into your API's error format.

## Parameter Validation

Path and query parameter validation may throw different exception types than
request body validation. Keep the final response shape consistent.

## Key Idea

Validation error responses should be client-actionable: field, reason, and a
stable error code.
