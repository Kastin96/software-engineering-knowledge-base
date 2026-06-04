# Testing Cheatsheet

## Test Scope

- Pure logic: unit test.
- Controller contract: `@WebMvcTest` or `@WebFluxTest`.
- Repository behavior: `@DataJpaTest` or `@DataMongoTest`.
- Full business flow: `@SpringBootTest`.
- Kafka or database integration: Testcontainers where needed.

## Good Tests

- assert behavior;
- use clear test data;
- cover failure paths;
- avoid testing framework internals;
- keep full-context tests limited.

## Spring Test Tools

```text
MockMvc
WebTestClient
StepVerifier
@MockitoBean
@DynamicPropertySource
Testcontainers
```

## Quick Questions

- What behavior is being proven?
- Does this test need Spring context?
- Is the dependency mocked or real for a reason?
- Is there a failure-path test?
