# Core Concepts

AOP has a small set of important terms.

## Aspect

An aspect is the module that contains cross-cutting behavior.

```java
@Aspect
@Component
class TimingAspect {
}
```

## Join Point

A join point is a point where advice can run.

In Spring AOP, the practical join points are method executions on Spring-managed
beans.

## Pointcut

A pointcut selects which join points should be advised.

```java
@Pointcut("execution(* com.example.orders..*(..))")
void orderApplicationMethods() {
}
```

## Advice

Advice is the behavior that runs at a matched join point.

Advice can run before, after, after returning, after throwing, or around a
method execution.

## Target Object and Proxy

The target object is the actual bean. The proxy is the object Spring exposes so
it can apply advice before delegating to the target.

## Key Idea

A pointcut decides where an aspect applies. Advice defines what happens there.
