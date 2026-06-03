# Bean Scopes and Lifecycle

Most Spring beans are singletons.

In Spring, singleton means one bean instance per application context, not one
instance per JVM process.

## Singleton Scope

This service is created once and reused:

```java
@Service
class CurrencyService {
    BigDecimal convert(BigDecimal amount) {
        return amount;
    }
}
```

That is fine when the bean does not store request-specific mutable state.

## Avoid Mutable Request State In Singleton Beans

This is risky:

```java
@Service
class CurrentUserService {
    private String currentUserId;
}
```

A singleton service is shared across requests. Request-specific data should
usually come from method arguments, request objects, the security context, or
properly scoped infrastructure.

## Lifecycle Hooks

Occasionally a bean needs setup after dependency injection:

```java
@Component
class CacheWarmup {
    @PostConstruct
    void load() {
        // load small reference data if needed
    }
}
```

Use lifecycle hooks carefully. Heavy startup work can slow down deployment and
make failures harder to diagnose.

## Key Idea

Spring beans are usually shared components. Keep service beans stateless when
possible, and keep lifecycle hooks focused on lightweight initialization.
