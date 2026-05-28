# Encapsulation

## Goal

Understand how encapsulation protects object state and keeps business rules in
one place.

## Why It Matters

Encapsulation is not only about writing `private` fields and getters. It is
about preventing invalid changes from spreading through the codebase. Good
encapsulation makes classes easier to test, refactor, and use safely.

## Basic Encapsulation

Keep fields private and expose behavior through methods.

```java
public class UserAccount {
    private final String email;
    private boolean locked;

    public UserAccount(String email) {
        if (email == null || email.isBlank()) {
            throw new IllegalArgumentException("email must not be blank");
        }

        this.email = email;
    }

    public String email() {
        return email;
    }

    public boolean isLocked() {
        return locked;
    }

    public void lock() {
        locked = true;
    }

    public void unlock() {
        locked = false;
    }
}
```

Callers can read the account state, but they cannot directly change fields.

## Avoid Public Mutable State

This is risky:

```java
public class UserAccount {
    public String email;
    public boolean locked;
}
```

Any caller can put the object into an invalid state.

```java
account.email = "";
account.locked = false;
```

The class loses control over its own rules.

## Getters Are Not Always Enough

Do not create setters automatically. A setter can still allow invalid state.

```java
public void setEmail(String email) {
    this.email = email;
}
```

Prefer behavior that describes a business action.

```java
public void changeEmail(String newEmail) {
    if (newEmail == null || newEmail.isBlank()) {
        throw new IllegalArgumentException("newEmail must not be blank");
    }

    this.email = newEmail;
}
```

The method name and validation make the allowed change explicit.

## Defensive Copies

Be careful with mutable collections.

```java
import java.util.ArrayList;
import java.util.List;

public class Order {
    private final List<String> itemIds;

    public Order(List<String> itemIds) {
        if (itemIds == null || itemIds.isEmpty()) {
            throw new IllegalArgumentException("order must have at least one item");
        }

        this.itemIds = new ArrayList<>(itemIds);
    }

    public List<String> itemIds() {
        return List.copyOf(itemIds);
    }
}
```

The constructor copies input data, and the accessor returns an unmodifiable copy.
Callers cannot mutate the order's internal list accidentally.

## Practical Example

```java
public class FailedLoginCounter {
    private static final int MAX_ATTEMPTS = 5;

    private int attempts;

    public void recordFailure() {
        if (!isLocked()) {
            attempts++;
        }
    }

    public void reset() {
        attempts = 0;
    }

    public boolean isLocked() {
        return attempts >= MAX_ATTEMPTS;
    }

    public int attempts() {
        return attempts;
    }
}
```

The object owns the rule for when an account becomes locked. Callers do not need
to know the threshold.

## Common Mistakes

- Making fields public for convenience.
- Generating setters for every field without thinking about valid transitions.
- Returning internal mutable collections directly.
- Splitting business rules across callers instead of keeping them in the class.
- Confusing encapsulation with hiding everything; useful behavior should still
  be easy to call.

## Interview Questions

1. What is encapsulation?
2. Why are public fields risky?
3. Why can automatic setters weaken a class design?
4. What is a defensive copy?
5. How does encapsulation help with testing?

## Practice

1. Create an `Order` class that cannot be created without items.
2. Add a method that cancels the order only if it has not shipped.
3. Store item IDs in a private list.
4. Return the item IDs without exposing the internal mutable list.

## Related Topics

- [Classes and Objects](classes_objects.md)
- [Methods](../01_language_basics/methods.md)
- [Inheritance](inheritance.md)

