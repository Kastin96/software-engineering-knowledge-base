# `equals`, `hashCode`, and `toString`

## Goal

Understand why Java objects often need correct `equals`, `hashCode`, and
`toString` methods.

## Why It Matters

These methods affect collection behavior, duplicate detection, tests, logs, and
debugging. A broken `equals` or `hashCode` can make `HashSet` and `HashMap`
behave incorrectly.

## Default Behavior

Every Java class inherits methods from `Object`.

```java
Object object = new Object();

System.out.println(object.equals(object));
System.out.println(object.hashCode());
System.out.println(object.toString());
```

By default, `equals` checks whether two references point to the same object.

```java
UserId first = new UserId("u-100");
UserId second = new UserId("u-100");

System.out.println(first == second);      // false
System.out.println(first.equals(second)); // false unless overridden
```

## Value-Like Objects

If two objects should be equal because their values are equal, override
`equals` and `hashCode`.

```java
import java.util.Objects;

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

    @Override
    public boolean equals(Object other) {
        if (this == other) {
            return true;
        }

        if (!(other instanceof UserId userId)) {
            return false;
        }

        return Objects.equals(value, userId.value);
    }

    @Override
    public int hashCode() {
        return Objects.hash(value);
    }

    @Override
    public String toString() {
        return "UserId[value=" + value + "]";
    }
}
```

This class is `final` so equality cannot be complicated by subclasses.

## The Contract

If two objects are equal according to `equals`, they must return the same
`hashCode`.

```java
UserId first = new UserId("u-100");
UserId second = new UserId("u-100");

System.out.println(first.equals(second)); // true
System.out.println(first.hashCode() == second.hashCode()); // true
```

Equal objects need the same hash code. Different objects may still have the same
hash code, but a good hash code reduces collisions.

## Collections Example

```java
import java.util.HashSet;
import java.util.Set;

Set<UserId> userIds = new HashSet<>();
userIds.add(new UserId("u-100"));
userIds.add(new UserId("u-100"));

System.out.println(userIds.size()); // 1 when equals/hashCode are correct
```

Without correct `equals` and `hashCode`, the set would keep both objects.

## Records

Records automatically implement value-based `equals`, `hashCode`, and
`toString`.

```java
public record ProductCode(String value) {
    public ProductCode {
        if (value == null || value.isBlank()) {
            throw new IllegalArgumentException("value must not be blank");
        }
    }
}
```

Records are a good fit for simple immutable value carriers.

## `toString`

`toString` should help debugging and logging.

```java
@Override
public String toString() {
    return "UserId[value=" + value + "]";
}
```

Do not include secrets such as passwords, access tokens, private keys, or full
payment card numbers.

## Practical Example

```java
public record EmailAddress(String value) {
    public EmailAddress {
        if (value == null || value.isBlank() || !value.contains("@")) {
            throw new IllegalArgumentException("invalid email address");
        }

        value = value.trim().toLowerCase();
    }
}
```

This record normalizes email values and gets correct equality behavior for free.

```java
EmailAddress first = new EmailAddress("Alex@Example.com");
EmailAddress second = new EmailAddress("alex@example.com");

System.out.println(first.equals(second)); // true
```

## Common Mistakes

- Overriding `equals` without overriding `hashCode`.
- Including mutable fields in equality and then changing them while the object is
  inside a `HashSet` or `HashMap`.
- Using `==` instead of `.equals` for value-like objects.
- Adding sensitive data to `toString`.
- Writing complex equality rules for classes that should be simple records.

## Interview Questions

1. What is the difference between `==` and `.equals`?
2. Why must `equals` and `hashCode` be consistent?
3. What can go wrong if a mutable field is used in `hashCode`?
4. Why are records useful for value-like objects?
5. What should you avoid putting in `toString`?

## Practice

1. Create a `ProductId` value object with correct equality.
2. Put duplicate `ProductId` objects into a `HashSet`.
3. Rewrite the value object as a record.
4. Add a `toString` that helps debugging but does not expose sensitive data.

## Related Topics

- [Classes and Objects](classes_objects.md)
- [Encapsulation](encapsulation.md)
- [Collections and Data Structures](../03_collections_and_data_structures/README.md)
