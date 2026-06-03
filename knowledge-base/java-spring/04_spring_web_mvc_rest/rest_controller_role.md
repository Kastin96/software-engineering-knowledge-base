# REST Controller Role

A REST controller is the HTTP adapter for an application use case.

It should translate HTTP input into application calls and translate application
results into HTTP responses. It should not own business rules, persistence
logic, or integration decisions.

## Typical Shape

```java
@RestController
@RequestMapping("/api/orders")
class OrderController {
    private final OrderService orderService;

    OrderController(OrderService orderService) {
        this.orderService = orderService;
    }

    @GetMapping("/{id}")
    OrderResponse findById(@PathVariable Long id) {
        return orderService.findById(id);
    }
}
```

The controller owns:

- URL mapping;
- HTTP method selection;
- request binding;
- response shape;
- status code decisions.

The service owns:

- application behavior;
- transaction boundaries;
- domain decisions;
- coordination with repositories or clients.

## Keep Controllers Thin

Thin does not mean empty. A controller can validate input shape, select status
codes, and map HTTP concepts. It should not calculate discounts, decide order
state transitions, or build database queries directly.

## Key Idea

Controllers are boundary classes. Keep HTTP concerns there and move application
behavior into services.
