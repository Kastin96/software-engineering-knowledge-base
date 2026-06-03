# Pointcuts

Pointcuts define where advice applies.

Spring AOP uses AspectJ-style pointcut expressions, but Spring AOP supports a
narrower runtime model focused on method execution.

## Execution Pointcut

```java
@Pointcut("execution(* com.example.orders..*(..))")
void orderPackageMethods() {
}
```

This matches method executions under the `com.example.orders` package.

## Annotation Pointcut

Annotation pointcuts are often easier to control:

```java
@Pointcut("@annotation(com.example.audit.Audited)")
void auditedMethods() {
}
```

Only methods explicitly annotated with `@Audited` are advised.

## Bean Pointcut

```java
@Pointcut("bean(*Service)")
void serviceBeans() {
}
```

This matches Spring beans by name pattern.

## Keep Pointcuts Narrow

Broad pointcuts can apply advice to methods that were never intended to be
advised.

Prefer explicit package, annotation, or bean boundaries over global expressions.

## Key Idea

Pointcuts are runtime targeting rules. Keep them readable, narrow, and aligned
with the concern being applied.
