# Abstract Classes

## Goal

Understand what abstract classes are, when they are useful, and how they differ
from interfaces.

## Why It Matters

Abstract classes are less common than interfaces in many modern Java services,
but they are still useful when related classes need shared state, shared
constructors, or a shared implementation skeleton.

## Basic Abstract Class

An abstract class cannot be instantiated directly.

```java
public abstract class ReportExporter {
    private final String reportName;

    protected ReportExporter(String reportName) {
        if (reportName == null || reportName.isBlank()) {
            throw new IllegalArgumentException("reportName must not be blank");
        }

        this.reportName = reportName;
    }

    public String reportName() {
        return reportName;
    }

    public abstract String export();
}
```

A subclass provides the missing behavior.

```java
public class CsvReportExporter extends ReportExporter {
    public CsvReportExporter(String reportName) {
        super(reportName);
    }

    @Override
    public String export() {
        return "csv:" + reportName();
    }
}
```

## Shared Template Method

Abstract classes can define a workflow and let subclasses customize a step.

```java
public abstract class ImportJob {
    public final void run() {
        validate();
        importData();
        markCompleted();
    }

    protected void validate() {
        System.out.println("Common validation");
    }

    protected abstract void importData();

    private void markCompleted() {
        System.out.println("Import completed");
    }
}
```

This is called the template method pattern. Use it when the workflow is stable
and only specific steps vary.

## Abstract Class vs Interface

Use an interface when you need a contract:

```java
public interface PaymentGateway {
    void charge(int amountInCents);
}
```

Use an abstract class when implementations share state or a meaningful base
workflow:

```java
public abstract class BasePaymentGateway {
    private final String providerName;

    protected BasePaymentGateway(String providerName) {
        this.providerName = providerName;
    }

    public String providerName() {
        return providerName;
    }

    public abstract void charge(int amountInCents);
}
```

Java supports implementing multiple interfaces, but a class can extend only one
class.

## Practical Example

```java
public abstract class AccountNotification {
    private final String recipient;

    protected AccountNotification(String recipient) {
        if (recipient == null || recipient.isBlank()) {
            throw new IllegalArgumentException("recipient must not be blank");
        }

        this.recipient = recipient;
    }

    public final String message() {
        return subject() + ": " + body();
    }

    protected String recipient() {
        return recipient;
    }

    protected abstract String subject();

    protected abstract String body();
}
```

```java
public class PasswordChangedNotification extends AccountNotification {
    public PasswordChangedNotification(String recipient) {
        super(recipient);
    }

    @Override
    protected String subject() {
        return "Security alert";
    }

    @Override
    protected String body() {
        return "Password changed for " + recipient();
    }
}
```

The abstract class centralizes shared recipient validation and message assembly.

## Common Mistakes

- Using an abstract class when an interface would be enough.
- Creating a base class only to share unrelated helper methods.
- Making protected state mutable and hard for subclasses to reason about.
- Building inheritance chains that are more than one or two levels deep.
- Forgetting that Java allows only single class inheritance.

## Interview Questions

1. What is an abstract class?
2. Can an abstract class have constructors?
3. Can an abstract class contain both concrete and abstract methods?
4. How is an abstract class different from an interface?
5. When would an abstract class be a poor design choice?

## Practice

1. Create an abstract `FileParser` with a shared `parse` workflow.
2. Add two subclasses for CSV and JSON parsing.
3. Move only truly shared logic into the abstract class.
4. Explain whether an interface would be better for your design.

## Related Topics

- [Interfaces](interfaces.md)
- [Inheritance](inheritance.md)
- [Polymorphism](polymorphism.md)

