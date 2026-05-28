# Java Testing Cheatsheet

## Basic JUnit Test

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class CalculatorTest {
    @Test
    void addsTwoNumbers() {
        assertEquals(5, new Calculator().add(2, 3));
    }
}
```

## Arrange, Act, Assert

```java
@Test
void calculatesTotal() {
    PriceCalculator calculator = new PriceCalculator();

    int total = calculator.totalInCents(2500, 3);

    assertEquals(7500, total);
}
```

## Exception Assertion

```java
IllegalArgumentException exception = assertThrows(
        IllegalArgumentException.class,
        () -> calculator.totalInCents(-1, 2)
);

assertEquals("unitPriceInCents must not be negative", exception.getMessage());
```

## Parameterized Test

```java
@ParameterizedTest
@CsvSource({
        "0, 0",
        "10000, 10"
})
void calculatesDiscount(int total, int expected) {
    assertEquals(expected, policy.discountPercent(total));
}
```

## Mockito Shape

```java
UserRepository repository = mock(UserRepository.class);
when(repository.existsByEmail("alex@example.com")).thenReturn(true);

assertThrows(DuplicateUserException.class, () -> service.register("alex@example.com"));

verify(repository, never()).save(any());
```

## Good Test Habits

- Test behavior, not private implementation.
- Keep tests deterministic.
- Use real value objects.
- Mock external dependencies.
- Name tests after expected behavior.
- Keep one main behavior per test.

