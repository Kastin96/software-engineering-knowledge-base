# Functional Endpoints

WebFlux also supports functional endpoints.

Functional endpoints define routes and handlers as functions instead of
controller annotations.

## Router

```java
@Configuration
class OrderRoutes {
    @Bean
    RouterFunction<ServerResponse> routes(OrderHandler handler) {
        return RouterFunctions.route()
            .GET("/api/orders/{id}", handler::findById)
            .POST("/api/orders", handler::create)
            .build();
    }
}
```

## Handler

```java
@Component
class OrderHandler {
    private final OrderService orderService;

    Mono<ServerResponse> findById(ServerRequest request) {
        String id = request.pathVariable("id");

        return orderService.findById(id)
            .flatMap(order -> ServerResponse.ok().bodyValue(order));
    }
}
```

## When It Fits

Functional endpoints can fit when:

- route composition should be explicit;
- endpoint logic is small and composable;
- the team prefers function-based request handling;
- annotation scanning should be minimized.

Annotated controllers are still more common in many Spring teams.

## Key Idea

Functional endpoints are an alternative programming model on the same WebFlux
runtime, not a separate framework.
