# Common Misconceptions

Reactive and asynchronous code often fail in production because the model is
misunderstood rather than because the library is weak.

## Reactive Means Faster

Reactive code is not automatically faster.

It can improve scalability for I/O-heavy non-blocking systems, but it can also
add overhead and complexity. For simple blocking CRUD services, Spring MVC may
be easier to build, test, and operate.

## Async Means Non-Blocking

Async code can still block a thread.

Submitting a JDBC call to an executor makes the caller asynchronous, but the
JDBC call still occupies a thread until it completes.

## Virtual Threads Replace All Reactive Code

Virtual threads reduce the cost of blocking I/O, but they do not provide
backpressure, stream operators, or reactive composition semantics.

They are a strong option for many service workloads, not a universal replacement
for reactive streams.

## WebFlux Fixes Slow Databases

WebFlux does not make a slow database query fast.

If the bottleneck is query design, missing indexes, lock contention, or an
undersized connection pool, changing the web framework will not solve the
underlying problem.

## Calling block Is Just A Shortcut

Calling `block()` inside a reactive request path can consume event-loop threads
and break the non-blocking model.

If blocking is necessary, isolate it deliberately and use the appropriate
scheduler or choose a blocking architecture.

## Key Idea

Reactive code is valuable when its assumptions match the system. Treat execution
model mismatches as architecture problems, not syntax problems.
