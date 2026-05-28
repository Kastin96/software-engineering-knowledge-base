# JVM Basics

## Goal

Understand what the JVM does and how Java source code becomes a running program.

## Why It Matters

Java developers do not only write `.java` files. They work with compiled
bytecode, dependencies, memory settings, build tools, and runtime behavior. A
basic JVM mental model helps with debugging, interviews, performance issues, and
project setup.

## JDK, JRE, and JVM

The JVM runs Java bytecode.

The JRE is the runtime environment needed to run Java applications. Modern JDK
distributions usually include the runtime pieces needed for development and
execution.

The JDK is the development kit. It includes tools such as:

- `javac` for compiling Java source;
- `java` for running Java programs;
- `jar` for packaging archives;
- debugging and diagnostic tools.

In daily development, install a JDK.

## Source Code to Running Program

```text
Main.java -> javac -> Main.class -> java -> running JVM process
```

Java source code is compiled into bytecode. The JVM loads and executes that
bytecode.

## Bytecode

Bytecode is the compiled instruction format understood by the JVM.

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

Compiling this creates `Main.class`.

```powershell
javac Main.java
java Main
```

The `.class` file contains bytecode, not source code.

## Platform Independence

Java source code can be compiled once and run on any compatible JVM.

```text
Java source -> bytecode -> JVM for Windows/Linux/macOS
```

The JVM hides many operating-system differences, but not all runtime concerns.
File paths, encodings, native libraries, and environment configuration still
matter.

## JIT Compilation

The JVM can interpret bytecode and also compile frequently used code into native
machine code at runtime. This is called just-in-time compilation.

You do not usually control JIT behavior directly in everyday application code,
but it explains why Java programs may warm up under load.

## Practical Example

```java
public class RuntimeInfo {
    public static void main(String[] args) {
        System.out.println("Java version: " + System.getProperty("java.version"));
        System.out.println("JVM name: " + System.getProperty("java.vm.name"));
        System.out.println("OS: " + System.getProperty("os.name"));
    }
}
```

This prints runtime information from the JVM process.

## Common Mistakes

- Installing only a runtime when development tools require a JDK.
- Thinking `.java` files are run directly in ordinary compiled Java projects.
- Confusing Java language version with JVM runtime version.
- Assuming platform independence means path and environment issues disappear.
- Ignoring runtime flags and memory settings in production.

## Interview Questions

1. What does the JVM do?
2. What is bytecode?
3. What is the difference between JDK and JVM?
4. Why is Java often described as platform independent?
5. What is JIT compilation?

## Practice

1. Compile and run a `Main.java` file from the command line.
2. Locate the generated `.class` file.
3. Print Java runtime properties with `System.getProperty`.
4. Explain the path from source code to a running JVM process.

## Related Topics

- [Compile and Run](compile_run.md)
- [Classpath and JAR Files](classpath_jar.md)
- [Memory: Stack and Heap](memory_stack_heap.md)

