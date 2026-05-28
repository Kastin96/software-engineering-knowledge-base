# Java Streams and Optional Cheatsheet

## Stream Pipeline

```java
List<String> activeEmails = users.stream()
        .filter(User::active)
        .map(User::email)
        .toList();
```

## Common Operations

```java
filter(value -> condition)
map(value -> transformedValue)
flatMap(value -> value.items().stream())
sorted(Comparator.comparing(User::email))
distinct()
limit(10)
count()
findFirst()
anyMatch(rule)
```

## Collectors

```java
Map<String, List<Order>> byStatus = orders.stream()
        .collect(Collectors.groupingBy(Order::status));

Map<String, User> byId = users.stream()
        .collect(Collectors.toMap(User::id, user -> user));

int total = orders.stream()
        .mapToInt(Order::totalInCents)
        .sum();
```

## Optional

```java
Optional<User> user = findById("u-100");

String email = user
        .map(User::email)
        .orElse("unknown@example.com");

User required = user.orElseThrow(() -> new UserNotFoundException("user not found"));
```

## Use Optional For

- method return values where absence is expected;
- lookup results;
- parsing results;
- avoiding unclear `null` at boundaries.

## Avoid Optional For

- fields in ordinary domain objects;
- method parameters in most cases;
- collection returns where an empty collection is clearer;
- calling `get()` without checking.

## Watch Outs

- Streams are lazy until a terminal operation.
- Avoid side effects inside `filter` and `map`.
- `Stream.toList()` returns an unmodifiable list.
- Use `orElseGet` for expensive fallback creation.

