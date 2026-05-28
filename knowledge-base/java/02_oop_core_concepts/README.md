# Java OOP Core Concepts

This section explains object-oriented programming in Java.

After finishing it, you should be able to model simple business concepts with
classes, protect object state with encapsulation, use interfaces and abstract
classes appropriately, recognize when inheritance is risky, and implement common
object methods correctly.

## Topics

- 01\. [Classes and Objects](classes_objects.md)
- 02\. [Encapsulation](encapsulation.md)
- 03\. [Inheritance](inheritance.md)
- 04\. [Polymorphism](polymorphism.md)
- 05\. [Interfaces](interfaces.md)
- 06\. [Abstract Classes](abstract_classes.md)
- 07\. [`equals`, `hashCode`, and `toString`](equals_hashcode_tostring.md)

## Suggested Learning Flow

Start with classes and objects. Then learn encapsulation because it shapes how
objects protect their data. After that, study inheritance, polymorphism,
interfaces, and abstract classes. Finish with `equals`, `hashCode`, and
`toString`, because these methods affect collections, logging, debugging, and
tests.

## Mini Goal

By the end of this section, write a small domain model that:

- represents a real concept such as user, account, order, or notification;
- keeps fields private;
- validates state in constructors or methods;
- exposes behavior through clear methods;
- uses an interface for a replaceable dependency;
- compares value-like objects correctly.

## Interview Readiness

You should be able to answer:

- What is the difference between a class and an object?
- What does encapsulation protect against?
- Why is composition often preferred over inheritance?
- How do interfaces support polymorphism?
- When would you use an abstract class instead of an interface?
- Why must `equals` and `hashCode` be consistent?

