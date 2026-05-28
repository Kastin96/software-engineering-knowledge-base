# Java Syntax Cheatsheet

## Minimal Program

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

## Variables

```java
int count = 3;
long id = 100L;
double price = 19.99;
boolean active = true;
String email = "alex@example.com";
final int maxAttempts = 5;
var name = "Alex";
```

Use `var` only for local variables when the type is obvious.

## Methods

```java
static int add(int left, int right) {
    return left + right;
}
```

## Conditions

```java
if (score >= 90) {
    return "A";
} else if (score >= 80) {
    return "B";
} else {
    return "Needs improvement";
}
```

## Switch Expression

```java
String label = switch (status) {
    case "NEW" -> "New order";
    case "PAID" -> "Paid order";
    default -> "Unknown";
};
```

## Records

```java
public record User(String id, String email) {
    public User {
        if (email == null || !email.contains("@")) {
            throw new IllegalArgumentException("email must be valid");
        }
    }
}
```

## Common Rules

- Public class name must match the file name.
- Packages usually match folder structure.
- Use `.equals` for value comparison.
- Use `BigDecimal` or integer cents for money.
- Keep fields private by default.

