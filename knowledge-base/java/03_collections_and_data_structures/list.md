# `List`

## Goal

Understand how Java `List` stores ordered values, when to use `ArrayList`, and
how to avoid common mutability mistakes.

## Why It Matters

`List` is one of the most common Java collection types. It is used for API
responses, database query results, request items, validation errors, IDs, and
ordered business data.

## Basic Usage

```java
import java.util.ArrayList;
import java.util.List;

List<String> emails = new ArrayList<>();

emails.add("alex@example.com");
emails.add("sam@example.com");
emails.add("mira@example.com");

System.out.println(emails.get(0)); // alex@example.com
System.out.println(emails.size()); // 3
```

`List` preserves insertion order and allows duplicates.

## Prefer the Interface Type

Use the interface in variable declarations and method signatures.

```java
List<String> emails = new ArrayList<>();
```

This keeps code flexible. The caller usually should not care whether the
implementation is `ArrayList`, `LinkedList`, or another `List`.

## `ArrayList` vs `LinkedList`

`ArrayList` is the default choice for most application code.

- Fast indexed access.
- Good iteration performance.
- Efficient appends at the end in typical cases.

`LinkedList` is rarely needed in modern application code. If you need queue or
stack-like operations, `ArrayDeque` is usually a better first choice. `LinkedList`
has more memory overhead and poor random access.

When unsure, start with `ArrayList`.

## Immutable and Unmodifiable Lists

Use `List.of` for small unmodifiable lists.

```java
List<String> roles = List.of("admin", "manager", "user");
```

This list cannot be changed.

```java
// Throws UnsupportedOperationException:
// roles.add("guest");
```

Use `List.copyOf` to return a safe copy from a class.

```java
public List<String> roles() {
    return List.copyOf(roles);
}
```

## Filtering Manually

Before streams, make sure you can read ordinary loops.

```java
import java.util.ArrayList;
import java.util.List;

List<String> emails = List.of(
        "alex@example.com",
        "invalid-email",
        "sam@example.com"
);

List<String> validEmails = new ArrayList<>();

for (String email : emails) {
    if (email.contains("@")) {
        validEmails.add(email);
    }
}

System.out.println(validEmails);
```

This shape appears often in coding interviews and simple backend logic.

## Practical Example

```java
import java.util.ArrayList;
import java.util.List;

public class ValidationErrors {
    public static void main(String[] args) {
        List<String> errors = validateUser("", 16);

        if (errors.isEmpty()) {
            System.out.println("User is valid");
        } else {
            System.out.println(errors);
        }
    }

    static List<String> validateUser(String email, int age) {
        List<String> errors = new ArrayList<>();

        if (email == null || email.isBlank() || !email.contains("@")) {
            errors.add("email is invalid");
        }

        if (age < 18) {
            errors.add("user must be adult");
        }

        return List.copyOf(errors);
    }
}
```

This example uses a list for a realistic reason: one validation pass can produce
multiple errors.

## Common Mistakes

- Using `ArrayList` in method signatures when `List` is enough.
- Assuming `List.of` returns a mutable list.
- Removing items inside an enhanced `for` loop.
- Calling `get(0)` without checking whether the list is empty.
- Using a list for frequent lookups by id when a `Map` would be better.

## Interview Questions

1. Does `List` allow duplicates?
2. Does `List` preserve order?
3. Why is `ArrayList` usually the default implementation?
4. What is the difference between `List.of` and `new ArrayList<>()`?
5. When would a `Map` be better than a `List`?

## Practice

1. Create a list of order amounts and calculate the total.
2. Filter invalid emails into a separate list.
3. Return an unmodifiable copy from a method.
4. Replace a list lookup loop with a `Map` lookup after reading the `Map` topic.

## Related Topics

- [Arrays](arrays.md)
- [`Map`](map.md)
- [`Queue` and `Deque`](queue_deque.md)
- [Iterators](iterators.md)
