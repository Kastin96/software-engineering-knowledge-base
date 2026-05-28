# Virtual Threads

## Goal

Understand virtual threads, what they are good for, and what they do not solve.

## Why It Matters

Virtual threads are a major modern Java concurrency feature. They make it
practical to use a thread-per-task style for many blocking I/O-heavy workloads
without creating a large number of expensive platform threads.

## Version Note

Virtual threads became a final feature in Java 21. They are instances of
`java.lang.Thread`, but they are managed by the JDK rather than mapped one-to-one
to operating system threads.

## Platform Threads vs Virtual Threads

Platform threads are backed by operating system threads. They are relatively
expensive, so applications usually use pools to limit how many exist.

Virtual threads are lightweight threads managed by the JDK. You can create many
more of them, especially for workloads that spend much of their time waiting on
blocking I/O.

## Creating a Virtual Thread

```java
public class VirtualThreadExample {
    public static void main(String[] args) throws InterruptedException {
        Thread thread = Thread.startVirtualThread(() -> {
            System.out.println("Running in virtual thread");
        });

        thread.join();
    }
}
```

The code still uses `Thread`, but the thread is virtual.

## Virtual-Thread-Per-Task Executor

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class VirtualThreadExecutorExample {
    public static void main(String[] args) {
        try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
            executor.submit(() -> System.out.println("Load user"));
            executor.submit(() -> System.out.println("Load orders"));
        }
    }
}
```

The executor creates a new virtual thread for each submitted task.

## Good Fit

Virtual threads are a good fit for:

- blocking I/O-heavy tasks;
- request-per-thread server code;
- database calls where the driver and connection pool are the real limits;
- network calls;
- code that is easier to write in a direct blocking style.

They are not a magic replacement for all concurrency tools.

## Not a Good Fit

Virtual threads do not make CPU-heavy work faster by themselves.

If you have CPU-bound tasks, the limit is still CPU cores. Creating more virtual
threads will not create more CPU.

Virtual threads also do not remove the need for:

- timeouts;
- cancellation;
- resource limits;
- connection pool sizing;
- thread-safe shared state.

## Practical Example

```java
import java.util.List;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class BlockingLoads {
    public static void main(String[] args) throws Exception {
        try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
            Future<String> user = executor.submit(() -> loadUser());
            Future<List<String>> orders = executor.submit(() -> loadOrders());

            System.out.println(user.get());
            System.out.println(orders.get());
        }
    }

    static String loadUser() throws InterruptedException {
        Thread.sleep(200);
        return "alex@example.com";
    }

    static List<String> loadOrders() throws InterruptedException {
        Thread.sleep(200);
        return List.of("o-1", "o-2");
    }
}
```

This keeps a direct blocking style while letting the JDK schedule lightweight
virtual threads.

## Watch Resource Limits

Virtual threads are cheap, but external resources are not unlimited.

If 10,000 virtual threads all try to use a database and the connection pool has
20 connections, the pool is still the bottleneck. Use bulkheads, semaphores,
timeouts, and sensible pool sizes where needed.

## Common Mistakes

- Expecting virtual threads to speed up CPU-bound work.
- Ignoring database, network, or rate-limit bottlenecks.
- Sharing mutable state unsafely because the threads are virtual.
- Mixing virtual threads with code that assumes small thread counts.
- Using `ThreadLocal` casually with very large numbers of virtual threads.

## Interview Questions

1. What is a virtual thread?
2. In which Java version did virtual threads become final?
3. What workloads are virtual threads best suited for?
4. Why do virtual threads not automatically speed up CPU-bound code?
5. Why do resource limits still matter with virtual threads?

## Practice

1. Start a virtual thread with `Thread.startVirtualThread`.
2. Use `Executors.newVirtualThreadPerTaskExecutor`.
3. Run two simulated blocking calls concurrently.
4. Explain why a database connection pool can still limit throughput.

## Related Topics

- [Threads](threads.md)
- [Executors](executors.md)
- [Thread Safety](thread_safety.md)

