# Test Structure and Naming

## Goal

Learn how to structure and name tests so failures are easy to understand.

## Why It Matters

Tests are read more often than they are written. Clear test names, focused setup,
and simple structure make the test suite easier to trust and maintain.

## Test Class Names

Name test classes after the class or behavior being tested.

```java
class PriceCalculatorTest {
}
```

For broader behavior:

```java
class UserRegistrationTest {
}
```

## Test Method Names

Use names that describe behavior.

```java
@Test
void rejectsDuplicateEmail() {
}

@Test
void calculatesDiscountAtThreshold() {
}
```

Avoid names like:

```java
@Test
void test1() {
}
```

## Arrange, Act, Assert Layout

```java
@Test
void createsActiveUser() {
    RegistrationService service = new RegistrationService();
    String email = "alex@example.com";

    User user = service.register(email);

    assertEquals(email, user.email());
    assertTrue(user.active());
}
```

Whitespace can separate the three phases.

## Keep Setup Local When Possible

If setup is short and specific, keep it in the test.

```java
@Test
void rejectsBlankEmail() {
    RegistrationService service = new RegistrationService();

    assertThrows(
            IllegalArgumentException.class,
            () -> service.register(" ")
    );
}
```

Use `@BeforeEach` for shared setup only when it removes real duplication without
hiding important test data.

## Builders and Helpers

Use helpers for noisy object creation.

```java
private User activeUser(String email) {
    return new User(email, true);
}
```

Keep helpers obvious. If a helper hides too much, readers cannot understand the
test.

## Practical Example

```java
class OrderServiceTest {
    @Test
    void rejectsPaymentForAlreadyPaidOrder() {
        Order order = paidOrder("o-100");
        OrderService service = new OrderService();

        InvalidOrderStateException exception = assertThrows(
                InvalidOrderStateException.class,
                () -> service.pay(order)
        );

        assertEquals("order is already paid: o-100", exception.getMessage());
    }

    private Order paidOrder(String id) {
        Order order = new Order(id);
        order.markPaid();
        return order;
    }
}
```

The test name states the behavior. The helper removes setup noise but keeps the
state understandable.

## Common Mistakes

- Naming tests after implementation details.
- Hiding important test data in large setup methods.
- Combining many behaviors in one test.
- Using helpers that make tests harder to read.
- Writing assertions far away from the action.

## Interview Questions

1. What makes a good test name?
2. Why is arrange-act-assert useful?
3. When should setup stay inside a test method?
4. When are test helpers useful?
5. Why should one test usually focus on one behavior?

## Practice

1. Rename vague test methods to behavior-focused names.
2. Split one large test into two focused tests.
3. Move noisy object creation into a small helper.
4. Remove a shared setup method that hides important data.

## Related Topics

- [Unit Testing](unit_testing.md)
- [Assertions](assertions.md)
- [Mocking](mocking.md)

