# Scheduled Executors

`ScheduledExecutorService` runs tasks after a delay or on a recurring schedule.

It is useful for periodic background work such as cleanup, polling, cache
refresh, retry scans, and maintenance tasks.

## One-Time Delay

```java
ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(2);

scheduler.schedule(
    () -> retryService.retryFailedDeliveries(),
    30,
    TimeUnit.SECONDS
);
```

## Fixed Rate versus Fixed Delay

`scheduleAtFixedRate` tries to keep a regular start interval.

```java
scheduler.scheduleAtFixedRate(
    metricsReporter::flush,
    0,
    10,
    TimeUnit.SECONDS
);
```

`scheduleWithFixedDelay` waits until one run completes, then waits the delay
before starting the next run.

```java
scheduler.scheduleWithFixedDelay(
    outboxPublisher::publishPending,
    0,
    5,
    TimeUnit.SECONDS
);
```

For work that may take variable time, fixed delay is often safer because runs do
not overlap.

## Failure Behavior

If a recurring scheduled task throws an exception, future executions can be
suppressed. Catch and report expected failures inside the task.

```java
scheduler.scheduleWithFixedDelay(() -> {
    try {
        outboxPublisher.publishPending();
    } catch (Exception ex) {
        logger.error("Outbox publish cycle failed", ex);
    }
}, 0, 5, TimeUnit.SECONDS);
```

## Key Idea

Scheduled executors are lifecycle-sensitive infrastructure. Treat recurring task
failure, overlap, shutdown, and observability as part of the design.
