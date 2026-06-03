# Common Mistakes

AOP mistakes usually come from applying invisible behavior too broadly.

## Hiding Business Logic

Business behavior should be visible in services or domain code. Aspects should
handle cross-cutting infrastructure concerns.

## Overly Broad Pointcuts

```java
@Around("execution(* com.example..*(..))")
```

This can advise too much code and create surprising behavior. Narrow the target.

## Ignoring Self-Invocation

Internal method calls may bypass the Spring proxy, so advice may not run.

## Swallowing Exceptions In Around Advice

An aspect that catches and hides exceptions can change application semantics in
ways callers do not expect.

## Using AOP Instead Of Clear APIs

If a concern belongs to one workflow, call a method explicitly. AOP is for
repeated cross-cutting behavior.

## Forgetting Test Coverage

Aspects can affect many methods. Test both the advice logic and the pointcut
scope.

## Key Idea

AOP should make repeated infrastructure behavior consistent, not make application
behavior harder to see.
