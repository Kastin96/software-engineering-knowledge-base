# Common Mistakes

These issues appear often in Spring services, especially when framework wiring
starts to hide design problems.

## Hiding Dependencies With Field Injection

```java
@Service
class UserService {
    @Autowired
    private UserRepository userRepository;
}
```

Prefer constructor injection. It makes required dependencies visible and keeps
tests simpler.

## Putting Business Logic In Controllers

Controllers should translate HTTP input and output. Business rules usually
belong in application services.

```java
@RestController
class UserController {
    private final UserService userService;

    UserController(UserService userService) {
        this.userService = userService;
    }
}
```

The controller delegates. The service decides.

## Making Everything A Bean

DTOs, request records, response records, and simple domain values usually do not
need annotations.

```java
record UserResponse(Long id, String email) {
}
```

This is just data.

## Using Spring To Avoid Design Decisions

Spring can wire a class with ten dependencies, but that does not mean the class
has a good design. Too many dependencies often means the class has too many
responsibilities.

## Forgetting Package Structure

If Spring cannot find a component, check where the main application class lives.
Component scan starts there by default and scans subpackages.

## Key Idea

Spring should make application wiring explicit and maintainable. It should not
hide unclear ownership, oversized services, or accidental coupling.
