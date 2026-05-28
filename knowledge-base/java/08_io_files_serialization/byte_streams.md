# Byte Streams

## Goal

Understand byte streams and when to use them for binary data.

## Why It Matters

Not all files are text. Images, PDFs, ZIP files, uploaded documents, encrypted
payloads, and network data are byte-oriented. Using text APIs for binary data
can corrupt content.

## InputStream and OutputStream

`InputStream` reads bytes. `OutputStream` writes bytes.

```java
import java.io.IOException;
import java.io.InputStream;
import java.nio.file.Files;
import java.nio.file.Path;

try (InputStream input = Files.newInputStream(Path.of("image.png"))) {
    int firstByte = input.read();
    System.out.println(firstByte);
}
```

`read()` returns one byte as an int, or `-1` when the stream ends.

## Copying Bytes

```java
import java.io.InputStream;
import java.io.OutputStream;
import java.nio.file.Files;
import java.nio.file.Path;

Path source = Path.of("uploads", "document.pdf");
Path target = Path.of("archive", "document.pdf");

Files.createDirectories(target.getParent());

try (
        InputStream input = Files.newInputStream(source);
        OutputStream output = Files.newOutputStream(target)
) {
    input.transferTo(output);
}
```

`transferTo` is a clear way to copy all bytes from one stream to another.

## Reading All Bytes

Use `Files.readAllBytes` only for files that are safely small enough.

```java
byte[] bytes = Files.readAllBytes(Path.of("small-logo.png"));
```

For larger files, stream the data instead of loading everything into memory.

## Buffered Streams

Buffering reduces the number of low-level read/write calls.

```java
try (
        InputStream input = new java.io.BufferedInputStream(Files.newInputStream(source));
        OutputStream output = new java.io.BufferedOutputStream(Files.newOutputStream(target))
) {
    input.transferTo(output);
}
```

Many higher-level APIs already buffer internally, but buffering is still useful
when reading and writing many small chunks.

## Practical Example

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.nio.file.Files;
import java.nio.file.Path;

public class FileStorage {
    public void storeUpload(Path source, String fileName) {
        Path target = Path.of("storage", fileName);

        try {
            Files.createDirectories(target.getParent());

            try (
                    InputStream input = Files.newInputStream(source);
                    OutputStream output = Files.newOutputStream(target)
            ) {
                input.transferTo(output);
            }
        } catch (IOException exception) {
            throw new StorageException("Could not store file: " + fileName, exception);
        }
    }
}

class StorageException extends RuntimeException {
    StorageException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

This is close to real upload/storage code: build a target path, create
directories, stream bytes, and keep the original cause on failure.

## Common Mistakes

- Reading binary files with `Reader` or `Files.readString`.
- Loading large files fully into a byte array.
- Forgetting try-with-resources.
- Ignoring partial failures when copying files.
- Treating bytes as text without specifying encoding.

## Interview Questions

1. What is the difference between byte streams and character streams?
2. When should you use `InputStream`?
3. Why is `Files.readAllBytes` risky for large files?
4. What does try-with-resources do for streams?
5. Why can text APIs corrupt binary data?

## Practice

1. Copy a PDF or image using `InputStream` and `OutputStream`.
2. Rewrite the copy using `transferTo`.
3. Add buffering to the stream copy.
4. Explain why `Files.readString` would be wrong for the same file.

## Related Topics

- [Files and Paths](files_paths.md)
- [Readers and Writers](readers_writers.md)
- [I/O Best Practices](io_best_practices.md)

