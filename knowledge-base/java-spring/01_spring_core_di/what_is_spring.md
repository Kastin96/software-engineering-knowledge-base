# What Is Spring

Spring is a framework for building Java applications where many parts need to
work together.

In a small Java program, you can create objects manually:

```java
UserRepository repository = new UserRepository();
UserService service = new UserService(repository);
UserController controller = new UserController(service);
```

That is fine for a tiny app. In a real backend service, there may be dozens of
services, repositories, clients, validators, schedulers, and configuration
objects. Wiring them manually becomes noisy, and changing one dependency can
touch many places.

Spring solves this by creating and connecting application objects for you.
Those managed objects are called beans.

## Real Backend Example

A REST endpoint might need this chain:

```text
UserController -> UserService -> UserRepository
```

The controller should not know how to create the service. The service should not
know how to create the repository. Each class should focus on its own job.

```java
@RestController
class UserController {
    private final UserService userService;

    UserController(UserService userService) {
        this.userService = userService;
    }
}
```

The controller asks for `UserService`. Spring provides it.

## What Spring Is Not

Spring is not only annotations. The annotations are just a way to tell Spring
which classes it should manage and how they should be connected.

Spring is also not a replacement for clean code. If a service has too much
logic, too many dependencies, or unclear names, Spring will still run it, but the
code will be hard to maintain.

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

Spring helps with object creation, wiring, configuration, and infrastructure so
your code can focus on business behavior.
