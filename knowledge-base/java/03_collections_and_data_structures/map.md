# `Map`

## Goal

Understand how Java `Map` stores key-value pairs and when it is better than
scanning a list.

## Why It Matters

`Map` is essential for lookups, grouping, counting, caching, request metadata,
configuration, and indexing objects by id. Choosing a `Map` can turn repeated
linear searches into direct lookups.

## Basic Usage

```java
import java.util.HashMap;
import java.util.Map;

Map<String, String> userNamesById = new HashMap<>();

userNamesById.put("u-100", "Alex");
userNamesById.put("u-200", "Sam");

System.out.println(userNamesById.get("u-100")); // Alex
System.out.println(userNamesById.containsKey("u-300")); // false
```

A map stores values by key. Keys are unique. Values can repeat.

## Common Implementations

`HashMap` is the default choice for most lookups.

- Fast average `put`, `get`, and `containsKey`.
- No guaranteed iteration order.
- Requires correct `equals` and `hashCode` for custom keys.

`LinkedHashMap` preserves insertion order.

```java
Map<String, Integer> orderedCounts = new java.util.LinkedHashMap<>();
```

`TreeMap` keeps keys sorted.

```java
Map<String, Integer> sortedCounts = new java.util.TreeMap<>();
```

## Safe Lookup

`get` returns `null` when the key is missing.

```java
String name = userNamesById.get("u-999");

if (name == null) {
    System.out.println("User not found");
}
```

Use `getOrDefault` when a fallback is enough.

```java
int count = countsByStatus.getOrDefault("failed", 0);
```

## Counting Values

```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;

List<String> statuses = List.of("new", "paid", "new", "failed", "paid");
Map<String, Integer> countsByStatus = new HashMap<>();

for (String status : statuses) {
    int currentCount = countsByStatus.getOrDefault(status, 0);
    countsByStatus.put(status, currentCount + 1);
}

System.out.println(countsByStatus);
```

This pattern appears often in coding tasks and reporting logic.

## Indexing by ID

```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;

record Order(String id, int totalInCents) {
}

List<Order> orders = List.of(
        new Order("o-1", 2500),
        new Order("o-2", 4900)
);

Map<String, Order> ordersById = new HashMap<>();

for (Order order : orders) {
    ordersById.put(order.id(), order);
}

Order order = ordersById.get("o-2");
System.out.println(order.totalInCents());
```

Use a map when you repeatedly need to find objects by a unique key.

## Practical Example

```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class UserRoleLookup {
    public static void main(String[] args) {
        List<UserRole> roles = List.of(
                new UserRole("alex@example.com", "admin"),
                new UserRole("sam@example.com", "user")
        );

        Map<String, String> roleByEmail = new HashMap<>();

        for (UserRole role : roles) {
            roleByEmail.put(role.email(), role.role());
        }

        String role = roleByEmail.getOrDefault("mira@example.com", "guest");
        System.out.println(role);
    }

    record UserRole(String email, String role) {
    }
}
```

This mirrors real authorization and lookup flows.

## Common Mistakes

- Using a list scan for repeated lookups by id.
- Forgetting that `get` can return `null`.
- Using mutable objects as map keys.
- Expecting `HashMap` iteration order to be stable.
- Calling `containsValue` frequently; it scans values and is usually not the
  reason to choose a map.

## Interview Questions

1. What is the difference between a key and a value?
2. Can a `Map` have duplicate keys?
3. Why is `HashMap` fast for lookups on average?
4. Why are mutable keys dangerous?
5. When would you choose `LinkedHashMap` or `TreeMap`?

## Practice

1. Count how many times each status appears in a list.
2. Build a map of products by product id.
3. Use `getOrDefault` for a missing key.
4. Replace a repeated list search with a map lookup.

## Related Topics

- [`List`](list.md)
- [`Set`](set.md)
- [`equals`, `hashCode`, and `toString`](../02_oop_core_concepts/equals_hashcode_tostring.md)

