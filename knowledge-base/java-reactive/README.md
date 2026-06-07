# Java Reactive Learning Path

This section is a practical reference for concurrency, asynchronous programming,
and reactive Java.

It assumes solid Java fundamentals and general backend experience. The focus is
on choosing the right execution model for production services: platform threads,
executors, futures, `CompletableFuture`, virtual threads, Java Flow API,
Reactive Streams, Project Reactor, WebFlux, WebClient, and reactive data access.

The goal is not to make every Java service reactive. The goal is to understand
the trade-offs between blocking, asynchronous, and reactive styles, and to use
each one where it fits the workload and infrastructure.

## Recommended Order

- 01\. [Concurrency versus Async versus Reactive](01_concurrency_vs_async_vs_reactive/README.md)
- 02\. [Threads and Runnable](02_threads_and_runnable/README.md)
- 03\. [ExecutorService](03_executor_service/README.md)
- 04\. [Callable and Future](04_callable_future/README.md)

## Planned Topics

- concurrency, asynchronous programming, and reactive programming boundaries
- platform threads, `Runnable`, and `Thread`
- `ExecutorService`, `Callable`, and `Future`
- `CompletableFuture` for async composition
- virtual threads and blocking-style concurrency
- fork/join and parallel streams
- Java Flow API and Reactive Streams
- Project Reactor fundamentals
- `Mono`, `Flux`, operators, schedulers, and backpressure
- Spring WebFlux and `WebClient`
- reactive data access and reactive MongoDB
- testing asynchronous and reactive code
- performance risks and common production mistakes

## How To Use This Section

Read the topics in order if you are building the mental model from the ground
up. If you already use WebFlux or Reactor, use the early sections to clarify
terminology and execution models before going deeper into operators and
schedulers.

Examples should be small and production-oriented: request fan-out, external
client calls, background work, timeouts, retries, cancellation, resource limits,
and clear boundaries between blocking and non-blocking code.

## What This Section Should Prepare You For

After finishing this section, you should be able to:

- explain the difference between concurrency, parallelism, async, and reactive;
- choose between threads, executors, `CompletableFuture`, virtual threads, and
  reactive streams;
- reason about blocking and non-blocking service paths;
- avoid mixing execution models accidentally;
- discuss Project Reactor and WebFlux in interview-friendly language;
- identify common scalability and maintainability risks in reactive code.
