# Control Flow

## Goal

Understand how Java chooses between branches and repeats work using `if`,
`switch`, `for`, `while`, and loop control statements.

## Why It Matters

Most real programs are decision trees: validate input, choose a path, repeat
work for each item, stop when a condition is reached, and handle invalid cases.
Clear control flow makes that logic easier to test and maintain.

## If Statements

Use `if` when code should run only under a condition.

```java
int age = 20;

if (age >= 18) {
    System.out.println("Adult");
}
```

Use `else if` and `else` for mutually exclusive branches.

```java
int score = 82;

if (score >= 90) {
    System.out.println("A");
} else if (score >= 80) {
    System.out.println("B");
} else if (score >= 70) {
    System.out.println("C");
} else {
    System.out.println("Needs improvement");
}
```

## Guard Clauses

Guard clauses handle invalid or special cases early.

```java
static String formatUsername(String username) {
    if (username == null || username.isBlank()) {
        return "Guest";
    }

    return username.trim();
}
```

This avoids deeply nested code.

## Switch

Use `switch` when one value maps to several known cases.

```java
String role = "admin";

switch (role) {
    case "admin":
        System.out.println("Full access");
        break;
    case "manager":
        System.out.println("Team access");
        break;
    case "user":
        System.out.println("Basic access");
        break;
    default:
        System.out.println("No access");
}
```

Modern Java also supports switch expressions.

```java
String role = "manager";

String accessLevel = switch (role) {
    case "admin" -> "full";
    case "manager" -> "team";
    case "user" -> "basic";
    default -> "none";
};

System.out.println(accessLevel);
```

Use switch expressions when you need to produce a value clearly.

## For Loops

Use a traditional `for` loop when you need an index.

```java
java.util.List<String> users = java.util.List.of("Alex", "Sam", "Mira");

for (int i = 0; i < users.size(); i++) {
    System.out.println(i + ": " + users.get(i));
}
```

Use an enhanced `for` loop when you only need each item.

```java
for (String user : users) {
    System.out.println(user);
}
```

## While Loops

Use `while` when the number of iterations depends on a condition.

```java
int attemptsLeft = 3;

while (attemptsLeft > 0) {
    System.out.println("Trying...");
    attemptsLeft--;
}
```

Use `do while` only when the body must run at least once.

```java
int page = 1;

do {
    System.out.println("Loading page " + page);
    page++;
} while (page <= 3);
```

## Break and Continue

`break` stops a loop. `continue` skips to the next iteration.

```java
java.util.List<String> emails = java.util.List.of(
        "alex@example.com",
        "",
        "sam@example.com"
);

for (String email : emails) {
    if (email.isBlank()) {
        continue;
    }

    if (email.equals("sam@example.com")) {
        break;
    }

    System.out.println(email);
}
```

Use them sparingly. If a loop becomes hard to follow, extract a method.

## Practical Example

```java
import java.util.List;

public class OrderValidator {
    public static void main(String[] args) {
        List<Integer> itemQuantities = List.of(2, 1, 0, 5);
        boolean valid = true;

        for (int quantity : itemQuantities) {
            if (quantity <= 0) {
                valid = false;
                break;
            }
        }

        String status = valid ? "valid" : "invalid";
        System.out.println("Order is " + status);
    }
}
```

This is realistic validation logic: inspect every item and stop when an invalid
value is found.

## Common Mistakes

- Writing deeply nested `if` blocks instead of using guard clauses.
- Forgetting `break` in old-style `switch` statements.
- Using an index loop when an enhanced `for` loop would be clearer.
- Creating infinite `while` loops by forgetting to update the condition.
- Mixing validation, calculation, and output in one large block.

## Interview Questions

1. When would you choose `if` over `switch`?
2. What is the difference between a statement switch and a switch expression?
3. When should you use an enhanced `for` loop?
4. What is a guard clause?
5. How can `break` and `continue` make code clearer or harder to read?

## Practice

1. Write code that prints a grade based on a numeric score.
2. Use a switch expression to map a user role to an access level.
3. Loop through a list of prices and calculate the total.
4. Stop processing when you find an invalid value.

## Related Topics

- [Operators](operators.md)
- [Methods](methods.md)
- [Data Types](data_types.md)

