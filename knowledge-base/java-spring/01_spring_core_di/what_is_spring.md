# What Is Spring

Spring is an application framework centered around inversion of control,
dependency injection, and infrastructure integration.

In a plain Java application, object wiring is explicit:

```java
UserRepository repository = new UserRepository();
UserService service = new UserService(repository);
UserController controller = new UserController(service);
```

That approach is valid, but in a backend service the object graph quickly grows:
controllers, services, repositories, HTTP clients, validators, schedulers,
transaction managers, metrics, and configuration objects. Spring centralizes
that wiring in the application context.

Spring-managed objects are called beans. The framework creates them, resolves
their dependencies, applies configuration, and integrates them with surrounding
infrastructure.

## Backend Layering Example

A common service path looks like this:

```text
UserController -> UserService -> UserRepository
```

The controller handles HTTP concerns. The service owns application behavior. The
repository owns persistence access.

```java
@RestController
class UserController {
    private final UserService userService;

    UserController(UserService userService) {
        this.userService = userService;
    }
}
```

The controller declares a dependency on `UserService`. Spring resolves it from
the application context.

## Framework Boundary

Spring annotations are not the architecture itself. They are metadata that tell
the framework how classes participate in the application.

Spring will happily wire an oversized service with unclear responsibilities.
That does not make the design good. Keep business rules, persistence concerns,
HTTP mapping, and infrastructure configuration separated.

## Common Use In Backend Work

Spring is commonly used to:

- create REST APIs;
- connect services to databases;
- manage configuration and profiles;
- validate requests;
- handle errors consistently;
- add security;
- add logging, metrics, and health checks;
- test application layers.

## Key Idea

Spring manages object creation, dependency wiring, configuration, and common
infrastructure so application code can stay focused on behavior and boundaries.
