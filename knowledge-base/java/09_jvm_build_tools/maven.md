# Maven

## Goal

Understand Maven project structure, dependencies, lifecycle phases, and common
commands.

## Why It Matters

Maven is widely used in Java backend projects. Even if an IDE hides much of it,
developers still need to read `pom.xml`, understand dependencies, run tests, and
debug build failures.

## Standard Project Layout

```text
project/
  pom.xml
  src/
    main/
      java/
      resources/
    test/
      java/
      resources/
```

This layout is one reason Maven projects are predictable.

## `pom.xml`

Minimal shape:

```xml
<project>
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.example</groupId>
  <artifactId>order-service</artifactId>
  <version>1.0.0</version>
</project>
```

- `groupId` identifies the organization or namespace.
- `artifactId` identifies the project artifact.
- `version` identifies the artifact version.

## Dependencies

```xml
<dependencies>
  <dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.2</version>
    <scope>test</scope>
  </dependency>
</dependencies>
```

Dependencies are downloaded from configured repositories and placed on the
compile or test classpath depending on scope.

## Common Scopes

- `compile`: available for main code and consumers; default scope.
- `test`: available only for tests.
- `provided`: needed to compile but provided by the runtime environment.
- `runtime`: not needed to compile main code but needed at runtime.

Use the narrowest scope that matches the dependency's purpose.

## Lifecycle Commands

```powershell
mvn compile
mvn test
mvn package
mvn clean package
```

- `compile` compiles main code.
- `test` runs tests.
- `package` builds the artifact, such as a JAR.
- `clean` removes generated build output.

## Wrapper

Many projects include Maven Wrapper:

```text
mvnw
mvnw.cmd
```

Use it when available:

```powershell
.\mvnw test
```

The wrapper helps the team use a consistent Maven version.

## Practical Example

```xml
<project>
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.example</groupId>
  <artifactId>hello-java</artifactId>
  <version>1.0.0</version>

  <properties>
    <maven.compiler.release>21</maven.compiler.release>
  </properties>
</project>
```

The `maven.compiler.release` property tells Maven which Java API level to compile
against.

## Common Mistakes

- Editing generated `target/` files instead of source files.
- Committing build output from `target/`.
- Adding dependencies with overly broad scopes.
- Ignoring dependency conflicts.
- Running a different Maven version than the project expects when a wrapper is
  available.

## Interview Questions

1. What is Maven used for?
2. What is `pom.xml`?
3. What is Maven's standard project layout?
4. What is the difference between `compile` and `test` dependency scope?
5. What does `mvn clean package` do?

## Practice

1. Create a minimal Maven `pom.xml`.
2. Add a test dependency with `test` scope.
3. Run `mvn test`.
4. Explain what appears in the `target/` directory.

## Related Topics

- [Gradle](gradle.md)
- [Classpath and JAR Files](classpath_jar.md)
- [Build Tool Best Practices](build_tool_best_practices.md)

