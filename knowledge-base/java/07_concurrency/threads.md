# Threads

## Goal

Understand what Java threads are, how they run work concurrently, and why raw
thread management is usually not the best abstraction for application code.

## Why It Matters

Java applications often handle many tasks at once: HTTP requests, database calls,
file processing, scheduled jobs, message handling, and background work. Threads
are the foundation of that concurrency model, even when higher-level APIs hide
the details.

## Concurrency vs Parallelism

Concurrency means a program can make progress on multiple tasks during the same
period of time.

Parallelism means multiple tasks literally run at the same time on different CPU
cores.

A program can be concurrent without being parallel. For example, one CPU core can
switch between tasks.

## Creating a Thread

```java
public class ThreadExample {
    public static void main(String[] args) throws InterruptedException {
        Thread worker = new Thread(() -> {
            System.out.println("Running in " + Thread.currentThread().getName());
        });

        worker.start();
        worker.join();

        System.out.println("Done");
    }
}
```

Use `start()` to run the thread. Calling `run()` directly does not create a new
thread.

## `join`

`join` waits for another thread to finish.

```java
Thread worker = new Thread(() -> performWork());

worker.start();
worker.join();
```

Without `join`, the main thread may continue before the worker completes.

## Sleep and Interruptions

```java
Thread worker = new Thread(() -> {
    try {
        Thread.sleep(1_000);
        System.out.println("Finished work");
    } catch (InterruptedException exception) {
        Thread.currentThread().interrupt();
        System.out.println("Worker was interrupted");
    }
});
```

If you catch `InterruptedException`, usually restore the interrupt flag with
`Thread.currentThread().interrupt()`. This lets higher-level code know the thread
was interrupted.

## Prefer Higher-Level APIs

Manually creating threads is useful for learning, but application code usually
uses executors or framework-managed threads.

```java
java.util.concurrent.ExecutorService executor =
        java.util.concurrent.Executors.newFixedThreadPool(4);
```

Executors manage thread reuse, task submission, and shutdown.

## Practical Example

```java
public class IndependentTasks {
    public static void main(String[] args) throws InterruptedException {
        Thread priceLoader = new Thread(() -> loadPrices());
        Thread inventoryLoader = new Thread(() -> loadInventory());

        priceLoader.start();
        inventoryLoader.start();

        priceLoader.join();
        inventoryLoader.join();

        System.out.println("All data loaded");
    }

    static void loadPrices() {
        System.out.println("Loading prices");
    }

    static void loadInventory() {
        System.out.println("Loading inventory");
    }
}
```

This shows independent work, but an executor would be better in real application
code.

## Common Mistakes

- Calling `run()` instead of `start()`.
- Creating unbounded numbers of platform threads manually.
- Ignoring `InterruptedException`.
- Sharing mutable state between threads without protection.
- Assuming code is thread-safe because it worked once locally.

## Interview Questions

1. What is a thread?
2. What is the difference between `start()` and `run()`?
3. What does `join()` do?
4. Why should interrupted status often be restored?
5. Why are executors usually preferred over manual thread creation?

## Practice

1. Start two threads that print different messages.
2. Use `join` to wait for both threads.
3. Add `Thread.sleep` and handle `InterruptedException`.
4. Rewrite the same example using an executor after reading the executors topic.

## Related Topics

- [Executors](executors.md)
- [Thread Safety](thread_safety.md)
- [Virtual Threads](virtual_threads.md)

