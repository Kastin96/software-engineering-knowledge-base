# Data Access Overview

This section covers data access decisions in Spring applications.

Before choosing JDBC, JPA, Spring Data, MongoDB, or another persistence tool,
you need a clear boundary: what part of the application owns queries, what part
owns transactions, how persistence models relate to API DTOs, and how database
failures are translated.

## Topics

- 01\. [Data Access Role](data_access_role.md)
- 02\. [Choosing an Access Style](choosing_access_style.md)
- 03\. [DataSource and Connection Pools](datasource_connection_pools.md)
- 04\. [Repositories and DAOs](repositories_daos.md)
- 05\. [Transaction Boundaries](transaction_boundaries.md)
- 06\. [Query Ownership](query_ownership.md)
- 07\. [SQL and NoSQL Boundaries](sql_nosql_boundaries.md)
- 08\. [Persistence Model Boundaries](persistence_model_boundaries.md)
- 09\. [Exception Translation](exception_translation_overview.md)
- 10\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with the role of the persistence layer and the access style options. Then
review connection pools, repositories, transactions, and query ownership. After
that, study SQL/NoSQL boundaries, persistence model boundaries, exception
translation, and common mistakes.

## Mini Goal

By the end of this section, you should be able to design a persistence layer
where:

- controllers do not access databases directly;
- services own transaction-level use cases;
- repositories or DAOs own persistence operations;
- access style is chosen based on query complexity and model shape;
- database exceptions are translated into application meaning;
- entities and database records do not leak into public API contracts.

## Interview Readiness

You should be able to answer:

- When would you use JDBC instead of JPA?
- What does a connection pool do?
- Where should transactions usually be placed?
- What should a repository be responsible for?
- Why should controllers not build queries?
- How do SQL and NoSQL persistence models differ?
- Why should database exceptions be translated?
