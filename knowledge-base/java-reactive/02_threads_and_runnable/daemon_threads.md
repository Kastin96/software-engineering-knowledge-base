# Daemon Threads

A daemon thread does not keep the JVM alive.

When only daemon threads remain, the JVM can exit. This makes daemon threads
useful for supporting work, but risky for business-critical processing.

## Creating A Daemon Thread

```java
Thread metricsReporter = new Thread(
    () -> metricsClient.flushPeriodically(),
    "metrics-reporter"
);
metricsReporter.setDaemon(true);
metricsReporter.start();
```

The daemon flag must be set before the thread starts.

## Appropriate Usage

Daemon threads can be appropriate for:

- metrics reporters;
- cache cleanup helpers;
- internal monitoring helpers;
- best-effort supporting tasks.

They are usually inappropriate for:

- payment processing;
- outbox publishing;
- audit log delivery;
- database migrations;
- any task that must complete before shutdown.

## Shutdown Risk

Daemon threads may be stopped when the JVM exits without completing cleanup.
They should not own critical data consistency work.

For important background jobs, prefer a lifecycle-managed executor or scheduler
with explicit shutdown behavior.

## Key Idea

Use daemon threads only for work that is safe to abandon when the JVM exits.
