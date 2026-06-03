# Bean Validation Basics

Spring Boot integrates with Jakarta Bean Validation when validation support is
on the classpath.

In Boot applications, this is commonly added through:

```text
spring-boot-starter-validation
```

## Common Constraints

```java
public record CreateOrderRequest(
    @NotNull Long customerId,
    @NotEmpty List<CreateOrderItemRequest> items,
    @Size(max = 500) String note
) {
}
```

Common annotations:

- `@NotNull` requires a non-null value;
- `@NotBlank` requires non-null text with non-whitespace content;
- `@NotEmpty` requires a non-null collection, map, array, or string with content;
- `@Size` checks string, collection, map, or array size;
- `@Min` and `@Max` check numeric ranges;
- `@Positive` and `@PositiveOrZero` check positive numbers;
- `@Email` checks email-like shape;
- `@Pattern` checks a regular expression.

## Null Handling

Many constraints ignore `null`. For example, `@Size(max = 100)` allows `null`.
Combine it with `@NotNull` or `@NotBlank` when the value is required.

```java
public record CreateTagRequest(
    @NotBlank
    @Size(max = 40)
    String name
) {
}
```

## Key Idea

Bean Validation expresses structural input constraints. Be explicit about both
requiredness and allowed shape.
