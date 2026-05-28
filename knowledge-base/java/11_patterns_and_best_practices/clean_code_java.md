# Clean Code in Java

## Goal

Learn practical Java habits that make code easier to read and maintain.

## Why It Matters

Most professional Java work is changing existing code. Clear names, focused
methods, small classes, and explicit boundaries reduce the cost of change and
make bugs easier to spot.

## Clear Names

Prefer names that explain business meaning.

```java
int failedLoginAttempts = 3;
boolean accountLocked = failedLoginAttempts >= 5;
```

Avoid vague names:

```java
int x = 3;
boolean flag = x >= 5;
```

## Focused Methods

Keep methods focused on one level of abstraction.

```java
public void register(String email, int age) {
    validateSignup(email, age);
    User user = createUser(email);
    userRepository.save(user);
    welcomeEmailSender.sendTo(email);
}
```

Each line explains a step. Details can live in smaller methods.

## Avoid Deep Nesting

Use guard clauses for invalid or special cases.

```java
public String displayName(User user) {
    if (user == null) {
        return "Guest";
    }

    if (user.name().isBlank()) {
        return user.email();
    }

    return user.name();
}
```

This is easier to read than nested `if` blocks.

## Prefer Meaningful Types

Instead of passing several unrelated strings:

```java
sendEmail(String email, String subject, String body);
```

consider a value object when the data travels together:

```java
record EmailMessage(String recipient, String subject, String body) {
}
```

Meaningful types reduce parameter confusion.

## Practical Example

Before:

```java
void p(User u) {
    if (u != null) {
        if (u.active()) {
            if (u.email().contains("@")) {
                sender.send(u.email());
            }
        }
    }
}
```

After:

```java
void sendWelcomeEmail(User user) {
    if (!canReceiveWelcomeEmail(user)) {
        return;
    }

    sender.send(user.email());
}

boolean canReceiveWelcomeEmail(User user) {
    return user != null && user.active() && user.email().contains("@");
}
```

The refactored version names the business rule.

## Common Mistakes

- Short names that hide meaning.
- Methods that validate, calculate, persist, and notify all at once.
- Comments explaining confusing code instead of improving the code.
- Boolean flags that change method behavior in unrelated ways.
- Large utility classes with mixed responsibilities.

## Interview Questions

1. What makes a method readable?
2. Why are guard clauses useful?
3. When should you introduce a value object?
4. Why can comments be a smell?
5. What is a code smell?

## Practice

1. Rename vague variables in a small method.
2. Split a long method into named steps.
3. Replace nested conditionals with guard clauses.
4. Introduce a record for data that travels together.

## Related Topics

- [Refactoring Habits](refactoring_habits.md)
- [Code Organization](code_organization.md)
- [Testing](../10_testing/README.md)

