# Thread Safety

## Goal

Understand thread safety, shared mutable state, race conditions, and visibility
problems.

## Why It Matters

Most concurrency bugs come from multiple threads reading and writing the same
data without a clear rule. These bugs can be intermittent, hard to reproduce,
and dangerous in production.

## Shared Mutable State

This class is not thread-safe:

```java
public class Counter {
    private int value;

    public void increment() {
        value++;
    }

    public int value() {
        return value;
    }
}
```

`value++` looks simple, but it is a read-modify-write operation. Multiple threads
can overwrite each other's updates.

## Race Condition

A race condition happens when the result depends on timing between threads.

```java
Counter counter = new Counter();

Thread first = new Thread(counter::increment);
Thread second = new Thread(counter::increment);

first.start();
second.start();
```

The expected value is `2`, but without synchronization the code has no safe
guarantee.

## Immutability Helps

Immutable data is naturally easier to share.

```java
public record UserSnapshot(String id, String email, boolean active) {
}
```

If an object cannot change after creation, multiple threads can read it safely
without coordination.

## Use Local Variables When Possible

Local variables are usually safer because each thread has its own stack.

```java
static int calculateTotal(java.util.List<Integer> prices) {
    int total = 0;

    for (int price : prices) {
        total += price;
    }

    return total;
}
```

The local `total` is not shared between calls.

## Visibility

One thread may update a value while another thread does not see the update at the
time you expect.

```java
class StopFlag {
    private boolean stopped;

    void stop() {
        stopped = true;
    }

    boolean stopped() {
        return stopped;
    }
}
```

For simple visibility flags, `volatile` can be appropriate.

```java
class StopFlag {
    private volatile boolean stopped;

    void stop() {
        stopped = true;
    }

    boolean stopped() {
        return stopped;
    }
}
```

`volatile` helps with visibility, but it does not make compound operations like
`count++` atomic.

## Practical Example

```java
import java.util.concurrent.atomic.AtomicInteger;

public class SafeCounter {
    private final AtomicInteger value = new AtomicInteger();

    public void increment() {
        value.incrementAndGet();
    }

    public int value() {
        return value.get();
    }
}
```

For a simple shared counter, `AtomicInteger` is clearer than manual locking.

## Common Mistakes

- Sharing mutable objects between threads without a rule.
- Thinking `volatile` makes all operations atomic.
- Mutating collections from multiple threads without a thread-safe collection or
  external synchronization.
- Assuming a bug is gone because it does not reproduce every time.
- Mixing thread-safe and non-thread-safe access paths to the same state.

## Interview Questions

1. What does thread-safe mean?
2. What is a race condition?
3. Why is `value++` not atomic?
4. What problem does `volatile` solve?
5. Why does immutability help concurrency?

## Practice

1. Create a non-thread-safe counter and explain the race.
2. Replace it with an `AtomicInteger`.
3. Create an immutable record and explain why it is easier to share.
4. Explain why `volatile int count` is not enough for safe increments.

## Related Topics

- [Synchronization](synchronization.md)
- [Locks and Atomics](locks_atomics.md)
- [Common Concurrency Mistakes](common_concurrency_mistakes.md)

