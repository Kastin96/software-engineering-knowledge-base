# Common Mistakes

Validation mistakes often come from mixing input shape checks with business
rules or assuming annotations cover more than they actually do.

## Missing `@Valid` On Request Body

Constraints on a DTO do not run unless validation is triggered.

```java
ResponseEntity<UserResponse> create(@RequestBody CreateUserRequest request)
```

Use:

```java
ResponseEntity<UserResponse> create(@Valid @RequestBody CreateUserRequest request)
```

## Missing Nested Validation

Validating a collection does not automatically validate each nested element.
Use `List<@Valid ItemRequest>` when item constraints matter.

## Using `@Size` Without Requiredness

`@Size(max = 100)` allows `null`. Add `@NotBlank`, `@NotNull`, or `@NotEmpty`
when the value is required.

## Putting Persistence Checks In Constraints

Repository calls inside validators can create hidden database access during
request binding and blur error semantics. Keep stateful rules in services.

## Reusing One DTO Everywhere

One DTO for create, update, patch, and response often becomes full of nullable
fields and validation groups. Separate DTOs are usually clearer.

## Key Idea

Validation should make the API contract explicit. It should not hide business
workflow or persistence decisions behind annotations.
