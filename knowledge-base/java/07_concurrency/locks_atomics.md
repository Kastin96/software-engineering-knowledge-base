# Locks and Atomics

## Goal

Understand when to use explicit locks and atomic variables instead of basic
`synchronized` blocks.

## Why It Matters

Java gives several tools for protecting shared state. `synchronized` is often
enough, but `ReentrantLock` gives more control and atomic classes are simpler for
small lock-free counters and references.

## ReentrantLock

```java
import java.util.concurrent.locks.ReentrantLock;

public class LockedCounter {
    private final ReentrantLock lock = new ReentrantLock();
    private int value;

    public void increment() {
        lock.lock();
        try {
            value++;
        } finally {
            lock.unlock();
        }
    }

    public int value() {
        lock.lock();
        try {
            return value;
        } finally {
            lock.unlock();
        }
    }
}
```

Always unlock in a `finally` block.

## Why Use ReentrantLock?

`ReentrantLock` can be useful when you need:

- `tryLock`;
- interruptible lock acquisition;
- timed lock acquisition;
- multiple condition variables;
- more explicit lock control.

Do not use it just because it looks more advanced.

## AtomicInteger

```java
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicCounter {
    private final AtomicInteger value = new AtomicInteger();

    public int increment() {
        return value.incrementAndGet();
    }

    public int value() {
        return value.get();
    }
}
```

This is a good fit for simple counters.

## Compare and Set

Atomic classes support compare-and-set operations.

```java
AtomicInteger status = new AtomicInteger(0);

boolean changed = status.compareAndSet(0, 1);
```

This changes the value only if it currently equals the expected value.

## AtomicReference

```java
import java.util.concurrent.atomic.AtomicReference;

public class CurrentUserHolder {
    private final AtomicReference<String> currentUser = new AtomicReference<>();

    public void setCurrentUser(String email) {
        currentUser.set(email);
    }

    public String currentUser() {
        return currentUser.get();
    }
}
```

Use `AtomicReference` for simple atomic replacement of an object reference. It
does not make the referenced object itself immutable or thread-safe.

## Practical Example

```java
import java.util.concurrent.atomic.AtomicInteger;

public class LoginAttempts {
    private static final int MAX_ATTEMPTS = 5;

    private final AtomicInteger attempts = new AtomicInteger();

    public boolean recordFailureAndCheckLocked() {
        int current = attempts.incrementAndGet();
        return current >= MAX_ATTEMPTS;
    }

    public void reset() {
        attempts.set(0);
    }
}
```

The shared counter is small and independent, so an atomic variable is a good fit.

## Common Mistakes

- Forgetting to unlock a `ReentrantLock` in `finally`.
- Using atomics for multi-step invariants that need a real lock.
- Thinking `AtomicReference<User>` makes `User` thread-safe.
- Mixing locks and atomics without a clear ownership rule.
- Replacing simple `synchronized` code with more complex lock code for no
  benefit.

## Interview Questions

1. When would you use `ReentrantLock` instead of `synchronized`?
2. Why must `unlock` be in `finally`?
3. What is `AtomicInteger` useful for?
4. What does compare-and-set do?
5. Why are atomics not enough for every shared-state problem?

## Practice

1. Rewrite a synchronized counter using `AtomicInteger`.
2. Create a `ReentrantLock` example with `try/finally`.
3. Use `compareAndSet` to change a status from `NEW` to `PROCESSING`.
4. Explain when a lock is clearer than an atomic variable.

## Related Topics

- [Synchronization](synchronization.md)
- [Thread Safety](thread_safety.md)
- [Common Concurrency Mistakes](common_concurrency_mistakes.md)

