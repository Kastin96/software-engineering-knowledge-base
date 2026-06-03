# Connection Pools and Threads

Pools protect expensive resources, but bad pool sizing can create incidents.

Common pools in Spring services:

- Tomcat or Jetty request threads;
- HikariCP database connections;
- HTTP client connections;
- Kafka consumer threads;
- executor pools for async work.

## HikariCP Example

```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 3000
```

Do not set the database pool to a huge number to "fix" slow queries. That often
makes the database slower.

## Request Threads

```yaml
server:
  tomcat:
    threads:
      max: 200
```

More request threads can increase concurrency, but they can also increase
pressure on the database and downstream services.

## Thread Starvation Symptoms

Symptoms:

- requests hang while CPU is not high;
- database pool is exhausted;
- thread dumps show many blocked threads;
- downstream calls wait for connections;
- latency rises before error rate rises.

## Practical Rule

Size pools together. A service with 200 request threads and 10 database
connections may be fine if most requests do not hit the database. It may be a
problem if every request holds a connection for a long query.

## Key Idea

Pool sizing is about the whole flow. Threads, database connections, HTTP
connections, and downstream capacity must make sense together.
