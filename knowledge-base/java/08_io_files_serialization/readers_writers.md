# Readers and Writers

## Goal

Understand character-based I/O with readers and writers.

## Why It Matters

Text files are not just bytes. They have an encoding. Readers and writers convert
between bytes and characters, which makes them appropriate for CSV, logs, plain
text, and line-based imports.

## Reader and Writer

`Reader` reads characters. `Writer` writes characters.

```java
import java.io.BufferedReader;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

try (BufferedReader reader = Files.newBufferedReader(
        Path.of("data", "users.csv"),
        StandardCharsets.UTF_8
)) {
    String firstLine = reader.readLine();
    System.out.println(firstLine);
}
```

Use explicit encoding unless the source truly depends on the platform default.

## Writing Text

```java
import java.io.BufferedWriter;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

Path path = Path.of("reports", "users.txt");

Files.createDirectories(path.getParent());

try (BufferedWriter writer = Files.newBufferedWriter(path, StandardCharsets.UTF_8)) {
    writer.write("alex@example.com");
    writer.newLine();
    writer.write("sam@example.com");
}
```

`newLine()` uses the platform's line separator.

## Line-Based Reading

```java
try (BufferedReader reader = Files.newBufferedReader(path, StandardCharsets.UTF_8)) {
    String line;

    while ((line = reader.readLine()) != null) {
        if (!line.isBlank()) {
            System.out.println(line.trim());
        }
    }
}
```

This pattern works well for files that should be processed line by line.

## `Files.readAllLines`

Use `Files.readAllLines` only for reasonably small files.

```java
java.util.List<String> lines = Files.readAllLines(path, StandardCharsets.UTF_8);
```

For large files, prefer streaming or a `BufferedReader` loop.

## Practical Example

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.ArrayList;
import java.util.List;

public class EmailImporter {
    public List<String> importEmails(Path path) {
        List<String> emails = new ArrayList<>();

        try (BufferedReader reader = Files.newBufferedReader(path, StandardCharsets.UTF_8)) {
            String line;

            while ((line = reader.readLine()) != null) {
                String email = line.trim().toLowerCase();

                if (!email.isBlank() && email.contains("@")) {
                    emails.add(email);
                }
            }

            return List.copyOf(emails);
        } catch (IOException exception) {
            throw new EmailImportException("Could not import emails from " + path, exception);
        }
    }
}

class EmailImportException extends RuntimeException {
    EmailImportException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

This example reads text safely, normalizes values, filters invalid rows, and
preserves the original I/O failure.

## Common Mistakes

- Using readers/writers for binary files.
- Forgetting character encoding.
- Loading large files with `readAllLines`.
- Returning mutable internal lists from import methods.
- Ignoring malformed input instead of deciding how to report it.

## Interview Questions

1. What is the difference between `InputStream` and `Reader`?
2. Why does encoding matter for text files?
3. When is `BufferedReader` useful?
4. Why can `readAllLines` be risky?
5. Why should import code define how invalid rows are handled?

## Practice

1. Read a UTF-8 text file line by line.
2. Write several lines to a report file.
3. Normalize and validate emails from a file.
4. Explain when you would use byte streams instead.

## Related Topics

- [Byte Streams](byte_streams.md)
- [Working With Larger Files](large_files.md)
- [Exceptions and Debugging](../04_exceptions_and_debugging/README.md)

