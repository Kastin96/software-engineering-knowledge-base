# Uncaught Exceptions

An uncaught exception terminates the thread where it occurred.

It does not automatically fail the parent thread, restart the task, or make the
application healthy again. Background thread failures must be observed
deliberately.

## Default Handler

An uncaught exception handler gives the application a final reporting boundary.

```java
Thread worker = new Thread(task, "outbox-worker");
worker.setUncaughtExceptionHandler((thread, error) ->
    logger.error("Background thread failed: {}", thread.getName(), error)
);
worker.start();
```

This is not a recovery strategy by itself. It only makes the failure visible.

## Failure Semantics

For background work, decide what should happen after failure:

- log and stop;
- retry with limits;
- mark the application unhealthy;
- trigger an alert;
- hand the work back to a queue;
- let a supervisor restart the process.

Raw threads do not provide this policy. Higher-level frameworks or executors
usually make it easier to define.

## Avoid Broad Silent Catch Blocks

```java
try {
    task.run();
} catch (Exception ignored) {
    // Bad: the service loses the failure signal.
}
```

Silent failure is worse than a visible crash for many backend workflows.

## Key Idea

Every background execution boundary needs a failure policy. A thread that dies
silently is a production incident waiting to happen.
