# Unit Testing

## Goal

Understand what unit tests are and what makes them useful.

## Why It Matters

Good unit tests give fast feedback. They catch regressions, document expected
behavior, and make refactoring safer. Bad unit tests create noise, slow down
changes, and make developers distrust the test suite.

## What Is a Unit Test?

A unit test verifies a small piece of behavior in isolation.

```java
public class PriceCalculator {
    public int totalInCents(int unitPriceInCents, int quantity) {
        if (unitPriceInCents < 0) {
            throw new IllegalArgumentException("unitPriceInCents must not be negative");
        }

        if (quantity < 0) {
            throw new IllegalArgumentException("quantity must not be negative");
        }

        return unitPriceInCents * quantity;
    }
}
```

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class PriceCalculatorTest {
    private final PriceCalculator calculator = new PriceCalculator();

    @Test
    void calculatesTotal() {
        int total = calculator.totalInCents(2500, 3);

        assertEquals(7500, total);
    }
}
```

The test checks one behavior: price times quantity.

## Arrange, Act, Assert

A common structure:

```java
@Test
void calculatesDiscountForLargeOrder() {
    PriceCalculator calculator = new PriceCalculator();

    int discount = calculator.discountPercent(15_000);

    assertEquals(10, discount);
}
```

- Arrange: create inputs and dependencies.
- Act: call the method under test.
- Assert: verify the result.

## Fast and Deterministic

Unit tests should be:

- fast;
- repeatable;
- independent;
- focused on behavior;
- clear when they fail.

Avoid hidden dependencies on current time, random values, network calls,
databases, and file system state unless those dependencies are controlled.

## Testing Behavior, Not Implementation

Prefer testing externally visible behavior.

Good:

```java
assertEquals(7500, calculator.totalInCents(2500, 3));
```

Weak:

```java
// Testing that a private helper was called would be implementation-focused.
```

Private methods are usually tested through public behavior.

## Practical Example

```java
public class EmailValidator {
    public boolean isValid(String email) {
        return email != null && !email.isBlank() && email.contains("@");
    }
}
```

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertTrue;

class EmailValidatorTest {
    private final EmailValidator validator = new EmailValidator();

    @Test
    void acceptsEmailWithAtSign() {
        assertTrue(validator.isValid("alex@example.com"));
    }

    @Test
    void rejectsBlankEmail() {
        assertFalse(validator.isValid(" "));
    }
}
```

The tests cover meaningful behavior without depending on implementation details.

## Common Mistakes

- Testing too many behaviors in one test.
- Writing tests that depend on execution order.
- Testing private implementation details.
- Using real network, database, or clock behavior in unit tests.
- Making assertions too vague.

## Interview Questions

1. What is a unit test?
2. What is arrange-act-assert?
3. Why should unit tests be deterministic?
4. Why should tests focus on behavior?
5. When is a unit test too broad?

## Practice

1. Write tests for a price calculator.
2. Add a test for invalid negative input.
3. Rewrite a multi-behavior test into two focused tests.
4. Identify one external dependency that should be controlled in a unit test.

## Related Topics

- [JUnit Jupiter](junit_jupiter.md)
- [Assertions](assertions.md)
- [Test Cases](test_cases.md)

