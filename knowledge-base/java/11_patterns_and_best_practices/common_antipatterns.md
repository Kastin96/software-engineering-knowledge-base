# Common Antipatterns

## Goal

Recognize Java code shapes that often make systems harder to maintain.

## Why It Matters

Antipatterns are not always bugs. They are design choices that tend to create
future bugs, slow changes, and confusing tests. Spotting them early helps you
refactor before the code becomes expensive to change.

## God Class

A class that does too much.

```java
class UserManager {
    void validate() {}
    void saveToDatabase() {}
    void sendEmail() {}
    void generateReport() {}
    void audit() {}
}
```

Split by responsibility: validation, persistence, notification, reporting, and
audit.

## Utility Dump

```java
class AppUtils {
    static String formatEmail(String email) {}
    static int calculateTax(int cents) {}
    static void sendAuditLog(String message) {}
}
```

Large utility classes hide domain concepts. Prefer focused classes with clear
names.

## Primitive Obsession

Using raw strings and integers for meaningful concepts.

```java
void pay(String userId, String orderId, int amount) {
}
```

Better:

```java
record UserId(String value) {}
record OrderId(String value) {}
record Money(int cents, String currency) {}
```

Meaningful types reduce accidental parameter swaps.

## Boolean Parameter Trap

```java
sendEmail(user, true);
```

What does `true` mean?

Prefer named methods or types.

```java
sendWelcomeEmail(user);
sendPasswordResetEmail(user);
```

## Catch-All Exception Handling

```java
try {
    process();
} catch (Exception exception) {
    return;
}
```

This hides failures. Handle specific exceptions at useful boundaries.

## Over-Mocking

Tests that mock every collaborator and verify every internal call are brittle.

Prefer real value objects and verify meaningful outcomes.

## Practical Example

Before:

```java
class OrderProcessor {
    void process(String userId, String orderId, int cents, boolean email) {
        // validation, payment, persistence, email, audit
    }
}
```

After:

```java
record UserId(String value) {}
record OrderId(String value) {}
record Money(int cents, String currency) {}

class OrderPaymentService {
    void pay(UserId userId, OrderId orderId, Money amount) {
        // focused payment behavior
    }
}
```

The refactor introduces meaningful types and a focused service.

## Common Mistakes

- Keeping a god class because splitting it feels inconvenient.
- Calling everything a utility.
- Passing many primitives instead of modeling concepts.
- Catching broad exceptions to make errors disappear.
- Adding patterns that create more complexity than they remove.

## Interview Questions

1. What is a god class?
2. What is primitive obsession?
3. Why are boolean parameters often unclear?
4. Why is catch-all exception handling risky?
5. How can tests reveal antipatterns?

## Practice

1. Split a god class into two focused classes.
2. Replace two string IDs with records.
3. Replace a boolean parameter with two named methods.
4. Replace a catch-all block with specific exception handling.

## Related Topics

- [Clean Code in Java](clean_code_java.md)
- [Immutability](immutability.md)
- [Testing Best Practices](../10_testing/testing_best_practices.md)

