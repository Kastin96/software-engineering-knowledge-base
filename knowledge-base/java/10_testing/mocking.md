# Mocking

## Goal

Understand when to use mocks and how to keep mocked tests focused on behavior.

## Why It Matters

Mocks are useful when a unit depends on external behavior such as a repository,
email client, payment gateway, clock, or remote API. They are harmful when they
make tests verify implementation details instead of meaningful outcomes.

## What Is a Mock?

A mock is a test double that can provide controlled responses and verify
interactions.

Example dependency:

```java
public interface UserRepository {
    boolean existsByEmail(String email);
    void save(User user);
}
```

Service:

```java
public class RegistrationService {
    private final UserRepository repository;

    public RegistrationService(UserRepository repository) {
        this.repository = repository;
    }

    public void register(String email) {
        if (repository.existsByEmail(email)) {
            throw new DuplicateUserException("user already exists");
        }

        repository.save(new User(email));
    }
}
```

## Mockito Example

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.never;
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;

class RegistrationServiceTest {
    private final UserRepository repository = mock(UserRepository.class);
    private final RegistrationService service = new RegistrationService(repository);

    @Test
    void rejectsDuplicateEmail() {
        when(repository.existsByEmail("alex@example.com")).thenReturn(true);

        assertThrows(
                DuplicateUserException.class,
                () -> service.register("alex@example.com")
        );

        verify(repository, never()).save(org.mockito.ArgumentMatchers.any());
    }
}
```

The mock controls the repository result and verifies that duplicate users are not
saved.

## Mock Behavior, Not Everything

Mock external dependencies. Do not mock simple value objects.

Good candidates:

- repositories;
- HTTP clients;
- email clients;
- payment gateways;
- clocks;
- message publishers.

Poor candidates:

- records;
- DTOs;
- simple collections;
- the class under test.

## State vs Interaction

Prefer state assertions when possible.

Use interaction verification when the behavior is the interaction itself, such
as sending an email or publishing an event.

```java
verify(emailClient).sendWelcomeEmail("alex@example.com");
```

Do not verify every internal method call.

## Practical Example

```java
public class PasswordResetService {
    private final UserRepository repository;
    private final EmailClient emailClient;

    public PasswordResetService(UserRepository repository, EmailClient emailClient) {
        this.repository = repository;
        this.emailClient = emailClient;
    }

    public void requestReset(String email) {
        if (!repository.existsByEmail(email)) {
            return;
        }

        emailClient.sendPasswordReset(email);
    }
}
```

```java
@Test
void sendsResetEmailForKnownUser() {
    when(repository.existsByEmail("alex@example.com")).thenReturn(true);

    service.requestReset("alex@example.com");

    verify(emailClient).sendPasswordReset("alex@example.com");
}
```

The important behavior is the email interaction.

## Common Mistakes

- Mocking the class under test.
- Verifying every internal call.
- Mocking simple data objects.
- Creating tests that mirror implementation too closely.
- Using mocks when an in-memory fake would be simpler.

## Interview Questions

1. What is a mock?
2. When should you use a mock?
3. What is the difference between stubbing and verification?
4. Why should you not mock value objects?
5. When is an in-memory fake better than a mock?

## Practice

1. Mock a repository dependency.
2. Stub a duplicate-email check.
3. Verify an email client was called.
4. Remove unnecessary interaction verification from a test.

## Related Topics

- [Unit Testing](unit_testing.md)
- [Test Structure and Naming](test_structure_naming.md)
- [Interfaces](../02_oop_core_concepts/interfaces.md)

