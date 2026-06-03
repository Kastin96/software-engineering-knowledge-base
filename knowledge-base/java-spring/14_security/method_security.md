# Method Security

Method security enforces authorization at the service method level.

It is useful when access rules depend on application behavior or when the same
service method can be called from multiple entry points.

## Enable Method Security

```java
@Configuration
@EnableMethodSecurity
class MethodSecurityConfig {
}
```

## Example

```java
@Service
class UserAdminService {
    @PreAuthorize("hasRole('ADMIN')")
    void disableUser(Long userId) {
        // admin-only operation
    }
}
```

## Permission Based Example

```java
@PreAuthorize("hasAuthority('SCOPE_orders:write')")
OrderResponse createOrder(CreateOrderCommand command) {
    return orderService.create(command);
}
```

## Web Rules And Method Rules

Web authorization is good for path-level access. Method security is good for
use-case-level access.

Using both can be appropriate when the method is sensitive and should remain
protected even if called from another controller or job.

## Key Idea

Use method security for service-level operations whose access rules should stay
close to the use case.
