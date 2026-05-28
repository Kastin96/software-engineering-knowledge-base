# Stack Traces

## Goal

Learn how to read Java stack traces and use them to find the failing code path.

## Why It Matters

Stack traces are one of the fastest debugging tools in Java. A developer who can
read them calmly can usually locate the real failure much faster than someone
who only scans the first error line.

## Basic Stack Trace

Example:

```text
Exception in thread "main" java.lang.IllegalArgumentException: email must be valid
    at UserRegistration.validateEmail(UserRegistration.java:18)
    at UserRegistration.register(UserRegistration.java:10)
    at UserRegistration.main(UserRegistration.java:4)
```

Read it like this:

- exception type: `IllegalArgumentException`;
- message: `email must be valid`;
- failing line: `UserRegistration.java:18`;
- call path: `main` called `register`, which called `validateEmail`.

The top application frame is usually the best place to start.

## Caused By

Wrapped exceptions show a cause chain.

```text
Exception in thread "main" ImportFailedException: Could not import file users.csv
    at UserImporter.importUsers(UserImporter.java:22)
Caused by: java.io.IOException: Access is denied
    at java.base/sun.nio.fs.WindowsException.translateToIOException(...)
```

The top exception explains the current layer's context. The `Caused by` section
shows the lower-level root failure.

Do not ignore the cause chain.

## Application Frames vs Library Frames

Stack traces may include framework or JDK internals.

```text
at java.base/java.util.Objects.requireNonNull(Objects.java:...)
at com.example.orders.OrderService.pay(OrderService.java:42)
```

Your code is usually under your package name, such as `com.example`. Start there
unless the stack trace clearly points elsewhere.

## Line Numbers Matter

If a stack trace points to a line with several operations, split it.

Harder to debug:

```java
return usersById.get(id).email().toLowerCase();
```

Easier to debug:

```java
User user = usersById.get(id);
String email = user.email();
return email.toLowerCase();
```

Now it is clearer whether the missing value is `user` or `email`.

## Practical Example

```java
import java.util.Map;

public class UserEmailLookup {
    public static void main(String[] args) {
        Map<String, User> usersById = Map.of(
                "u-100", new User("alex@example.com")
        );

        System.out.println(normalizedEmail(usersById, "u-999"));
    }

    static String normalizedEmail(Map<String, User> usersById, String id) {
        User user = usersById.get(id);

        if (user == null) {
            throw new UserNotFoundException("User not found: " + id);
        }

        return user.email().toLowerCase();
    }

    record User(String email) {
    }
}

class UserNotFoundException extends RuntimeException {
    UserNotFoundException(String message) {
        super(message);
    }
}
```

The explicit exception gives a clearer stack trace than allowing a
`NullPointerException` on `user.email()`.

## Common Mistakes

- Reading only the first line and ignoring the application frames.
- Ignoring `Caused by` sections.
- Starting in library code instead of the first relevant application frame.
- Hiding line numbers by catching and rethrowing without the cause.
- Logging only `exception.getMessage()` and losing the stack trace.

## Interview Questions

1. What information does a stack trace provide?
2. How do you identify the line that failed?
3. What does `Caused by` mean?
4. Why is preserving the exception cause important?
5. Why can splitting a long line help debugging?

## Practice

1. Trigger a `NumberFormatException` and identify the failing line.
2. Wrap an `IOException` and inspect the `Caused by` section.
3. Rewrite a long chained expression into separate lines.
4. Explain the call path from a stack trace in plain English.

## Related Topics

- [Exceptions](exceptions.md)
- [Custom Exceptions](custom_exceptions.md)
- [Debugging](debugging.md)

