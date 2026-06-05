# Choosing an Execution Model

The right model depends on workload shape, dependency behavior, team experience,
and operational constraints.

## Blocking Spring MVC

Choose a blocking model when the service mostly uses blocking dependencies such
as JDBC, JPA, or synchronous SDKs.

This is often the best default for standard CRUD services.

## Executor-Based Concurrency

Choose executors when the service needs controlled background work, bounded
parallelism, or explicit task scheduling.

The important decision is the boundary: which work enters the pool, how much can
run at once, and what happens under load.

## CompletableFuture

Choose `CompletableFuture` when a workflow needs to compose a small number of
independent asynchronous operations.

It is useful for fan-out/fan-in flows, but the execution context and error
handling must stay explicit.

## Virtual Threads

Choose virtual threads when the code is naturally blocking but needs to handle
many concurrent I/O-bound tasks without complex reactive pipelines.

Virtual threads are especially attractive when existing libraries are blocking
and the service benefits from simple sequential code.

## Reactive Streams and Reactor

Choose Reactor or another Reactive Streams implementation when the service can
stay non-blocking across the request path and benefits from stream composition,
backpressure, or event-driven pipelines.

## Quick Decision Guide

```text
Mostly JDBC/JPA CRUD service        -> Spring MVC, blocking model
Bounded background task execution   -> ExecutorService
Independent async calls             -> CompletableFuture
Many blocking I/O tasks             -> virtual threads
Non-blocking I/O pipeline           -> Reactor / WebFlux
Streaming with demand control       -> Reactive Streams
```

## Key Idea

Start from dependencies and operational constraints, not from the framework.
Execution models should make the service easier to operate, not only more
modern-looking.
