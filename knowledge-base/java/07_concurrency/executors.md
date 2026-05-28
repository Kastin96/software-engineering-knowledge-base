# Executors

## Goal

Understand how Java executors run tasks without manually managing every thread.

## Why It Matters

Executors are the standard way to submit background work, limit concurrency,
reuse platform threads, and manage task lifecycle. They are safer and more
practical than creating many raw threads manually.

## ExecutorService

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExecutorExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(4);

        try {
            executor.submit(() -> System.out.println("Run task"));
        } finally {
            executor.shutdown();
        }
    }
}
```

Always shut down executors that your code owns.

## `Runnable` vs `Callable`

`Runnable` does not return a value.

```java
executor.submit(() -> System.out.println("Send email"));
```

`Callable<T>` returns a value and can throw checked exceptions.

```java
java.util.concurrent.Future<Integer> future = executor.submit(() -> 42);
```

## Future

```java
try {
    Integer result = future.get();
    System.out.println(result);
} catch (InterruptedException exception) {
    Thread.currentThread().interrupt();
} catch (java.util.concurrent.ExecutionException exception) {
    throw new RuntimeException("Task failed", exception.getCause());
}
```

`Future.get()` waits for the task result. Handle interruption and task failures
intentionally.

## Fixed Thread Pool

```java
ExecutorService executor = Executors.newFixedThreadPool(8);
```

A fixed pool limits the number of concurrent platform threads. This is useful
when you need backpressure against CPU or external resources.

Do not blindly create huge pools. Size depends on workload, CPU cores, blocking
behavior, and resource limits.

## Scheduled Executor

```java
java.util.concurrent.ScheduledExecutorService scheduler =
        Executors.newScheduledThreadPool(1);

scheduler.schedule(
        () -> System.out.println("Run later"),
        5,
        java.util.concurrent.TimeUnit.SECONDS
);
```

Use scheduled executors for delayed or repeated work in simple Java programs.
Frameworks often provide their own scheduling abstraction.

## Practical Example

```java
import java.util.List;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class ParallelLoad {
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newFixedThreadPool(2);

        try {
            Future<List<String>> products = executor.submit(() -> loadProducts());
            Future<List<String>> prices = executor.submit(() -> loadPrices());

            System.out.println(products.get());
            System.out.println(prices.get());
        } finally {
            executor.shutdown();
        }
    }

    static List<String> loadProducts() {
        return List.of("keyboard", "mouse");
    }

    static List<String> loadPrices() {
        return List.of("$49.99", "$19.99");
    }
}
```

The two independent loads can run concurrently.

## Common Mistakes

- Forgetting to shut down an executor.
- Creating a new executor for every small task.
- Using an unbounded executor without thinking about load.
- Blocking forever on `Future.get()` without timeout in production code.
- Swallowing `InterruptedException`.

## Interview Questions

1. What problem does `ExecutorService` solve?
2. What is the difference between `Runnable` and `Callable`?
3. What does `Future.get()` do?
4. Why should executors be shut down?
5. Why can unbounded concurrency be dangerous?

## Practice

1. Submit three `Runnable` tasks to a fixed thread pool.
2. Submit a `Callable<Integer>` and read the result.
3. Add shutdown in a `finally` block.
4. Use a scheduled executor to run a task after a delay.

## Related Topics

- [Threads](threads.md)
- [`CompletableFuture`](completable_future.md)
- [Virtual Threads](virtual_threads.md)

