# Syntax

## Goal

Understand the basic structure of a Java program and the syntax rules that make
Java code readable and executable.

## Why It Matters

Java is stricter than many scripting languages. The compiler checks class names,
types, method signatures, braces, semicolons, and access rules before the program
runs. This strictness can feel verbose at first, but it catches many mistakes
early.

## Minimal Program

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, Java");
    }
}
```

What each part means:

- `public class Main` defines a class named `Main`.
- `main` is the entry point used when the program starts.
- `String[] args` receives command-line arguments.
- `System.out.println(...)` prints a line to the console.
- semicolons end most statements.
- braces define blocks.

## File Names and Class Names

If a class is `public`, the file name must match the class name.

```java
public class UserReport {
    public static void main(String[] args) {
        System.out.println("Report is ready");
    }
}
```

This should be saved as:

```text
UserReport.java
```

## Packages

Packages organize related classes and avoid name conflicts.

```java
package com.example.reports;

public class ReportFormatter {
    public String format(String title) {
        return "Report: " + title;
    }
}
```

In real projects, packages usually follow the folder structure:

```text
src/main/java/com/example/reports/ReportFormatter.java
```

## Imports

Imports let you use classes from other packages without writing the full name
every time.

```java
import java.time.LocalDate;

public class TodayPrinter {
    public static void main(String[] args) {
        LocalDate today = LocalDate.now();
        System.out.println(today);
    }
}
```

Classes from `java.lang`, such as `String`, `Math`, and `System`, are imported
automatically.

## Comments

Use comments to explain why something is done, not to repeat what the code
already says.

```java
// The threshold comes from the business rule for manual review.
int manualReviewThreshold = 10_000;
```

Avoid noisy comments:

```java
// Bad: increments count by one
count++;
```

## Practical Example

```java
import java.time.LocalDate;

public class AccountSummary {
    public static void main(String[] args) {
        String userName = "Alex";
        int unreadMessages = 3;
        LocalDate today = LocalDate.now();

        System.out.println("User: " + userName);
        System.out.println("Unread messages: " + unreadMessages);
        System.out.println("Date: " + today);
    }
}
```

This example uses a class, `main`, variables, an import, and console output in a
shape similar to small scripts or coding assessment warmups.

## Common Mistakes

- Naming the file differently from a public class.
- Forgetting semicolons after statements.
- Writing code outside a class or method.
- Importing classes that are already in `java.lang`.
- Putting too much logic directly in `main` in larger programs.

## Interview Questions

1. Why does Java need a `main` method?
2. Why must a public class name match the file name?
3. What is the purpose of a package?
4. What is imported automatically from `java.lang`?
5. Why is Java often described as a statically typed language?

## Practice

1. Create a class named `WelcomeMessage` that prints your name and current year.
2. Add a package declaration that matches a realistic project structure.
3. Import `java.time.LocalDateTime` and print the current date and time.
4. Move the output message into a separate variable before printing it.

## Related Topics

- [Variables](variables.md)
- [Data Types](data_types.md)
- [Methods](methods.md)

