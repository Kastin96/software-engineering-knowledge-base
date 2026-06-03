# Exception Types

Not all exceptions should be handled the same way.

Useful error handling starts by separating failure categories.

## Common Categories

```text
validation error        -> request shape is invalid
not found error         -> requested resource does not exist
conflict error          -> requested action conflicts with current state
authorization error     -> user cannot perform the action
downstream error        -> external system failed
database error          -> persistence infrastructure failed
programming error       -> bug or unexpected internal condition
```

## Application Exceptions

Application-specific exceptions should express business or use-case meaning:

```java
public class OrderNotFoundException extends RuntimeException {
    private final Long orderId;

    public OrderNotFoundException(Long orderId) {
        super("Order not found: " + orderId);
        this.orderId = orderId;
    }

    public Long orderId() {
        return orderId;
    }
}
```

The message can help logs, but the API response should be built deliberately by
the handler.

## Framework Exceptions

Spring already throws exceptions for common web failures:

- unreadable JSON request body;
- missing request parameter;
- type conversion failure;
- validation failure;
- unsupported HTTP method;
- unsupported content type.

Handle these consistently instead of letting each one produce a different
response shape.

## Infrastructure Exceptions

Database and downstream exceptions should usually not be returned directly to
clients. Translate them into stable API responses and log the technical details.

## Key Idea

Categorize exceptions by API meaning, not only by Java class name.
