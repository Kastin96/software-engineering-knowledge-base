# Raw Types and Type Safety

## Goal

Understand raw types, unchecked warnings, and how to keep generic Java code type
safe.

## Why It Matters

Raw types are still seen in old Java code, legacy APIs, and careless examples.
They weaken the compiler's ability to catch bugs. In interview and production
code, avoiding raw types is one of the simplest signs of modern Java fluency.

## What Is a Raw Type?

A raw type uses a generic class without type arguments.

```java
java.util.List values = new java.util.ArrayList();
```

Modern code should specify the element type.

```java
java.util.List<String> values = new java.util.ArrayList<>();
```

## Why Raw Types Are Dangerous

```java
import java.util.ArrayList;
import java.util.List;

List values = new ArrayList();
values.add("alex@example.com");
values.add(123);

for (Object value : values) {
    String email = (String) value;
    System.out.println(email.toLowerCase());
}
```

This compiles with warnings but fails at runtime when it reaches the integer.

## Unchecked Warnings

Unchecked warnings mean the compiler cannot fully prove type safety.

```java
List raw = new ArrayList();
raw.add("alex@example.com");

List<String> emails = raw; // unchecked assignment warning
```

Do not ignore these warnings casually. They often point to future
`ClassCastException` bugs.

## Fix With Proper Types

```java
List<String> emails = new ArrayList<>();
emails.add("alex@example.com");
```

If the values are truly unknown, use a wildcard.

```java
static void printValues(List<?> values) {
    for (Object value : values) {
        System.out.println(value);
    }
}
```

`List<?>` is safer than raw `List` because it prevents unsafe writes.

## Suppressing Warnings

Sometimes legacy integration requires an unchecked cast. Keep suppression as
narrow as possible and explain why it is safe.

```java
@SuppressWarnings("unchecked")
static List<String> readEmailsFromLegacyApi(Object response) {
    return (List<String>) response;
}
```

This is risky unless the boundary has external guarantees. Prefer validating or
converting values when possible.

Safer conversion:

```java
static List<String> toEmailList(List<?> values) {
    List<String> emails = new ArrayList<>();

    for (Object value : values) {
        if (!(value instanceof String email)) {
            throw new IllegalArgumentException("expected only strings");
        }

        emails.add(email);
    }

    return List.copyOf(emails);
}
```

## Practical Example

```java
import java.util.ArrayList;
import java.util.List;

public class LegacyImportAdapter {
    public List<String> extractEmails(List<?> rawValues) {
        List<String> emails = new ArrayList<>();

        for (Object rawValue : rawValues) {
            if (rawValue instanceof String email && email.contains("@")) {
                emails.add(email);
            }
        }

        return List.copyOf(emails);
    }
}
```

The adapter accepts unknown legacy values but returns a clean, typed result.

## Common Mistakes

- Using raw `List`, `Map`, or `Set` in new code.
- Suppressing warnings at a whole class level.
- Treating unchecked warnings as harmless.
- Casting from raw collections without validating contents.
- Using `Object` when a generic type parameter would preserve type safety.

## Interview Questions

1. What is a raw type?
2. Why are raw types unsafe?
3. What is an unchecked warning?
4. When is `@SuppressWarnings("unchecked")` acceptable?
5. Why is `List<?>` safer than raw `List`?

## Practice

1. Create a raw list example that causes a `ClassCastException`.
2. Fix it with `List<String>`.
3. Rewrite a raw `List` parameter as `List<?>`.
4. Convert a `List<?>` into a validated `List<String>`.

## Related Topics

- [Generics Basics](generics_basics.md)
- [Type Erasure](type_erasure.md)
- [Wildcards](wildcards.md)

