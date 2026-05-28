# `Queue` and `Deque`

## Goal

Understand how Java `Queue` and `Deque` represent ordered processing, when to
use `ArrayDeque`, and when `PriorityQueue` is the right data structure.

## Why It Matters

Not every collection is a list. Real systems often need to process work in
first-in-first-out order, keep a small backlog, model retries, walk a graph, or
always handle the highest-priority item next. `Queue` and `Deque` express those
intentions better than manually managing indexes in a `List`.

## `Queue`

A `Queue` usually represents first-in-first-out processing.

```java
import java.util.ArrayDeque;
import java.util.Queue;

Queue<String> tasks = new ArrayDeque<>();

tasks.add("validate order");
tasks.add("charge payment");
tasks.add("send receipt");

while (!tasks.isEmpty()) {
    String task = tasks.poll();
    System.out.println("Processing: " + task);
}
```

Common queue methods:

- `add` inserts and throws if it cannot insert.
- `offer` inserts and returns `false` if it cannot insert.
- `remove` removes and throws if empty.
- `poll` removes and returns `null` if empty.
- `element` reads the head and throws if empty.
- `peek` reads the head and returns `null` if empty.

In ordinary application code, `offer`, `poll`, and `peek` are often safer
because they avoid exceptions for expected empty states.

## `ArrayDeque`

`ArrayDeque` is usually the best default implementation for `Queue` and `Deque`
when you do not need thread-safety or priority ordering.

```java
Queue<String> queue = new ArrayDeque<>();
queue.offer("first");
queue.offer("second");

System.out.println(queue.poll()); // first
System.out.println(queue.poll()); // second
```

Prefer `ArrayDeque` over `LinkedList` for most queue/deque use cases. It is
resizable, efficient, and has lower object overhead.

## `Deque`

A `Deque` is a double-ended queue. You can add and remove from both ends.

```java
import java.util.ArrayDeque;
import java.util.Deque;

Deque<String> history = new ArrayDeque<>();

history.addLast("open dashboard");
history.addLast("open orders");
history.addLast("open order details");

System.out.println(history.removeLast()); // open order details
System.out.println(history.removeLast()); // open orders
```

Useful methods:

- `addFirst`, `offerFirst`, `removeFirst`, `pollFirst`, `peekFirst`;
- `addLast`, `offerLast`, `removeLast`, `pollLast`, `peekLast`.

## Stack-Like Use

Prefer `Deque` over the legacy `Stack` class.

```java
Deque<String> stack = new ArrayDeque<>();

stack.push("first");
stack.push("second");

System.out.println(stack.pop()); // second
System.out.println(stack.pop()); // first
```

`Stack` is an older synchronized class. For modern single-threaded stack-like
logic, `ArrayDeque` is the usual choice.

## `PriorityQueue`

`PriorityQueue` processes elements by priority, not insertion order.

```java
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.Queue;

record SupportTicket(String id, int priority) {
}

Queue<SupportTicket> tickets = new PriorityQueue<>(
        Comparator.comparingInt(SupportTicket::priority).reversed()
);

tickets.offer(new SupportTicket("t-1", 2));
tickets.offer(new SupportTicket("t-2", 5));
tickets.offer(new SupportTicket("t-3", 1));

while (!tickets.isEmpty()) {
    System.out.println(tickets.poll());
}
```

This processes the highest-priority ticket first. Use `PriorityQueue` for
scheduling, ranking, shortest-path algorithms, and "next best item" problems.

Do not use `PriorityQueue` when you need stable insertion order.

## Practical Example

```java
import java.util.ArrayDeque;
import java.util.Queue;

public class RetryQueue {
    public static void main(String[] args) {
        Queue<Job> jobs = new ArrayDeque<>();

        jobs.offer(new Job("send-email", 0));
        jobs.offer(new Job("sync-profile", 1));

        while (!jobs.isEmpty()) {
            Job job = jobs.poll();

            if (shouldRetry(job)) {
                jobs.offer(new Job(job.name(), job.attempts() + 1));
                continue;
            }

            System.out.println("Completed: " + job.name());
        }
    }

    static boolean shouldRetry(Job job) {
        return job.name().equals("send-email") && job.attempts() < 2;
    }

    record Job(String name, int attempts) {
    }
}
```

This is a simplified version of retry/backlog processing. The queue makes the
processing order explicit.

## Thread-Safe Queues

Collections like `ArrayDeque` and `PriorityQueue` are not thread-safe. For
multi-threaded producer/consumer code, use concurrency utilities such as
`BlockingQueue`, `ArrayBlockingQueue`, `LinkedBlockingQueue`, or
`ConcurrentLinkedQueue`.

Those belong in the concurrency section, but it is important to know that normal
collections do not become thread-safe just because they are queues.

## Common Mistakes

- Using `List.remove(0)` repeatedly as a queue; it shifts elements and is often
  inefficient.
- Using legacy `Stack` instead of `Deque`.
- Expecting `PriorityQueue` to preserve insertion order.
- Calling `remove` or `element` on an empty queue when `poll` or `peek` would be
  safer.
- Assuming `ArrayDeque` is thread-safe.
- Adding `null` to `ArrayDeque`; it does not permit null elements.

## Interview Questions

1. What is the difference between `Queue` and `Deque`?
2. Why is `ArrayDeque` usually preferred over `LinkedList` for queue behavior?
3. Why is `Stack` considered legacy?
4. How does `PriorityQueue` choose the next element?
5. What is the difference between `poll` and `remove`?
6. Which queue implementations would you consider for multi-threaded code?

## Practice

1. Process a list of jobs in first-in-first-out order using `ArrayDeque`.
2. Implement browser-like back navigation using `Deque`.
3. Use `PriorityQueue` to process support tickets by priority.
4. Replace a loop that repeatedly calls `list.remove(0)` with a queue.

## Related Topics

- [`List`](list.md)
- [`Map`](map.md)
- [Sorting](sorting.md)

