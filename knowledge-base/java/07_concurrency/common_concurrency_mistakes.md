# Common Concurrency Mistakes

## Goal

Recognize common Java concurrency mistakes before they become production bugs.

## Why It Matters

Concurrency bugs are often intermittent. They may pass tests, fail only under
load, or disappear when logging is added. Knowing common failure patterns helps
you design safer code from the start.

## Sharing Mutable State Without Protection

```java
class Cart {
    private final java.util.List<String> items = new java.util.ArrayList<>();

    void add(String item) {
        items.add(item);
    }
}
```

This is unsafe if multiple threads call `add` at the same time.

Fix by using synchronization, a concurrent collection, immutability, or a design
where each thread owns its own data.

## Check-Then-Act Race

```java
if (!usersByEmail.containsKey(email)) {
    usersByEmail.put(email, user);
}
```

With multiple threads, another thread can insert the same email between the
check and the put.

For concurrent maps, use atomic operations.

```java
usersByEmail.putIfAbsent(email, user);
```

## Ignoring Interruption

```java
try {
    Thread.sleep(1_000);
} catch (InterruptedException exception) {
    // ignored
}
```

Better:

```java
try {
    Thread.sleep(1_000);
} catch (InterruptedException exception) {
    Thread.currentThread().interrupt();
    return;
}
```

Interruption is a cancellation signal. Do not erase it casually.

## Blocking Without Timeouts

Blocking forever is dangerous in production systems.

```java
String result = future.get();
```

Prefer timeouts at real boundaries.

```java
String result = future.get(2, java.util.concurrent.TimeUnit.SECONDS);
```

Choose timeout values based on real service expectations, not random guesses.

## Unbounded Concurrency

Starting unlimited tasks can overwhelm the process or external dependencies.

```java
for (Order order : orders) {
    executor.submit(() -> process(order));
}
```

If `orders` can be huge, add limits, batching, backpressure, or bounded
resources. More concurrency is not automatically more throughput.

## Assuming Thread-Safe Collections Solve Everything

Concurrent collections protect their own internal state. They do not make your
whole workflow atomic.

```java
if (!map.containsKey(key)) {
    map.put(key, value);
}
```

Use the collection's atomic methods when needed.

```java
map.putIfAbsent(key, value);
```

## Practical Checklist

Before adding concurrency, ask:

- What data is shared?
- Who owns mutation?
- What limits concurrency?
- How are failures reported?
- How is cancellation handled?
- What timeout applies?
- How are executors shut down?
- What test or load scenario can expose the risk?

## Practical Example

```java
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.ConcurrentMap;

public class UserRegistry {
    private final ConcurrentMap<String, User> usersByEmail = new ConcurrentHashMap<>();

    public boolean register(User user) {
        return usersByEmail.putIfAbsent(user.email(), user) == null;
    }

    record User(String email) {
    }
}
```

`putIfAbsent` makes the check-and-insert operation atomic for this map.

## Common Mistakes

- Adding threads before identifying the bottleneck.
- Sharing mutable state without a clear protection rule.
- Using `volatile` for compound operations.
- Blocking without timeouts.
- Forgetting executor shutdown.
- Assuming virtual threads remove all resource limits.
- Hiding concurrency failures with broad exception handling.

## Interview Questions

1. What is a check-then-act race?
2. Why is ignoring interruption dangerous?
3. Why do timeouts matter?
4. Why can unbounded concurrency hurt throughput?
5. Why does a concurrent collection not make every workflow atomic?

## Practice

1. Find a check-then-act race and replace it with an atomic map operation.
2. Add timeout handling to a blocking future call.
3. Restore interrupted status in a catch block.
4. Describe the shared mutable state in a small concurrent program.

## Related Topics

- [Thread Safety](thread_safety.md)
- [Executors](executors.md)
- [Virtual Threads](virtual_threads.md)

