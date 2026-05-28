# `Optional`

## Goal

Understand what `Optional` is for and how to use it without making code awkward.

## Why It Matters

`Optional` makes possible absence explicit in method return types. It is common
in repository lookups, parsing, configuration, and search methods. Used well, it
reduces unclear `null` handling. Used poorly, it makes code noisy and hides
simple control flow.

## Basic Usage

```java
import java.util.Optional;

Optional<String> email = Optional.of("alex@example.com");
Optional<String> missing = Optional.empty();
```

Use `ofNullable` when the source may be `null`.

```java
String value = findEmailOrNull();
Optional<String> email = Optional.ofNullable(value);
```

## Return `Optional` for Missing Values

```java
import java.util.List;
import java.util.Optional;

static Optional<User> findByEmail(List<User> users, String email) {
    for (User user : users) {
        if (user.email().equals(email)) {
            return Optional.of(user);
        }
    }

    return Optional.empty();
}

record User(String email) {
}
```

The caller is forced to handle the missing case.

## `map`

Use `map` to transform a present value.

```java
Optional<String> email = findByEmail(users, "alex@example.com")
        .map(User::email);
```

If the user is missing, the result is `Optional.empty()`.

## `orElse` vs `orElseGet`

`orElse` always evaluates its argument.

```java
String email = maybeEmail.orElse(defaultEmail());
```

`orElseGet` calls the supplier only when needed.

```java
String email = maybeEmail.orElseGet(() -> defaultEmail());
```

Use `orElseGet` when the fallback is expensive or has side effects.

## `orElseThrow`

Use `orElseThrow` when absence is an error at this boundary.

```java
User user = findByEmail(users, "alex@example.com")
        .orElseThrow(() -> new UserNotFoundException("user not found"));
```

Do not call `get()` without checking presence.

```java
// Avoid:
// User user = optionalUser.get();
```

## Optional Is Usually for Return Types

Use `Optional` mainly as a return type.

```java
Optional<User> findById(String id) {
    // ...
}
```

Avoid using `Optional` for fields, method parameters, or collection elements in
ordinary domain code unless there is a strong reason. A clear nullable boundary,
validation, or overloaded method is often better.

## Practical Example

```java
import java.util.List;
import java.util.Optional;

public class UserLookup {
    public static void main(String[] args) {
        List<User> users = List.of(
                new User("u-100", "alex@example.com"),
                new User("u-200", "sam@example.com")
        );

        String email = findById(users, "u-999")
                .map(User::email)
                .orElse("unknown@example.com");

        System.out.println(email);
    }

    static Optional<User> findById(List<User> users, String id) {
        return users.stream()
                .filter(user -> user.id().equals(id))
                .findFirst();
    }

    record User(String id, String email) {
    }
}
```

The missing user case is explicit and handled in one place.

## Common Mistakes

- Calling `optional.get()` without checking presence.
- Returning `null` from a method that returns `Optional`.
- Using `Optional` for every field or parameter.
- Wrapping collections in `Optional` instead of returning an empty collection.
- Using `orElse` when `orElseGet` is needed for expensive fallback creation.

## Interview Questions

1. What problem does `Optional` solve?
2. When should a method return `Optional`?
3. Why is `Optional.get()` risky?
4. What is the difference between `orElse` and `orElseGet`?
5. Why should methods returning collections usually return an empty collection
   instead of `Optional<List<T>>`?

## Practice

1. Write a `findById` method that returns `Optional<User>`.
2. Transform an optional user into an optional email.
3. Use `orElseThrow` for a required lookup.
4. Replace `Optional.get()` with safer handling.

## Related Topics

- [Streams](streams.md)
- [Method References](method_references.md)
- [Exceptions and Debugging](../04_exceptions_and_debugging/README.md)

