# Concurrency versus Async versus Reactive

This section defines the execution models that often get grouped together in
Java backend discussions: concurrency, asynchronous programming, and reactive
programming.

They are related, but they solve different problems. A service can be concurrent
without being asynchronous, asynchronous without being reactive, and reactive
without being faster for a given workload.

## Topics

- 01\. [Core Terms](core_terms.md)
- 02\. [Concurrency](concurrency.md)
- 03\. [Asynchronous Programming](asynchronous_programming.md)
- 04\. [Reactive Programming](reactive_programming.md)
- 05\. [Choosing an Execution Model](choosing_execution_model.md)
- 06\. [Common Misconceptions](common_misconceptions.md)

## Suggested Learning Flow

Start with the terms. Then compare concurrency, asynchronous programming, and
reactive programming as separate design choices. Finish with selection criteria
and common misconceptions, because many production issues come from choosing a
model for the wrong reason.

## Mini Goal

By the end of this section, you should be able to look at a backend workflow and
decide whether it is better suited for:

- simple blocking code;
- an executor-backed task model;
- `CompletableFuture` composition;
- virtual threads;
- a reactive pipeline.

## Interview Readiness

You should be able to answer:

- What is the difference between concurrency and parallelism?
- What does asynchronous code change about the control flow?
- What makes a stream reactive?
- Is WebFlux always faster than Spring MVC?
- When are virtual threads a better fit than Reactor?
- Why is blocking inside an event-loop pipeline dangerous?
