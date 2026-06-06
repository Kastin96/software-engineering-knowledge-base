# Thread Lifecycle

A thread moves through a small set of states while it is created, scheduled,
blocked, and completed.

Understanding these states is useful when reading thread dumps or diagnosing
slow shutdowns and stuck background workers.

## Common States

- `NEW`: the thread object exists, but `start()` has not been called.
- `RUNNABLE`: the thread is eligible to run or is currently running.
- `BLOCKED`: the thread is waiting to enter a synchronized block or method.
- `WAITING`: the thread is waiting indefinitely for another action.
- `TIMED_WAITING`: the thread is waiting with a timeout.
- `TERMINATED`: the `run()` method has completed or failed.

## Start Once

A `Thread` instance can be started only once.

```java
Thread thread = new Thread(task, "invoice-worker");
thread.start();
```

Calling `start()` schedules the new thread and invokes `run()` on that new
execution path. Calling `run()` directly is just a normal method call on the
current thread.

## Join

`join()` lets one thread wait for another thread to finish.

```java
worker.start();
worker.join(Duration.ofSeconds(10));
```

Use bounded waiting when possible. Unbounded joins can make shutdown or startup
hang indefinitely if the worker never completes.

## Lifecycle Ownership

Every manually started thread needs an owner responsible for:

- starting it;
- naming it;
- stopping it;
- waiting for it when appropriate;
- reporting failures.

In application servers and Spring services, this is one reason executors,
schedulers, and lifecycle-managed beans are usually better than raw threads.

## Key Idea

Thread lifecycle is operational behavior. If the lifecycle is unclear, the
service will be harder to stop, debug, and support.
