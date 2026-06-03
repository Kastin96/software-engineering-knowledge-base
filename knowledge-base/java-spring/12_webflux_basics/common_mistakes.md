# Common Mistakes

WebFlux mistakes usually come from mixing reactive and blocking models without a
clear boundary.

## Blocking In The Pipeline

Calling `block()` inside WebFlux request handling defeats the non-blocking model
and can harm concurrency.

## Using WebFlux With Mostly Blocking Dependencies

If most dependencies are JDBC, JPA, or synchronous SDKs, WebFlux may add
complexity without meaningful benefit.

## Calling `subscribe()` In Application Code

In controller and service pipelines, return the `Mono` or `Flux`. Let the
framework subscribe at the boundary.

## Treating `Mono` Like A Value

`Mono<Order>` is not an `Order`. It is a description of asynchronous work that
may produce an order later.

## Hiding Errors With Broad Fallbacks

Broad `onErrorResume` usage can turn real failures into empty responses or
misleading success paths.

## Mixing MVC And WebFlux Accidentally

Be deliberate about whether the application is Servlet MVC, full WebFlux, or MVC
with `WebClient`. The dependencies and execution model matter.

## Key Idea

WebFlux requires consistency. Keep the request path reactive or choose a simpler
blocking stack.
