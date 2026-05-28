# Advanced Exercises

## 1. CSV Import Result

Build a CSV importer for lines shaped like:

```text
id,email,active
```

Expected behavior:

- reads UTF-8 text line by line;
- skips a header row;
- collects valid users;
- collects invalid row messages with line numbers;
- does not fail the whole import for one invalid row;
- wraps I/O failures with a custom exception.

## 2. Retry Queue

Create a small in-memory retry processor.

Expected behavior:

- uses `Queue<Job>`;
- processes jobs in FIFO order;
- retries failed jobs up to 3 attempts;
- moves exhausted jobs to a failed list;
- does not use `List.remove(0)` as a queue.

## 3. Concurrent Profile Loader

Load user data and orders concurrently.

Expected behavior:

- uses an executor or `CompletableFuture`;
- combines user and orders into `UserProfile`;
- shuts down owned executor resources;
- preserves interrupt status when interrupted;
- has a clear timeout strategy.

## 4. Generic First Match

Write a generic method:

```java
static <T> Optional<T> firstMatching(List<T> values, Predicate<T> rule)
```

Expected behavior:

- returns first matching value;
- returns `Optional.empty()` when no value matches;
- rejects null list or null rule;
- does not mutate the input list.

## 5. Refactor a God Method

Start with one method that validates an order, calculates a total, saves it, and
sends a notification.

Refactor it into focused pieces:

- validator;
- calculator;
- repository boundary;
- notifier boundary;
- service orchestration.

Expected behavior:

- behavior stays the same;
- names become clearer;
- external dependencies are interfaces;
- unit tests cover the service behavior.

