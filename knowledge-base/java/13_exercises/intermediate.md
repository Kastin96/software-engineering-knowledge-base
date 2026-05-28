# Intermediate Exercises

## 1. Order Totals by Status

Given:

```java
record Order(String id, String status, int totalInCents) {
}
```

Write a method that returns `Map<String, Integer>` with total order amount by
status.

Expected behavior:

- groups by status;
- sums totals in cents;
- rejects negative totals;
- returns an unmodifiable map.

## 2. User Lookup

Write a method that accepts `List<User>` and returns `Map<String, User>` keyed by
email.

Expected behavior:

- normalizes email to lowercase;
- rejects duplicate emails;
- rejects blank emails;
- preserves no unnecessary mutable state.

## 3. File Line Counter

Write a method:

```java
static int countNonBlankLines(Path path)
```

Expected behavior:

- reads UTF-8 text;
- counts non-blank lines;
- uses try-with-resources;
- wraps `IOException` in a custom runtime exception.

## 4. Repository-Backed Registration

Create a `RegistrationService` with:

```java
interface UserRepository {
    boolean existsByEmail(String email);
    void save(User user);
}
```

Expected behavior:

- validates email;
- normalizes email;
- rejects duplicates;
- saves new users;
- can be unit-tested with a fake or mock repository.

## 5. Top N Orders

Write a method that returns the top `n` orders by total descending.

Expected behavior:

- does not mutate the input list;
- rejects negative `n`;
- returns empty list when `n` is `0`;
- uses a stable tie-breaker by order id.

