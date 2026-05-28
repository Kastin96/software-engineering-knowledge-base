# Memory: Stack and Heap

## Goal

Understand the basic difference between stack and heap memory in Java.

## Why It Matters

Memory questions are common in interviews. A simple, accurate model helps with
debugging `NullPointerException`, `StackOverflowError`, memory leaks, and
performance problems.

## Stack

Each thread has its own call stack.

The stack stores method call frames, including:

- local primitive values;
- local reference variables;
- method parameters;
- return addresses and call state.

Example:

```java
static int add(int left, int right) {
    int result = left + right;
    return result;
}
```

`left`, `right`, and `result` are local variables in a stack frame.

## Heap

The heap stores objects.

```java
User user = new User("alex@example.com");
```

The local variable `user` is a reference in the stack frame. The `User` object is
on the heap.

```java
record User(String email) {
}
```

## References

Multiple variables can reference the same object.

```java
User first = new User("alex@example.com");
User second = first;

System.out.println(first == second); // true
```

There is one object on the heap and two references to it.

## StackOverflowError

Deep or infinite recursion can overflow the stack.

```java
static void recurse() {
    recurse();
}
```

This eventually throws `StackOverflowError`.

Application code usually should not catch `StackOverflowError`; fix the
recursion problem.

## OutOfMemoryError

The heap can run out of memory if too many live objects remain reachable.

```java
java.util.List<byte[]> data = new java.util.ArrayList<>();

while (true) {
    data.add(new byte[10_000_000]);
}
```

This can eventually throw `OutOfMemoryError`.

## Practical Example

```java
public class MemoryExample {
    public static void main(String[] args) {
        User user = new User("alex@example.com");
        printEmail(user);
    }

    static void printEmail(User user) {
        String email = user.email();
        System.out.println(email);
    }

    record User(String email) {
    }
}
```

The `User` object is on the heap. Each method call has its own stack frame with
local references.

## Common Mistakes

- Saying all variables are stored on the heap.
- Thinking a reference variable is the object itself.
- Catching `Error` types instead of fixing the cause.
- Loading huge files or result sets into memory without limits.
- Keeping objects reachable longer than needed.

## Interview Questions

1. What is stored on the stack?
2. What is stored on the heap?
3. Where is an object created with `new` stored?
4. What can cause `StackOverflowError`?
5. What can cause `OutOfMemoryError`?

## Practice

1. Draw stack and heap for a method that creates a `User`.
2. Explain what happens when one reference is assigned to another.
3. Write a recursive method and explain why it can overflow the stack.
4. Explain why loading a huge file into a list can pressure the heap.

## Related Topics

- [Garbage Collection Basics](garbage_collection.md)
- [JVM Basics](jvm_basics.md)
- [Concurrency](../07_concurrency/README.md)

