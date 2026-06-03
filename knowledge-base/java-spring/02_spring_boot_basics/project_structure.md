# Project Structure

A Spring Boot project should make application boundaries easy to see.

The main application class is usually placed at the root package so component
scanning covers the application packages below it.

```text
com.example.orders
  OrdersApplication
  order
    OrderController
    OrderService
    OrderRepository
    Order
    CreateOrderRequest
    OrderResponse
  payment
    PaymentClient
    PaymentProperties
  config
    WebConfig
```

This keeps scanning predictable and avoids custom component scan configuration.

## Layer-Oriented Structure

Some projects group by technical layer:

```text
controller
service
repository
model
dto
```

This is simple, but it can become noisy as the codebase grows. A feature-based
package structure often scales better because related code stays near the
business capability it belongs to.

## Feature-Oriented Structure

```text
order
  OrderController
  OrderService
  OrderRepository
  OrderMapper
  OrderStatus
```

This makes it easier to reason about one feature without jumping through many
global folders.

## Keep Infrastructure Separate

Infrastructure-specific configuration should not be mixed with business logic.

```text
config
  JacksonConfig
  WebClientConfig
  SecurityConfig
```

That separation keeps services focused on behavior instead of framework setup.

## Key Idea

Choose a structure that keeps component scanning simple and business capability
boundaries visible.
