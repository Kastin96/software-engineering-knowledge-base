# JSON Basics

## Goal

Understand how Java applications usually work with JSON and why JSON should be
handled with a library.

## Why It Matters

JSON is one of the most common formats in backend development. Java services use
it for REST APIs, configuration, events, logs, tests, and integration payloads.
Correct JSON handling matters for escaping, types, dates, unknown fields, and
security.

## JSON Is Text

Example JSON:

```json
{
  "id": "u-100",
  "email": "alex@example.com",
  "active": true
}
```

JSON values can be objects, arrays, strings, numbers, booleans, or `null`.

## Use a Library

Do not build JSON with string concatenation.

```java
// Avoid:
String json = "{\"email\":\"" + email + "\"}";
```

This breaks when values contain quotes, backslashes, newlines, or other special
characters.

Use a JSON library such as Jackson or Gson in real projects. Spring Boot uses
Jackson by default for HTTP JSON serialization and deserialization.

## Jackson Example

This example assumes Jackson is available as a project dependency.

```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class JsonExample {
    public static void main(String[] args) throws Exception {
        ObjectMapper objectMapper = new ObjectMapper();

        User user = new User("u-100", "alex@example.com", true);
        String json = objectMapper.writeValueAsString(user);

        User parsed = objectMapper.readValue(json, User.class);

        System.out.println(json);
        System.out.println(parsed);
    }

    record User(String id, String email, boolean active) {
    }
}
```

This is the realistic approach in most Java backend projects: model data with a
class or record and let the library handle JSON syntax.

## JSON Arrays

```java
String json = """
        [
          {"id": "u-100", "email": "alex@example.com"},
          {"id": "u-200", "email": "sam@example.com"}
        ]
        """;
```

With Jackson, generic collections need type information.

```java
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;

java.util.List<User> users = objectMapper.readValue(
        json,
        new TypeReference<java.util.List<User>>() {}
);
```

This is needed because Java generics use type erasure.

## Dates and Unknown Fields

Decide how dates should be represented. ISO-8601 strings are common.

```json
{
  "createdAt": "2026-05-28T10:15:30Z"
}
```

Also decide how your application handles unknown fields. Public APIs often need
clear compatibility rules.

## Practical Example

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import java.nio.file.Files;
import java.nio.file.Path;

public class UserJsonFile {
    private final ObjectMapper objectMapper = new ObjectMapper();

    public User readUser(Path path) {
        try {
            String json = Files.readString(path);
            return objectMapper.readValue(json, User.class);
        } catch (Exception exception) {
            throw new UserJsonException("Could not read user JSON from " + path, exception);
        }
    }

    record User(String id, String email, boolean active) {
    }
}

class UserJsonException extends RuntimeException {
    UserJsonException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

The method keeps file and JSON failures visible with contextual error handling.

## Common Mistakes

- Building JSON with string concatenation.
- Parsing JSON with ad hoc string operations.
- Forgetting that generic collection parsing needs type information.
- Ignoring date/time format decisions.
- Swallowing parsing errors and returning partially valid data.
- Trusting JSON input without validation.

## Interview Questions

1. Why should JSON be handled with a library?
2. What are common JSON value types?
3. Why does parsing `List<User>` need extra type information?
4. How should date/time formats be handled in JSON APIs?
5. Why is JSON parsing not the same thing as validation?

## Practice

1. Create a `User` record that maps to JSON fields.
2. Serialize a user to JSON using Jackson.
3. Parse a JSON array into `List<User>`.
4. Add validation after parsing a JSON object.

## Related Topics

- [Files and Paths](files_paths.md)
- [Type Erasure](../05_generics_and_type_system/type_erasure.md)
- [Exceptions and Debugging](../04_exceptions_and_debugging/README.md)

