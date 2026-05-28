# Java Generics and Type System

This section explains Java generics and the type-system rules that show up in
real application code.

After finishing it, you should be able to read generic collection types, create
simple generic classes and methods, use bounded type parameters, understand
wildcards, avoid raw types, and explain type erasure in interview-friendly
language.

## Topics

- 01\. [Generics Basics](generics_basics.md)
- 02\. [Generic Classes and Methods](generic_classes_methods.md)
- 03\. [Bounded Type Parameters](bounded_type_parameters.md)
- 04\. [Wildcards](wildcards.md)
- 05\. [Type Erasure](type_erasure.md)
- 06\. [Raw Types and Type Safety](raw_types_type_safety.md)
- 07\. [`var`, Diamond, and Type Inference](var_diamond_type_inference.md)

## Suggested Learning Flow

Start with generics basics and how collections use type parameters. Then learn
generic classes and methods. After that, study bounds and wildcards because they
make API signatures more flexible. Finish with type erasure, raw types, and type
inference so you understand the limits and tradeoffs.

## Mini Goal

By the end of this section, write a small generic utility that:

- works with a specific type instead of `Object`;
- exposes a generic method;
- uses an upper bounded type parameter;
- reads from a wildcard collection safely;
- avoids raw types and unchecked casts;
- has method signatures that are flexible but still readable.

## Interview Readiness

You should be able to answer:

- Why does Java have generics?
- What problem do raw types create?
- What is type erasure?
- What is the difference between `List<Object>` and `List<?>`?
- What does `? extends T` mean?
- What does `? super T` mean?
- When should you prefer a generic method over a generic class?

