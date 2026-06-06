# Threads and Runnable

This section covers platform threads and `Runnable` as the lowest-level
concurrency model most Java developers encounter.

Modern backend code usually relies on higher-level APIs such as
`ExecutorService`, `CompletableFuture`, virtual threads, or reactive libraries.
Still, understanding `Thread` and `Runnable` is important because those APIs are
built on the same concepts: units of work, scheduling, lifecycle, interruption,
thread-local state, and failure boundaries.

## Topics

- 01\. [Platform Threads](platform_threads.md)
- 02\. [`Runnable`](runnable.md)
- 03\. [Thread Lifecycle](thread_lifecycle.md)
- 04\. [Interruption](interruption.md)
- 05\. [Daemon Threads](daemon_threads.md)
- 06\. [Uncaught Exceptions](uncaught_exceptions.md)
- 07\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with platform threads and `Runnable`. Then review lifecycle and
interruption, because production shutdown behavior depends on them. Finish with
daemon threads, uncaught exceptions, and common mistakes before moving to
executor-based concurrency.

## Mini Goal

By the end of this section, you should be able to reason about a simple
thread-backed worker where:

- work is represented as a `Runnable`;
- the thread has a useful name;
- shutdown uses interruption instead of unsafe stopping;
- unexpected failures are logged or reported;
- the design has a clear reason for using raw `Thread` instead of an executor.

## Interview Readiness

You should be able to answer:

- What is the difference between `Thread` and `Runnable`?
- Why is manually creating many threads risky?
- What are the main states in a thread lifecycle?
- What does interruption actually do?
- When would a daemon thread be appropriate?
- What happens when a thread throws an uncaught exception?
