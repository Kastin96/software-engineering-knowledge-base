# SQL with JDBC and JdbcTemplate

This section covers relational database access with JDBC and Spring's
`JdbcTemplate`.

`JdbcTemplate` keeps SQL explicit while removing much of the repetitive JDBC
resource management. It is useful when the application needs direct control over
queries, joins, projections, and database-specific SQL behavior.

## Topics

- 01\. [JDBC and JdbcTemplate Role](jdbc_jdbctemplate_role.md)
- 02\. [DataSource and JdbcTemplate Setup](datasource_jdbctemplate_setup.md)
- 03\. [Query Methods](query_methods.md)
- 04\. [Row Mapping](row_mapping.md)
- 05\. [Parameters and Prepared Statements](parameters_prepared_statements.md)
- 06\. [Inserts, Updates, and Batch Operations](inserts_updates_batch.md)
- 07\. [Transactions with JDBC](transactions_with_jdbc.md)
- 08\. [Exception Translation](exception_translation.md)
- 09\. [Testing JDBC Repositories](testing_jdbc_repositories.md)
- 10\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with the role of JDBC and `JdbcTemplate`. Then review setup, query
methods, row mapping, and parameter binding. After that, study write operations,
transactions, exception translation, repository tests, and common mistakes.

## Mini Goal

By the end of this section, you should be able to implement a JDBC repository
that:

- uses `JdbcTemplate` instead of manual connection handling;
- keeps SQL readable and parameterized;
- maps rows into records or persistence objects;
- performs inserts and updates intentionally;
- participates in service-level transactions;
- translates database errors into application meaning;
- has focused repository tests.

## Interview Readiness

You should be able to answer:

- Why use `JdbcTemplate` instead of raw JDBC?
- When is JDBC a better fit than JPA?
- How do prepared statements reduce SQL injection risk?
- What is a `RowMapper`?
- Where should transactions be placed for JDBC repositories?
- How does Spring translate SQL exceptions?
- How would you test a JDBC repository?
