# Proxy Mechanics and Self-Invocation

Spring transaction management is commonly applied through proxies.

That means the transactional behavior is applied when a method call goes through
the Spring-managed proxy. A direct method call inside the same class may bypass
the proxy.

## Self-Invocation Problem

```java
@Service
class OrderService {
    public void createOrder() {
        saveOrder();
    }

    @Transactional
    public void saveOrder() {
        // transactional work
    }
}
```

`createOrder()` calls `saveOrder()` directly on the same object. Depending on
proxy setup, the `@Transactional` behavior on `saveOrder()` may not apply.

## Better Shape

Put the transaction on the public use-case method called from another bean:

```java
@Service
class OrderService {
    @Transactional
    public void createOrder() {
        saveOrder();
    }

    private void saveOrder() {
        // persistence work
    }
}
```

## Why This Matters

Self-invocation issues can make code look transactional while actually running
without the expected transaction boundary.

## Key Idea

Place transactional annotations on service methods that are invoked through the
Spring bean boundary, not on internal helper methods.
