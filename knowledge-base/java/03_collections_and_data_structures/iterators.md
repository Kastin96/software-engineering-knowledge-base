# Iterators

## Goal

Understand how Java iterators work and how to modify collections safely while
iterating.

## Why It Matters

Most code uses enhanced `for` loops, but iterators matter when you need safe
removal during traversal or when you need to understand
`ConcurrentModificationException`.

## Enhanced For Loop

```java
import java.util.List;

List<String> emails = List.of("alex@example.com", "sam@example.com");

for (String email : emails) {
    System.out.println(email);
}
```

Enhanced `for` is readable and should be the default when you only need to read
items.

## Iterator Basics

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

List<String> emails = new ArrayList<>();
emails.add("alex@example.com");
emails.add("sam@example.com");

Iterator<String> iterator = emails.iterator();

while (iterator.hasNext()) {
    String email = iterator.next();
    System.out.println(email);
}
```

An iterator gives controlled access to each element.

## Safe Removal

Do not remove from a collection directly inside an enhanced `for` loop.

```java
List<String> emails = new ArrayList<>(List.of(
        "alex@example.com",
        "",
        "sam@example.com"
));

Iterator<String> iterator = emails.iterator();

while (iterator.hasNext()) {
    String email = iterator.next();

    if (email.isBlank()) {
        iterator.remove();
    }
}

System.out.println(emails);
```

Use `iterator.remove()` after `next()` to remove the current element safely.

## `removeIf`

For simple removal conditions, prefer `removeIf`.

```java
List<String> emails = new ArrayList<>(List.of(
        "alex@example.com",
        "",
        "sam@example.com"
));

emails.removeIf(String::isBlank);

System.out.println(emails);
```

This is shorter and clearer when the rule is simple.

## Concurrent Modification

This can throw `ConcurrentModificationException`:

```java
List<String> emails = new ArrayList<>(List.of("a@example.com", ""));

for (String email : emails) {
    if (email.isBlank()) {
        emails.remove(email);
    }
}
```

The enhanced loop uses an iterator internally. Directly modifying the list
outside that iterator breaks the traversal.

This exception is about unsafe structural modification during iteration. It is
not the same thing as multi-threaded concurrency, even though the name can sound
similar.

## Practical Example

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class ExpiredSessionCleanup {
    public static void main(String[] args) {
        List<Session> sessions = new ArrayList<>(List.of(
                new Session("s-1", false),
                new Session("s-2", true),
                new Session("s-3", false)
        ));

        Iterator<Session> iterator = sessions.iterator();

        while (iterator.hasNext()) {
            Session session = iterator.next();

            if (session.expired()) {
                iterator.remove();
            }
        }

        System.out.println(sessions);
    }

    record Session(String id, boolean expired) {
    }
}
```

This mirrors cleanup logic where invalid or expired items must be removed from a
mutable collection.

## Common Mistakes

- Removing directly from a list inside an enhanced `for` loop.
- Calling `iterator.remove()` before calling `next()`.
- Using an iterator when `removeIf` would be simpler.
- Confusing `ConcurrentModificationException` with thread-safety guarantees.
- Modifying a collection while another part of the code is iterating over it.

## Interview Questions

1. What is an iterator?
2. Why can direct removal during an enhanced `for` loop fail?
3. When should you use `iterator.remove()`?
4. When is `removeIf` better than a manual iterator?
5. What does `ConcurrentModificationException` usually indicate?

## Practice

1. Remove blank strings from a mutable list using an iterator.
2. Rewrite the same logic using `removeIf`.
3. Explain why direct removal inside an enhanced `for` loop is unsafe.
4. Iterate over a map using `entrySet`.

## Related Topics

- [`List`](list.md)
- [`Set`](set.md)
- [`Map`](map.md)
- [`Queue` and `Deque`](queue_deque.md)
