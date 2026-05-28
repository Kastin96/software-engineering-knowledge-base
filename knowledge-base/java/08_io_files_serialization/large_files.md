# Working With Larger Files

## Goal

Understand how to process larger files without loading the entire file into
memory.

## Why It Matters

Small examples often use `readString` or `readAllLines`, but production files can
be much larger than expected. Imports, logs, exports, and data feeds should often
be processed incrementally.

## Avoid Loading Everything

This is fine for small files:

```java
String content = Files.readString(path, StandardCharsets.UTF_8);
```

This is risky for large files because the entire file is loaded into memory.

For large files, process line by line.

## BufferedReader Loop

```java
import java.io.BufferedReader;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

try (BufferedReader reader = Files.newBufferedReader(path, StandardCharsets.UTF_8)) {
    String line;

    while ((line = reader.readLine()) != null) {
        processLine(line);
    }
}
```

This keeps memory usage much lower.

## `Files.lines`

`Files.lines` returns a lazy stream of lines.

```java
try (java.util.stream.Stream<String> lines = Files.lines(path, StandardCharsets.UTF_8)) {
    long validCount = lines
            .filter(line -> !line.isBlank())
            .count();

    System.out.println(validCount);
}
```

The stream must be closed. Use try-with-resources.

## Batch Processing

When importing data, process in batches instead of one huge list.

```java
import java.util.ArrayList;
import java.util.List;

List<String> batch = new ArrayList<>();

try (BufferedReader reader = Files.newBufferedReader(path, StandardCharsets.UTF_8)) {
    String line;

    while ((line = reader.readLine()) != null) {
        batch.add(line);

        if (batch.size() == 500) {
            saveBatch(batch);
            batch.clear();
        }
    }

    if (!batch.isEmpty()) {
        saveBatch(batch);
    }
}
```

Batching helps control memory and database load.

## Practical Example

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class OrderImportCounter {
    public int countPaidOrders(Path path) {
        int paidCount = 0;

        try (BufferedReader reader = Files.newBufferedReader(path, StandardCharsets.UTF_8)) {
            String line;

            while ((line = reader.readLine()) != null) {
                if (line.contains(",PAID,")) {
                    paidCount++;
                }
            }

            return paidCount;
        } catch (IOException exception) {
            throw new OrderImportException("Could not read order import file: " + path, exception);
        }
    }
}

class OrderImportException extends RuntimeException {
    OrderImportException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

This counts matching rows without keeping the entire file in memory.

## Common Mistakes

- Using `readAllLines` for unknown file sizes.
- Forgetting to close `Files.lines`.
- Collecting a large stream into a list too early.
- Processing huge imports without batching.
- Ignoring malformed rows without tracking or reporting them.

## Interview Questions

1. Why is `readAllLines` risky for large files?
2. How does `BufferedReader` help with memory usage?
3. Why must a `Files.lines` stream be closed?
4. What is batch processing?
5. How would you handle invalid lines in a large import?

## Practice

1. Count non-blank lines in a large file using `BufferedReader`.
2. Repeat the count using `Files.lines` with try-with-resources.
3. Process rows in batches of 500.
4. Add a counter for invalid rows.

## Related Topics

- [Readers and Writers](readers_writers.md)
- [I/O Best Practices](io_best_practices.md)
- [Streams](../06_functional_java/streams.md)

