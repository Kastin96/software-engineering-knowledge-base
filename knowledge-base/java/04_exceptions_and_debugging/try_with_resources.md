# Try-With-Resources

## Goal

Understand how try-with-resources closes files, streams, database resources, and
other closeable objects safely.

## Why It Matters

Resource leaks are real production bugs. Open files, sockets, streams, and
database connections must be closed. Try-with-resources makes cleanup reliable
even when an exception is thrown.

## Basic Example

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;

public class FileLineReader {
    public static void main(String[] args) throws IOException {
        try (BufferedReader reader = Files.newBufferedReader(Path.of("users.txt"))) {
            String line = reader.readLine();
            System.out.println(line);
        }
    }
}
```

The reader is closed automatically at the end of the `try` block.

## AutoCloseable

Try-with-resources works with objects that implement `AutoCloseable` or
`Closeable`.

```java
class TimerResource implements AutoCloseable {
    TimerResource() {
        System.out.println("Start timer");
    }

    @Override
    public void close() {
        System.out.println("Stop timer");
    }
}
```

```java
try (TimerResource timer = new TimerResource()) {
    System.out.println("Run operation");
}
```

`close()` is called automatically.

## Multiple Resources

Resources are closed in reverse order.

```java
try (
        TimerResource first = new TimerResource();
        TimerResource second = new TimerResource()
) {
    System.out.println("Using resources");
}
```

This matters when one resource depends on another.

## Avoid Manual Close

Manual cleanup is easy to get wrong.

```java
BufferedReader reader = Files.newBufferedReader(Path.of("users.txt"));

try {
    System.out.println(reader.readLine());
} finally {
    reader.close();
}
```

This works, but try-with-resources is shorter, safer, and more idiomatic.

## Practical Example

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.ArrayList;
import java.util.List;

public class EmailFileLoader {
    public List<String> loadEmails(Path path) {
        List<String> emails = new ArrayList<>();

        try (BufferedReader reader = Files.newBufferedReader(path)) {
            String line;

            while ((line = reader.readLine()) != null) {
                if (!line.isBlank() && line.contains("@")) {
                    emails.add(line.trim());
                }
            }

            return List.copyOf(emails);
        } catch (IOException exception) {
            throw new EmailLoadException("Could not load emails from " + path, exception);
        }
    }
}

class EmailLoadException extends RuntimeException {
    EmailLoadException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

The method closes the file reader automatically and wraps the low-level I/O
failure with business context.

## Suppressed Exceptions

If both the `try` block and `close()` throw exceptions, Java keeps the main
exception and stores close failures as suppressed exceptions.

```java
try {
    // failing code
} catch (RuntimeException exception) {
    for (Throwable suppressed : exception.getSuppressed()) {
        System.out.println("Suppressed: " + suppressed.getMessage());
    }
}
```

You do not need this often, but it explains how Java avoids losing cleanup
failures.

## Common Mistakes

- Forgetting to close files, streams, or database resources.
- Using `finally` for cleanup when try-with-resources is available.
- Catching `IOException` and returning an empty result without making the failure
  visible.
- Wrapping an exception without passing the original cause.
- Assuming try-with-resources handles objects that do not implement
  `AutoCloseable`.

## Interview Questions

1. What problem does try-with-resources solve?
2. What interface must a resource implement?
3. In what order are multiple resources closed?
4. Why is try-with-resources usually better than manual `finally` cleanup?
5. What is a suppressed exception?

## Practice

1. Read all lines from a file using try-with-resources.
2. Create a simple `AutoCloseable` class and verify that `close` runs.
3. Wrap an `IOException` with a custom exception that keeps the cause.
4. Explain what would happen if file reading succeeds but closing fails.

## Related Topics

- [`try`, `catch`, and `finally`](try_catch_finally.md)
- [Checked and Unchecked Exceptions](checked_unchecked.md)
- [Custom Exceptions](custom_exceptions.md)

