# Cancellation

`Future.cancel` requests cancellation of a task.

Cancellation is cooperative. If a task is already running, cancellation depends
on whether it reacts to interruption or reaches a cancellable blocking point.

## Cancel Before Start

If the task has not started yet, cancellation can remove it from the executor
queue.

```java
boolean cancelled = future.cancel(false);
```

Passing `false` means do not interrupt if the task is already running.

## Cancel Running Work

```java
boolean cancelled = future.cancel(true);
```

Passing `true` requests interruption if the task is running. It does not
forcibly kill the thread.

## Interruption-Aware Task

```java
Callable<ImportSummary> importTask = () -> {
    ImportSummary summary = new ImportSummary();

    while (!Thread.currentThread().isInterrupted()) {
        Optional<Record> next = source.next();
        if (next.isEmpty()) {
            return summary;
        }
        importer.importRecord(next.get());
    }

    throw new InterruptedException("Import was cancelled");
};
```

Long-running tasks should check interruption and use timeouts for blocking
dependencies.

## Cancellation Is A Contract

Before using cancellation, define what happens to partial work:

- should imported records stay committed;
- should the job be retried;
- should the caller receive partial results;
- should external requests be abandoned;
- should the operation be marked failed.

## Key Idea

Cancellation requests are only useful when tasks are designed to cooperate with
interruption and partial-work rules are clear.
