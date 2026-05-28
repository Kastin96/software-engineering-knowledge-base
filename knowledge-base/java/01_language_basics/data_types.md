# Data Types

## Goal

Understand Java primitive types, reference types, strings, wrappers, and the
difference between value comparison and reference comparison.

## Why It Matters

Java's type system helps prevent many runtime mistakes. Choosing the right type
also makes code easier to read, safer to change, and more accurate in real
business logic.

## Primitive Types

Primitive types store simple values directly.

```java
int age = 32;
long userId = 9_223_372_036L;
double price = 19.99;
boolean active = true;
char grade = 'A';
```

Common primitive types:

- `byte`, `short`, `int`, `long` for whole numbers;
- `float`, `double` for decimal approximations;
- `boolean` for true/false values;
- `char` for a single UTF-16 code unit.

Use `int` for most ordinary whole numbers. Use `long` for identifiers, counts,
timestamps, and values that may exceed `int`.

## Reference Types

Reference types point to objects.

```java
String email = "alex@example.com";
java.time.LocalDate createdAt = java.time.LocalDate.now();
```

Reference variables can be `null`.

```java
String middleName = null;
```

Use `null` carefully. In modern Java code, prefer clear validation, empty
collections, or `Optional` for method return values where absence is expected.

## Strings

`String` is immutable. Operations that appear to change a string create a new
string.

```java
String name = "Alex";
String upperName = name.toUpperCase();

System.out.println(name);      // Alex
System.out.println(upperName); // ALEX
```

Use `.equals` for string content comparison.

```java
String expected = "admin";
String actual = "admin";

System.out.println(expected.equals(actual)); // true
```

Avoid `==` for string content comparison.

```java
// Incorrect for content checks:
// if (expected == actual) { ... }
```

## Wrapper Types

Each primitive has a wrapper class.

```java
Integer count = 10;
Long id = 123L;
Boolean enabled = true;
Double rate = 0.15;
```

Wrappers are useful with collections and APIs that need objects.

```java
java.util.List<Integer> scores = java.util.List.of(90, 85, 100);
```

Be careful: wrappers can be `null`, primitives cannot.

```java
Integer maybeCount = null;

// Throws NullPointerException:
// int count = maybeCount;
```

## Numeric Precision

`float` and `double` are not exact decimal money types.

```java
double result = 0.1 + 0.2;
System.out.println(result); // often prints 0.30000000000000004
```

For money, prefer `BigDecimal` or integer minor units.

```java
import java.math.BigDecimal;

BigDecimal price = new BigDecimal("19.99");
BigDecimal taxRate = new BigDecimal("0.08");
BigDecimal tax = price.multiply(taxRate);
```

## Practical Example

```java
import java.math.BigDecimal;
import java.time.LocalDate;

public class InvoiceLine {
    public static void main(String[] args) {
        long productId = 1001L;
        String productName = "Keyboard";
        int quantity = 2;
        BigDecimal unitPrice = new BigDecimal("49.99");
        LocalDate invoiceDate = LocalDate.now();

        BigDecimal total = unitPrice.multiply(BigDecimal.valueOf(quantity));

        System.out.println(productId + " - " + productName);
        System.out.println("Date: " + invoiceDate);
        System.out.println("Total: " + total);
    }
}
```

This is more realistic than using `double` for money and shows common backend
types: identifiers, text, counts, dates, and precise decimal values.

## Common Mistakes

- Comparing strings with `==` instead of `.equals`.
- Using `double` for financial calculations.
- Forgetting that wrapper types can be `null`.
- Using `int` for values that can grow beyond its range.
- Treating `char` as a full Unicode character in every situation.

## Interview Questions

1. What is the difference between primitive and reference types?
2. Why should strings be compared with `.equals`?
3. Why is `BigDecimal` usually better than `double` for money?
4. What is autoboxing?
5. Why can unboxing a wrapper cause `NullPointerException`?

## Practice

1. Model a product with id, name, price, and active status.
2. Calculate a total price using `BigDecimal`.
3. Compare two email strings safely.
4. Create a list of integer scores using `List<Integer>`.

## Related Topics

- [Variables](variables.md)
- [Operators](operators.md)
- [Methods](methods.md)

