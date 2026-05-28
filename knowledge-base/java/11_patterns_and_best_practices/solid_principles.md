# SOLID Principles in Practice

## Goal

Understand SOLID as practical design heuristics, not as rules to apply blindly.

## Why It Matters

SOLID principles help Java code stay easier to change, test, and extend. They
are common interview topics, but the useful skill is recognizing the design
pressure in real code.

## Single Responsibility

A class should have one main reason to change.

```java
class InvoiceCalculator {
    int totalInCents(Invoice invoice) {
        return invoice.items().stream()
                .mapToInt(InvoiceItem::totalInCents)
                .sum();
    }
}
```

Do not mix calculation, persistence, email sending, and PDF generation in one
class.

## Open/Closed

Code should be open for extension but closed for modification where variation is
expected.

```java
interface DiscountPolicy {
    int discountPercent(Order order);
}
```

```java
class CheckoutService {
    private final DiscountPolicy discountPolicy;

    CheckoutService(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }
}
```

New discount policies can be added without rewriting checkout logic.

## Liskov Substitution

Subtypes should be usable wherever the parent type is expected without breaking
expectations.

If a subclass overrides a method to throw unsupported exceptions for normal
parent behavior, the hierarchy may be wrong.

Prefer interfaces or composition when inheritance becomes awkward.

## Interface Segregation

Prefer focused interfaces.

```java
interface EmailSender {
    void sendEmail(String recipient, String subject, String body);
}
```

Avoid forcing classes to implement methods they do not need.

## Dependency Inversion

High-level policy should not depend directly on low-level infrastructure.

```java
class RegistrationService {
    private final UserRepository repository;

    RegistrationService(UserRepository repository) {
        this.repository = repository;
    }
}
```

The service depends on an interface, not a database implementation.

## Practical Example

```java
interface NotificationSender {
    void send(String recipient, String message);
}

class PasswordResetService {
    private final NotificationSender notificationSender;

    PasswordResetService(NotificationSender notificationSender) {
        this.notificationSender = notificationSender;
    }

    void requestReset(String email) {
        notificationSender.send(email, "Password reset requested");
    }
}
```

The service is easy to test and does not know whether the notification is email,
SMS, or something else.

## Common Mistakes

- Creating interfaces for every class without a real variation point.
- Using inheritance when composition would be simpler.
- Treating SOLID as a checklist instead of a design aid.
- Splitting code into tiny classes with no clear benefit.
- Depending on infrastructure details in core business logic.

## Interview Questions

1. What does single responsibility mean?
2. What is dependency inversion in practical Java code?
3. Why can inheritance violate Liskov substitution?
4. What is interface segregation?
5. When can applying SOLID go too far?

## Practice

1. Split a class that calculates, saves, and emails into focused classes.
2. Introduce an interface for a real external dependency.
3. Replace an inheritance hierarchy with composition.
4. Explain which SOLID principle helped each refactor.

## Related Topics

- [Interfaces](../02_oop_core_concepts/interfaces.md)
- [Common Java Patterns](common_patterns.md)
- [Refactoring Habits](refactoring_habits.md)

