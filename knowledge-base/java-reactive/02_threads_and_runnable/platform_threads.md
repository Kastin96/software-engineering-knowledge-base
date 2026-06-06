# Platform Threads

A platform thread is a Java wrapper around an operating-system thread.

Platform threads are the traditional Java threading model. They are suitable for
running independent work concurrently, but they are relatively expensive
resources compared with ordinary objects or virtual threads.

## Direct Thread Creation

Creating a thread directly is useful for understanding the model, but it should
be uncommon in application service code.

```java
Thread worker = new Thread(() -> auditLogExporter.exportPending(), "audit-exporter");
worker.start();
```

The thread runs independently from the caller. The caller does not receive a
return value, and exceptions do not propagate back to the thread that started it.

## Production Concerns

Raw threads create operational questions that the code must answer explicitly:

- how many threads can exist;
- how they are named;
- how they stop during shutdown;
- where failures are reported;
- whether they keep the JVM alive;
- how work is retried or abandoned.

An executor handles many of these concerns more cleanly by separating work
submission from thread management.

## Thread Naming

Thread names matter in logs, thread dumps, metrics, and production debugging.

```java
Thread worker = new Thread(task, "payment-reconciliation-worker");
```

A useful name often saves time when diagnosing blocked requests, slow shutdowns,
or unexpected background work.

## When Raw Threads Are Reasonable

Direct `Thread` usage can be reasonable for:

- a small standalone utility;
- a dedicated long-running process with explicit lifecycle management;
- learning low-level concurrency behavior;
- infrastructure code where a higher-level abstraction would hide important
  details.

## Key Idea

Use raw platform threads deliberately. In backend applications, prefer
executor-based APIs unless the thread lifecycle is truly part of the design.
