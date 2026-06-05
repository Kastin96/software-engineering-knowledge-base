# Concurrency

Concurrency is the ability to manage multiple tasks whose lifetimes overlap.

In Java services, concurrency appears in request handling, background jobs,
scheduled tasks, message consumers, batch processing, and fan-out calls to
external systems.

## Common Java Options

- platform threads;
- `ExecutorService`;
- `Callable` and `Future`;
- fork/join pools;
- parallel streams;
- virtual threads.

## Executor Example

An executor is useful when the application needs a bounded place for background
work.

```java
ExecutorService executor = Executors.newFixedThreadPool(8);

Future<InvoiceSummary> result = executor.submit(() ->
    invoiceService.calculateSummary(accountId)
);
```

The pool size is a production decision. It should reflect the workload, resource
limits, and failure behavior. An unbounded amount of submitted work can become a
memory and latency problem.

## Virtual Threads

Virtual threads are useful when the code is mostly blocking I/O and the
application benefits from a simple thread-per-task style.

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    Future<CustomerProfile> profile = executor.submit(() ->
        customerClient.fetchProfile(customerId)
    );
}
```

Virtual threads reduce the cost of blocking waits, but they do not remove the
need for timeouts, connection limits, transaction boundaries, or thread-safe
state.

## Production Concerns

Concurrency decisions should account for:

- maximum number of in-flight tasks;
- timeouts and cancellation;
- shared mutable state;
- connection pool limits;
- transaction duration;
- memory pressure;
- shutdown behavior.

## Key Idea

Concurrency is not a framework choice. It is a resource-management decision.
Choose the model that makes work scheduling, limits, and failure behavior clear.
