# Test Cases

## Goal

Learn how to choose useful test cases instead of only testing the happy path.

## Why It Matters

The value of a test suite depends heavily on the examples it checks. Good test
cases cover normal behavior, boundaries, invalid input, and important business
rules without trying to test every possible value.

## Common Test Case Categories

For a method, consider:

- normal case;
- boundary values;
- invalid input;
- empty input;
- null input when allowed or expected;
- duplicates;
- ordering;
- error cases;
- business-rule transitions.

## Example: Discount Rule

```java
public int discountPercent(int totalInCents) {
    if (totalInCents < 0) {
        throw new IllegalArgumentException("totalInCents must not be negative");
    }

    if (totalInCents >= 10_000) {
        return 10;
    }

    return 0;
}
```

Useful cases:

- `0` returns `0`;
- `9_999` returns `0`;
- `10_000` returns `10`;
- negative input throws.

The boundary is more important than a random value like `4_321`.

## Equivalence Classes

Group inputs that should behave the same way.

For adult validation:

```java
age >= 18 -> adult
age < 18 -> not adult
age < 0 -> invalid
```

Choose representative examples from each group.

## Table-Like Tests

Parameterized tests are useful for multiple examples of the same rule.

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

import static org.junit.jupiter.api.Assertions.assertEquals;

class DiscountPolicyTest {
    private final DiscountPolicy policy = new DiscountPolicy();

    @ParameterizedTest
    @CsvSource({
            "0, 0",
            "9999, 0",
            "10000, 10",
            "15000, 10"
    })
    void calculatesDiscount(int totalInCents, int expectedDiscount) {
        assertEquals(expectedDiscount, policy.discountPercent(totalInCents));
    }
}
```

## Practical Example

```java
class EmailValidator {
    boolean isValid(String email) {
        return email != null
                && !email.isBlank()
                && email.contains("@")
                && !email.startsWith("@")
                && !email.endsWith("@");
    }
}
```

Useful test cases:

- valid email;
- `null`;
- blank string;
- missing `@`;
- starts with `@`;
- ends with `@`.

These cases directly map to behavior.

## Common Mistakes

- Testing only the happy path.
- Choosing random values instead of boundary values.
- Adding many redundant cases that test the same equivalence class.
- Forgetting invalid input.
- Writing tests without understanding the business rule.

## Interview Questions

1. What is a boundary value?
2. What is an equivalence class?
3. Why is `10_000` more important than `12_345` in a threshold rule?
4. When are parameterized tests useful?
5. How do you avoid redundant test cases?

## Practice

1. Choose test cases for an age validation method.
2. Choose test cases for a pagination method.
3. Convert repeated examples into a parameterized test.
4. Remove redundant cases from an overgrown test suite.

## Related Topics

- [Unit Testing](unit_testing.md)
- [JUnit Jupiter](junit_jupiter.md)
- [Assertions](assertions.md)

