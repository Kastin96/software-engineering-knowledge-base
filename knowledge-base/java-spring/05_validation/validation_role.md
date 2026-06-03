# Validation Role

Validation is the first contract check at the application boundary.

For a REST API, validation should answer: "Is this request structurally
acceptable enough to enter the application layer?"

## Good Validation Targets

Request validation is appropriate for:

- required fields;
- string length;
- numeric ranges;
- email or URL shape;
- enum-like values;
- collection size;
- nested object structure;
- basic date constraints.

```java
public record CreateUserRequest(
    @NotBlank String email,
    @Size(max = 100) String displayName
) {
}
```

## What Usually Belongs Elsewhere

Rules that require database state or business workflow usually belong in the
service or domain layer:

- email must be unique;
- order can be cancelled only in specific states;
- user has permission to update a resource;
- account balance is sufficient;
- discount can be applied only for a contract type.

Those checks are not just input shape. They are application behavior.

## Key Idea

Validation protects the boundary. Business rules protect the use case.
