# JavaScript Core Concepts

This section explains the core JavaScript concepts that make the language work.

After finishing it, you should understand how JavaScript finds variables, how
functions remember values, how `this` works, how objects share behavior, and how
modern JavaScript code is organized with classes and modules.

## Topics

- 01\. [Scope](scope.md)
- 02\. [Hoisting](hoisting.md)
- 03\. [Closures](closures.md)
- 04\. [`this`](this.md)
- 05\. [Prototypes](prototypes.md)
- 06\. [Classes](classes.md)
- 07\. [Modules](modules.md)
- 08\. [Strict Mode](strict_mode.md)

## Suggested Learning Flow

Start with scope and hoisting because they explain how JavaScript reads names.
Then study closures because they build on scope. After that, learn `this`,
prototypes, and classes to understand objects. Finish with modules and strict
mode because they are important in modern project structure.

## Mini Goal

By the end of this section, try to write a small module that:

- exports a class;
- uses private data through a closure or class field;
- creates object methods;
- imports the class in another file;
- avoids relying on unclear `this` behavior.

