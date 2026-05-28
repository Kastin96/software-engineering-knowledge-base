# Classpath and JAR Files

## Goal

Understand how Java finds classes and how JAR files package compiled code.

## Why It Matters

Many Java errors are classpath errors: missing dependencies, wrong versions,
wrong main class, or classes compiled into unexpected folders. Build tools hide
the details most of the time, but the mental model is still useful.

## Classpath

The classpath tells the JVM where to look for compiled classes and JAR files.

```powershell
java -cp out com.example.app.Main
```

Here `out` is a directory containing compiled `.class` files.

## Fully Qualified Class Names

If a class has a package:

```java
package com.example.app;

public class Main {
}
```

Its fully qualified name is:

```text
com.example.app.Main
```

The JVM uses this name to locate the class on the classpath.

## JAR Files

A JAR is a ZIP-based archive that packages compiled classes and resources.

Create a JAR:

```powershell
jar --create --file app.jar -C out .
```

Use a JAR on the classpath:

```powershell
java -cp app.jar com.example.app.Main
```

## Executable JAR

An executable JAR has a manifest that declares the main class.

```text
Main-Class: com.example.app.Main
```

Run:

```powershell
java -jar app.jar
```

Build tools normally create executable JARs for you.

## Dependencies

If your program depends on another JAR, it must be available at runtime.

```powershell
java -cp "app.jar;libs/library.jar" com.example.app.Main
```

On Windows, classpath entries are separated by `;`. On Linux/macOS, they are
usually separated by `:`.

Build tools handle this difference for most projects.

## Practical Example

```text
project/
  out/
    com/example/app/Main.class
  app.jar
```

Run from compiled classes:

```powershell
java -cp out com.example.app.Main
```

Run from a JAR on classpath:

```powershell
java -cp app.jar com.example.app.Main
```

Run executable JAR:

```powershell
java -jar app.jar
```

These are different launch styles.

## Common Mistakes

- Confusing source path with classpath.
- Forgetting dependency JARs at runtime.
- Running a packaged class with only its short name.
- Assuming `java -jar` also uses `-cp` entries the way classpath mode does.
- Committing generated `.class` or build output.

## Interview Questions

1. What is the classpath?
2. What is a fully qualified class name?
3. What is a JAR file?
4. What makes a JAR executable?
5. Why can code compile but fail with `ClassNotFoundException` at runtime?

## Practice

1. Compile a packaged class into `out`.
2. Run it with `java -cp out`.
3. Package compiled classes into a JAR.
4. Explain how Maven or Gradle simplifies classpath handling.

## Related Topics

- [Compile and Run](compile_run.md)
- [Maven](maven.md)
- [Gradle](gradle.md)

