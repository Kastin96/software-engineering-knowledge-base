# Java Exceptions and I/O Cheatsheet

## Throwing

```java
if (email == null || !email.contains("@")) {
    throw new IllegalArgumentException("email must be valid");
}
```

## Catching

```java
try {
    int value = Integer.parseInt(input);
} catch (NumberFormatException exception) {
    throw new IllegalArgumentException("input must be numeric", exception);
}
```

## Try-With-Resources

```java
try (BufferedReader reader = Files.newBufferedReader(path, StandardCharsets.UTF_8)) {
    return reader.readLine();
}
```

## Custom Exception

```java
class ImportFailedException extends RuntimeException {
    ImportFailedException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

## Files

```java
Path path = Path.of("data", "users.txt");
String text = Files.readString(path, StandardCharsets.UTF_8);
Files.writeString(path, text, StandardCharsets.UTF_8);
```

## Line-Based Reading

```java
try (BufferedReader reader = Files.newBufferedReader(path, StandardCharsets.UTF_8)) {
    String line;

    while ((line = reader.readLine()) != null) {
        process(line);
    }
}
```

## Binary Copy

```java
try (
        InputStream input = Files.newInputStream(source);
        OutputStream output = Files.newOutputStream(target)
) {
    input.transferTo(output);
}
```

## Watch Outs

- Preserve original exception causes.
- Do not swallow exceptions.
- Use explicit encoding for text.
- Stream large files instead of loading them fully.
- Do not deserialize untrusted Java object data.

