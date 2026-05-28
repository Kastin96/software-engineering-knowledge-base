# `Set`

## Goal

Understand how Java `Set` stores unique values and how equality rules affect
duplicate detection.

## Why It Matters

`Set` is useful when uniqueness matters: user roles, permissions, tags, IDs,
processed event keys, and deduplicated emails. A `Set` communicates intent more
clearly than a `List` plus manual duplicate checks.

## Basic Usage

```java
import java.util.HashSet;
import java.util.Set;

Set<String> roles = new HashSet<>();

roles.add("admin");
roles.add("user");
roles.add("admin");

System.out.println(roles.size()); // 2
System.out.println(roles.contains("admin")); // true
```

`Set` does not allow duplicates.

## Common Implementations

`HashSet` is the default choice for uniqueness checks.

- Fast average add, remove, and contains.
- No guaranteed iteration order.
- Requires correct `equals` and `hashCode`.

`LinkedHashSet` keeps insertion order.

```java
Set<String> orderedRoles = new java.util.LinkedHashSet<>();
```

`TreeSet` keeps values sorted.

```java
Set<String> sortedRoles = new java.util.TreeSet<>();
```

Use `TreeSet` when sorted uniqueness is part of the requirement, not just for
display once.

## Deduplication

```java
import java.util.LinkedHashSet;
import java.util.List;
import java.util.Set;

List<String> emails = List.of(
        "alex@example.com",
        "sam@example.com",
        "alex@example.com"
);

Set<String> uniqueEmails = new LinkedHashSet<>(emails);

System.out.println(uniqueEmails); // preserves first-seen order
```

`LinkedHashSet` is useful when you want uniqueness and stable display order.

## Equality Matters

For custom objects, `HashSet` uses `equals` and `hashCode`.

```java
public record UserId(String value) {
    public UserId {
        if (value == null || value.isBlank()) {
            throw new IllegalArgumentException("value must not be blank");
        }
    }
}
```

Records provide value-based equality automatically.

```java
Set<UserId> ids = new HashSet<>();
ids.add(new UserId("u-100"));
ids.add(new UserId("u-100"));

System.out.println(ids.size()); // 1
```

## Practical Example

```java
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class DuplicateOrderDetector {
    public static void main(String[] args) {
        List<String> orderIds = List.of("o-1", "o-2", "o-1", "o-3");
        Set<String> seen = new HashSet<>();
        Set<String> duplicates = new HashSet<>();

        for (String orderId : orderIds) {
            if (!seen.add(orderId)) {
                duplicates.add(orderId);
            }
        }

        System.out.println("Duplicates: " + duplicates);
    }
}
```

`seen.add(orderId)` returns `false` when the value already exists. This is a
clean way to detect duplicates.

## Common Mistakes

- Expecting `HashSet` iteration order to be stable.
- Forgetting that custom objects need correct equality behavior.
- Using `Set` when duplicates are meaningful business data.
- Mutating fields used by `equals` or `hashCode` after adding an object to a set.
- Sorting a `HashSet` repeatedly instead of using a sorted collection or sorting
  only at the output boundary.

## Interview Questions

1. What is the main difference between `List` and `Set`?
2. Does `HashSet` preserve insertion order?
3. When would you use `LinkedHashSet`?
4. Why do `equals` and `hashCode` matter for `HashSet`?
5. What can go wrong if a set element is mutated?

## Practice

1. Remove duplicate emails from a list.
2. Preserve first-seen order while deduplicating.
3. Detect duplicate order IDs.
4. Create a record value object and store it in a `HashSet`.

## Related Topics

- [`List`](list.md)
- [`Map`](map.md)
- [`Queue` and `Deque`](queue_deque.md)
- [`equals`, `hashCode`, and `toString`](../02_oop_core_concepts/equals_hashcode_tostring.md)
