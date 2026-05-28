# Checked and Unchecked Exceptions

## Goal

Understand the difference between checked and unchecked exceptions and when each
style is appropriate.

## Why It Matters

Java is unusual because it has checked exceptions. This affects method
signatures, API design, file handling, database code, and interview questions.
Good Java developers know when a caller should be forced to handle a failure and
when an exception should represent a programming or business rule problem.

## Checked Exceptions

Checked exceptions must be caught or declared with `throws`.

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;

public class FileReaderExample {
    public static void main(String[] args) throws IOException {
        String content = Files.readString(Path.of("config.txt"));
        System.out.println(content);
    }
}
```

`IOException` is checked because file access can fail for reasons outside the
program's direct control: missing file, permissions, disk issues, or interrupted
I/O.

## Catching a Checked Exception

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;

public class ConfigLoader {
    public static void main(String[] args) {
        try {
            String content = Files.readString(Path.of("config.txt"));
            System.out.println(content);
        } catch (IOException exception) {
            System.out.println("Could not read config.txt: " + exception.getMessage());
        }
    }
}
```

Catch a checked exception when the current layer can do something useful:
recover, return a fallback, add context, or report the failure clearly.

## Unchecked Exceptions

Unchecked exceptions extend `RuntimeException`. They do not need to be declared.

```java
static int parsePort(String value) {
    if (value == null || value.isBlank()) {
        throw new IllegalArgumentException("port must not be blank");
    }

    int port = Integer.parseInt(value);

    if (port < 1 || port > 65_535) {
        throw new IllegalArgumentException("port must be between 1 and 65535");
    }

    return port;
}
```

`IllegalArgumentException` is unchecked because it usually means the caller
violated the method contract.

## When To Use Checked Exceptions

Checked exceptions can make sense when:

- the caller can realistically recover;
- the failure is expected from an external resource;
- the API wants to force callers to think about the failure path.

Examples:

- reading a file;
- opening a network connection;
- parsing a required external document format in a low-level API.

## When To Use Unchecked Exceptions

Unchecked exceptions are common when:

- the caller passed invalid arguments;
- the object is in an invalid state;
- the failure is a programming error;
- the current layer cannot reasonably recover.

Examples:

- `IllegalArgumentException`;
- `IllegalStateException`;
- `NullPointerException`;
- domain exceptions such as `OrderAlreadyPaidException`.

## Wrapping Exceptions

Sometimes a low-level checked exception should be wrapped with domain context.

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;

public class ReportTemplateLoader {
    public String loadTemplate(String name) {
        try {
            return Files.readString(Path.of("templates", name + ".txt"));
        } catch (IOException exception) {
            throw new TemplateLoadException("Could not load report template: " + name, exception);
        }
    }
}

class TemplateLoadException extends RuntimeException {
    TemplateLoadException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

The original exception is preserved as the cause. This keeps the stack trace and
root error available.

## Practical Example

```java
public class AccountService {
    public void withdraw(Account account, int amountInCents) {
        if (amountInCents <= 0) {
            throw new IllegalArgumentException("amountInCents must be positive");
        }

        if (account.balanceInCents() < amountInCents) {
            throw new InsufficientFundsException("account has insufficient funds");
        }

        account.withdraw(amountInCents);
    }
}

class InsufficientFundsException extends RuntimeException {
    InsufficientFundsException(String message) {
        super(message);
    }
}

class Account {
    private int balanceInCents;

    Account(int balanceInCents) {
        this.balanceInCents = balanceInCents;
    }

    int balanceInCents() {
        return balanceInCents;
    }

    void withdraw(int amountInCents) {
        balanceInCents -= amountInCents;
    }
}
```

Invalid withdrawal amount is a caller contract problem. Insufficient funds is a
business rule failure. Both are reasonable unchecked exceptions in many service
layers.

## Common Mistakes

- Declaring `throws Exception` instead of a specific checked exception.
- Catching checked exceptions only to print and continue with invalid state.
- Wrapping an exception without preserving the original cause.
- Making every business rule failure a checked exception.
- Assuming unchecked exceptions are less important because the compiler does not
  force handling.

## Interview Questions

1. What is a checked exception?
2. What is an unchecked exception?
3. Why does `IOException` usually make sense as a checked exception?
4. When would you wrap a checked exception in an unchecked exception?
5. Why is `throws Exception` usually a weak method signature?

## Practice

1. Write a method that reads a file and declares `throws IOException`.
2. Rewrite it to catch `IOException` and throw a custom runtime exception with a
   cause.
3. Create an unchecked exception for a business rule failure.
4. Explain which failures in your code are recoverable and which are not.

## Related Topics

- [Exceptions](exceptions.md)
- [Custom Exceptions](custom_exceptions.md)
- [Try-With-Resources](try_with_resources.md)
