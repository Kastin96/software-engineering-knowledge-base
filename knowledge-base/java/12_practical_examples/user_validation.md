# User Validation

## Problem

Validate signup input and return all validation errors instead of failing after
the first problem.

## Why This Example Matters

Real validation often needs to show multiple errors at once. A `List<String>` is
a simple way to collect validation messages before moving to framework-level
validation tools.

## Code

```java
import java.util.ArrayList;
import java.util.List;

public class SignupValidator {
    public ValidationResult validate(SignupRequest request) {
        List<String> errors = new ArrayList<>();

        if (request.email() == null || request.email().isBlank() || !request.email().contains("@")) {
            errors.add("email must be valid");
        }

        if (request.age() < 18) {
            errors.add("user must be adult");
        }

        if (request.password() == null || request.password().length() < 12) {
            errors.add("password must contain at least 12 characters");
        }

        return new ValidationResult(errors.isEmpty(), List.copyOf(errors));
    }

    public record SignupRequest(String email, int age, String password) {
    }

    public record ValidationResult(boolean valid, List<String> errors) {
    }
}
```

## What It Demonstrates

- records for simple request/result data;
- collecting multiple errors;
- returning an immutable copy;
- keeping validation in a focused class.

## Common Extension

Move each rule into a small private method if validation grows:

```java
private boolean hasValidEmail(SignupRequest request) {
    return request.email() != null
            && !request.email().isBlank()
            && request.email().contains("@");
}
```

## Practice

1. Add a rule that rejects passwords without digits.
2. Add a rule that rejects emails from a blocked domain.
3. Write unit tests for valid input, blank email, underage user, and weak
   password.

