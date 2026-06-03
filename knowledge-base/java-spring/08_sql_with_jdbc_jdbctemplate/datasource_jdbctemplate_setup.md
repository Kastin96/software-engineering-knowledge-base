# DataSource and JdbcTemplate Setup

Spring Boot can auto-configure `DataSource` and `JdbcTemplate` when the JDBC
starter and a database driver are present.

## Typical Dependencies

```text
spring-boot-starter-jdbc
database driver, for example PostgreSQL, MySQL, or Oracle
```

## Basic Configuration

```yaml
spring:
  datasource:
    url: ${ORDERS_DB_URL}
    username: ${ORDERS_DB_USERNAME}
    password: ${ORDERS_DB_PASSWORD}
    hikari:
      maximum-pool-size: 20
```

Boot creates a `DataSource`. It can also provide a `JdbcTemplate` bean using
that data source.

## Repository Injection

```java
@Repository
class CustomerJdbcRepository {
    private final JdbcTemplate jdbcTemplate;

    CustomerJdbcRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }
}
```

## Multiple Data Sources

Multiple databases require explicit configuration. Avoid adding a second data
source casually; it affects transaction management, testing, operational
visibility, and failure modes.

## Key Idea

Let Spring Boot configure the common JDBC infrastructure, but keep database
settings explicit and environment-driven.
