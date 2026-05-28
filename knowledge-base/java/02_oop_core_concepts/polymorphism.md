# Polymorphism

## Goal

Understand how polymorphism lets Java code work with a general type while using
different concrete implementations at runtime.

## Why It Matters

Polymorphism is everywhere in backend Java: service interfaces, repositories,
validators, strategy objects, payment processors, notification senders, and test
doubles. It helps code depend on behavior instead of specific classes.

## Basic Polymorphism

```java
public interface NotificationSender {
    void send(String recipient, String message);
}
```

```java
public class EmailSender implements NotificationSender {
    @Override
    public void send(String recipient, String message) {
        System.out.println("Email to " + recipient + ": " + message);
    }
}
```

```java
public class SmsSender implements NotificationSender {
    @Override
    public void send(String recipient, String message) {
        System.out.println("SMS to " + recipient + ": " + message);
    }
}
```

The caller can use the interface.

```java
public class AlertService {
    private final NotificationSender sender;

    public AlertService(NotificationSender sender) {
        this.sender = sender;
    }

    public void sendPasswordResetAlert(String recipient) {
        sender.send(recipient, "Your password reset was requested");
    }
}
```

`AlertService` does not care whether the sender is email, SMS, or something
else.

## Runtime Dispatch

Java chooses the overridden method based on the actual object.

```java
NotificationSender sender = new EmailSender();
sender.send("alex@example.com", "Welcome");
```

The variable type is `NotificationSender`, but the runtime object is
`EmailSender`.

## Polymorphism With Collections

```java
import java.util.List;

List<NotificationSender> senders = List.of(
        new EmailSender(),
        new SmsSender()
);

for (NotificationSender sender : senders) {
    sender.send("alex@example.com", "System maintenance tonight");
}
```

This is useful when the same operation must run across multiple implementations.

## Practical Example

```java
public interface DiscountPolicy {
    int discountPercentFor(int orderTotalInCents);
}
```

```java
public class StandardDiscountPolicy implements DiscountPolicy {
    @Override
    public int discountPercentFor(int orderTotalInCents) {
        return orderTotalInCents >= 10_000 ? 5 : 0;
    }
}
```

```java
public class VipDiscountPolicy implements DiscountPolicy {
    @Override
    public int discountPercentFor(int orderTotalInCents) {
        return orderTotalInCents >= 10_000 ? 15 : 10;
    }
}
```

```java
public class CheckoutService {
    private final DiscountPolicy discountPolicy;

    public CheckoutService(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }

    public int finalTotalInCents(int orderTotalInCents) {
        int discount = discountPolicy.discountPercentFor(orderTotalInCents);
        return orderTotalInCents - (orderTotalInCents * discount / 100);
    }
}
```

The discount rule can change without rewriting `CheckoutService`.

## Common Mistakes

- Using `if` or `switch` on type names instead of polymorphism.
- Depending on concrete classes when an interface would be enough.
- Creating too many tiny interfaces before there is a real variation point.
- Returning broad types like `Object` instead of meaningful abstractions.
- Hiding important behavior behind unclear method names.

## Interview Questions

1. What is polymorphism?
2. What is runtime method dispatch?
3. How do interfaces support polymorphism?
4. How does polymorphism improve testability?
5. When can polymorphism be overengineering?

## Practice

1. Create a `PaymentProcessor` interface.
2. Implement `CardPaymentProcessor` and `BankTransferPaymentProcessor`.
3. Write a `PaymentService` that depends only on the interface.
4. Add a fake implementation that can be used in a simple test.

## Related Topics

- [Interfaces](interfaces.md)
- [Inheritance](inheritance.md)
- [Abstract Classes](abstract_classes.md)

