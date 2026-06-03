# AOP

This section covers Spring AOP for cross-cutting concerns.

Spring AOP is proxy-based. In typical Spring applications, it applies advice
around method executions on Spring-managed beans. It is useful for concerns that
cut across many application components, such as metrics, logging, auditing,
authorization checks, retries, and transaction behavior.

## Topics

- 01\. [AOP Role](aop_role.md)
- 02\. [Core Concepts](core_concepts.md)
- 03\. [Spring AOP Proxy Model](spring_aop_proxy_model.md)
- 04\. [Pointcuts](pointcuts.md)
- 05\. [Advice Types](advice_types.md)
- 06\. [Around Advice](around_advice.md)
- 07\. [Annotation-Based Aspects](annotation_based_aspects.md)
- 08\. [Practical Use Cases](practical_use_cases.md)
- 09\. [Testing Aspects](testing_aspects.md)
- 10\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with the role of AOP and the core terms. Then study Spring's proxy model,
pointcuts, advice types, and around advice. After that, review annotation-based
aspects, practical use cases, testing, and common mistakes.

## Mini Goal

By the end of this section, you should be able to design an aspect that:

- targets a clear cross-cutting concern;
- uses a narrow pointcut;
- does not hide business behavior;
- understands Spring proxy limitations;
- records useful operational data;
- is testable without making application behavior invisible.

## Interview Readiness

You should be able to answer:

- What problem does AOP solve?
- What is an aspect, advice, pointcut, and join point?
- How does Spring AOP differ from full AspectJ weaving?
- Why can self-invocation bypass Spring AOP?
- When is `@Around` advice appropriate?
- What are good and bad use cases for AOP?
