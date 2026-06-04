# REST and Validation Cheatsheet

## REST

- Controllers handle HTTP, not business workflows.
- Request DTOs model input.
- Response DTOs model output.
- Entities should not be returned directly.
- Use pagination for collections.
- Use stable error response shapes.

## Validation

```text
@Valid
@NotNull
@NotBlank
@NotEmpty
@Size
@Min
@Max
@Email
```

## Error Handling

Use `@RestControllerAdvice` for:

- validation errors;
- not found errors;
- business rule errors;
- unexpected errors.

## Quick Questions

- What status code should this failure return?
- Is the request shape different from the entity?
- Are validation errors useful to the client?
- Is the endpoint returning too much data?
