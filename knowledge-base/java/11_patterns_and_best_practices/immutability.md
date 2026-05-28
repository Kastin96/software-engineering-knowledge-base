# Immutability

## Goal

Understand immutability and how to use it in Java code.

## Why It Matters

Immutable objects are easier to reason about, safer to share between threads,
and less likely to be accidentally changed by distant code. They are especially
useful for value objects, DTOs, configuration, and snapshots.

## Immutable Record

Records are a concise way to model immutable data.

```java
public record Money(int cents, String currency) {
    public Money {
        if (cents < 0) {
            throw new IllegalArgumentException("cents must not be negative");
        }

        if (currency == null || currency.isBlank()) {
            throw new IllegalArgumentException("currency must not be blank");
        }
    }
}
```

The fields are final, accessors are generated, and equality is value-based.

## Immutable Class

```java
public final class UserId {
    private final String value;

    public UserId(String value) {
        if (value == null || value.isBlank()) {
            throw new IllegalArgumentException("value must not be blank");
        }

        this.value = value;
    }

    public String value() {
        return value;
    }
}
```

Use `final` fields and avoid setters.

## Defensive Copies

Collections are mutable unless protected.

```java
import java.util.List;

public final class OrderSnapshot {
    private final List<String> itemIds;

    public OrderSnapshot(List<String> itemIds) {
        this.itemIds = List.copyOf(itemIds);
    }

    public List<String> itemIds() {
        return itemIds;
    }
}
```

`List.copyOf` creates an unmodifiable copy and prevents callers from changing
internal state.

## Updating Immutable Data

Immutable objects are changed by creating new values.

```java
record UserProfile(String email, boolean active) {
    UserProfile deactivate() {
        return new UserProfile(email, false);
    }
}
```

This makes state transitions explicit.

## Practical Example

```java
public record Address(String street, String city, String postalCode) {
    public Address {
        if (street == null || street.isBlank()) {
            throw new IllegalArgumentException("street must not be blank");
        }

        if (city == null || city.isBlank()) {
            throw new IllegalArgumentException("city must not be blank");
        }
    }
}
```

An address is a good immutable value object: it represents data, has validation,
and should not change silently.

## Common Mistakes

- Returning mutable internal collections.
- Assuming `final` makes the referenced object immutable.
- Adding setters to value objects.
- Using immutable objects for workflows that genuinely need controlled mutation.
- Forgetting validation in constructors or compact record constructors.

## Interview Questions

1. What is immutability?
2. Why are immutable objects easier to share?
3. Does `final List<String>` make the list immutable?
4. Why are records useful for value objects?
5. What is a defensive copy?

## Practice

1. Convert a mutable DTO into a record.
2. Add validation to a compact record constructor.
3. Protect a list field with `List.copyOf`.
4. Create an immutable `deactivate` method that returns a new object.

## Related Topics

- [Clean Code in Java](clean_code_java.md)
- [Thread Safety](../07_concurrency/thread_safety.md)
- [`equals`, `hashCode`, and `toString`](../02_oop_core_concepts/equals_hashcode_tostring.md)

