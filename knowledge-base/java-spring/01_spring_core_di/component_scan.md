# Component Scan

Component scan is how Spring finds classes annotated with `@Component`,
`@Service`, `@Repository`, `@Controller`, and `@RestController`.

In Spring Boot, scanning usually starts from the package where the main
application class lives.

```java
package com.example.orders;

@SpringBootApplication
public class OrdersApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrdersApplication.class, args);
    }
}
```

Spring will scan `com.example.orders` and its subpackages.

## Good Package Shape

```text
com.example.orders
  OrdersApplication
  orders
    OrderController
    OrderService
    OrderRepository
  payments
    PaymentService
```

This works well because application code is below the main package.

## Common Problem

This can fail:

```text
com.example.app
  AppApplication

com.example.orders
  OrderService
```

If `OrderService` is outside the scanned package tree, Spring may not find it.

## Do Not Overuse Manual Scanning

You can customize scanning with `@ComponentScan`, but most Spring Boot projects
do not need that. A clean package structure is usually better.

## Key Idea

Place the main application class at the root of the application package. That
keeps component discovery predictable and avoids manual scan configuration.
