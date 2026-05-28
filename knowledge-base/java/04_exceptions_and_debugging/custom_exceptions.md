# Custom Exceptions

## Goal

Understand when to create custom exceptions and how to design them so they add
useful context instead of noise.

## Why It Matters

Custom exceptions help express domain failures clearly: duplicate user, invalid
order state, missing account, failed import, or unsupported operation. They are
most useful when callers can handle a specific failure differently or when the
exception name makes logs and tests easier to understand.

## Basic Custom Runtime Exception

```java
public class DuplicateUserException extends RuntimeException {
    public DuplicateUserException(String email) {
        super("User already exists: " + email);
    }
}
```

Use it when the domain rule fails.

```java
if (userRepository.existsByEmail(email)) {
    throw new DuplicateUserException(email);
}
```

The exception name communicates the rule more clearly than a generic
`RuntimeException`.

## Preserve the Cause

When wrapping another exception, keep the cause.

```java
public class ImportFailedException extends RuntimeException {
    public ImportFailedException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

```java
try {
    importFile(path);
} catch (java.io.IOException exception) {
    throw new ImportFailedException("Could not import file " + path, exception);
}
```

Preserving the cause keeps the original stack trace available.

## Add Useful Fields Carefully

Sometimes a custom exception can expose structured context.

```java
public class OrderNotFoundException extends RuntimeException {
    private final String orderId;

    public OrderNotFoundException(String orderId) {
        super("Order not found: " + orderId);
        this.orderId = orderId;
    }

    public String orderId() {
        return orderId;
    }
}
```

This can help tests or boundary layers map the exception to a response. Avoid
putting sensitive data into exception messages or fields.

## Checked or Unchecked?

Most custom business exceptions in service-layer Java code are unchecked.

```java
public class InvalidOrderStateException extends RuntimeException {
    public InvalidOrderStateException(String message) {
        super(message);
    }
}
```

Use checked custom exceptions only when callers are expected to recover and the
API should force them to handle the failure.

## Practical Example

```java
public class OrderService {
    public void pay(Order order) {
        if (order == null) {
            throw new IllegalArgumentException("order must not be null");
        }

        if (order.paid()) {
            throw new InvalidOrderStateException("order is already paid: " + order.id());
        }

        order.markPaid();
    }
}

class InvalidOrderStateException extends RuntimeException {
    InvalidOrderStateException(String message) {
        super(message);
    }
}

class Order {
    private final String id;
    private boolean paid;

    Order(String id) {
        this.id = id;
    }

    String id() {
        return id;
    }

    boolean paid() {
        return paid;
    }

    void markPaid() {
        paid = true;
    }
}
```

`IllegalArgumentException` handles a bad method argument. The custom exception
handles a domain state problem.

## Common Mistakes

- Creating custom exceptions that add no meaning over existing exception types.
- Losing the original cause when wrapping low-level failures.
- Putting passwords, tokens, or full personal data in exception messages.
- Creating checked exceptions for every business rule.
- Catching custom exceptions immediately after throwing them in the same layer.

## Interview Questions

1. When should you create a custom exception?
2. Should custom business exceptions usually be checked or unchecked?
3. Why is preserving the cause important?
4. What information should not be included in exception messages?
5. How can custom exceptions improve tests?

## Practice

1. Create `ProductNotFoundException` with a product id field.
2. Create `InvalidOrderStateException` for invalid order transitions.
3. Wrap an `IOException` in a custom import exception.
4. Review an exception message and remove sensitive or unnecessary data.

## Related Topics

- [Checked and Unchecked Exceptions](checked_unchecked.md)
- [Stack Traces](stack_traces.md)
- [Encapsulation](../02_oop_core_concepts/encapsulation.md)

