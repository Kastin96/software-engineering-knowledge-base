# Annotated Controllers

WebFlux supports annotation-based controllers similar to Spring MVC.

The main difference is that handler methods commonly return reactive types.

## Example

```java
@RestController
@RequestMapping("/api/orders")
class OrderController {
    private final OrderService orderService;

    OrderController(OrderService orderService) {
        this.orderService = orderService;
    }

    @GetMapping("/{id}")
    Mono<OrderResponse> findById(@PathVariable String id) {
        return orderService.findById(id);
    }

    @PostMapping
    Mono<ResponseEntity<OrderResponse>> create(@Valid @RequestBody Mono<CreateOrderRequest> request) {
        return request
            .flatMap(orderService::create)
            .map(created -> ResponseEntity.status(HttpStatus.CREATED).body(created));
    }
}
```

## Request Body Choices

For simple JSON request bodies, a controller may accept the DTO directly or as a
`Mono<DTO>`, depending on the endpoint design.

```java
Mono<OrderResponse> create(@Valid @RequestBody CreateOrderRequest request)
```

or:

```java
Mono<OrderResponse> create(@RequestBody Mono<CreateOrderRequest> request)
```

Keep the method shape readable and aligned with how the request is processed.

## Key Idea

Annotated WebFlux controllers reuse familiar Spring mapping annotations, but the
method body should compose reactive work instead of blocking for results.
