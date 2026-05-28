# Functional Style Best Practices

## Goal

Learn when functional-style Java improves code and when a normal loop or method
is better.

## Why It Matters

Functional features are tools, not a rulebook. A stream pipeline can make a data
transformation obvious, but it can also become unreadable when it hides
branching, mutation, exception handling, or business concepts.

## Prefer Clear Data Flow

Good stream code reads like a data transformation.

```java
List<String> activeEmails = users.stream()
        .filter(User::active)
        .map(User::email)
        .toList();
```

This is a clear pipeline: filter, map, collect.

## Avoid Side Effects in Pipelines

Avoid mutating external state inside stream operations.

```java
List<String> emails = new ArrayList<>();

users.stream()
        .filter(User::active)
        .forEach(user -> emails.add(user.email()));
```

Prefer collecting the result.

```java
List<String> emails = users.stream()
        .filter(User::active)
        .map(User::email)
        .toList();
```

The second version is easier to reason about and safer to refactor.

## Extract Named Methods

If a lambda becomes complex, name it.

```java
List<Order> reviewOrders = orders.stream()
        .filter(OrderRules::requiresManualReview)
        .toList();
```

```java
class OrderRules {
    static boolean requiresManualReview(Order order) {
        return order.totalInCents() >= 10_000
                || order.shippingCountry().equals("RESTRICTED");
    }
}
```

The method name explains the business meaning.

## Use Loops for Complex Control Flow

A loop is often clearer when logic needs multiple branches, early exits, or
careful error handling.

```java
List<String> errors = new ArrayList<>();

for (Order order : orders) {
    if (order.totalInCents() < 0) {
        errors.add("negative total for " + order.id());
        continue;
    }

    if (order.items().isEmpty()) {
        errors.add("empty order " + order.id());
    }
}
```

For validation logic like this, a loop can be more maintainable than a stream
pipeline with nested lambdas.

## Be Careful With Parallel Streams

Do not use `parallelStream()` as a quick performance switch.

Parallel streams can be useful for CPU-heavy independent work on large data
sets, but they can hurt performance or correctness when:

- the data set is small;
- operations block on I/O;
- shared mutable state is used;
- ordering matters;
- the common fork-join pool is already busy.

Measure before and after.

## Keep Exceptions Understandable

Checked exceptions do not fit neatly into standard functional interfaces.

```java
// Files.readString throws IOException, which does not fit Function<Path, String>
```

Do not bury checked exceptions in clever wrappers unless the boundary is clear.
Sometimes a loop with normal `try/catch` is better.

## Practical Example

```java
import java.util.List;

public class OrderFiltering {
    public static void main(String[] args) {
        List<Order> orders = List.of(
                new Order("o-1", "PAID", 5000),
                new Order("o-2", "FAILED", 1500),
                new Order("o-3", "PAID", 12_000)
        );

        List<Order> reviewOrders = orders.stream()
                .filter(OrderFiltering::requiresManualReview)
                .toList();

        System.out.println(reviewOrders);
    }

    static boolean requiresManualReview(Order order) {
        return order.status().equals("FAILED") || order.totalInCents() >= 10_000;
    }

    record Order(String id, String status, int totalInCents) {
    }
}
```

The stream stays short, and the business rule has a name.

## Common Mistakes

- Turning every loop into a stream.
- Using `peek` for business logic.
- Mutating external collections inside stream pipelines.
- Writing long inline lambdas with hidden business rules.
- Using `parallelStream` without measurement.
- Returning `Optional` from every method, even when absence is not a meaningful
  result.

## Interview Questions

1. When is a stream clearer than a loop?
2. When is a loop clearer than a stream?
3. Why are side effects inside stream pipelines risky?
4. Why should `parallelStream` be used carefully?
5. Why is naming a predicate method often better than writing a long lambda?

## Practice

1. Rewrite a side-effecting `forEach` pipeline as a `map` plus `toList`.
2. Extract a long filter lambda into a named method.
3. Choose between a loop and a stream for validation logic and explain why.
4. Identify one place where `parallelStream` would be a bad idea.

## Related Topics

- [Streams](streams.md)
- [Stream Collectors](stream_collectors.md)
- [`Optional`](optional.md)

