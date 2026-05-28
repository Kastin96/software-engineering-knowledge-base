# Inheritance

## Goal

Understand how Java inheritance works, when it is useful, and why it should be
used carefully.

## Why It Matters

Inheritance is a powerful OOP tool, but it creates tight coupling between a
parent class and child classes. In modern Java application code, composition and
interfaces are often safer than deep inheritance hierarchies.

## Basic Inheritance

Use `extends` when one class is a specialized form of another class.

```java
public class Notification {
    private final String recipient;

    public Notification(String recipient) {
        this.recipient = recipient;
    }

    public String recipient() {
        return recipient;
    }
}
```

```java
public class EmailNotification extends Notification {
    private final String subject;

    public EmailNotification(String recipient, String subject) {
        super(recipient);
        this.subject = subject;
    }

    public String subject() {
        return subject;
    }
}
```

`EmailNotification` inherits from `Notification`.

## Overriding Methods

A child class can override a parent method.

```java
public class Notification {
    public String channel() {
        return "generic";
    }
}
```

```java
public class SmsNotification extends Notification {
    @Override
    public String channel() {
        return "sms";
    }
}
```

Always use `@Override`. It lets the compiler catch mistakes in method names or
signatures.

## The `final` Keyword

Use `final` on a class to prevent inheritance.

```java
public final class Money {
    private final int cents;

    public Money(int cents) {
        this.cents = cents;
    }
}
```

Use `final` when subclasses would make the class harder to reason about or when
the class represents a stable value concept.

## Prefer Composition for Reused Behavior

Do not use inheritance only to reuse code. Composition is often clearer.

```java
public class AuditLogger {
    public void log(String message) {
        System.out.println("[AUDIT] " + message);
    }
}
```

```java
public class UserService {
    private final AuditLogger auditLogger;

    public UserService(AuditLogger auditLogger) {
        this.auditLogger = auditLogger;
    }

    public void deactivateUser(String email) {
        auditLogger.log("Deactivated user " + email);
    }
}
```

`UserService` uses an `AuditLogger`. It does not need to inherit from it.

## Practical Example

```java
public abstract class PaymentMethod {
    public abstract boolean supportsCurrency(String currency);
}
```

```java
public class CardPaymentMethod extends PaymentMethod {
    @Override
    public boolean supportsCurrency(String currency) {
        return "USD".equals(currency) || "EUR".equals(currency);
    }
}
```

This inheritance is reasonable because each subclass is a kind of payment
method, and the parent defines a common contract.

For many real services, an interface would be even more flexible. The best
choice depends on whether shared state or shared implementation is truly needed.

## Common Mistakes

- Creating deep inheritance trees.
- Using inheritance only to share helper methods.
- Overriding methods in ways that break parent class expectations.
- Forgetting `@Override`.
- Making a class extensible without designing it for extension.

## Interview Questions

1. What does `extends` do?
2. What is method overriding?
3. Why should `@Override` be used?
4. Why is composition often preferred over inheritance?
5. When would you make a class `final`?

## Practice

1. Create a base `Notification` class and two specialized notification classes.
2. Override a method that returns the notification channel.
3. Refactor one reused helper into a composed dependency instead of a parent
   class.
4. Add `final` to a value-like class and explain why inheritance is blocked.

## Related Topics

- [Polymorphism](polymorphism.md)
- [Interfaces](interfaces.md)
- [Abstract Classes](abstract_classes.md)

