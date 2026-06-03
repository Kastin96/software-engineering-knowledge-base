# Common Mistakes

Data access mistakes usually show up as tight coupling, unclear transaction
boundaries, or production performance problems.

## Controllers Calling Repositories Directly

This skips the service layer where authorization, transactions, and use-case
logic usually belong.

## Hiding Queries In Services

Services should not assemble SQL strings or know database column names. Keep
query construction in repositories or DAOs.

## Treating JPA, JDBC, And MongoDB As Interchangeable

Each access style has different trade-offs. Switching technologies changes
modeling, transactions, query behavior, and performance characteristics.

## No Transaction Ownership

If transactions are scattered across repositories, use cases can become
partially committed. Put transaction boundaries around complete service
operations.

## Exposing Persistence Models

Entities and records should not leak into API responses unless that coupling is
intentional and accepted.

## Ignoring Connection Pool Limits

Database capacity is finite. Connection pool settings should match service
traffic and database limits.

## Key Idea

Data access design is not only about reading and writing data. It defines
boundaries, consistency, performance, and operational behavior.
