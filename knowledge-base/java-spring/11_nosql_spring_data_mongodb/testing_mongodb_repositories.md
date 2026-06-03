# Testing MongoDB Repositories

MongoDB repository tests should verify mapping, queries, indexes, updates, and
aggregation behavior that matters to the application.

Mocking `MongoRepository` or `MongoTemplate` is usually not enough for
persistence behavior.

## What To Test

Test:

- document mapping;
- derived repository queries;
- custom `@Query` methods;
- `MongoTemplate` criteria queries;
- partial updates;
- aggregation pipelines;
- unique constraints and index-dependent behavior.

## Test Setup

Common approaches:

- Spring data test slices where appropriate;
- Testcontainers with MongoDB for realistic behavior;
- dedicated test database cleaned between tests;
- test fixtures that represent real document shapes.

## Example Focus

```java
@Test
void findsPaidOrdersForCustomer() {
    // insert documents
    // call repository or template method
    // assert returned documents and ordering
}
```

## Key Idea

Test MongoDB data access against MongoDB behavior. Query shape, mapping, and
indexes are part of production logic.
