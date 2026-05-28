# Functional Interfaces

## Goal

Understand functional interfaces and the standard Java interfaces most often
used with lambdas.

## Why It Matters

A lambda is useful only because it matches a functional interface. Knowing the
standard interfaces makes Java code easier to read and prevents unnecessary
custom types.

## What Is a Functional Interface?

A functional interface has exactly one abstract method.

```java
@FunctionalInterface
public interface EmailValidator {
    boolean isValid(String email);
}
```

The `@FunctionalInterface` annotation is optional but useful. It tells the
compiler to reject the interface if someone adds a second abstract method.

## `Predicate<T>`

Use `Predicate<T>` for yes/no checks.

```java
import java.util.function.Predicate;

Predicate<String> validEmail = email -> email != null && email.contains("@");

System.out.println(validEmail.test("alex@example.com")); // true
```

Practical use:

```java
static java.util.List<String> filter(
        java.util.List<String> values,
        Predicate<String> rule
) {
    java.util.List<String> result = new java.util.ArrayList<>();

    for (String value : values) {
        if (rule.test(value)) {
            result.add(value);
        }
    }

    return java.util.List.copyOf(result);
}
```

## `Function<T, R>`

Use `Function<T, R>` to convert one value into another.

```java
import java.util.function.Function;

Function<String, String> normalizeEmail = email -> email.trim().toLowerCase();

System.out.println(normalizeEmail.apply(" Alex@Example.com "));
```

`T` is the input type. `R` is the result type.

## `Consumer<T>`

Use `Consumer<T>` for an operation that accepts a value and returns nothing.

```java
import java.util.function.Consumer;

Consumer<String> printer = value -> System.out.println(value);

printer.accept("Hello");
```

Consumers are common for logging, event handling, and `forEach`. Avoid using
them for hidden business mutations when a returned value would be clearer.

## `Supplier<T>`

Use `Supplier<T>` for a value provider that takes no input.

```java
import java.time.Instant;
import java.util.function.Supplier;

Supplier<Instant> clock = () -> Instant.now();

System.out.println(clock.get());
```

Suppliers are useful for lazy values, factories, and testable time providers.

## Other Useful Interfaces

```java
import java.util.function.BiFunction;
import java.util.function.UnaryOperator;
import java.util.function.BinaryOperator;
```

- `BiFunction<T, U, R>` accepts two inputs and returns a result.
- `UnaryOperator<T>` accepts and returns the same type.
- `BinaryOperator<T>` accepts two values of the same type and returns that type.

Example:

```java
BinaryOperator<Integer> add = (left, right) -> left + right;
System.out.println(add.apply(2, 3)); // 5
```

## Practical Example

```java
import java.util.List;
import java.util.function.Function;
import java.util.function.Predicate;

public class UserSearch {
    public static void main(String[] args) {
        List<User> users = List.of(
                new User("alex@example.com", true),
                new User("sam@test.com", false)
        );

        List<String> activeEmails = findAndMap(
                users,
                User::active,
                User::email
        );

        System.out.println(activeEmails);
    }

    static <T, R> List<R> findAndMap(
            List<T> values,
            Predicate<T> filter,
            Function<T, R> mapper
    ) {
        java.util.ArrayList<R> result = new java.util.ArrayList<>();

        for (T value : values) {
            if (filter.test(value)) {
                result.add(mapper.apply(value));
            }
        }

        return List.copyOf(result);
    }

    record User(String email, boolean active) {
    }
}
```

The method receives behavior as parameters while preserving type safety.

## Common Mistakes

- Creating custom interfaces when `Predicate`, `Function`, `Consumer`, or
  `Supplier` would be clearer.
- Using `Consumer` for logic that should return a value.
- Forgetting that `Function<T, R>` may change the type.
- Naming lambda parameters too vaguely.
- Passing complex lambdas instead of named methods.

## Interview Questions

1. What is a functional interface?
2. Why use `@FunctionalInterface`?
3. What is the difference between `Predicate` and `Function`?
4. When would you use `Supplier`?
5. Why can overusing `Consumer` make code harder to test?

## Practice

1. Create a `Predicate<User>` for active users.
2. Create a `Function<User, String>` that extracts email.
3. Create a `Supplier<LocalDate>` for today's date.
4. Rewrite a custom one-method interface using a standard functional interface.

## Related Topics

- [Lambdas](lambdas.md)
- [Method References](method_references.md)
- [Generics and Type System](../05_generics_and_type_system/README.md)

