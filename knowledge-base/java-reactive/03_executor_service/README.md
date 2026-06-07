# ExecutorService

This section covers `ExecutorService` as the standard Java abstraction for
running tasks without manually managing individual threads.

Executors separate task submission from thread ownership. That separation is
important in backend services: it makes concurrency limits, queueing, shutdown,
thread naming, error reporting, and resource usage explicit.

## Topics

- 01\. [ExecutorService Role](executor_service_role.md)
- 02\. [execute versus submit](execute_vs_submit.md)
- 03\. [Fixed Thread Pools](fixed_thread_pool.md)
- 04\. [Cached Thread Pools](cached_thread_pool.md)
- 05\. [Scheduled Executors](scheduled_executor.md)
- 06\. [Thread Pool Sizing](thread_pool_sizing.md)
- 07\. [Shutdown and Cancellation](shutdown_cancellation.md)
- 08\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with the role of `ExecutorService` and the difference between `execute`
and `submit`. Then review fixed, cached, and scheduled executors. Finish with
pool sizing, shutdown, cancellation, and common production mistakes.

## Mini Goal

By the end of this section, you should be able to design a small executor-backed
worker where:

- task submission is separated from thread creation;
- the pool has a clear concurrency limit;
- threads have useful names;
- failures are observable;
- shutdown is bounded and interruption-aware;
- the queue cannot grow silently without operational consequences.

## Interview Readiness

You should be able to answer:

- What problem does `ExecutorService` solve compared with raw `Thread`?
- What is the difference between `execute` and `submit`?
- When would you use a fixed thread pool?
- Why can cached thread pools be dangerous in services?
- How do you shut down an executor correctly?
- How should pool size relate to CPU-bound and I/O-bound work?
