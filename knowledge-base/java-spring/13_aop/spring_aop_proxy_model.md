# Spring AOP Proxy Model

Spring AOP is proxy-based.

Spring creates a proxy around a bean and applies advice when calls go through
that proxy.

## Basic Flow

```text
caller -> Spring proxy -> advice -> target method
```

The caller interacts with the proxy, not directly with the target object.

## Self-Invocation Limitation

Self-invocation can bypass advice:

```java
@Service
class OrderService {
    public void outer() {
        inner();
    }

    @TimedOperation
    public void inner() {
    }
}
```

`outer()` calls `inner()` on the same object. That call may not pass through the
Spring proxy, so advice targeting `inner()` may not run.

## Method Execution Focus

Spring AOP is narrower than full AspectJ weaving. It is usually concerned with
method execution join points on Spring beans, not field access, constructor
calls, or arbitrary code locations.

## Interface and Class Proxies

Spring can use JDK dynamic proxies or class-based proxies depending on the bean
shape and configuration.

This can matter for final classes, final methods, visibility, and how the bean
is injected.

## Key Idea

Spring AOP works through Spring-managed proxies. If the call does not go through
the proxy, the advice may not apply.
