# WebFlux Role

Spring WebFlux is the reactive-stack web framework in Spring.

It is designed for non-blocking request processing and Reactive Streams-based
composition. It can be used for full reactive HTTP services or for specific
reactive clients such as `WebClient`.

## What WebFlux Provides

WebFlux provides:

- reactive HTTP request handling;
- annotated controllers;
- functional endpoints;
- reactive request and response body handling;
- integration with Project Reactor;
- `WebClient` for outbound HTTP calls;
- `WebTestClient` for endpoint tests.

## When It Fits

WebFlux can fit when:

- the request path is mostly non-blocking I/O;
- the service composes multiple asynchronous calls;
- streaming responses are useful;
- reactive database or messaging clients are used;
- high concurrency with I/O-bound work matters.

## When It Does Not Help Much

WebFlux is less useful if most work is blocking:

- JDBC calls;
- JPA/Hibernate calls;
- blocking file I/O;
- synchronous SDKs;
- CPU-heavy processing without careful scheduling.

Blocking APIs can be isolated, but if the service is mostly blocking, Spring MVC
is often simpler.

## Key Idea

WebFlux is a different execution model, not a faster annotation set. Use it when
the service can actually stay non-blocking.
