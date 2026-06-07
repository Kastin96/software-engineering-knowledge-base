# Cached Thread Pools

A cached thread pool creates new threads as needed and reuses idle threads.

It can be useful for short-lived bursts in controlled environments, but it is
risky as a default backend service executor because it has no practical upper
bound on thread creation.

## Example

```java
ExecutorService executor = Executors.newCachedThreadPool();
```

This executor can create many platform threads when tasks arrive faster than
existing threads become available.

## Risk In Backend Services

An unbounded pool can amplify incidents:

- slow downstream service causes tasks to pile up;
- more tasks create more threads;
- more threads increase memory and context switching;
- latency gets worse;
- the service becomes harder to recover.

This is the opposite of backpressure.

## Better Default

Prefer bounded executors for service workloads.

```java
ExecutorService executor = new ThreadPoolExecutor(
    8,
    16,
    30,
    TimeUnit.SECONDS,
    new ArrayBlockingQueue<>(1000),
    namedThreadFactory("customer-sync-"),
    new ThreadPoolExecutor.AbortPolicy()
);
```

The rejection policy should match the caller contract. Sometimes failing fast is
better than accepting work the service cannot complete in time.

## When Cached Pools Can Fit

Cached pools may fit:

- command-line tools;
- short-lived internal utilities;
- workloads with strict external rate limits;
- cases where another layer already enforces hard concurrency bounds.

## Key Idea

Cached thread pools trade simplicity for weak limits. In production services,
weak limits usually become operational risk.
