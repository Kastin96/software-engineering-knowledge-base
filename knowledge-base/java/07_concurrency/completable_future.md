# `CompletableFuture`

## Goal

Understand how `CompletableFuture` represents async work and composes async
results.

## Why It Matters

`CompletableFuture` is common for running independent calls concurrently,
combining results, adding timeouts, and composing async workflows. It is useful,
but it can become hard to read if every step is chained without clear names.

## Basic Async Task

```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureExample {
    public static void main(String[] args) {
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "hello");

        String result = future.join();
        System.out.println(result);
    }
}
```

`supplyAsync` runs a task and returns a future result.

`join()` waits and throws unchecked `CompletionException` on failure. `get()`
waits and throws checked exceptions.

## Transforming Results

```java
CompletableFuture<String> normalizedEmail =
        CompletableFuture.supplyAsync(() -> " Alex@Example.com ")
                .thenApply(String::trim)
                .thenApply(String::toLowerCase);
```

Use `thenApply` for synchronous transformations of a successful result.

## Combining Independent Results

```java
CompletableFuture<User> userFuture =
        CompletableFuture.supplyAsync(() -> loadUser("u-100"));

CompletableFuture<Account> accountFuture =
        CompletableFuture.supplyAsync(() -> loadAccount("u-100"));

CompletableFuture<UserProfile> profileFuture =
        userFuture.thenCombine(accountFuture, UserProfile::new);
```

`thenCombine` is useful when two independent async results are both needed.

## Handling Failures

```java
CompletableFuture<String> result =
        CompletableFuture.supplyAsync(() -> loadRemoteValue())
                .exceptionally(exception -> "fallback");
```

Use failure handlers intentionally. A fallback should be a real business choice,
not a way to hide unknown failures.

## Custom Executor

By default, async operations often use the common fork-join pool. In application
code, pass an executor when you need control.

```java
java.util.concurrent.ExecutorService executor =
        java.util.concurrent.Executors.newFixedThreadPool(4);

try {
    CompletableFuture<String> future =
            CompletableFuture.supplyAsync(() -> loadRemoteValue(), executor);

    System.out.println(future.join());
} finally {
    executor.shutdown();
}
```

Use a custom executor to avoid accidentally mixing unrelated workloads.

## Practical Example

```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ProfileLoader {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);

        try {
            CompletableFuture<User> user =
                    CompletableFuture.supplyAsync(() -> loadUser("u-100"), executor);

            CompletableFuture<Preferences> preferences =
                    CompletableFuture.supplyAsync(() -> loadPreferences("u-100"), executor);

            UserProfile profile = user
                    .thenCombine(preferences, UserProfile::new)
                    .join();

            System.out.println(profile);
        } finally {
            executor.shutdown();
        }
    }

    static User loadUser(String id) {
        return new User(id, "alex@example.com");
    }

    static Preferences loadPreferences(String id) {
        return new Preferences("dark");
    }

    record User(String id, String email) {
    }

    record Preferences(String theme) {
    }

    record UserProfile(User user, Preferences preferences) {
    }
}
```

The user and preferences loads are independent, so they can run concurrently.

## Common Mistakes

- Blocking inside async chains without understanding the executor.
- Using `CompletableFuture` for simple sequential code.
- Hiding failures with broad fallback handlers.
- Forgetting to control the executor for application workloads.
- Creating long chains that are harder to debug than named methods.

## Interview Questions

1. What does `CompletableFuture` represent?
2. What is the difference between `thenApply` and `thenCombine`?
3. What is the difference between `join()` and `get()`?
4. Why might you pass a custom executor?
5. When is `CompletableFuture` unnecessary?

## Practice

1. Run two independent tasks with `CompletableFuture.supplyAsync`.
2. Combine their results with `thenCombine`.
3. Add a fallback with `exceptionally`.
4. Repeat the example with a custom executor.

## Related Topics

- [Executors](executors.md)
- [Functional Java](../06_functional_java/README.md)
- [Common Concurrency Mistakes](common_concurrency_mistakes.md)

