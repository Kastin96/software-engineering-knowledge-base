# Stream Collectors

## Goal

Understand common ways to collect stream results into lists, maps, groups, and
summary values.

## Why It Matters

Filtering and mapping are only part of stream usage. Real Java code often needs
to group orders by status, index users by id, count values, sum totals, or build
specific collection types. Collectors are the standard tool for that.

## `toList`

For a simple list result, use `toList()`.

```java
List<String> emails = users.stream()
        .map(User::email)
        .toList();
```

The list returned by `Stream.toList()` is unmodifiable. If you need a mutable
list, collect into one explicitly.

```java
List<String> mutableEmails = users.stream()
        .map(User::email)
        .collect(java.util.stream.Collectors.toCollection(java.util.ArrayList::new));
```

## `toSet`

```java
import java.util.Set;
import java.util.stream.Collectors;

Set<String> uniqueStatuses = orders.stream()
        .map(Order::status)
        .collect(Collectors.toSet());
```

Use a specific set type when order matters.

```java
Set<String> orderedStatuses = orders.stream()
        .map(Order::status)
        .collect(Collectors.toCollection(java.util.LinkedHashSet::new));
```

## `toMap`

```java
import java.util.Map;
import java.util.stream.Collectors;

Map<String, User> usersById = users.stream()
        .collect(Collectors.toMap(User::id, user -> user));
```

Duplicate keys throw an exception unless you provide a merge function.

```java
Map<String, User> usersByEmail = users.stream()
        .collect(Collectors.toMap(
                User::email,
                user -> user,
                (first, second) -> first
        ));
```

Use a merge function only when duplicate handling is intentional.

## Grouping

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

Map<String, List<Order>> ordersByStatus = orders.stream()
        .collect(Collectors.groupingBy(Order::status));
```

This is common for reporting, batching, and response shaping.

## Counting and Summing

```java
Map<String, Long> countByStatus = orders.stream()
        .collect(Collectors.groupingBy(Order::status, Collectors.counting()));
```

```java
int paidTotal = orders.stream()
        .filter(order -> order.status().equals("PAID"))
        .mapToInt(Order::totalInCents)
        .sum();
```

Use primitive streams like `mapToInt` for simple numeric summaries.

## Joining Strings

```java
String csv = users.stream()
        .map(User::email)
        .collect(Collectors.joining(","));
```

This is useful for logs, export formats, and simple display values.

## Practical Example

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class OrderReports {
    public static void main(String[] args) {
        List<Order> orders = List.of(
                new Order("o-1", "PAID", 5000),
                new Order("o-2", "NEW", 1500),
                new Order("o-3", "PAID", 9000)
        );

        Map<String, Integer> totalByStatus = orders.stream()
                .collect(Collectors.groupingBy(
                        Order::status,
                        Collectors.summingInt(Order::totalInCents)
                ));

        System.out.println(totalByStatus);
    }

    record Order(String id, String status, int totalInCents) {
    }
}
```

This mirrors reporting logic: group by a category and calculate totals.

## Common Mistakes

- Forgetting duplicate keys in `Collectors.toMap`.
- Assuming `Stream.toList()` returns a mutable list.
- Using grouping when a simple map lookup would be clearer.
- Building very complex collectors instead of extracting named steps.
- Using streams for financial totals without thinking about numeric type and
  precision.

## Interview Questions

1. What does a collector do?
2. How do you collect a stream into a map?
3. What happens when `toMap` sees duplicate keys?
4. How do you group values by a field?
5. What is the difference between `toList()` and collecting to `ArrayList`?

## Practice

1. Collect user emails into a list.
2. Build a map of users by id.
3. Group orders by status.
4. Calculate total order amount by status.

## Related Topics

- [Streams](streams.md)
- [`Map`](../03_collections_and_data_structures/map.md)
- [`Set`](../03_collections_and_data_structures/set.md)

