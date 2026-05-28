# Synchronization

## Goal

Understand how `synchronized` protects shared mutable state and what tradeoffs
come with locking.

## Why It Matters

Synchronization is one of Java's oldest and most important concurrency tools. It
can make shared state safe, but incorrect locking can cause deadlocks,
performance bottlenecks, or code that is hard to reason about.

## Synchronized Method

```java
public class SynchronizedCounter {
    private int value;

    public synchronized void increment() {
        value++;
    }

    public synchronized int value() {
        return value;
    }
}
```

Only one thread can execute a synchronized instance method on the same object at
a time.

## Synchronized Block

Use a dedicated lock object when you want more control.

```java
public class Account {
    private final Object lock = new Object();
    private int balanceInCents;

    public void deposit(int amountInCents) {
        if (amountInCents <= 0) {
            throw new IllegalArgumentException("amountInCents must be positive");
        }

        synchronized (lock) {
            balanceInCents += amountInCents;
        }
    }

    public int balanceInCents() {
        synchronized (lock) {
            return balanceInCents;
        }
    }
}
```

A private lock object prevents outside code from synchronizing on your object's
public identity.

## What Synchronization Provides

Synchronization provides:

- mutual exclusion, so only one thread enters a protected block at a time;
- visibility, so changes made inside synchronized blocks become visible to other
  synchronized access using the same lock.

Both matter.

## Keep Critical Sections Small

Do only the shared-state operation inside the lock.

```java
String message;

synchronized (lock) {
    balanceInCents += amountInCents;
    message = "new balance: " + balanceInCents;
}

System.out.println(message);
```

Avoid slow I/O, network calls, or long computations while holding a lock.

## Practical Example

```java
public class Inventory {
    private final Object lock = new Object();
    private int available;

    public Inventory(int available) {
        this.available = available;
    }

    public boolean reserve(int quantity) {
        if (quantity <= 0) {
            throw new IllegalArgumentException("quantity must be positive");
        }

        synchronized (lock) {
            if (available < quantity) {
                return false;
            }

            available -= quantity;
            return true;
        }
    }

    public int available() {
        synchronized (lock) {
            return available;
        }
    }
}
```

The check and update must happen under the same lock. Otherwise two threads
could both see enough stock and oversell.

## Deadlock

Deadlock can happen when two threads wait for locks held by each other.

Avoid:

- acquiring multiple locks in inconsistent order;
- calling unknown external code while holding a lock;
- holding locks longer than needed.

## Common Mistakes

- Synchronizing writes but not reads.
- Locking on public objects, strings, or boxed values.
- Holding locks during slow I/O.
- Assuming `synchronized` makes unrelated objects coordinate.
- Using locks where an immutable design or atomic variable would be simpler.

## Interview Questions

1. What does `synchronized` do?
2. What is the difference between a synchronized method and block?
3. Why use a private lock object?
4. What is a deadlock?
5. Why should critical sections be small?

## Practice

1. Make a counter thread-safe with `synchronized`.
2. Create an inventory reservation method with check-and-update under one lock.
3. Explain why synchronizing only writes is not enough.
4. Identify a place where an atomic variable would be simpler.

## Related Topics

- [Thread Safety](thread_safety.md)
- [Locks and Atomics](locks_atomics.md)
- [Virtual Threads](virtual_threads.md)

