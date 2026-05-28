# Generic Classes and Methods

## Goal

Understand how to create generic classes and methods when one implementation
should work with multiple types.

## Why It Matters

Generic classes and methods help you avoid duplicate code without losing type
safety. They are common in reusable utilities, wrappers, repositories,
validators, response objects, caches, and test helpers.

## Generic Class

A generic class declares a type parameter after the class name.

```java
public class Box<T> {
    private final T value;

    public Box(T value) {
        this.value = value;
    }

    public T value() {
        return value;
    }
}
```

Use it with different types.

```java
Box<String> emailBox = new Box<>("alex@example.com");
Box<Integer> scoreBox = new Box<>(95);

String email = emailBox.value();
Integer score = scoreBox.value();
```

`T` stands in for the real type used by the caller.

## Naming Type Parameters

Common names:

- `T` for a general type;
- `E` for collection elements;
- `K` for map keys;
- `V` for map values;
- `R` for return/result type.

Use longer names when they make the API clearer.

```java
public class ApiResponse<Body> {
    private final Body body;

    public ApiResponse(Body body) {
        this.body = body;
    }

    public Body body() {
        return body;
    }
}
```

Most Java code still uses short conventional names for simple generics.

## Generic Method

A method can declare its own type parameter.

```java
public class ListUtils {
    public static <T> T first(java.util.List<T> values) {
        if (values == null || values.isEmpty()) {
            throw new IllegalArgumentException("values must not be empty");
        }

        return values.get(0);
    }
}
```

Usage:

```java
String firstName = ListUtils.first(java.util.List.of("Alex", "Sam"));
Integer firstScore = ListUtils.first(java.util.List.of(90, 85));
```

Use a generic method when the type parameter is needed only for one method, not
for the whole class.

## Multiple Type Parameters

```java
public record Pair<K, V>(K key, V value) {
}
```

```java
Pair<String, Integer> stock = new Pair<>("keyboard", 10);
```

This is useful for small generic value carriers, though in domain code a named
record like `ProductStock` is often clearer than a generic `Pair`.

## Practical Example

```java
import java.util.List;
import java.util.Optional;

public class Finder {
    public static <T> Optional<T> findFirst(List<T> values, Match<T> match) {
        for (T value : values) {
            if (match.matches(value)) {
                return Optional.of(value);
            }
        }

        return Optional.empty();
    }
}

@FunctionalInterface
interface Match<T> {
    boolean matches(T value);
}
```

```java
List<String> emails = List.of("alex@example.com", "sam@test.com");
Optional<String> companyEmail = Finder.findFirst(
        emails,
        email -> email.endsWith("@example.com")
);
```

The same method can search any list while preserving the element type.

## Common Mistakes

- Making a class generic when only one method needs the type parameter.
- Using generic names that hide business meaning in domain code.
- Returning generic `Pair` objects from public APIs when a named record would be
  clearer.
- Using `Object` and casts instead of generic type parameters.
- Overusing generics for code that has only one real type.

## Interview Questions

1. What is a generic class?
2. What is a generic method?
3. Where is a method type parameter declared?
4. When should a method be generic instead of the class?
5. Why can a named domain record be better than a generic `Pair`?

## Practice

1. Create a generic `Box<T>` class.
2. Create a generic `first` method for a list.
3. Create a `Pair<K, V>` record.
4. Replace a method that uses `Object` and casts with a generic method.

## Related Topics

- [Generics Basics](generics_basics.md)
- [Bounded Type Parameters](bounded_type_parameters.md)
- [`var`, Diamond, and Type Inference](var_diamond_type_inference.md)
