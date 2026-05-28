# File Import

## Problem

Read a line-based file, parse valid rows, collect invalid row numbers, and keep
I/O failures visible.

## Why This Example Matters

Imports are common in real systems. The important parts are resource handling,
line-by-line processing, validation, useful errors, and not loading huge files
into memory.

## Code

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.ArrayList;
import java.util.List;

public class UserCsvImporter {
    public ImportResult importUsers(Path path) {
        List<UserRow> users = new ArrayList<>();
        List<String> errors = new ArrayList<>();

        try (BufferedReader reader = Files.newBufferedReader(path, StandardCharsets.UTF_8)) {
            String line;
            int lineNumber = 0;

            while ((line = reader.readLine()) != null) {
                lineNumber++;

                if (line.isBlank()) {
                    continue;
                }

                String[] parts = line.split(",");

                if (parts.length != 2 || !parts[1].contains("@")) {
                    errors.add("line " + lineNumber + " is invalid");
                    continue;
                }

                users.add(new UserRow(parts[0].trim(), parts[1].trim().toLowerCase()));
            }

            return new ImportResult(List.copyOf(users), List.copyOf(errors));
        } catch (IOException exception) {
            throw new UserImportException("Could not import users from " + path, exception);
        }
    }

    public record UserRow(String id, String email) {
    }

    public record ImportResult(List<UserRow> users, List<String> errors) {
    }
}

class UserImportException extends RuntimeException {
    UserImportException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

## What It Demonstrates

- try-with-resources;
- UTF-8 text reading;
- line-by-line processing;
- import error collection;
- exception wrapping with cause.

## Practice

1. Add a header row and skip it.
2. Detect duplicate user ids.
3. Return the number of skipped blank lines.
4. Write tests for parsing logic by extracting a `parseLine` method.

