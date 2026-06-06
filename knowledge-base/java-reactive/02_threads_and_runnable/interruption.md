# Interruption

Interruption is Java's cooperative mechanism for asking a thread to stop what it
is doing.

It does not forcibly kill a thread. The running code must check the interrupt
status or react when a blocking method throws `InterruptedException`.

## Interrupting A Worker

```java
Thread worker = new Thread(() -> {
    while (!Thread.currentThread().isInterrupted()) {
        processNextBatch();
    }
}, "batch-worker");

worker.start();
worker.interrupt();
```

The loop cooperates with shutdown by checking the interrupt flag.

## Handling InterruptedException

When code catches `InterruptedException`, it should usually restore the
interrupt status and leave the current operation.

```java
try {
    queue.take();
} catch (InterruptedException ex) {
    Thread.currentThread().interrupt();
    return;
}
```

Restoring the status lets higher-level code observe that interruption happened.

## Bad Pattern

Swallowing interruption makes shutdown unreliable.

```java
try {
    Thread.sleep(1000);
} catch (InterruptedException ignored) {
    // Bad: shutdown signal is lost.
}
```

This can keep background work alive after the application has started shutting
down.

## Production Concerns

Interruption should be part of the service's shutdown design:

- loops should check the interrupt status;
- blocking operations should use timeouts where possible;
- cleanup should be bounded;
- tasks should not hide interruption behind broad catch blocks.

## Key Idea

Interruption is a signal, not a stop command. Production code should cooperate
with it intentionally.
