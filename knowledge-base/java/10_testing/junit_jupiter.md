# JUnit Jupiter

## Goal

Understand the JUnit Jupiter basics used in modern JUnit 5 tests.

## Why It Matters

JUnit Jupiter is the programming model most Java developers mean when they say
"JUnit 5". It provides annotations, assertions, lifecycle hooks, parameterized
tests, and integrations used by Maven, Gradle, IDEs, and CI pipelines.

## Basic Test

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class CalculatorTest {
    @Test
    void addsTwoNumbers() {
        Calculator calculator = new Calculator();

        int result = calculator.add(2, 3);

        assertEquals(5, result);
    }
}
```

Test classes and methods can be package-private. They do not need to be public.

## Common Annotations

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

class UserServiceTest {
    private UserService service;

    @BeforeEach
    void setUp() {
        service = new UserService();
    }

    @Test
    void createsUser() {
        // test
    }
}
```

Useful annotations:

- `@Test` marks a test method.
- `@BeforeEach` runs before each test.
- `@AfterEach` runs after each test.
- `@BeforeAll` runs once before all tests.
- `@AfterAll` runs once after all tests.
- `@Disabled` temporarily disables a test with a reason.

## Parameterized Tests

Use parameterized tests for the same behavior with multiple inputs.

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

import static org.junit.jupiter.api.Assertions.assertFalse;

class EmailValidatorTest {
    private final EmailValidator validator = new EmailValidator();

    @ParameterizedTest
    @ValueSource(strings = {"", " ", "missing-at-sign"})
    void rejectsInvalidEmails(String email) {
        assertFalse(validator.isValid(email));
    }
}
```

This avoids copy-paste tests with only input differences.

## Maven Setup

Use the current JUnit Jupiter version from your project or dependency management.
Typical dependency shape:

```xml
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter</artifactId>
  <scope>test</scope>
</dependency>
```

Many modern parent builds or BOMs manage the version. If not, declare one
explicitly.

## Gradle Setup

```groovy
dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter'
}

tasks.test {
    useJUnitPlatform()
}
```

Use the project wrapper and dependency management conventions.

## Practical Example

```java
class PasswordPolicy {
    boolean isStrong(String password) {
        return password != null
                && password.length() >= 12
                && password.chars().anyMatch(Character::isDigit);
    }
}
```

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.NullAndEmptySource;
import org.junit.jupiter.params.provider.ValueSource;

import static org.junit.jupiter.api.Assertions.assertFalse;

class PasswordPolicyTest {
    private final PasswordPolicy policy = new PasswordPolicy();

    @ParameterizedTest
    @NullAndEmptySource
    @ValueSource(strings = {"short1", "longpasswordwithoutdigit"})
    void rejectsWeakPasswords(String password) {
        assertFalse(policy.isStrong(password));
    }
}
```

The test names the behavior and keeps edge cases compact.

## Common Mistakes

- Mixing JUnit 4 and JUnit Jupiter imports accidentally.
- Forgetting `useJUnitPlatform()` in Gradle.
- Putting heavy shared mutable state in `@BeforeAll`.
- Disabling tests without a clear reason.
- Using lifecycle hooks when simple local setup would be clearer.

## Interview Questions

1. What is JUnit Jupiter?
2. What does `@Test` do?
3. When would you use `@BeforeEach`?
4. What are parameterized tests useful for?
5. Why should disabled tests include a reason?

## Practice

1. Write a simple JUnit Jupiter test.
2. Add `@BeforeEach` setup.
3. Convert three similar tests into one parameterized test.
4. Check that your Gradle or Maven build runs tests in CI.

## Related Topics

- [Unit Testing](unit_testing.md)
- [Assertions](assertions.md)
- [Maven](../09_jvm_build_tools/maven.md)

