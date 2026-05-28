# Files and Paths

## Goal

Understand modern Java file path handling with `Path` and common file operations
with `Files`.

## Why It Matters

Applications often read configuration, import data, write reports, store
temporary files, and move generated artifacts. Correct path handling makes code
more portable, safer, and easier to test.

## `Path`

Use `Path` to represent file and directory paths.

```java
import java.nio.file.Path;

Path reportPath = Path.of("reports", "orders.csv");

System.out.println(reportPath);
```

`Path.of("reports", "orders.csv")` builds a path using the operating system's
path rules. Avoid manually concatenating separators like `"/"` or `"\\"`.

## `Files`

`Files` provides static methods for common operations.

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;

public class FileCheck {
    public static void main(String[] args) throws IOException {
        Path path = Path.of("data", "users.txt");

        if (Files.exists(path)) {
            System.out.println("Size: " + Files.size(path));
        }
    }
}
```

## Reading Small Text Files

Use `Files.readString` for small text files.

```java
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

String content = Files.readString(
        Path.of("data", "message.txt"),
        StandardCharsets.UTF_8
);
```

Always be intentional about character encoding. UTF-8 is the common default for
modern text files.

## Writing Small Text Files

```java
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

Path output = Path.of("reports", "summary.txt");

Files.createDirectories(output.getParent());
Files.writeString(output, "Orders processed: 42", StandardCharsets.UTF_8);
```

Create parent directories before writing if they may not exist.

## Copying and Moving

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardCopyOption;

Path source = Path.of("data", "input.csv");
Path target = Path.of("archive", "input.csv");

Files.createDirectories(target.getParent());
Files.copy(source, target, StandardCopyOption.REPLACE_EXISTING);
```

Use explicit options so replacement behavior is clear.

## Practical Example

```java
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class ReportWriter {
    public void writeReport(String reportName, String content) {
        Path reportPath = Path.of("reports", reportName + ".txt");

        try {
            Files.createDirectories(reportPath.getParent());
            Files.writeString(reportPath, content, StandardCharsets.UTF_8);
        } catch (IOException exception) {
            throw new ReportWriteException("Could not write report: " + reportPath, exception);
        }
    }
}

class ReportWriteException extends RuntimeException {
    ReportWriteException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

The method builds a path safely, creates the directory, writes UTF-8 text, and
wraps the low-level exception with useful context.

## Common Mistakes

- Concatenating paths with hard-coded separators.
- Reading large files with `Files.readString`.
- Forgetting to specify text encoding.
- Assuming a directory exists before writing a file.
- Catching `IOException` and returning success anyway.

## Interview Questions

1. What is `Path` used for?
2. Why is `Path.of("a", "b")` better than `"a/b"`?
3. When is `Files.readString` appropriate?
4. Why should encoding be explicit?
5. Why should low-level `IOException` often be wrapped with context?

## Practice

1. Build a path to `data/users.csv` using `Path.of`.
2. Read a small UTF-8 file with `Files.readString`.
3. Write a summary file and create parent directories first.
4. Copy a file into an archive directory with replacement enabled.

## Related Topics

- [Readers and Writers](readers_writers.md)
- [Byte Streams](byte_streams.md)
- [Try-With-Resources](../04_exceptions_and_debugging/try_with_resources.md)

