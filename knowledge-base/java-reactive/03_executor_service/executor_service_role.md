# ExecutorService Role

`ExecutorService` runs submitted tasks using managed worker threads.

Instead of creating a new `Thread` for each unit of work, application code
submits `Runnable` or `Callable` tasks to an executor. The executor owns the
thread pool, queue, lifecycle, and scheduling policy.

## Basic Shape

```java
ExecutorService executor = Executors.newFixedThreadPool(8);

executor.execute(() -> auditService.exportPendingEvents());
```

The task describes the work. The executor decides when a worker thread runs it.

## Why This Boundary Matters

An executor makes important production decisions visible:

- how much work can run concurrently;
- whether additional work waits in a queue;
- how threads are named;
- how the application shuts down;
- where failures are observed;
- how saturation affects callers.

Raw threads hide these decisions across scattered call sites.

## Thread Factory

Thread names should identify the pool in logs and thread dumps.

```java
ThreadFactory factory = Thread.ofPlatform()
    .name("invoice-export-", 0)
    .factory();

ExecutorService executor = Executors.newFixedThreadPool(4, factory);
```

For older Java versions, use a custom `ThreadFactory` implementation.

## Framework Boundary

In Spring applications, prefer framework-managed executors when possible. A
configured `TaskExecutor` or scheduler can participate in application lifecycle,
configuration, metrics, and shutdown.

## Key Idea

`ExecutorService` is not only a convenience API. It is a boundary for
concurrency limits, lifecycle, and operational behavior.
