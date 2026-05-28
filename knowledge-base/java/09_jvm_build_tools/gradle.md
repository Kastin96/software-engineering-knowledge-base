# Gradle

## Goal

Understand Gradle project structure, build scripts, dependencies, tasks, and the
Gradle wrapper.

## Why It Matters

Gradle is widely used in Java, Android, and many modern backend projects. It is
more flexible than Maven, but that flexibility makes it important to understand
the build script instead of treating it as magic.

## Standard Project Layout

Gradle often uses the same Java source layout as Maven:

```text
project/
  build.gradle
  settings.gradle
  src/
    main/
      java/
      resources/
    test/
      java/
      resources/
```

Kotlin DSL projects use:

```text
build.gradle.kts
settings.gradle.kts
```

## Basic Gradle Build

Groovy DSL:

```groovy
plugins {
    id 'java'
}

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}

repositories {
    mavenCentral()
}
```

This applies the Java plugin, configures a Java toolchain, and uses Maven Central
for dependencies.

## Dependencies

```groovy
dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter:5.10.2'
}
```

Common configurations:

- `implementation`: needed by main code but hidden from consumers when possible;
- `api`: exposed to consumers in library projects using the Java Library plugin;
- `runtimeOnly`: needed only at runtime;
- `testImplementation`: needed only for tests.

## Common Tasks

```powershell
gradle compileJava
gradle test
gradle build
gradle clean build
```

With wrapper:

```powershell
.\gradlew test
.\gradlew build
```

Use the wrapper when the project provides it.

## Gradle Wrapper

Wrapper files usually include:

```text
gradlew
gradlew.bat
gradle/wrapper/gradle-wrapper.properties
```

The wrapper helps the team use the expected Gradle version without requiring a
global Gradle install.

## Practical Example

Kotlin DSL:

```kotlin
plugins {
    java
}

java {
    toolchain {
        languageVersion.set(JavaLanguageVersion.of(21))
    }
}

repositories {
    mavenCentral()
}

dependencies {
    testImplementation("org.junit.jupiter:junit-jupiter:5.10.2")
}

tasks.test {
    useJUnitPlatform()
}
```

This is a realistic minimal Java project using JUnit 5.

## Common Mistakes

- Running global `gradle` when the project provides `gradlew`.
- Editing generated `build/` output.
- Confusing `implementation` and `testImplementation`.
- Adding dependencies without understanding which module needs them.
- Creating overly clever build logic for a simple project.

## Interview Questions

1. What is Gradle used for?
2. What is the Gradle wrapper?
3. What is the difference between `implementation` and `testImplementation`?
4. What does the Java plugin provide?
5. Why might a project use Kotlin DSL instead of Groovy DSL?

## Practice

1. Create a minimal Gradle Java project.
2. Add a JUnit test dependency.
3. Run tests with the wrapper.
4. Explain what appears in the `build/` directory.

## Related Topics

- [Maven](maven.md)
- [Classpath and JAR Files](classpath_jar.md)
- [Build Tool Best Practices](build_tool_best_practices.md)

