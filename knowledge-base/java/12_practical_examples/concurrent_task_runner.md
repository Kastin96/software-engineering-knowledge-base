# Concurrent Task Runner

## Problem

Run two independent blocking tasks concurrently and combine their results.

## Why This Example Matters

Many backend operations fetch independent data from separate sources. Running
them concurrently can reduce latency, but the code still needs clear error
handling and controlled executor ownership.

## Code

```java
import java.util.List;
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ProfileAssembler {
    public UserProfile loadProfile(String userId) {
        ExecutorService executor = Executors.newFixedThreadPool(2);

        try {
            CompletableFuture<User> userFuture =
                    CompletableFuture.supplyAsync(() -> loadUser(userId), executor);

            CompletableFuture<List<Order>> ordersFuture =
                    CompletableFuture.supplyAsync(() -> loadOrders(userId), executor);

            return userFuture
                    .thenCombine(ordersFuture, UserProfile::new)
                    .join();
        } finally {
            executor.shutdown();
        }
    }

    private User loadUser(String userId) {
        return new User(userId, "alex@example.com");
    }

    private List<Order> loadOrders(String userId) {
        return List.of(new Order("o-100", 4900));
    }

    public record User(String id, String email) {
    }

    public record Order(String id, int totalInCents) {
    }

    public record UserProfile(User user, List<Order> orders) {
    }
}
```

## What It Demonstrates

- `CompletableFuture.supplyAsync`;
- combining independent results;
- custom executor ownership;
- `finally` shutdown;
- immutable response records.

## Production Note

In real services, the executor is usually owned by application configuration,
not created per method call. This example creates it locally to keep ownership
visible in a small standalone example.

## Practice

1. Add timeout handling.
2. Add a fallback for missing orders but not for missing user.
3. Refactor executor ownership outside the method.
4. Rewrite the example with virtual threads if using Java 21+.

