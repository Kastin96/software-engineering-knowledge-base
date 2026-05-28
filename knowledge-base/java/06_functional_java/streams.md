# Streams

## Goal

Understand Java streams for processing collections with readable pipelines.

## Why It Matters

Streams are common in modern Java for filtering, mapping, grouping, summing, and
building response objects. They can make data transformations concise, but they
are not automatically better than loops. Good Java code uses streams when they
make the data flow clearer.

## Basic Pipeline

```java
import java.util.List;

public class ActiveEmails {
    public static void main(String[] args) {
        List<User> users = List.of(
                new User("alex@example.com", true),
                new User("sam@example.com", false),
                new User("mira@example.com", true)
        );

        List<String> activeEmails = users.stream()
                .filter(User::active)
                .map(User::email)
                .toList();

        System.out.println(activeEmails);
    }

    record User(String email, boolean active) {
    }
}
```

This reads as: take users, keep active users, map to emails, collect to a list.

## Intermediate and Terminal Operations

Intermediate operations return another stream.

```java
.filter(User::active)
.map(User::email)
```

Terminal operations produce a result or side effect.

```java
.toList()
.count()
.forEach(System.out::println)
```

Streams are lazy. Intermediate operations do not run until a terminal operation
is called.

## `filter`

Use `filter` to keep values matching a condition.

```java
List<Order> largeOrders = orders.stream()
        .filter(order -> order.totalInCents() >= 10_000)
        .toList();
```

## `map`

Use `map` to transform each value.

```java
List<String> orderIds = orders.stream()
        .map(Order::id)
        .toList();
```

## `flatMap`

Use `flatMap` when each item produces multiple items and you want one flat
stream.

```java
record Order(String id, List<String> itemIds) {
}

List<String> allItemIds = orders.stream()
        .flatMap(order -> order.itemIds().stream())
        .toList();
```

`map` would produce a stream of lists. `flatMap` produces a stream of item ids.

## Finding Values

```java
java.util.Optional<User> user = users.stream()
        .filter(value -> value.email().equals("alex@example.com"))
        .findFirst();
```

Use `Optional` because the value may be missing.

## Practical Example

```java
import java.util.List;

public class OrderSummaryBuilder {
    public static void main(String[] args) {
        List<Order> orders = List.of(
                new Order("o-1", "PAID", 5000),
                new Order("o-2", "NEW", 1500),
                new Order("o-3", "PAID", 9000)
        );

        List<OrderSummary> paidSummaries = orders.stream()
                .filter(order -> order.status().equals("PAID"))
                .map(order -> new OrderSummary(order.id(), order.totalInCents()))
                .toList();

        System.out.println(paidSummaries);
    }

    record Order(String id, String status, int totalInCents) {
    }

    record OrderSummary(String id, int totalInCents) {
    }
}
```

This is a realistic API-response transformation.

## Common Mistakes

- Using streams for complex branching where a loop is clearer.
- Adding side effects inside `map` or `filter`.
- Forgetting that streams are lazy.
- Reusing a stream after a terminal operation.
- Using `parallelStream` without measuring or understanding thread-safety.

## Interview Questions

1. What is a stream?
2. What is the difference between intermediate and terminal operations?
3. Why are streams lazy?
4. What is the difference between `map` and `flatMap`?
5. When is a loop better than a stream?

## Practice

1. Filter active users from a list.
2. Map orders to order ids.
3. Use `flatMap` to collect item ids from orders.
4. Find the first user by email and return an `Optional`.

## Related Topics

- [Stream Collectors](stream_collectors.md)
- [`Optional`](optional.md)
- [`List`](../03_collections_and_data_structures/list.md)

