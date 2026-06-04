# Data Access Exercises

## Exercise 1: JPA Query

Implement order search by customer id, status, and created date range.

Acceptance criteria:

- query is tested with `@DataJpaTest`;
- pagination is supported;
- required indexes are documented;
- no N+1 query appears for the response fields.

## Exercise 2: JdbcTemplate Report

Build a read-only report query with `JdbcTemplate`.

Acceptance criteria:

- query returns a projection;
- SQL is readable;
- mapper handles nulls intentionally;
- test verifies query result.

## Exercise 3: MongoDB Aggregation

Aggregate orders by status for a customer.

Acceptance criteria:

- aggregation is implemented with Spring Data MongoDB;
- result DTO is explicit;
- test covers empty and non-empty result sets.
