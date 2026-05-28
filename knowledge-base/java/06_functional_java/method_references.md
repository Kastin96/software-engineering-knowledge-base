# Method References

## Goal

Understand method references and when they make lambda-based code more readable.

## Why It Matters

Method references are common in stream pipelines, sorting, mapping, and
callbacks. They are not a separate feature from lambdas; they are a shorter way
to point at an existing method.

## Static Method Reference

```java
import java.util.List;

public class EmailNormalizer {
    public static void main(String[] args) {
        List<String> emails = List.of(" Alex@Example.com ", "SAM@Example.com");

        List<String> normalized = emails.stream()
                .map(EmailNormalizer::normalize)
                .toList();

        System.out.println(normalized);
    }

    static String normalize(String email) {
        return email.trim().toLowerCase();
    }
}
```

`EmailNormalizer::normalize` is equivalent to:

```java
email -> EmailNormalizer.normalize(email)
```

## Instance Method Reference on a Particular Object

```java
class AuditLog {
    void write(String message) {
        System.out.println("[AUDIT] " + message);
    }
}
```

```java
AuditLog auditLog = new AuditLog();
java.util.List<String> events = java.util.List.of("created user", "locked account");

events.forEach(auditLog::write);
```

This points to a method on one existing object.

## Instance Method Reference on the Parameter

```java
java.util.List<String> names = java.util.List.of("Alex", "Sam", "Mira");

java.util.List<String> upperNames = names.stream()
        .map(String::toUpperCase)
        .toList();
```

`String::toUpperCase` is equivalent to:

```java
name -> name.toUpperCase()
```

## Constructor Reference

```java
record UserResponse(String email) {
}
```

```java
java.util.List<String> emails = java.util.List.of("alex@example.com", "sam@example.com");

java.util.List<UserResponse> responses = emails.stream()
        .map(UserResponse::new)
        .toList();
```

`UserResponse::new` is equivalent to:

```java
email -> new UserResponse(email)
```

## Comparator Example

```java
import java.util.Comparator;
import java.util.List;

record Order(String id, int totalInCents) {
}

List<Order> orders = new java.util.ArrayList<>(List.of(
        new Order("o-1", 5000),
        new Order("o-2", 1500)
));

orders.sort(Comparator.comparingInt(Order::totalInCents));
```

The method reference names the field used for sorting.

## Practical Example

```java
import java.util.List;

public class UserMapper {
    public static void main(String[] args) {
        List<User> users = List.of(
                new User("alex@example.com", true),
                new User("sam@example.com", false)
        );

        List<UserResponse> responses = users.stream()
                .filter(User::active)
                .map(UserMapper::toResponse)
                .toList();

        System.out.println(responses);
    }

    static UserResponse toResponse(User user) {
        return new UserResponse(user.email());
    }

    record User(String email, boolean active) {
    }

    record UserResponse(String email) {
    }
}
```

This keeps the stream readable because the mapping logic has a meaningful method
name.

## Common Mistakes

- Using a method reference when a lambda would be easier to read.
- Hiding complex behavior behind a vague method name.
- Forgetting that method references still need a matching functional interface.
- Using method references mainly for style instead of clarity.
- Chaining too many method references without naming intermediate concepts.

## Interview Questions

1. What is a method reference?
2. How is `String::toUpperCase` related to a lambda?
3. What is a constructor reference?
4. When is a method reference less readable than a lambda?
5. Why do method references need a target functional interface?

## Practice

1. Replace `value -> value.trim()` with a method reference if possible.
2. Use `User::email` in a stream mapping operation.
3. Use `UserResponse::new` as a constructor reference.
4. Sort orders with `Comparator.comparing(Order::createdAt)`.

## Related Topics

- [Lambdas](lambdas.md)
- [Functional Interfaces](functional_interfaces.md)
- [Streams](streams.md)

