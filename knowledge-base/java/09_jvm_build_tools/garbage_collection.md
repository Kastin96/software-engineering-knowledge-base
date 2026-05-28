# Garbage Collection Basics

## Goal

Understand what garbage collection is and what Java developers should know about
memory cleanup.

## Why It Matters

Java manages memory automatically, but automatic does not mean irrelevant.
Developers still need to avoid retaining unnecessary objects, understand memory
pressure, and recognize when GC behavior may affect performance.

## What Garbage Collection Does

Garbage collection reclaims heap memory from objects that are no longer
reachable.

```java
User user = new User("alex@example.com");
user = null;
```

After the reference is cleared, the `User` object may become eligible for garbage
collection if nothing else references it.

## Reachability

An object is reachable if it can be accessed from active references, such as:

- local variables in running methods;
- static fields;
- fields of reachable objects;
- thread stacks;
- JNI/native references.

If an object is reachable, GC will not reclaim it.

## You Do Not Free Objects Manually

Java does not have `free` or `delete` for normal objects.

```java
// No manual delete in Java
```

Set references to `null` only when it meaningfully shortens object lifetime.
Most local variables naturally go out of scope.

## Memory Leaks in Java

Java can still have memory leaks when objects remain reachable but are no longer
needed.

```java
static final java.util.List<byte[]> CACHE = new java.util.ArrayList<>();

static void addData(byte[] data) {
    CACHE.add(data);
}
```

If the cache grows forever, GC cannot reclaim the data because it is still
reachable.

## Resource Cleanup Is Different

Garbage collection reclaims memory. It does not replace closing external
resources such as files, streams, sockets, or database connections.

Use try-with-resources.

```java
try (var reader = java.nio.file.Files.newBufferedReader(path)) {
    return reader.readLine();
}
```

## Practical Example

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class LimitedCache<K, V> extends LinkedHashMap<K, V> {
    private final int maxEntries;

    public LimitedCache(int maxEntries) {
        super(16, 0.75f, true);
        this.maxEntries = maxEntries;
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > maxEntries;
    }
}
```

A bounded cache prevents unbounded object retention.

## Common JVM Memory Flags

Common heap flags:

```text
-Xms256m
-Xmx1024m
```

- `-Xms` sets initial heap size.
- `-Xmx` sets maximum heap size.

Do not tune memory blindly. Measure with logs, metrics, heap dumps, and realistic
load.

## Common Mistakes

- Assuming Java cannot have memory leaks.
- Forgetting to close external resources because GC exists.
- Keeping unbounded static collections.
- Calling `System.gc()` as a normal cleanup strategy.
- Tuning heap size without measuring.

## Interview Questions

1. What does garbage collection reclaim?
2. What does it mean for an object to be reachable?
3. Can Java have memory leaks?
4. Why does GC not replace try-with-resources?
5. What do `-Xms` and `-Xmx` configure?

## Practice

1. Explain when an object becomes eligible for GC.
2. Identify a Java memory leak caused by a static collection.
3. Rewrite an unbounded cache as a bounded one.
4. Explain why `System.gc()` should not be part of normal application logic.

## Related Topics

- [Memory: Stack and Heap](memory_stack_heap.md)
- [I/O, Files, and Serialization](../08_io_files_serialization/README.md)
- [Common Concurrency Mistakes](../07_concurrency/common_concurrency_mistakes.md)

