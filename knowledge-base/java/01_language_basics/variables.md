# Variables

## Goal

Understand how Java stores values using local variables, fields, constants, and
type inference with `var`.

## Why It Matters

Variables are the names a program uses to remember values. In Java, every
variable has a type, and that type controls what values can be stored and what
operations are allowed.

## Local Variables

A local variable is declared inside a method or block.

```java
public class OrderTotal {
    public static void main(String[] args) {
        int quantity = 3;
        double unitPrice = 19.99;
        double total = quantity * unitPrice;

        System.out.println(total);
    }
}
```

Local variables must be initialized before use.

```java
int count;

// Incorrect:
// System.out.println(count);

count = 5;
System.out.println(count);
```

## Reassignment

Use reassignment when the value naturally changes.

```java
int retryCount = 0;

retryCount++;
retryCount++;

System.out.println(retryCount); // 2
```

Avoid unnecessary reassignment when a value can be calculated once.

```java
double subtotal = 100.00;
double tax = subtotal * 0.08;
double total = subtotal + tax;
```

## Constants

Use `final` when a variable should not be reassigned.

```java
final double TAX_RATE = 0.08;
double subtotal = 100.00;
double tax = subtotal * TAX_RATE;
```

For class-level constants, use `static final`.

```java
public class PricingRules {
    public static final double TAX_RATE = 0.08;
}
```

`final` prevents reassignment of the variable. If the variable refers to an
object, the object may still be mutable.

```java
final StringBuilder message = new StringBuilder("Hello");
message.append(", Java");

// Incorrect:
// message = new StringBuilder("Different object");
```

## Type Inference With `var`

`var` lets Java infer the local variable type from the initializer.

```java
var name = "Alex";          // String
var attempts = 3;           // int
var total = 49.99;          // double
```

Use `var` when the type is obvious from the right side.

```java
var users = new java.util.ArrayList<String>();
```

Avoid `var` when it hides important information.

```java
// Less clear without reading the method return type:
var result = loadUserData();
```

`var` works only for local variables, not fields or method parameters.

## Naming

Use clear names that describe the value and its role.

```java
String userEmail = "alex@example.com";
boolean accountLocked = false;
int failedLoginAttempts = 2;
```

Avoid names that require guessing.

```java
String x = "alex@example.com"; // unclear
```

## Practical Example

```java
public class LoginCheck {
    private static final int MAX_FAILED_ATTEMPTS = 5;

    public static void main(String[] args) {
        String email = "alex@example.com";
        int failedAttempts = 2;
        boolean locked = failedAttempts >= MAX_FAILED_ATTEMPTS;

        System.out.println("Email: " + email);
        System.out.println("Locked: " + locked);
    }
}
```

This mirrors common backend logic: user data, a business threshold, and a boolean
decision.

## Common Mistakes

- Using unclear names like `a`, `b`, `data`, or `value` everywhere.
- Forgetting to initialize a local variable before reading it.
- Using `var` when the inferred type is not obvious.
- Assuming `final` makes an object immutable.
- Storing money in `double` in real financial systems instead of using
  `BigDecimal` or integer minor units.

## Interview Questions

1. What is the difference between a local variable and a field?
2. What does `final` mean for primitive values and object references?
3. When is `var` useful in Java?
4. Why must local variables be initialized before use?
5. Why is naming important in interview code and production code?

## Practice

1. Create variables for a user's email, age, and active status.
2. Add a `static final` constant for a minimum allowed age.
3. Calculate whether the user can access an adult-only feature.
4. Rewrite one obvious local variable using `var`.

## Related Topics

- [Data Types](data_types.md)
- [Operators](operators.md)
- [Control Flow](control_flow.md)

