# Assertions

## Goal

Understand common JUnit assertions and how to make failures useful.

## Why It Matters

Assertions are the part of a test that proves behavior. A test with weak or vague
assertions may run successfully without checking the thing that matters.

## Basic Assertions

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertTrue;

assertEquals(5, calculator.add(2, 3));
assertTrue(user.active());
assertFalse(errors.isEmpty());
```

Use the assertion that matches the intent.

## Null Assertions

```java
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertNull;

assertNotNull(user.id());
assertNull(result.error());
```

Do not overuse null assertions when a more specific assertion would be clearer.

## Exception Assertions

```java
import static org.junit.jupiter.api.Assertions.assertThrows;

IllegalArgumentException exception = assertThrows(
        IllegalArgumentException.class,
        () -> calculator.totalInCents(-1, 2)
);

assertEquals("unitPriceInCents must not be negative", exception.getMessage());
```

`assertThrows` verifies both that an exception is thrown and which type it is.

## Assert All

Use `assertAll` to check multiple related facts about one result.

```java
import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertTrue;

assertAll(
        () -> assertEquals("alex@example.com", user.email()),
        () -> assertTrue(user.active())
);
```

Keep assertions related. Do not use `assertAll` to combine unrelated behaviors.

## Helpful Messages

JUnit assertions can include messages.

```java
assertEquals(3, users.size(), "active users should be returned");
```

Use messages when the expected behavior is not obvious from the assertion.

## Practical Example

```java
record SignupResult(boolean accepted, String error) {
}
```

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;

class SignupServiceTest {
    @Test
    void rejectsUnderageUser() {
        SignupResult result = new SignupService().signup("alex@example.com", 16);

        assertAll(
                () -> assertFalse(result.accepted()),
                () -> assertEquals("user must be adult", result.error())
        );
    }
}
```

The assertions verify both the status and the reason.

## Common Mistakes

- Testing only that a result is not null.
- Using `assertTrue` for complex expressions instead of specific assertions.
- Not checking exception messages when the message is part of behavior.
- Writing multiple unrelated assertions in one test.
- Using assertion messages to explain unclear production code instead of naming
  things well.

## Interview Questions

1. What is an assertion?
2. When would you use `assertThrows`?
3. Why can `assertTrue(complexExpression)` be weak?
4. When is `assertAll` useful?
5. What makes an assertion failure easy to understand?

## Practice

1. Replace `assertTrue(result == 5)` with a better assertion.
2. Add an exception assertion for invalid input.
3. Use `assertAll` for related fields of one response object.
4. Improve a vague assertion failure with a clearer expected value.

## Related Topics

- [JUnit Jupiter](junit_jupiter.md)
- [Test Cases](test_cases.md)
- [Exceptions and Debugging](../04_exceptions_and_debugging/README.md)

