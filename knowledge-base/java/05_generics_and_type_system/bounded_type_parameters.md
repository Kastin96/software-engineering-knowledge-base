# Bounded Type Parameters

## Goal

Understand how bounds restrict generic type parameters to types that support
required behavior.

## Why It Matters

Sometimes a generic method should work with many types, but not every type. For
example, sorting needs comparable values, calculating totals may need numbers,
and validation may need objects that expose an id. Bounds let you express those
requirements in the type system.

## Upper Bounds

Use `extends` to say a type parameter must be a subtype of a class or interface.

```java
public static <T extends Number> double sum(java.util.List<T> values) {
    double total = 0;

    for (T value : values) {
        total += value.doubleValue();
    }

    return total;
}
```

This method accepts `List<Integer>`, `List<Long>`, `List<Double>`, and other
number types.

```java
double result = sum(java.util.List.of(10, 20, 30));
```

Because `T` extends `Number`, the method can call `doubleValue()`.

## Interface Bounds

```java
public static <T extends Comparable<T>> T max(java.util.List<T> values) {
    if (values == null || values.isEmpty()) {
        throw new IllegalArgumentException("values must not be empty");
    }

    T max = values.get(0);

    for (T value : values) {
        if (value.compareTo(max) > 0) {
            max = value;
        }
    }

    return max;
}
```

`Comparable<T>` is an interface. The bound lets the method compare values.

## Multiple Bounds

A type parameter can have multiple bounds.

```java
public static <T extends AutoCloseable & Runnable> void runAndClose(T task) throws Exception {
    try (task) {
        task.run();
    }
}
```

If there is a class bound, it must come first. Interface bounds come after it.

## Bounded Generic Class

```java
public class ScoreBoard<T extends Comparable<T>> {
    private final java.util.List<T> scores = new java.util.ArrayList<>();

    public void add(T score) {
        scores.add(score);
    }

    public T highest() {
        return scores.stream()
                .max(T::compareTo)
                .orElseThrow(() -> new IllegalStateException("no scores"));
    }
}
```

The class can safely compare values because `T` is bounded.

## Practical Example

```java
import java.util.List;

public class IdPrinter {
    public static <T extends HasId> void printIds(List<T> values) {
        for (T value : values) {
            System.out.println(value.id());
        }
    }
}

interface HasId {
    String id();
}

record User(String id, String email) implements HasId {
}

record Order(String id, int totalInCents) implements HasId {
}
```

The method works with any object that implements `HasId`, without accepting
unrelated types.

## Common Mistakes

- Using an unbounded type parameter and then needing casts.
- Using a bound that is too specific, making the API hard to reuse.
- Forgetting that `extends` is used for both class and interface bounds.
- Creating complex bounds that make method signatures harder to read than the
  logic they protect.
- Using `T extends Number` for money calculations where `BigDecimal` or integer
  minor units would be safer.

## Interview Questions

1. What is an upper bounded type parameter?
2. Why does `<T extends Number>` allow calling `doubleValue()`?
3. Can a type parameter have multiple bounds?
4. Why does Java use `extends` for interface bounds too?
5. When can bounds make an API too complicated?

## Practice

1. Write a generic `sum` method for numbers.
2. Write a generic `max` method for comparable values.
3. Create a `HasId` interface and a method that accepts only objects with ids.
4. Refactor a method that uses casts into one that uses a bound.

## Related Topics

- [Generic Classes and Methods](generic_classes_methods.md)
- [Wildcards](wildcards.md)
- [Sorting](../03_collections_and_data_structures/sorting.md)

