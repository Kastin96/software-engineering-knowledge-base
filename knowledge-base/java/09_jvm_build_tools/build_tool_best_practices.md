# Build Tool Best Practices

## Goal

Learn practical habits for working with Java build tools in real projects.

## Why It Matters

Build configuration controls dependencies, tests, packaging, Java versions, and
repeatability. Small build mistakes can cause "works on my machine" problems,
dependency conflicts, slow CI, or runtime failures.

## Use the Wrapper

If a project includes a wrapper, use it.

Maven:

```powershell
.\mvnw test
```

Gradle:

```powershell
.\gradlew test
```

The wrapper makes builds more reproducible across machines and CI.

## Keep Generated Output Out of Git

Do not commit build output.

Common generated directories:

```text
target/
build/
out/
```

Source files, build scripts, wrapper files, and configuration belong in Git.
Generated class files and packaged artifacts usually do not.

## Pin Java Version Intentionally

Use build-tool configuration to make the Java version explicit.

Maven:

```xml
<properties>
  <maven.compiler.release>21</maven.compiler.release>
</properties>
```

Gradle:

```groovy
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}
```

This avoids accidental differences between developer machines.

## Keep Dependency Scopes Narrow

Use test dependencies only for tests.

Maven:

```xml
<scope>test</scope>
```

Gradle:

```groovy
testImplementation 'org.junit.jupiter:junit-jupiter:5.10.2'
```

Avoid putting everything on the main runtime classpath.

## Understand Dependency Conflicts

Large Java projects can pull multiple versions of the same library transitively.

Useful commands:

```powershell
mvn dependency:tree
```

```powershell
.\gradlew dependencies
```

Use these when debugging unexpected runtime behavior or version conflicts.

## Keep Builds Boring

Prefer clear, conventional build configuration.

Avoid:

- hidden custom scripts for normal compile/test/package steps;
- unnecessary plugins;
- environment-specific hardcoded paths;
- build logic that only works in one IDE.

The easier a build is to understand, the easier it is to fix in CI.

## Practical Checklist

Before adding or changing build config, ask:

- Is the Java version explicit?
- Is the dependency scope correct?
- Is the wrapper present and used?
- Is generated output ignored?
- Does CI run the same command developers run?
- Is this custom plugin or script truly needed?
- Can someone new understand the build file quickly?

## Common Mistakes

- Committing generated output.
- Using different Java versions locally and in CI.
- Adding dependencies to the wrong scope.
- Ignoring transitive dependency conflicts.
- Depending on IDE-only configuration.
- Making the build more complex than the project needs.

## Interview Questions

1. Why do projects use Maven or Gradle wrappers?
2. Why should generated build output not be committed?
3. Why should Java version be configured in the build?
4. How do you inspect dependency conflicts?
5. What makes a build reproducible?

## Practice

1. Add `target/`, `build/`, and `out/` to `.gitignore` in a sample project.
2. Configure a Java toolchain or compiler release.
3. Move a test-only dependency to test scope.
4. Run a dependency tree command and identify transitive dependencies.

## Related Topics

- [Maven](maven.md)
- [Gradle](gradle.md)
- [JVM Basics](jvm_basics.md)

