# Simple Service Layer

## Problem

Create a service that validates input, checks a repository, saves a user, and
sends a notification through an interface.

## Why This Example Matters

Service classes are common in Java backend code. A good service keeps business
rules readable and depends on interfaces at external boundaries.

## Code

```java
public class RegistrationService {
    private final UserRepository userRepository;
    private final WelcomeNotifier welcomeNotifier;

    public RegistrationService(UserRepository userRepository, WelcomeNotifier welcomeNotifier) {
        this.userRepository = userRepository;
        this.welcomeNotifier = welcomeNotifier;
    }

    public User register(String email) {
        if (email == null || email.isBlank() || !email.contains("@")) {
            throw new IllegalArgumentException("email must be valid");
        }

        String normalizedEmail = email.trim().toLowerCase();

        if (userRepository.existsByEmail(normalizedEmail)) {
            throw new DuplicateUserException("user already exists: " + normalizedEmail);
        }

        User user = new User(normalizedEmail, true);
        userRepository.save(user);
        welcomeNotifier.sendWelcomeMessage(normalizedEmail);

        return user;
    }

    public record User(String email, boolean active) {
    }
}

interface UserRepository {
    boolean existsByEmail(String email);
    void save(RegistrationService.User user);
}

interface WelcomeNotifier {
    void sendWelcomeMessage(String email);
}

class DuplicateUserException extends RuntimeException {
    DuplicateUserException(String message) {
        super(message);
    }
}
```

## What It Demonstrates

- constructor dependency injection;
- interfaces for external boundaries;
- validation before persistence;
- normalized data;
- custom business exception;
- testable service design.

## Practice

1. Add a `Clock` dependency and store creation time.
2. Write tests with a mocked repository and notifier.
3. Add a rule that blocks specific email domains.
4. Split email validation into a separate class if it grows.

