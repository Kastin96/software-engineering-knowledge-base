# `var`, Diamond, and Type Inference

## Goal

Understand how Java infers types with the diamond operator and `var`, and when
explicit types make code clearer.

## Why It Matters

Modern Java lets you reduce repeated type information, but type inference should
not hide important intent. This matters especially with generics, where the
inferred type can be broader, narrower, or less readable than expected.

## Diamond Operator

The diamond operator lets the compiler infer constructor type arguments.

```java
import java.util.ArrayList;
import java.util.List;

List<String> emails = new ArrayList<>();
```

This is preferred over repeating the type.

```java
List<String> emails = new ArrayList<String>(); // still valid, more verbose
```

## `var`

`var` lets Java infer the local variable type from the initializer.

```java
var emails = new ArrayList<String>();
emails.add("alex@example.com");
```

The inferred type is `ArrayList<String>`, not `List<String>`.

That can matter if you want to program to the interface.

```java
List<String> emails = new ArrayList<>();
```

For public APIs and method signatures, use explicit types. `var` works only for
local variables.

## Good Uses of `var`

Use `var` when the type is obvious and the variable name carries the meaning.

```java
var today = java.time.LocalDate.now();
var usersById = new java.util.HashMap<String, User>();
```

Use explicit types when they improve readability.

```java
List<User> activeUsers = findActiveUsers();
```

This is clearer than:

```java
var activeUsers = findActiveUsers();
```

unless the return type is obvious from nearby code.

## Inference With Generic Methods

```java
static <T> T first(java.util.List<T> values) {
    if (values.isEmpty()) {
        throw new IllegalArgumentException("values must not be empty");
    }

    return values.get(0);
}
```

The compiler infers `T` from the argument.

```java
String firstEmail = first(java.util.List.of("alex@example.com", "sam@example.com"));
Integer firstScore = first(java.util.List.of(90, 85));
```

## Target Types

Sometimes the left side helps Java infer the type.

```java
java.util.List<String> names = java.util.List.of("Alex", "Sam");
```

Without a target type, Java still infers a type, but it may not be the one you
would choose for an API boundary.

```java
var names = java.util.List.of("Alex", "Sam");
```

This is fine for local code, but explicit types are usually better in fields,
parameters, and return types.

## Practical Example

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

public class UserIndex {
    public static void main(String[] args) {
        List<User> users = List.of(
                new User("u-100", "alex@example.com"),
                new User("u-200", "sam@example.com")
        );

        var usersById = new java.util.HashMap<String, User>();

        for (User user : users) {
            usersById.put(user.id(), user);
        }

        Map<String, User> readOnlyView = Map.copyOf(usersById);
        System.out.println(readOnlyView.get("u-100"));
    }

    record User(String id, String email) {
    }
}
```

`var` is acceptable for the local mutable map because the initializer is clear.
The read-only view uses the interface type to communicate how callers should
treat it.

## Common Mistakes

- Using `var` when the initializer does not reveal the type.
- Forgetting that `var list = new ArrayList<String>()` infers `ArrayList<String>`,
  not `List<String>`.
- Using explicit generic constructor types instead of the diamond operator.
- Assuming `var` makes Java dynamically typed.
- Letting inference hide important API intent.

## Interview Questions

1. What does the diamond operator do?
2. Is `var` dynamic typing?
3. Where can `var` be used?
4. Why might `List<String> values = new ArrayList<>()` be clearer than `var`?
5. How does Java infer type parameters for generic methods?

## Practice

1. Rewrite `new ArrayList<String>()` using the diamond operator.
2. Use `var` for a local variable where the initializer is obvious.
3. Replace a confusing `var` with an explicit type.
4. Write a generic `first` method and let Java infer the result type.

## Related Topics

- [Generic Classes and Methods](generic_classes_methods.md)
- [Generics Basics](generics_basics.md)
- [Variables](../01_language_basics/variables.md)

