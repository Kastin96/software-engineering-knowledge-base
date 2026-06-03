# DataSource and Connection Pools

A `DataSource` provides database connections to the application.

In production, a Spring Boot service usually uses a connection pool. The pool
keeps a limited set of reusable database connections instead of opening a new
connection for every query.

## Why Pools Matter

Connection pools help control:

- database connection count;
- connection reuse;
- request latency;
- resource exhaustion;
- behavior under load.

## Typical Configuration

```yaml
spring:
  datasource:
    url: ${ORDERS_DB_URL}
    username: ${ORDERS_DB_USERNAME}
    password: ${ORDERS_DB_PASSWORD}
    hikari:
      maximum-pool-size: 20
      connection-timeout: 2s
```

Exact settings depend on database capacity, service concurrency, query latency,
and deployment topology.

## Common Production Issue

If the pool is too small, requests wait for connections. If it is too large, the
database may be overloaded by too many concurrent connections.

Pool sizing should be coordinated with database limits and service concurrency.

## Key Idea

The connection pool is part of service capacity planning. It is not just a
default configuration detail.
