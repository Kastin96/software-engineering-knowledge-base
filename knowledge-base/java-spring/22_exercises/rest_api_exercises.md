# REST API Exercises

## Exercise 1: Create Product API

Build:

- `POST /api/products`;
- `GET /api/products/{id}`;
- `GET /api/products?page=0&size=20`.

Acceptance criteria:

- request validation exists;
- response DTO does not expose entity internals;
- list endpoint is paginated;
- not found returns a consistent error response;
- controller test covers success and validation failure.

## Exercise 2: Partial Update

Build `PATCH /api/products/{id}` for name and price changes.

Acceptance criteria:

- missing product returns `404`;
- invalid price returns `400`;
- unchanged fields stay unchanged;
- service method owns the transaction.

## Exercise 3: Search Endpoint

Build search by category and status.

Acceptance criteria:

- filters are optional;
- pagination is required;
- query behavior is covered by repository tests;
- endpoint does not return unbounded lists.
