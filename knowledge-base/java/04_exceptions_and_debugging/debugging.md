# Debugging

## Goal

Learn a practical debugging workflow for Java code.

## Why It Matters

Debugging is not guessing until the code works. A good workflow helps you move
from symptom to cause: reproduce the issue, read the error, inspect the data,
confirm the assumption, fix the smallest cause, and add a test when possible.

## Debugging Workflow

Use this order:

1. Reproduce the problem.
2. Read the full exception and stack trace.
3. Identify the first relevant line in your code.
4. Inspect the values at that line.
5. Work backward to find where the bad value came from.
6. Make the smallest fix.
7. Add or update a test if the behavior matters.

This keeps debugging focused and prevents random changes.

## Use Breakpoints

A breakpoint pauses the program at a specific line.

Useful things to inspect:

- method arguments;
- local variables;
- object fields;
- collection sizes and contents;
- branch conditions;
- exception objects.

If a condition is wrong, inspect the values that make it wrong.

## Conditional Breakpoints

Use a conditional breakpoint when a loop runs many times.

Example condition:

```text
order.id().equals("o-100")
```

This lets the debugger stop only for the suspicious case.

## Logging While Debugging

For small console programs, `System.out.println` can be enough.

```java
System.out.println("orderId=" + orderId);
System.out.println("items.size=" + items.size());
```

In real applications, use a logger instead of console output. Logs should include
context such as ids, status, and operation names, but not secrets.

## Debugging Nulls

Do not only patch nulls at the failing line. Find where the unexpected null came
from.

```java
User user = usersById.get(id);

if (user == null) {
    throw new UserNotFoundException("User not found: " + id);
}
```

This gives a meaningful failure instead of a later `NullPointerException`.

## Debugging Collections

When collection logic fails, inspect:

- collection size;
- whether the collection is mutable;
- whether values are duplicated;
- whether `equals` and `hashCode` are correct;
- whether sorting changed the original list;
- whether the code modifies the collection while iterating.

## Practical Example

```java
import java.util.List;

public class TotalCalculator {
    public static void main(String[] args) {
        List<Integer> prices = List.of(1000, 2500, -500);
        int total = calculateTotal(prices);
        System.out.println(total);
    }

    static int calculateTotal(List<Integer> prices) {
        int total = 0;

        for (int price : prices) {
            if (price < 0) {
                throw new IllegalArgumentException("price must not be negative: " + price);
            }

            total += price;
        }

        return total;
    }
}
```

If this fails, the exception message gives the invalid value. A breakpoint inside
the loop would show which list element caused the problem.

## Common Mistakes

- Changing code before reproducing the issue.
- Reading only part of the stack trace.
- Adding broad `try/catch` blocks instead of fixing the cause.
- Logging too little context or logging sensitive data.
- Fixing the symptom without understanding where the bad value started.
- Forgetting to add a regression test for an important bug.

## Interview Questions

1. How do you debug a Java exception?
2. What is the first line you look for in a stack trace?
3. When would you use a conditional breakpoint?
4. How do you debug a `NullPointerException` properly?
5. Why is adding a test after a bug fix useful?

## Practice

1. Create a small program that throws `IllegalArgumentException` for invalid
   input and debug it.
2. Add a breakpoint inside a loop and inspect each iteration.
3. Use a conditional breakpoint for one specific id.
4. Rewrite a vague exception into one that includes useful context.

## Related Topics

- [Stack Traces](stack_traces.md)
- [`try`, `catch`, and `finally`](try_catch_finally.md)
- [Collections and Data Structures](../03_collections_and_data_structures/README.md)

