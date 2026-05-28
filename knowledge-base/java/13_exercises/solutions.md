# Exercise Solutions

Use these solutions after attempting the exercises. They are intentionally
straightforward rather than clever.

## Beginner 1. Email Normalizer

```java
static String normalizeEmail(String email) {
    if (email == null || email.isBlank() || !email.contains("@")) {
        throw new IllegalArgumentException("email must be valid");
    }

    return email.trim().toLowerCase();
}
```

## Beginner 2. Price Total

```java
static int totalInCents(int unitPriceInCents, int quantity) {
    if (unitPriceInCents < 0) {
        throw new IllegalArgumentException("unitPriceInCents must not be negative");
    }

    if (quantity < 0) {
        throw new IllegalArgumentException("quantity must not be negative");
    }

    return unitPriceInCents * quantity;
}
```

## Beginner 3. Active User Emails

```java
import java.util.List;

record User(String email, boolean active) {
}

static List<String> activeEmails(List<User> users) {
    return users.stream()
            .filter(User::active)
            .map(User::email)
            .toList();
}
```

## Beginner 4. Duplicate Tags

```java
import java.util.Collections;
import java.util.LinkedHashSet;
import java.util.List;
import java.util.Set;

static Set<String> duplicateTags(List<String> tags) {
    Set<String> seen = new LinkedHashSet<>();
    Set<String> duplicates = new LinkedHashSet<>();

    for (String tag : tags) {
        if (tag == null || tag.isBlank()) {
            continue;
        }

        String normalized = tag.trim().toLowerCase();

        if (!seen.add(normalized)) {
            duplicates.add(normalized);
        }
    }

    return Collections.unmodifiableSet(new LinkedHashSet<>(duplicates));
}
```

## Beginner 5. Grade Label

```java
static String gradeLabel(int score) {
    if (score < 0 || score > 100) {
        throw new IllegalArgumentException("score must be between 0 and 100");
    }

    if (score >= 90) {
        return "A";
    }

    if (score >= 80) {
        return "B";
    }

    if (score >= 70) {
        return "C";
    }

    return "Needs improvement";
}
```

## Intermediate 1. Order Totals by Status

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

record Order(String id, String status, int totalInCents) {
}

static Map<String, Integer> totalByStatus(List<Order> orders) {
    for (Order order : orders) {
        if (order.totalInCents() < 0) {
            throw new IllegalArgumentException("order total must not be negative: " + order.id());
        }
    }

    return Map.copyOf(orders.stream()
            .collect(Collectors.groupingBy(
                    Order::status,
                    Collectors.summingInt(Order::totalInCents)
            )));
}
```

## Intermediate 2. User Lookup

```java
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

record User(String email) {
}

static Map<String, User> usersByEmail(List<User> users) {
    Map<String, User> result = new LinkedHashMap<>();

    for (User user : users) {
        if (user.email() == null || user.email().isBlank() || !user.email().contains("@")) {
            throw new IllegalArgumentException("email must be valid");
        }

        String email = user.email().trim().toLowerCase();

        if (result.putIfAbsent(email, new User(email)) != null) {
            throw new IllegalArgumentException("duplicate email: " + email);
        }
    }

    return Map.copyOf(result);
}
```

## Advanced 4. Generic First Match

```java
import java.util.List;
import java.util.Optional;
import java.util.function.Predicate;

static <T> Optional<T> firstMatching(List<T> values, Predicate<T> rule) {
    if (values == null) {
        throw new IllegalArgumentException("values must not be null");
    }

    if (rule == null) {
        throw new IllegalArgumentException("rule must not be null");
    }

    for (T value : values) {
        if (rule.test(value)) {
            return Optional.ofNullable(value);
        }
    }

    return Optional.empty();
}
```

## Notes

The advanced exercises are intentionally larger. A good solution should show
clear boundaries, tests, and error handling, not only code that happens to pass a
sample input.
