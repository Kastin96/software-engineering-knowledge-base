# Methods

## Goal

Understand how to define reusable behavior with Java methods, parameters,
return values, overloads, and simple method design.

## Why It Matters

Methods keep Java programs from becoming one long `main` block. They make logic
easier to read, test, reuse, and discuss in interviews.

## Basic Method

```java
public class GreetingApp {
    public static void main(String[] args) {
        String message = createGreeting("Alex");
        System.out.println(message);
    }

    static String createGreeting(String name) {
        return "Hello, " + name;
    }
}
```

Method parts:

- `static` means the method belongs to the class, not an object.
- `String` before the method name is the return type.
- `createGreeting` is the method name.
- `String name` is a parameter.
- `return` sends a value back to the caller.

## Void Methods

Use `void` when a method performs an action and does not return a value.

```java
static void printError(String message) {
    System.out.println("ERROR: " + message);
}
```

In business code, prefer returning values from methods that calculate or decide.
This makes them easier to test.

## Parameters and Arguments

Parameters are declared by the method. Arguments are the actual values passed
when calling the method.

```java
static int calculateTotal(int unitPrice, int quantity) {
    return unitPrice * quantity;
}

int total = calculateTotal(25, 4);
```

Here `unitPrice` and `quantity` are parameters. `25` and `4` are arguments.

## Early Return

Use early returns for validation and simple branch handling.

```java
static boolean canAccess(boolean active, boolean emailVerified, int age) {
    if (!active) {
        return false;
    }

    if (!emailVerified) {
        return false;
    }

    return age >= 18;
}
```

This is often easier to read than one large nested condition.

## Method Overloading

Overloading means multiple methods have the same name but different parameters.

```java
static String formatUser(String name) {
    return name;
}

static String formatUser(String firstName, String lastName) {
    return firstName + " " + lastName;
}
```

Use overloading when the methods represent the same operation with different
input shapes. Avoid overloads that make calls ambiguous or surprising.

## Static vs Instance Methods

Static methods are useful for simple utilities and examples.

```java
static int square(int number) {
    return number * number;
}
```

Instance methods belong to objects and are covered more deeply in OOP topics.

```java
class User {
    private final String email;

    User(String email) {
        this.email = email;
    }

    boolean hasCompanyEmail() {
        return email.endsWith("@example.com");
    }
}
```

## Practical Example

```java
public class SignupValidator {
    public static void main(String[] args) {
        String email = "alex@example.com";
        int age = 21;

        if (isValidSignup(email, age)) {
            System.out.println("Signup accepted");
        } else {
            System.out.println("Signup rejected");
        }
    }

    static boolean isValidSignup(String email, int age) {
        return isValidEmail(email) && isAdult(age);
    }

    static boolean isValidEmail(String email) {
        return email != null && email.contains("@") && !email.isBlank();
    }

    static boolean isAdult(int age) {
        return age >= 18;
    }
}
```

This example separates business decisions into small methods that can be tested
individually.

## Common Mistakes

- Putting all logic in `main`.
- Creating methods with vague names like `process`, `handle`, or `doStuff`.
- Mixing calculation, validation, printing, and data access in one method.
- Returning `void` when a return value would make the method easier to test.
- Using too many parameters instead of introducing a meaningful object later.

## Interview Questions

1. What is the difference between a parameter and an argument?
2. What does `void` mean?
3. What is method overloading?
4. Why are small methods easier to test?
5. When would you use a static method?

## Practice

1. Write a method that calculates a user's age category.
2. Write a method that validates an email string.
3. Write a method that accepts price and quantity and returns a total.
4. Split one large method into three smaller methods.

## Related Topics

- [Control Flow](control_flow.md)
- [Operators](operators.md)
- OOP Core Concepts
