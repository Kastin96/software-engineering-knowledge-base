# Nested DTO Validation

Nested DTOs are validated only when validation cascades into them.

Use `@Valid` on nested fields or collection elements.

## Example

```java
public record CreateOrderRequest(
    @NotNull Long customerId,
    @NotEmpty List<@Valid CreateOrderItemRequest> items
) {
}

public record CreateOrderItemRequest(
    @NotNull Long productId,
    @Positive int quantity
) {
}
```

Without `@Valid` on the list element, Spring may validate that `items` is not
empty but skip constraints inside each item.

## Nested Object

```java
public record CreateCustomerRequest(
    @NotBlank String name,
    @Valid AddressRequest address
) {
}

public record AddressRequest(
    @NotBlank String city,
    @NotBlank String postalCode
) {
}
```

## Collection Size

For collection requests, validate both the collection and the nested objects:

```java
public record BulkCreateRequest(
    @NotEmpty
    @Size(max = 100)
    List<@Valid CreateUserRequest> users
) {
}
```

## Key Idea

For nested request models, validate the container and explicitly cascade
validation into nested values.
