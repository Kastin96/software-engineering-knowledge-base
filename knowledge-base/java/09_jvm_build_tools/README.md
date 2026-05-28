# Java JVM and Build Tools

This section explains how Java code is compiled, run, packaged, and managed in
real projects.

After finishing it, you should understand the JVM, bytecode, classpath, stack
and heap memory, garbage collection basics, packages, modules, JAR files, Maven,
Gradle, dependency scopes, and common project layout.

## Topics

- 01\. [JVM Basics](jvm_basics.md)
- 02\. [Compile and Run](compile_run.md)
- 03\. [Classpath and JAR Files](classpath_jar.md)
- 04\. [Memory: Stack and Heap](memory_stack_heap.md)
- 05\. [Garbage Collection Basics](garbage_collection.md)
- 06\. [Packages and Modules](packages_modules.md)
- 07\. [Maven](maven.md)
- 08\. [Gradle](gradle.md)
- 09\. [Build Tool Best Practices](build_tool_best_practices.md)

## Suggested Learning Flow

Start with JVM basics and the compile/run flow. Then learn classpath and JAR
files, because build tools mostly automate those details. After that, study
memory and garbage collection basics. Finish with packages, modules, Maven,
Gradle, and practical build-tool habits.

## Mini Goal

By the end of this section, create a small Java project that:

- compiles and runs from the command line;
- uses packages correctly;
- produces or consumes a JAR;
- explains stack vs heap for a simple method call;
- uses Maven or Gradle to run tests;
- declares at least one dependency;
- avoids committing generated build output.

## Interview Readiness

You should be able to answer:

- What does the JVM do?
- What is bytecode?
- What is the difference between JDK, JRE, and JVM?
- What is the classpath?
- What is stored on the stack and heap?
- What is garbage collection?
- Why do Java projects use Maven or Gradle?
- What is the usual Maven/Gradle project layout?

