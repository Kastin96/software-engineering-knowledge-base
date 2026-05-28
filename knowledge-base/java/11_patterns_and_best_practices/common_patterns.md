# Common Java Patterns

## Goal

Understand common Java patterns that are useful in everyday application code.

## Why It Matters

Patterns are names for recurring design shapes. They help communication and
reduce repeated problem-solving. They are helpful when they match a real problem
and harmful when added just to make code look sophisticated.

## Strategy

Use Strategy when an algorithm or rule varies.

```java
interface DiscountPolicy {
    int discountPercent(Order order);
}

class VipDiscountPolicy implements DiscountPolicy {
    public int discountPercent(Order order) {
        return 15;
    }
}
```

The caller depends on the interface.

```java
class CheckoutService {
    private final DiscountPolicy discountPolicy;

    CheckoutService(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }
}
```

## Factory Method

Use a factory when object creation has meaningful logic.

```java
class PaymentClientFactory {
    PaymentClient create(String provider) {
        return switch (provider) {
            case "stripe" -> new StripePaymentClient();
            case "mock" -> new MockPaymentClient();
            default -> throw new IllegalArgumentException("unknown provider: " + provider);
        };
    }
}
```

Do not add a factory when `new` is clear and stable.

## Builder

Use a builder for objects with many optional fields or readable test data setup.

```java
class UserBuilder {
    private String email = "alex@example.com";
    private boolean active = true;

    UserBuilder email(String email) {
        this.email = email;
        return this;
    }

    UserBuilder inactive() {
        this.active = false;
        return this;
    }

    User build() {
        return new User(email, active);
    }
}
```

For small records with two or three fields, a builder is usually unnecessary.

## Adapter

Use Adapter to hide an external API behind your own interface.

```java
interface EmailSender {
    void send(String recipient, String subject, String body);
}

class ExternalEmailSenderAdapter implements EmailSender {
    private final ExternalEmailClient client;

    ExternalEmailSenderAdapter(ExternalEmailClient client) {
        this.client = client;
    }

    public void send(String recipient, String subject, String body) {
        client.deliver(recipient, subject, body);
    }
}
```

Your business code depends on `EmailSender`, not the external client.

## Template Method

Use Template Method when a workflow is fixed but steps vary.

```java
abstract class ImportJob {
    final void run() {
        validate();
        importData();
        publishSummary();
    }

    protected void validate() {
    }

    protected abstract void importData();

    private void publishSummary() {
    }
}
```

Use carefully. Composition is often more flexible than inheritance.

## Common Mistakes

- Adding patterns before the problem exists.
- Using singletons for global mutable state.
- Creating factories that only call one constructor.
- Building inheritance-heavy template hierarchies.
- Naming something `Strategy` or `Factory` without a clear responsibility.

## Interview Questions

1. What problem does Strategy solve?
2. When is a factory useful?
3. When is a builder unnecessary?
4. What does Adapter protect your code from?
5. Why can patterns become overengineering?

## Practice

1. Replace a discount `switch` with Strategy.
2. Add an adapter around an external client.
3. Create a test data builder for a noisy object.
4. Remove a factory that does not add value.

## Related Topics

- [SOLID Principles in Practice](solid_principles.md)
- [Common Antipatterns](common_antipatterns.md)
- [Interfaces](../02_oop_core_concepts/interfaces.md)

