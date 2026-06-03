# Spring MVC versus WebFlux

Spring MVC and Spring WebFlux can look similar at the controller level, but they
have different execution models.

## Spring MVC

Spring MVC is built around the Servlet stack.

It is a strong default for many backend services, especially when the service
uses blocking data access such as JDBC or JPA.

```java
@GetMapping("/{id}")
OrderResponse findById(@PathVariable Long id) {
    return orderService.findById(id);
}
```

## Spring WebFlux

WebFlux is built around reactive, non-blocking request handling.

```java
@GetMapping("/{id}")
Mono<OrderResponse> findById(@PathVariable String id) {
    return orderService.findById(id);
}
```

The method returns a reactive value that will eventually produce the response.

## Important Difference

In MVC, blocking in the request thread is normal. In WebFlux, blocking on the
event-loop path can damage scalability because a small number of event-loop
threads handle many requests.

## Mixed Usage

You can use `WebClient` in a Spring MVC application without making the whole
service WebFlux. This is common when only outbound HTTP calls benefit from a
reactive client.

## Key Idea

Choose MVC or WebFlux based on the service's I/O model and dependencies, not
because their controller annotations look similar.
