# Exceptions

## Goal

Understand what exceptions are, how Java uses them to signal failures, and how
to design code that fails clearly.

## Why It Matters

Every real Java application has failure paths: invalid input, missing files,
database issues, network timeouts, parsing errors, and programming bugs.
Exceptions let Java separate normal logic from failure handling, but careless
exception handling can hide bugs and make production issues much harder to
diagnose.

## What Is an Exception?

An exception is an object that represents an error or unexpected condition.

```java
public class AgeCheck {
    public static void main(String[] args) {
        int age = -1;

        if (age < 0) {
            throw new IllegalArgumentException("age must not be negative");
        }

        System.out.println("Age: " + age);
    }
}
```

`throw` stops the current normal flow and starts looking for code that can handle
the exception.

## Exception Hierarchy

The simplified hierarchy looks like this:

```text
Throwable
  Error
  Exception
    RuntimeException
```

`Error` represents serious JVM-level problems that application code normally
should not catch.

`Exception` represents conditions an application may handle.

`RuntimeException` represents unchecked exceptions, often caused by invalid
arguments, invalid state, null values, or programming mistakes.

## Common Runtime Exceptions

```java
String name = null;

// Throws NullPointerException:
// System.out.println(name.length());
```

```java
int[] scores = {90, 100};

// Throws ArrayIndexOutOfBoundsException:
// System.out.println(scores[2]);
```

```java
String input = "abc";

// Throws NumberFormatException:
// int value = Integer.parseInt(input);
```

These exceptions usually mean the code needs better validation, safer data
access, or clearer assumptions.

## Fail Fast

Fail fast when a method receives invalid input.

```java
static int calculateDiscount(int totalInCents) {
    if (totalInCents < 0) {
        throw new IllegalArgumentException("totalInCents must not be negative");
    }

    return totalInCents >= 10_000 ? 10 : 0;
}
```

This is better than allowing invalid data to move deeper into the system and
fail later in a confusing place.

## Do Not Use Exceptions for Normal Flow

Avoid using exceptions for expected everyday decisions.

```java
static boolean isInteger(String value) {
    if (value == null || value.isBlank()) {
        return false;
    }

    for (int i = 0; i < value.length(); i++) {
        if (!Character.isDigit(value.charAt(i))) {
            return false;
        }
    }

    return true;
}
```

If invalid input is expected and frequent, a validation result can be clearer
than throwing and catching exceptions repeatedly.

## Practical Example

```java
public class UserRegistration {
    public static void main(String[] args) {
        register("alex@example.com", 21);
    }

    static void register(String email, int age) {
        validateEmail(email);
        validateAdult(age);

        System.out.println("Registered " + email);
    }

    static void validateEmail(String email) {
        if (email == null || email.isBlank() || !email.contains("@")) {
            throw new IllegalArgumentException("email must be valid");
        }
    }

    static void validateAdult(int age) {
        if (age < 18) {
            throw new IllegalArgumentException("user must be at least 18");
        }
    }
}
```

The program validates close to the boundary and throws exceptions with messages
that explain the rule that failed.

## Common Mistakes

- Throwing vague exceptions with messages like `"error"` or `"bad input"`.
- Catching every exception too early.
- Swallowing exceptions without logging, returning, or rethrowing.
- Using exceptions for common successful control flow.
- Catching `Throwable` or `Error` in normal application code.

## Interview Questions

1. What is an exception in Java?
2. What is the difference between `Exception` and `RuntimeException`?
3. Why should exception messages be specific?
4. What does fail fast mean?
5. Why should application code usually avoid catching `Error`?

## Practice

1. Write a method that validates a product price and throws a clear exception.
2. Find three common runtime exceptions and explain what usually causes them.
3. Rewrite a vague exception message into a useful one.
4. Decide whether a validation failure in a method should return `false` or
   throw an exception.

## Related Topics

- [Checked and Unchecked Exceptions](checked_unchecked.md)
- [`try`, `catch`, and `finally`](try_catch_finally.md)
- [Stack Traces](stack_traces.md)

