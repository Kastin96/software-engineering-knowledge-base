# Fixed Thread Pools

A fixed thread pool uses a stable number of worker threads.

It is useful when the service needs a clear upper bound on concurrent work:
imports, exports, batch processing, external API fan-out, or background
publishing.

## Example

```java
ExecutorService executor = Executors.newFixedThreadPool(6);

List<Future<EnrichmentResult>> futures = requests.stream()
    .map(request -> executor.submit(() -> enrichmentClient.enrich(request)))
    .toList();
```

At most six tasks run at the same time. Additional submitted tasks wait.

## Why Fixed Pools Are Useful

Fixed pools protect downstream resources:

- database connection pools;
- HTTP client connection pools;
- message broker clients;
- CPU capacity;
- memory used by in-flight tasks.

They make concurrency limits explicit instead of depending on traffic volume.

## Queue Risk

The factory method `Executors.newFixedThreadPool` uses an unbounded queue.

That can be dangerous in services. If tasks arrive faster than workers complete
them, memory usage and latency can grow without a hard limit.

For production systems, consider `ThreadPoolExecutor` with an explicit queue
capacity and rejection policy.

```java
ExecutorService executor = new ThreadPoolExecutor(
    6,
    6,
    0L,
    TimeUnit.MILLISECONDS,
    new ArrayBlockingQueue<>(500),
    new ThreadPoolExecutor.CallerRunsPolicy()
);
```

## Key Idea

A fixed thread pool gives a concurrency limit, but the queue policy is just as
important as the thread count.
