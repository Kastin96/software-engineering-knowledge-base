# Refactoring Habits

## Goal

Learn how to improve Java code safely without changing behavior.

## Why It Matters

Refactoring is part of everyday development. The skill is making small, safe
changes that reduce complexity while tests or manual checks prove behavior still
works.

## Refactor in Small Steps

Prefer small changes:

- rename a variable;
- extract a method;
- introduce a value object;
- move a method to the class that owns the data;
- replace duplicated logic with a shared helper;
- split a class by responsibility.

Small steps are easier to review and easier to revert.

## Use Tests as a Safety Net

Before refactoring risky logic, add tests for current behavior.

```java
@Test
void rejectsPaymentForAlreadyPaidOrder() {
    // protects behavior before refactor
}
```

If tests are missing, add focused characterization tests around the behavior you
intend to preserve.

## Extract Method

Before:

```java
if (order.totalInCents() >= 10_000 || order.status().equals("FAILED")) {
    reviewQueue.add(order);
}
```

After:

```java
if (requiresManualReview(order)) {
    reviewQueue.add(order);
}

private boolean requiresManualReview(Order order) {
    return order.totalInCents() >= 10_000 || order.status().equals("FAILED");
}
```

The method name explains the business rule.

## Replace Primitive With Value Object

Before:

```java
void pay(String orderId, int cents) {
}
```

After:

```java
record OrderId(String value) {}
record Money(int cents, String currency) {}

void pay(OrderId orderId, Money amount) {
}
```

This reduces parameter confusion and centralizes validation.

## Split Responsibilities

Before:

```java
class ImportService {
    void readFile() {}
    void parseRows() {}
    void validateRows() {}
    void saveRows() {}
    void sendSummaryEmail() {}
}
```

After:

```text
ImportFileReader
ImportRowParser
ImportValidator
ImportRepository
ImportSummaryNotifier
```

Only split when it makes ownership clearer.

## Practical Refactoring Checklist

Before refactoring:

- What behavior must stay the same?
- Is there a test for it?
- Can the change be smaller?
- Will names become clearer?
- Will dependencies become simpler?
- Can this be reviewed easily?

After refactoring:

- Run relevant tests.
- Read the code from the caller's perspective.
- Remove dead code.
- Keep unrelated cleanup for another change.

## Common Mistakes

- Refactoring and changing behavior in the same commit without clarity.
- Starting with a large rewrite.
- Moving code without improving names or boundaries.
- Refactoring without tests around risky behavior.
- Mixing unrelated style cleanup into feature work.

## Interview Questions

1. What is refactoring?
2. Why are tests important before refactoring?
3. What is extract method useful for?
4. When would you introduce a value object?
5. Why should refactors be small?

## Practice

1. Extract a complex condition into a named method.
2. Introduce a value object for an id or money amount.
3. Split a class with two clear responsibilities.
4. Add a test before refactoring a method.

## Related Topics

- [Clean Code in Java](clean_code_java.md)
- [Common Antipatterns](common_antipatterns.md)
- [Testing](../10_testing/README.md)

