# `try`, `catch`, and `finally`

## Goal

Understand how `try`, `catch`, and `finally` control exception handling in Java.

## Why It Matters

Most bugs in exception handling are not about syntax. They come from catching
too broadly, hiding the real failure, logging without context, or cleaning up
resources manually when a safer tool exists.

## Basic `try` and `catch`

```java
public class NumberParser {
    public static void main(String[] args) {
        String input = "42";

        try {
            int number = Integer.parseInt(input);
            System.out.println(number);
        } catch (NumberFormatException exception) {
            System.out.println("Input is not a valid number: " + input);
        }
    }
}
```

The `try` block contains code that may fail. The `catch` block handles a specific
exception type.

## Catch Specific Exceptions

Prefer specific exceptions.

```java
try {
    int port = Integer.parseInt(value);
    System.out.println("Port: " + port);
} catch (NumberFormatException exception) {
    System.out.println("Port must be a number");
}
```

Avoid broad catches unless you are at a real boundary, such as a command-line
entry point, job runner, or request boundary.

```java
// Usually too broad inside business logic:
// catch (Exception exception) { ... }
```

## Multiple Catch Blocks

Order catch blocks from specific to general.

```java
try {
    int value = Integer.parseInt(input);
    int result = 100 / value;
    System.out.println(result);
} catch (NumberFormatException exception) {
    System.out.println("Input must be numeric");
} catch (ArithmeticException exception) {
    System.out.println("Input must not be zero");
}
```

If a general exception type comes first, more specific catch blocks may become
unreachable.

## Multi-Catch

Use multi-catch when different exception types receive the same handling.

```java
try {
    int value = Integer.parseInt(input);
    int result = 100 / value;
    System.out.println(result);
} catch (NumberFormatException | ArithmeticException exception) {
    System.out.println("Input must be a non-zero number");
}
```

Use this only when the response is truly the same.

## `finally`

`finally` runs whether the `try` block succeeds or fails.

```java
try {
    System.out.println("Running job");
} finally {
    System.out.println("Cleaning up");
}
```

Historically, `finally` was used for closing resources. For resources that
implement `AutoCloseable`, prefer try-with-resources.

## Do Not Swallow Exceptions

This is dangerous:

```java
try {
    processPayment();
} catch (Exception exception) {
    // ignored
}
```

The caller thinks the payment was processed, but the failure disappeared.

At minimum, handle the failure intentionally.

```java
try {
    processPayment();
} catch (PaymentException exception) {
    System.out.println("Payment failed: " + exception.getMessage());
    throw exception;
}
```

## Practical Example

```java
public class CsvLineParser {
    public static void main(String[] args) {
        ParseResult result = parseQuantity("item-100,3");
        System.out.println(result);
    }

    static ParseResult parseQuantity(String line) {
        try {
            String[] parts = line.split(",");
            String itemId = parts[0];
            int quantity = Integer.parseInt(parts[1]);
            return new ParseResult(itemId, quantity, null);
        } catch (ArrayIndexOutOfBoundsException | NumberFormatException exception) {
            return new ParseResult(null, 0, "Invalid line: " + line);
        }
    }

    record ParseResult(String itemId, int quantity, String error) {
    }
}
```

This parser treats invalid CSV input as an expected boundary problem and returns
a result object instead of crashing the whole program.

## Common Mistakes

- Catching `Exception` too early.
- Swallowing exceptions silently.
- Logging and rethrowing at every layer, which creates noisy duplicate logs.
- Catching an exception and continuing with partially initialized data.
- Using `finally` for resource cleanup when try-with-resources is available.

## Interview Questions

1. What does a `catch` block do?
2. Why should catch blocks usually be specific?
3. What is multi-catch?
4. Does `finally` run when an exception is thrown?
5. Why is swallowing exceptions dangerous?

## Practice

1. Parse a string into an integer and handle invalid input.
2. Add separate handling for invalid number and division by zero.
3. Rewrite two identical catch blocks using multi-catch.
4. Find a place where catching broadly would hide useful debugging information.

## Related Topics

- [Exceptions](exceptions.md)
- [Try-With-Resources](try_with_resources.md)
- [Stack Traces](stack_traces.md)

