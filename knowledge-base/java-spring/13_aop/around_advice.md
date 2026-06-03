# Around Advice

`@Around` advice wraps a method execution.

It can run code before and after the method, inspect arguments, catch
exceptions, change the return value, or skip method execution entirely.

## Timing Example

```java
@Aspect
@Component
class TimingAspect {
    @Around("@annotation(TimedOperation)")
    Object time(ProceedingJoinPoint joinPoint) throws Throwable {
        long started = System.nanoTime();

        try {
            return joinPoint.proceed();
        } finally {
            long elapsed = System.nanoTime() - started;
            String method = joinPoint.getSignature().toShortString();
            log.info("method={} elapsedNanos={}", method, elapsed);
        }
    }
}
```

## Proceed Exactly Once

Most around advice should call `joinPoint.proceed()` exactly once.

Calling it zero times skips the method. Calling it multiple times invokes the
method multiple times. Both are valid only for very specific use cases.

## Exception Handling

Do not swallow exceptions unless the aspect's purpose is explicitly to provide a
fallback.

```java
try {
    return joinPoint.proceed();
} catch (Throwable ex) {
    // log and rethrow unless fallback is intentional
    throw ex;
}
```

## Key Idea

`@Around` advice is powerful because it controls method execution. Use it
carefully and keep behavior obvious.
