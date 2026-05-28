# Interfaces

## Goal

Understand how Java interfaces define contracts and support replaceable
implementations.

## Why It Matters

Interfaces are one of the most practical tools in Java application design. They
let services depend on behavior, not on concrete classes. This helps with
testing, swapping infrastructure, and keeping business logic separate from
technical details.

## Basic Interface

```java
public interface EmailClient {
    void sendEmail(String recipient, String subject, String body);
}
```

A class implements the interface.

```java
public class ConsoleEmailClient implements EmailClient {
    @Override
    public void sendEmail(String recipient, String subject, String body) {
        System.out.println("To: " + recipient);
        System.out.println("Subject: " + subject);
        System.out.println(body);
    }
}
```

The interface defines what can be done. The implementation defines how it is
done.

## Interfaces as Dependencies

```java
public class WelcomeService {
    private final EmailClient emailClient;

    public WelcomeService(EmailClient emailClient) {
        this.emailClient = emailClient;
    }

    public void welcome(String email) {
        emailClient.sendEmail(email, "Welcome", "Thanks for signing up.");
    }
}
```

`WelcomeService` is easier to test because it does not create or depend on a
specific email client.

## Default Methods

Interfaces can have default methods.

```java
public interface TextFormatter {
    String format(String value);

    default String formatOrEmpty(String value) {
        if (value == null) {
            return "";
        }

        return format(value);
    }
}
```

Use default methods carefully. They are useful for shared interface behavior,
but too many default methods can turn an interface into a hidden base class.

## Static Methods

Interfaces can also contain static helper methods.

```java
public interface EmailRules {
    static boolean looksValid(String email) {
        return email != null && email.contains("@") && !email.isBlank();
    }
}
```

Use this for small helpers closely related to the interface concept.

## Functional Interfaces

A functional interface has one abstract method. It can be used with lambdas.

```java
@FunctionalInterface
public interface UserFilter {
    boolean matches(String email);
}
```

```java
UserFilter companyEmail = email -> email.endsWith("@example.com");
System.out.println(companyEmail.matches("alex@example.com")); // true
```

Java already has many standard functional interfaces, such as `Predicate`,
`Function`, `Supplier`, and `Consumer`.

## Practical Example

```java
public interface UserRepository {
    boolean existsByEmail(String email);
    void save(String email);
}
```

```java
public class RegistrationService {
    private final UserRepository userRepository;

    public RegistrationService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public void register(String email) {
        if (email == null || email.isBlank()) {
            throw new IllegalArgumentException("email must not be blank");
        }

        if (userRepository.existsByEmail(email)) {
            throw new IllegalStateException("user already exists");
        }

        userRepository.save(email);
    }
}
```

The service owns the registration rule. The repository interface hides the
storage mechanism.

## Common Mistakes

- Creating interfaces for every class without a real need.
- Naming interfaces with vague words like `Manager` or `Handler`.
- Putting unrelated methods into one large interface.
- Using default methods as a dumping ground for shared logic.
- Depending on concrete implementations when an interface is already available.

## Interview Questions

1. What is an interface?
2. How is an interface different from a class?
3. Why are interfaces useful for testing?
4. What is a functional interface?
5. When is creating an interface unnecessary?

## Practice

1. Create a `NotificationSender` interface.
2. Implement `EmailNotificationSender` and `ConsoleNotificationSender`.
3. Write a service that depends on `NotificationSender`.
4. Create a functional interface for validating a string.

## Related Topics

- [Polymorphism](polymorphism.md)
- [Abstract Classes](abstract_classes.md)
- [Methods](../01_language_basics/methods.md)

