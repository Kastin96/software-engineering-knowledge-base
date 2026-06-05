# Core Terms

Reactive Java discussions are easier when the vocabulary is precise.

## Concurrency

Concurrency means multiple tasks can make progress during the same time period.
The tasks may run on one CPU core through scheduling, or on multiple cores in
parallel.

Concurrency is about structure and coordination.

## Parallelism

Parallelism means multiple tasks are executing at the same instant, usually on
different CPU cores.

Parallelism is about simultaneous execution.

## Blocking

Blocking means a thread waits until an operation completes.

Typical blocking operations include database calls through JDBC, file I/O, HTTP
calls through synchronous clients, and waiting on locks.

Blocking is not automatically bad. It is often the simplest and most reliable
model when enough threads are available and the workload is understood.

## Non-Blocking

Non-blocking code starts work without occupying a thread while waiting for the
result.

This is useful for I/O-heavy systems, but it requires compatible libraries and a
clear execution model. One blocking call in the wrong place can damage the
benefit.

## Asynchronous

Asynchronous code starts work and receives the result later through a callback,
future, completion stage, or reactive signal.

The important change is control flow: the caller does not receive the final
value immediately.

## Reactive

Reactive programming models asynchronous streams of data and events. It also
defines how producers and consumers coordinate demand, errors, and completion.

In Java, this usually means Reactive Streams concepts and libraries such as
Project Reactor.

## Key Idea

Do not use these terms as synonyms. They describe different parts of the
execution model: scheduling, waiting, control flow, data flow, and coordination.
