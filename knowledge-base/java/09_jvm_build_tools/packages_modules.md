# Packages and Modules

## Goal

Understand Java packages and the basics of the Java Platform Module System.

## Why It Matters

Packages are used in every Java project. Modules are less common in typical
Spring-style application code, but they are part of modern Java and appear in
JDK internals, libraries, and some modular applications.

## Packages

Packages organize classes and avoid naming conflicts.

```java
package com.example.orders;

public class OrderService {
}
```

Folder layout usually matches the package:

```text
src/main/java/com/example/orders/OrderService.java
```

## Imports

```java
import java.time.LocalDate;
import java.util.List;
```

Imports let code use simple class names instead of fully qualified names.

```java
LocalDate today = LocalDate.now();
List<String> names = List.of("Alex", "Sam");
```

Classes from `java.lang`, such as `String`, `System`, and `Math`, are imported
automatically.

## Package Naming

Common package names use reversed domain names:

```text
com.example.orders
com.company.project.feature
```

For personal projects, use a stable namespace that avoids collisions.

## Access and Packages

Java access modifiers interact with packages:

- `public` is visible everywhere;
- `protected` is visible to subclasses and same package;
- no modifier means package-private;
- `private` is visible only inside the class.

Package-private is useful for implementation details that should not be public
API.

## Modules

Java modules are declared with `module-info.java`.

```java
module com.example.orders {
    requires java.sql;
    exports com.example.orders.api;
}
```

`requires` declares module dependencies.

`exports` exposes packages to other modules.

## When Modules Matter

Modules are useful for:

- strong encapsulation;
- library boundaries;
- smaller custom runtime images;
- large systems that intentionally use the module system.

Many backend applications still use classpath-based builds without explicit
module descriptors.

## Practical Example

Package layout:

```text
src/main/java/com/example/orders/api/OrderController.java
src/main/java/com/example/orders/domain/Order.java
src/main/java/com/example/orders/service/OrderService.java
```

The package names communicate responsibility: API boundary, domain model, and
service logic.

## Common Mistakes

- Putting unrelated classes into one large package.
- Using vague packages like `utils` for everything.
- Making implementation classes public when package-private would be enough.
- Creating package cycles that make code harder to change.
- Adding modules to a simple project without a real need.

## Interview Questions

1. What is a package?
2. Why do Java packages usually match folder structure?
3. What is package-private access?
4. What is `module-info.java`?
5. Why might many backend projects not use explicit Java modules?

## Practice

1. Create packages for `api`, `service`, and `domain`.
2. Move a helper class to package-private access.
3. Explain which classes should be public and which should not.
4. Write a simple `module-info.java` and explain `requires` and `exports`.

## Related Topics

- [Compile and Run](compile_run.md)
- [Classpath and JAR Files](classpath_jar.md)
- [Encapsulation](../02_oop_core_concepts/encapsulation.md)

