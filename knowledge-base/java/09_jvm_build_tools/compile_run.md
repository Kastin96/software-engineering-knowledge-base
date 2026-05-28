# Compile and Run

## Goal

Understand how to compile and run simple Java programs without a build tool.

## Why It Matters

Maven and Gradle automate most project work, but knowing the raw `javac` and
`java` flow helps you understand what build tools are doing and makes basic
debugging easier.

## Single File

Example file:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, Java");
    }
}
```

Compile:

```powershell
javac Main.java
```

Run:

```powershell
java Main
```

`javac` creates `Main.class`. `java Main` runs the class named `Main`.

## Command-Line Arguments

```java
public class ArgsExample {
    public static void main(String[] args) {
        if (args.length == 0) {
            System.out.println("No name provided");
            return;
        }

        System.out.println("Hello, " + args[0]);
    }
}
```

Run:

```powershell
javac ArgsExample.java
java ArgsExample Alex
```

`args` receives values after the class name.

## Packages

Package declaration:

```java
package com.example.app;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello from package");
    }
}
```

Folder layout:

```text
src/com/example/app/Main.java
```

Compile to an output directory:

```powershell
javac -d out src/com/example/app/Main.java
```

Run with classpath:

```powershell
java -cp out com.example.app.Main
```

The fully qualified class name includes the package.

## Multiple Files

```text
src/com/example/app/Main.java
src/com/example/app/MessageService.java
```

Compile all Java files:

```powershell
javac -d out src/com/example/app/*.java
```

In real projects, build tools handle this.

## Practical Example

```java
package com.example.app;

public class MessageService {
    public String messageFor(String name) {
        return "Hello, " + name;
    }
}
```

```java
package com.example.app;

public class Main {
    public static void main(String[] args) {
        MessageService service = new MessageService();
        String name = args.length == 0 ? "Guest" : args[0];

        System.out.println(service.messageFor(name));
    }
}
```

This example shows why packages and output directories matter when running Java
outside a build tool.

## Common Mistakes

- Running `java Main.java` in a normal compiled project flow.
- Forgetting that public class names must match file names.
- Running a packaged class without its fully qualified name.
- Forgetting `-cp out` when compiled classes are not in the current directory.
- Mixing source files and generated `.class` files in the same committed folder.

## Interview Questions

1. What does `javac` do?
2. What does the `java` command do?
3. What does `-d out` do during compilation?
4. Why does a packaged class need a fully qualified name when running?
5. Why do build tools matter if `javac` exists?

## Practice

1. Compile and run a single-file Java program.
2. Add command-line argument handling.
3. Move the class into a package and compile with `-d out`.
4. Run the packaged class with `-cp out`.

## Related Topics

- [JVM Basics](jvm_basics.md)
- [Classpath and JAR Files](classpath_jar.md)
- [Packages and Modules](packages_modules.md)

