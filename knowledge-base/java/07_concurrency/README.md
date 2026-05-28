# Java Concurrency

This section explains the Java concurrency concepts most useful for backend and
application development.

After finishing it, you should understand platform threads, virtual threads,
thread safety, synchronization, executors, locks, atomics, `CompletableFuture`,
and common concurrency mistakes.

## Version Note

Virtual threads became a final feature in Java 21. They are useful for many
blocking I/O-heavy workloads, but they do not remove the need to understand
thread safety, shared mutable state, timeouts, cancellation, and resource limits.

## Topics

- 01\. [Threads](threads.md)
- 02\. [Thread Safety](thread_safety.md)
- 03\. [Synchronization](synchronization.md)
- 04\. [Executors](executors.md)
- 05\. [Locks and Atomics](locks_atomics.md)
- 06\. [`CompletableFuture`](completable_future.md)
- 07\. [Virtual Threads](virtual_threads.md)
- 08\. [Common Concurrency Mistakes](common_concurrency_mistakes.md)

## Suggested Learning Flow

Start with threads and thread safety. Then learn synchronization, executors,
locks, and atomics. After that, study `CompletableFuture` for async composition
and virtual threads for modern blocking-style concurrency. Finish with common
mistakes, because concurrency bugs often come from small incorrect assumptions.

## Mini Goal

By the end of this section, write a small program that:

- runs two independent tasks concurrently;
- uses an executor instead of manually managing many threads;
- protects shared mutable state correctly;
- uses an atomic counter where appropriate;
- composes two async results with `CompletableFuture`;
- tries a virtual-thread-per-task executor for blocking-style work;
- shuts down resources cleanly.

## Interview Readiness

You should be able to answer:

- What is the difference between concurrency and parallelism?
- Why is shared mutable state dangerous?
- What does `synchronized` protect?
- Why should you usually prefer executors over manually creating many threads?
- What is the difference between a lock and an atomic variable?
- What problem does `CompletableFuture` solve?
- What are virtual threads good for, and what are they not good for?

