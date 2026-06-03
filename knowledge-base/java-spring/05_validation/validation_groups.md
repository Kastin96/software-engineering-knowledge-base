# Validation Groups

Validation groups allow different constraints to apply in different contexts.

They can be useful for create versus update operations, but they also make DTOs
harder to read if overused.

## Example

```java
interface Create {
}

interface Update {
}
```

```java
public record UserRequest(
    @Null(groups = Create.class)
    @NotNull(groups = Update.class)
    Long id,

    @NotBlank
    String email
) {
}
```

```java
@PostMapping
UserResponse create(@Validated(Create.class) @RequestBody UserRequest request) {
    return userService.create(request);
}

@PutMapping("/{id}")
UserResponse update(@Validated(Update.class) @RequestBody UserRequest request) {
    return userService.update(request);
}
```

## Trade-Off

Groups reduce DTO duplication but increase cognitive load. Separate request
types are often clearer:

```java
CreateUserRequest
UpdateUserRequest
```

## Key Idea

Validation groups are useful for shared DTOs with context-specific constraints,
but separate DTOs are often more maintainable.
