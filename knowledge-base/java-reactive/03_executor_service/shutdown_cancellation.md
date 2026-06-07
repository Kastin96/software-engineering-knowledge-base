# Shutdown and Cancellation

Executors must be shut down deliberately.

Without shutdown, non-daemon worker threads can keep the JVM alive. During
application shutdown, unfinished tasks can also lose work unless cancellation
and cleanup are handled intentionally.

## Basic Shutdown

```java
executor.shutdown();

if (!executor.awaitTermination(30, TimeUnit.SECONDS)) {
    executor.shutdownNow();
}
```

`shutdown()` stops accepting new tasks and lets submitted tasks finish.
`shutdownNow()` attempts to interrupt running tasks and returns tasks that never
started.

## Preserve Interruption

```java
try {
    if (!executor.awaitTermination(30, TimeUnit.SECONDS)) {
        executor.shutdownNow();
    }
} catch (InterruptedException ex) {
    executor.shutdownNow();
    Thread.currentThread().interrupt();
}
```

Shutdown code should not swallow interruption.

## Cancelling A Future

```java
Future<?> task = executor.submit(() -> reportExporter.export(reportId));

boolean cancelled = task.cancel(true);
```

Passing `true` requests interruption if the task is running. The task still has
to cooperate with interruption.

## Task Design

Tasks should:

- use timeouts for external calls;
- check interruption in long loops;
- release resources in `finally` blocks;
- avoid unbounded blocking;
- make retry and partial work behavior explicit.

## Key Idea

Executor shutdown is part of application correctness. A pool that starts work
must also have a clear policy for stopping work.
