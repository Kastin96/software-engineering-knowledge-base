# Wildcards

## Goal

Understand Java wildcards, especially `?`, `? extends T`, and `? super T`.

## Why It Matters

Wildcards make generic APIs flexible. They are also one of the most common Java
interview topics because they expose an important rule: a `List<Integer>` is not
a subtype of `List<Number>`.

## Invariance

Generics are invariant by default.

```java
java.util.List<Integer> integers = java.util.List.of(1, 2, 3);

// Does not compile:
// java.util.List<Number> numbers = integers;
```

This prevents unsafe writes. If the assignment were allowed, someone could add a
`Double` to a list that is actually a `List<Integer>`.

## Unbounded Wildcard

Use `List<?>` when you can work with values as unknown objects.

```java
static void printAll(java.util.List<?> values) {
    for (Object value : values) {
        System.out.println(value);
    }
}
```

You can read values as `Object`, but you cannot add normal values because Java
does not know the list's element type.

## `? extends T`

Use `? extends T` when you need to read values as `T`.

```java
static double sum(java.util.List<? extends Number> values) {
    double total = 0;

    for (Number value : values) {
        total += value.doubleValue();
    }

    return total;
}
```

This accepts `List<Integer>`, `List<Long>`, and `List<Double>`.

```java
sum(java.util.List.of(1, 2, 3));
sum(java.util.List.of(1.5, 2.5));
```

You generally should not add to a `? extends T` collection, because Java does
not know the exact subtype.

## `? super T`

Use `? super T` when you need to write values of type `T`.

```java
static void addDefaultRoles(java.util.List<? super String> roles) {
    roles.add("user");
    roles.add("viewer");
}
```

This can accept `List<String>`, `List<CharSequence>`, or `List<Object>`.

When reading from `? super T`, values are safely read as `Object`.

## PECS

The common rule is PECS:

```text
Producer Extends, Consumer Super
```

If a collection produces values for your method to read, use `extends`.

If a collection consumes values your method writes, use `super`.

## Practical Example

```java
import java.util.ArrayList;
import java.util.List;

public class CopyExample {
    public static <T> void copy(List<? extends T> source, List<? super T> target) {
        for (T value : source) {
            target.add(value);
        }
    }

    public static void main(String[] args) {
        List<Integer> source = List.of(1, 2, 3);
        List<Number> target = new ArrayList<>();

        copy(source, target);

        System.out.println(target);
    }
}
```

The source produces `T`; the target consumes `T`.

## Common Mistakes

- Expecting `List<Integer>` to be assignable to `List<Number>`.
- Using `List<Object>` when `List<?>` is the right read-only unknown type.
- Adding values to `List<? extends T>`.
- Reading specific values from `List<? super T>` without casting.
- Using wildcards everywhere and making APIs harder to read.

## Interview Questions

1. Why is `List<Integer>` not a subtype of `List<Number>`?
2. What does `List<?>` mean?
3. What does `? extends Number` allow?
4. What does `? super String` allow?
5. What does PECS mean?

## Practice

1. Write a method that prints any list using `List<?>`.
2. Write a method that sums `List<? extends Number>`.
3. Write a method that adds default strings to `List<? super String>`.
4. Implement a generic copy method using both `extends` and `super`.

## Related Topics

- [Bounded Type Parameters](bounded_type_parameters.md)
- [Raw Types and Type Safety](raw_types_type_safety.md)
- [`List`](../03_collections_and_data_structures/list.md)

