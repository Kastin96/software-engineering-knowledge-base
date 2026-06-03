# Path and Query Parameter Validation

Path variables and query parameters can also be validated.

For method parameter validation, the controller class is usually annotated with
`@Validated`.

```java
@Validated
@RestController
@RequestMapping("/api/orders")
class OrderController {
    @GetMapping("/{id}")
    OrderResponse findById(@PathVariable @Positive Long id) {
        return orderService.findById(id);
    }

    @GetMapping
    Page<OrderResponse> search(
        @RequestParam(required = false) @Size(max = 30) String status,
        @RequestParam(defaultValue = "20") @Min(1) @Max(100) int size
    ) {
        return orderService.search(status, size);
    }
}
```

## Good Parameter Constraints

Useful constraints include:

- positive IDs;
- maximum page size;
- minimum page number;
- allowed string length;
- required query parameters;
- simple format constraints.

## Avoid Overloading Controllers

If query logic becomes complex, bind parameters into a request object or filter
object and validate that object.

```java
public record OrderSearchRequest(
    @Size(max = 30) String status,
    @Min(1) @Max(100) int size
) {
}
```

## Key Idea

Validate HTTP parameters at the boundary, especially IDs, limits, and filters
that affect query cost.
