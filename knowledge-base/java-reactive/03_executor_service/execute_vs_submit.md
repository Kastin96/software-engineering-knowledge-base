# execute versus submit

`execute` and `submit` both schedule work on an executor, but they expose
failures differently.

## execute

`execute` accepts a `Runnable` and does not return a handle.

```java
executor.execute(() -> outboxPublisher.publishPending());
```

If the task throws an unchecked exception, the worker thread's uncaught
exception handling path can observe it.

Use `execute` for fire-and-forget work only when there is a clear reporting
boundary inside the task or executor infrastructure.

## submit

`submit` returns a `Future`.

```java
Future<ExportResult> result = executor.submit(() ->
    reportExporter.export(reportId)
);
```

If the task fails, the exception is captured inside the `Future` and rethrown
from `get()` as an `ExecutionException`.

```java
try {
    ExportResult export = result.get(30, TimeUnit.SECONDS);
} catch (ExecutionException ex) {
    logger.error("Report export failed", ex.getCause());
}
```

## Common Failure Trap

Submitting a task and ignoring the returned `Future` can hide failures.

```java
executor.submit(() -> outboxPublisher.publishPending());
```

If nobody calls `get()` and the task does not log internally, the failure may be
lost.

## Practical Rule

Use `execute` when the task owns its own failure reporting. Use `submit` when the
caller needs a result, timeout, cancellation, or explicit failure handling.

## Key Idea

The method choice affects observability. Do not use `submit` as fire-and-forget
unless failure handling is deliberately handled elsewhere.
