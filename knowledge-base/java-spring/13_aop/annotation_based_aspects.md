# Annotation-Based Aspects

Annotation-based aspects make AOP targeting explicit at the method or class
level.

This is often clearer than broad package-based pointcuts.

## Marker Annotation

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Audited {
    String action();
}
```

## Usage

```java
@Service
class OrderService {
    @Audited(action = "cancel-order")
    public void cancelOrder(Long orderId) {
        // use-case behavior
    }
}
```

## Aspect

```java
@Aspect
@Component
class AuditAspect {
    @AfterReturning("@annotation(audited)")
    void auditSuccess(Audited audited) {
        auditLogger.record(audited.action(), "SUCCESS");
    }
}
```

## Why It Helps

The advised method clearly declares that the cross-cutting behavior applies.
This reduces the surprise factor compared with package-wide pointcuts.

## Key Idea

Use annotation-based aspects when the method owner should explicitly opt in to
cross-cutting behavior.
