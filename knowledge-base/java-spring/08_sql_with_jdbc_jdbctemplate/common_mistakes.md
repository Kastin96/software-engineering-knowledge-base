# Common Mistakes

JDBC mistakes often come from either treating SQL as raw strings without
discipline or hiding database behavior behind vague repository methods.

## Concatenating User Input Into SQL

Always bind values as parameters. String concatenation creates SQL injection
risk and unstable query behavior.

## Using `select *`

Select only the columns needed by the mapper. This keeps row mapping explicit
and avoids pulling unnecessary data.

## Ignoring Affected Row Counts

Update and delete operations should often check affected rows. If zero rows were
updated, the resource may not exist or the state condition may not match.

## Putting SQL In Services

Services should express use-case behavior. Repositories should own SQL and row
mapping.

## Overusing QueryForObject

`queryForObject` is fine when exactly one result is expected. For optional
lookups, make missing-row behavior explicit with `Optional`.

## Testing Only With Mocks

Mocking `JdbcTemplate` does not verify SQL syntax, row mapping, constraints, or
database-specific behavior.

## Key Idea

With JDBC, SQL is part of the source code. Treat it as production logic: keep it
explicit, parameterized, reviewed, and tested.
