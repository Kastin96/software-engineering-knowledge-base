# Lambdas

## Goal

Understand Java lambda expressions and when they make code clearer.

## Why It Matters

Lambdas are used throughout modern Java: streams, sorting, callbacks, validators,
event handlers, asynchronous APIs, and tests. They let you pass behavior as a
value without creating a full anonymous class.

## Basic Lambda

```java
import java.util.List;

public class LambdaExample {
    public static void main(String[] args) {
        List<String> emails = List.of("alex@example.com", "sam@test.com");

        emails.forEach(email -> System.out.println(email));
    }
}
```

The lambda `email -> System.out.println(email)` receives one value and performs
an action.

## Lambda Syntax

One parameter:

```java
email -> email.toLowerCase()
```

Multiple parameters:

```java
(first, second) -> first.compareTo(second)
```

Block body:

```java
email -> {
    String normalized = email.trim().toLowerCase();
    System.out.println(normalized);
}
```

Use a block when the logic needs more than one expression. If the block grows,
extract a method.

## Lambdas Need a Target Type

A lambda must match a functional interface.

```java
@FunctionalInterface
interface EmailValidator {
    boolean isValid(String email);
}
```

```java
EmailValidator validator = email -> email != null && email.contains("@");

System.out.println(validator.isValid("alex@example.com")); // true
```

The compiler knows the lambda parameter and return types from the target
interface.

## Replacing Anonymous Classes

Before lambdas, simple behavior often used anonymous classes.

```java
Runnable task = new Runnable() {
    @Override
    public void run() {
        System.out.println("Run task");
    }
};
```

With a lambda:

```java
Runnable task = () -> System.out.println("Run task");
```

## Capturing Variables

Lambdas can use local variables that are final or effectively final.

```java
String domain = "@example.com";

EmailValidator companyEmail = email -> email.endsWith(domain);
```

`domain` is effectively final because it is not reassigned after initialization.

This does not compile:

```java
String domain = "@example.com";
// domain = "@test.com";
// EmailValidator validator = email -> email.endsWith(domain);
```

## Practical Example

```java
import java.util.ArrayList;
import java.util.List;

public class ActiveUserFilter {
    public static void main(String[] args) {
        List<User> users = List.of(
                new User("alex@example.com", true),
                new User("sam@example.com", false),
                new User("mira@example.com", true)
        );

        List<User> activeUsers = filter(users, user -> user.active());
        System.out.println(activeUsers);
    }

    static List<User> filter(List<User> users, UserRule rule) {
        List<User> result = new ArrayList<>();

        for (User user : users) {
            if (rule.matches(user)) {
                result.add(user);
            }
        }

        return List.copyOf(result);
    }

    record User(String email, boolean active) {
    }
}

@FunctionalInterface
interface UserRule {
    boolean matches(ActiveUserFilter.User user);
}
```

The lambda makes the filtering rule replaceable without hard-coding it into the
method.

## Common Mistakes

- Writing large lambdas instead of extracting a named method.
- Using lambdas with side effects that make code hard to reason about.
- Forgetting that captured local variables must be effectively final.
- Creating a custom functional interface when a standard one already exists.
- Using lambdas only because they look modern, even when a loop is clearer.

## Interview Questions

1. What is a lambda expression?
2. What is a target type for a lambda?
3. What does effectively final mean?
4. How do lambdas differ from anonymous classes in everyday code?
5. When should a lambda be extracted into a method?

## Practice

1. Create a lambda that validates an email.
2. Rewrite an anonymous `Runnable` as a lambda.
3. Write a lambda that compares two strings by length.
4. Refactor a large lambda into a named method.

## Related Topics

- [Functional Interfaces](functional_interfaces.md)
- [Method References](method_references.md)
- [Streams](streams.md)

