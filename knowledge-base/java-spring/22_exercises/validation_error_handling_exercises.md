# Validation and Error Handling Exercises

## Exercise 1: Validation Groups

Create separate validation behavior for create and update requests.

Acceptance criteria:

- create requires all mandatory fields;
- update allows partial changes;
- validation errors use one response format.

## Exercise 2: Business Exception

Add `OrderCannotBeCancelledException`.

Acceptance criteria:

- cancelled or shipped orders cannot be cancelled again;
- API returns a business error code;
- logs do not expose sensitive request data.

## Exercise 3: Error Response Contract

Design one error response shape for validation, not found, and business errors.

Acceptance criteria:

- every error has a stable code;
- validation errors include field-level details;
- tests verify response JSON structure.
