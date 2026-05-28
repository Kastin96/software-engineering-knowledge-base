# Java Serialization

## Goal

Understand Java native serialization, what it was designed for, and why it
should be used carefully.

## Why It Matters

Java has built-in object serialization through `Serializable`, but modern backend
applications usually prefer explicit formats such as JSON, Avro, Protocol
Buffers, or database records. Native Java deserialization can be risky when used
with untrusted data.

## Serializable

A class can opt into native Java serialization by implementing `Serializable`.

```java
import java.io.Serializable;

public class UserSession implements Serializable {
    private static final long serialVersionUID = 1L;

    private final String userId;

    public UserSession(String userId) {
        this.userId = userId;
    }

    public String userId() {
        return userId;
    }
}
```

`serialVersionUID` helps Java check compatibility between serialized data and
class versions.

## Writing an Object

```java
import java.io.ObjectOutputStream;
import java.nio.file.Files;
import java.nio.file.Path;

UserSession session = new UserSession("u-100");

try (ObjectOutputStream output = new ObjectOutputStream(
        Files.newOutputStream(Path.of("session.bin"))
)) {
    output.writeObject(session);
}
```

## Reading an Object

```java
import java.io.ObjectInputStream;
import java.nio.file.Files;
import java.nio.file.Path;

try (ObjectInputStream input = new ObjectInputStream(
        Files.newInputStream(Path.of("session.bin"))
)) {
    UserSession session = (UserSession) input.readObject();
    System.out.println(session.userId());
}
```

This requires casting and can throw `ClassNotFoundException` as well as I/O
exceptions.

## `transient`

Use `transient` for fields that should not be serialized.

```java
public class LoginSession implements java.io.Serializable {
    private static final long serialVersionUID = 1L;

    private final String userId;
    private transient String accessToken;

    public LoginSession(String userId, String accessToken) {
        this.userId = userId;
        this.accessToken = accessToken;
    }
}
```

Do not assume `transient` is a complete security strategy. Sensitive data should
be handled carefully throughout the design.

## Security Warning

Do not deserialize untrusted data with native Java serialization.

Deserialization can construct object graphs and trigger code paths during object
creation. This has historically been a serious security risk in Java systems.
Use safer, explicit formats and validate input.

## Prefer Explicit Formats

For many applications, prefer:

- JSON for REST APIs and simple integration;
- database rows/documents for persistence;
- Avro or Protocol Buffers for schema-based events;
- explicit DTOs for boundaries.

Native Java serialization is mainly relevant for legacy code, controlled
internal use cases, and interview understanding.

## Practical Example

```java
import java.io.Serializable;

public record UserSnapshot(String id, String email, boolean active) implements Serializable {
    private static final long serialVersionUID = 1L;
}
```

Records can implement `Serializable`, but that does not remove the security and
compatibility concerns of native serialization.

## Common Mistakes

- Deserializing untrusted data.
- Forgetting `serialVersionUID`.
- Treating serialized objects as a stable long-term storage format.
- Serializing sensitive data accidentally.
- Using native serialization when JSON or a schema-based format would be clearer.

## Interview Questions

1. What does `Serializable` do?
2. What is `serialVersionUID` for?
3. What does `transient` mean?
4. Why is Java deserialization risky with untrusted data?
5. What formats are often preferred over native Java serialization?

## Practice

1. Create a small `Serializable` class with `serialVersionUID`.
2. Mark one field as `transient`.
3. Explain why deserializing user-provided bytes is dangerous.
4. Choose JSON instead of native serialization for a simple API payload.

## Related Topics

- [Byte Streams](byte_streams.md)
- [JSON Basics](json_basics.md)
- [I/O Best Practices](io_best_practices.md)

