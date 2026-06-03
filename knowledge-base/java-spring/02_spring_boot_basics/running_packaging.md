# Running and Packaging

Spring Boot applications are commonly packaged as executable jars.

The jar contains the application classes and runtime dependencies, making it
straightforward to run in local environments, CI, containers, and servers.

## Running Locally

With Maven:

```text
mvn spring-boot:run
```

With Gradle:

```text
./gradlew bootRun
```

For local development, this is convenient because the build tool manages the
runtime classpath.

## Building A Jar

With Maven:

```text
mvn clean package
```

With Gradle:

```text
./gradlew clean bootJar
```

Then run:

```text
java -jar target/orders-service.jar
```

The exact jar path depends on the build tool and project configuration.

## Runtime Configuration

Profiles and properties are usually supplied outside the jar:

```text
SPRING_PROFILES_ACTIVE=prod java -jar orders-service.jar
```

This keeps the same artifact deployable to different environments.

## Key Idea

Build one application artifact, then vary runtime behavior through environment
configuration rather than rebuilding the service for each environment.
