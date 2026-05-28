# Classes and Objects

## Goal

Understand how Java classes define structure and behavior, and how objects
represent actual runtime instances.

## Why It Matters

Most Java backend code is organized around classes: controllers, services,
repositories, DTOs, configuration objects, exceptions, and domain models. Even
when a framework creates the objects for you, you still need to design classes
that are readable, testable, and hard to misuse.

## Class vs Object

A class is a blueprint. An object is an instance created from that blueprint.

```java
public class User {
    private final String email;
    private boolean active;

    public User(String email, boolean active) {
        this.email = email;
        this.active = active;
    }

    public String email() {
        return email;
    }

    public boolean isActive() {
        return active;
    }

    public void deactivate() {
        active = false;
    }
}
```

```java
User user = new User("alex@example.com", true);

System.out.println(user.email());
System.out.println(user.isActive());
```

`User` is the class. `user` is an object.

## Fields

Fields store object state.

```java
private final String email;
private boolean active;
```

Use `private` by default. Expose state through meaningful methods instead of
making fields public.

Use `final` when a field should be assigned once and never point to another
value.

## Constructors

Constructors create valid objects.

```java
public User(String email, boolean active) {
    if (email == null || email.isBlank()) {
        throw new IllegalArgumentException("email must not be blank");
    }

    this.email = email;
    this.active = active;
}
```

A constructor should not allow an object to start in an invalid state.

## Behavior

Objects should expose behavior, not just data.

```java
public boolean canLogin() {
    return active && email.endsWith("@example.com");
}
```

This is better than forcing every caller to repeat the same condition.

## Records

Records are useful for simple immutable data carriers.

```java
public record UserSummary(String email, boolean active) {
}
```

Records automatically provide a constructor, accessors, `equals`, `hashCode`,
and `toString`.

Use records for value-like data such as DTOs, query results, and simple
responses. Avoid records when the object needs rich behavior, complex invariants,
or controlled mutation.

## Practical Example

```java
public class BankAccount {
    private final String accountNumber;
    private int balanceInCents;

    public BankAccount(String accountNumber, int openingBalanceInCents) {
        if (accountNumber == null || accountNumber.isBlank()) {
            throw new IllegalArgumentException("accountNumber must not be blank");
        }

        if (openingBalanceInCents < 0) {
            throw new IllegalArgumentException("opening balance must not be negative");
        }

        this.accountNumber = accountNumber;
        this.balanceInCents = openingBalanceInCents;
    }

    public void deposit(int amountInCents) {
        if (amountInCents <= 0) {
            throw new IllegalArgumentException("deposit amount must be positive");
        }

        balanceInCents += amountInCents;
    }

    public int balanceInCents() {
        return balanceInCents;
    }
}
```

This class keeps state private and forces all balance changes through validated
behavior.

## Common Mistakes

- Creating classes that only expose public fields.
- Allowing constructors to create invalid objects.
- Putting unrelated responsibilities into one class.
- Naming classes after technical actions only, such as `Processor`, without
  clarifying the business concept.
- Using a mutable class where an immutable record would be enough.

## Interview Questions

1. What is the difference between a class and an object?
2. What is the role of a constructor?
3. Why are fields usually private?
4. When is a Java record a good choice?
5. What does it mean for an object to have behavior?

## Practice

1. Create a `Product` class with name, price, and active status.
2. Validate constructor arguments.
3. Add a method that marks the product inactive.
4. Create a `ProductSummary` record for read-only display data.

## Related Topics

- [Encapsulation](encapsulation.md)
- [Methods](../01_language_basics/methods.md)
- [`equals`, `hashCode`, and `toString`](equals_hashcode_tostring.md)

