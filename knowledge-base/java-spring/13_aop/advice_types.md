# Advice Types

Advice defines what an aspect does at a matched join point.

## Before Advice

Runs before the method.

```java
@Before("@annotation(Audited)")
void beforeAuditedMethod() {
    // record intent before method execution
}
```

## After Returning Advice

Runs after successful return.

```java
@AfterReturning(pointcut = "@annotation(Audited)", returning = "result")
void afterSuccess(Object result) {
    // record success
}
```

## After Throwing Advice

Runs when the method throws.

```java
@AfterThrowing(pointcut = "@annotation(Audited)", throwing = "ex")
void afterFailure(Throwable ex) {
    // record failure
}
```

## Around Advice

Wraps the method execution and can control whether and when the method proceeds.

```java
@Around("@annotation(TimedOperation)")
Object time(ProceedingJoinPoint joinPoint) throws Throwable {
    return joinPoint.proceed();
}
```

## Key Idea

Use the least powerful advice type that fits. `@Around` is flexible but easier
to misuse.
