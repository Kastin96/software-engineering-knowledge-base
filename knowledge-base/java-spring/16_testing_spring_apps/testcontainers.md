# Testcontainers

Testcontainers runs real infrastructure in containers during tests.

It is useful when behavior depends on the actual database, message broker, or
external service protocol.

## PostgreSQL Example

```java
@Testcontainers
@DataJpaTest
class OrderRepositoryContainerTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:16");

    @DynamicPropertySource
    static void databaseProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }
}
```

## When To Use

Use Testcontainers for:

- database-specific SQL;
- real MongoDB queries and aggregation;
- Kafka integration tests;
- migration verification;
- behavior that embedded databases cannot represent.

## Cost

Containers add startup time and operational requirements. Use them where
realism matters, not for every unit test.

## Key Idea

Use Testcontainers when infrastructure behavior is part of what the test must
prove.
