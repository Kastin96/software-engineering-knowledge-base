# I/O Best Practices

## Goal

Learn practical habits that make Java I/O code safer, clearer, and easier to
debug.

## Why It Matters

I/O code touches external state: files, directories, encodings, permissions,
uploads, network resources, and user-controlled input. Small assumptions can
become production bugs or security issues.

## Use Try-With-Resources

Close streams, readers, writers, and other closeable resources reliably.

```java
try (var reader = Files.newBufferedReader(path, java.nio.charset.StandardCharsets.UTF_8)) {
    return reader.readLine();
}
```

Do not rely on garbage collection to close resources.

## Be Explicit About Encoding

```java
Files.writeString(path, content, java.nio.charset.StandardCharsets.UTF_8);
```

Avoid platform-default surprises, especially for imports, exports, logs, and
tests.

## Validate Paths at Boundaries

If file names come from users, validate them carefully.

```java
Path storageRoot = Path.of("storage").toAbsolutePath().normalize();
Path target = storageRoot.resolve(fileName).normalize();

if (!target.startsWith(storageRoot)) {
    throw new IllegalArgumentException("invalid file path");
}
```

This helps protect against path traversal inputs such as `../secret.txt`.

## Avoid Loading Unknown File Sizes

Prefer streaming for unknown or large files.

```java
try (var lines = Files.lines(path, java.nio.charset.StandardCharsets.UTF_8)) {
    long count = lines.count();
}
```

If you collect a large stream into a list, you may still run out of memory. Keep
the processing incremental when possible.

## Keep Boundary Errors Contextual

Wrap low-level exceptions with useful context.

```java
try {
    return Files.readString(path, java.nio.charset.StandardCharsets.UTF_8);
} catch (java.io.IOException exception) {
    throw new ConfigLoadException("Could not read config file: " + path, exception);
}
```

Keep the original cause.

## Do Not Trust Parsed Data

Parsing JSON, CSV, or text only tells you the syntax is readable. It does not
prove the data is valid for your business rules.

```java
if (user.email() == null || !user.email().contains("@")) {
    throw new IllegalArgumentException("email is invalid");
}
```

Validate after parsing.

## Practical Checklist

Before writing I/O code, ask:

- Is this text or binary data?
- What encoding applies?
- Can the file be large?
- Who controls the path or file name?
- What should happen when the file is missing?
- Should invalid rows fail the whole import or be reported individually?
- Is the format safe for untrusted input?
- Does the error message include enough context?

## Practical Example

```java
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class SafeTextStorage {
    private final Path root;

    public SafeTextStorage(Path root) {
        this.root = root.toAbsolutePath().normalize();
    }

    public void write(String fileName, String content) {
        Path target = root.resolve(fileName).normalize();

        if (!target.startsWith(root)) {
            throw new IllegalArgumentException("invalid file name");
        }

        try {
            Files.createDirectories(target.getParent());
            Files.writeString(target, content, StandardCharsets.UTF_8);
        } catch (IOException exception) {
            throw new StorageException("Could not write file: " + target, exception);
        }
    }
}

class StorageException extends RuntimeException {
    StorageException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

This example handles path normalization, parent directories, UTF-8, and
contextual failures.

## Common Mistakes

- Forgetting to close resources.
- Using platform-default encoding accidentally.
- Trusting user-provided paths.
- Loading large files into memory.
- Deserializing untrusted data.
- Logging sensitive file contents or secrets.
- Returning empty data for failed reads without making the failure visible.

## Interview Questions

1. Why is try-with-resources important?
2. Why should encoding be explicit?
3. What is path traversal?
4. How do you avoid loading a large file into memory?
5. Why should parsed data still be validated?

## Practice

1. Add UTF-8 explicitly to file reads and writes.
2. Normalize and validate a user-provided file name.
3. Rewrite a `readAllLines` import into line-by-line processing.
4. Wrap an `IOException` with context and preserve the cause.

## Related Topics

- [Files and Paths](files_paths.md)
- [Working With Larger Files](large_files.md)
- [Java Serialization](java_serialization.md)

