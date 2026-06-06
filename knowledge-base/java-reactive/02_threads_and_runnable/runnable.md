# Runnable

`Runnable` represents a unit of work that takes no arguments and returns no
value.

It is the simplest task abstraction used by `Thread`, executors, schedulers, and
many framework internals.

## Runnable As A Task Boundary

```java
Runnable task = () -> notificationService.sendPendingNotifications();

Thread thread = new Thread(task, "notification-worker");
thread.start();
```

The `Runnable` describes the work. The `Thread` decides where that work runs.
Keeping those responsibilities separate makes the code easier to move later to
an executor.

## Handling Dependencies

In production code, avoid hiding dependency creation inside the task. Pass
dependencies into the object that owns the work.

```java
final class OutboxPublisher implements Runnable {
    private final OutboxRepository repository;
    private final MessageBroker broker;

    OutboxPublisher(OutboxRepository repository, MessageBroker broker) {
        this.repository = repository;
        this.broker = broker;
    }

    @Override
    public void run() {
        repository.findPending().forEach(broker::publish);
    }
}
```

This keeps the task testable and avoids coupling thread startup to
infrastructure construction.

## Limitations

`Runnable` cannot return a result and cannot throw checked exceptions directly.

For work that needs a result, use `Callable`, `Future`, or
`CompletableFuture`. For work that must report failures, catch exceptions and
send them to the correct logging, monitoring, or retry boundary.

## Key Idea

`Runnable` is a work description, not a concurrency strategy. Decide separately
how the task should be scheduled, limited, cancelled, and observed.
