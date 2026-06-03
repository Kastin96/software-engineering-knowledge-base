# WebFlux Basics

This section covers Spring WebFlux basics for backend services.

WebFlux is Spring's reactive-stack web framework. It supports annotated
controllers and functional endpoints, runs on non-blocking server infrastructure,
and uses reactive types such as `Mono` and `Flux`. It is useful when the service
can keep the request path non-blocking and compose asynchronous I/O.

This section focuses on Spring WebFlux integration. Deeper Project Reactor
topics belong in the separate reactive Java learning path.

## Topics

- 01\. [WebFlux Role](webflux_role.md)
- 02\. [Spring MVC versus WebFlux](mvc_vs_webflux.md)
- 03\. [`Mono` and `Flux` at the Web Boundary](mono_flux_web_boundary.md)
- 04\. [Annotated Controllers](annotated_controllers.md)
- 05\. [Functional Endpoints](functional_endpoints.md)
- 06\. [WebClient Basics](webclient_basics.md)
- 07\. [Blocking Calls and Schedulers](blocking_calls_schedulers.md)
- 08\. [Reactive Error Handling](reactive_error_handling.md)
- 09\. [WebTestClient](webtestclient.md)
- 10\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with WebFlux's role and the difference from Spring MVC. Then study `Mono`
and `Flux` at the HTTP boundary, annotated controllers, functional endpoints,
and `WebClient`. After that, review blocking-call risks, error handling, testing,
and common mistakes.

## Mini Goal

By the end of this section, you should be able to design a small WebFlux API
where:

- reactive return types represent asynchronous response values;
- controller methods do not block event-loop threads;
- `WebClient` calls are composed without calling `block()`;
- errors are mapped intentionally;
- endpoint tests use `WebTestClient`;
- WebFlux is chosen for a reason, not because it sounds newer.

## Interview Readiness

You should be able to answer:

- What is Spring WebFlux?
- How is WebFlux different from Spring MVC?
- What do `Mono` and `Flux` represent at the web layer?
- When should you avoid WebFlux?
- Why is blocking inside WebFlux dangerous?
- What is `WebClient` used for?
- How do you test WebFlux endpoints?
