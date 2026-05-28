# Operators

## Goal

Understand the operators Java uses for arithmetic, comparison, boolean logic,
assignment, and safe everyday expressions.

## Why It Matters

Operators are small, but they control important decisions: totals, validation,
access checks, filtering, and error conditions. Many interview bugs come from
operator precedence, integer division, or incorrect boolean logic.

## Arithmetic Operators

```java
int a = 10;
int b = 3;

System.out.println(a + b); // 13
System.out.println(a - b); // 7
System.out.println(a * b); // 30
System.out.println(a / b); // 3
System.out.println(a % b); // 1
```

When both operands are integers, division returns an integer.

```java
int completed = 1;
int total = 2;

System.out.println(completed / total); // 0
```

Use a decimal type when you need a decimal result.

```java
double ratio = (double) completed / total;
System.out.println(ratio); // 0.5
```

## Assignment Operators

```java
int count = 0;

count += 5; // count = count + 5
count -= 2;
count++;
count--;
```

Use compound assignment when it keeps the code clear. Do not use it to compress
complex logic.

## Comparison Operators

```java
int age = 20;

System.out.println(age >= 18); // true
System.out.println(age == 21); // false
System.out.println(age != 0);  // true
```

For objects, `==` compares references. Use `.equals` for value-like comparison
when the class supports it.

```java
String role = "admin";

if ("admin".equals(role)) {
    System.out.println("Admin access");
}
```

Putting the constant first avoids `NullPointerException` if `role` is `null`.

## Boolean Operators

```java
boolean active = true;
boolean emailVerified = false;

System.out.println(active && emailVerified); // false
System.out.println(active || emailVerified); // true
System.out.println(!active);                 // false
```

`&&` and `||` short-circuit. Java does not evaluate the right side if the result
is already known.

```java
String email = null;

if (email != null && email.endsWith("@example.com")) {
    System.out.println("Company email");
}
```

This is safe because `email.endsWith(...)` runs only when `email != null`.

## Ternary Operator

Use the ternary operator for simple conditional values.

```java
int score = 82;
String result = score >= 70 ? "pass" : "fail";
```

Avoid nested ternaries in business logic. Use `if` or a method instead.

## Practical Example

```java
public class AccessPolicy {
    public static void main(String[] args) {
        int age = 22;
        boolean active = true;
        boolean emailVerified = true;
        String country = "US";

        boolean adult = age >= 18;
        boolean supportedCountry = "US".equals(country) || "CA".equals(country);
        boolean canAccess = adult && active && emailVerified && supportedCountry;

        System.out.println("Can access: " + canAccess);
    }
}
```

This example mirrors real access checks: several small boolean expressions are
named and then combined into a final decision.

## Common Mistakes

- Expecting integer division to return a decimal result.
- Comparing strings or objects with `==` when content equality is needed.
- Writing long boolean expressions without naming intermediate decisions.
- Using `&` or `|` instead of `&&` or `||` in ordinary boolean checks.
- Relying on operator precedence when parentheses would make the intent clearer.

## Interview Questions

1. What is the result of `5 / 2` in Java, and why?
2. What is short-circuit evaluation?
3. What is the difference between `==` and `.equals`?
4. When is the ternary operator appropriate?
5. Why can naming boolean expressions make code easier to review?

## Practice

1. Calculate a discount amount and final price.
2. Write a boolean expression that checks whether a user can log in.
3. Convert an integer division expression into a decimal ratio.
4. Rewrite a long condition by extracting named boolean variables.

## Related Topics

- [Data Types](data_types.md)
- [Control Flow](control_flow.md)
- [Methods](methods.md)

