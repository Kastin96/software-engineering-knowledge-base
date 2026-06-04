# Data Access Cheatsheet

## JDBC / JdbcTemplate

Use when:

- SQL should be explicit;
- report query is easier as SQL;
- mapping is simple and controlled.

## JPA / Hibernate

Use when:

- domain entities and relationships matter;
- transaction-based persistence is useful;
- repository abstraction improves productivity.

Watch for:

- N+1 queries;
- lazy loading in API serialization;
- missing indexes;
- unclear transaction boundaries.

## MongoDB

Use when:

- document shape fits the access pattern;
- flexible read models are useful;
- aggregation or document queries match the problem.

## Rule

Choose data access by query shape, consistency needs, and operational reality,
not only by framework preference.
