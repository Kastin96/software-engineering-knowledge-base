# Spring Boot Test Slices

Test slices load a focused part of the Spring application context.

They are useful when framework behavior matters, but the whole application does
not need to start.

## Common Slices

```text
@WebMvcTest      -> MVC controllers, JSON mapping, validation, MVC config
@DataJpaTest     -> JPA repositories, entity mapping, persistence context
@JdbcTest        -> JDBC components
@DataMongoTest   -> MongoDB repositories and mapping
@WebFluxTest     -> WebFlux controllers
```

## Why Slices Help

Slices reduce:

- startup time;
- unrelated bean failures;
- test setup noise;
- accidental end-to-end coupling.

## Slice Limitation

A slice intentionally excludes many beans. If a controller depends on a service,
the service is usually mocked or explicitly imported.

That is the point: the test focuses on the web layer, not the whole application.

## When Not To Use A Slice

Use a fuller integration test when the behavior depends on multiple real layers
working together, such as controller, service, transaction, repository, and
database.

## Key Idea

Use test slices when you need Spring behavior for one layer without loading the
entire application.
