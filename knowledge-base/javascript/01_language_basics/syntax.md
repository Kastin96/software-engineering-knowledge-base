# Syntax

## Goal

Understand what JavaScript code looks like and how basic statements are written.

## Why It Matters

Syntax is the grammar of a programming language. If you understand the basic
shape of JavaScript code, it becomes easier to read examples, documentation, and
real project files.

## Explanation

JavaScript code is made of statements. A statement is an instruction for the
program to execute.

```javascript
const message = "Hello, JavaScript";
console.log(message);
```

JavaScript usually ignores extra spaces and line breaks, but clean formatting
makes code much easier to read.

```javascript
const name = "Alex";
const age = 30;

console.log(name);
console.log(age);
```

Semicolons are optional in many cases because JavaScript has automatic semicolon
insertion. Still, using semicolons can make beginner code more predictable.

```javascript
const city = "London";
console.log(city);
```

Blocks use curly braces. Blocks group related code together.

```javascript
if (true) {
  console.log("This code is inside a block");
}
```

Comments explain code for humans. JavaScript ignores comments when the program
runs.

```javascript
// This is a single-line comment.

/*
  This is a multi-line comment.
  It can contain more than one line.
*/
```

## Examples

Basic script:

```javascript
const productName = "Keyboard";
const price = 99;

console.log("Product:", productName);
console.log("Price:", price);
```

Simple block:

```javascript
const isLoggedIn = true;

if (isLoggedIn) {
  console.log("Welcome back");
}
```

## Common Mistakes

- Forgetting closing quotes:

```javascript
// Incorrect
// const name = "Alex;
```

- Forgetting closing braces:

```javascript
// Incorrect
// if (true) {
//   console.log("Missing closing brace");
```

- Writing unclear formatting:

```javascript
// Harder to read
const name="Alex";if(name){console.log(name);}
```

Prefer readable formatting:

```javascript
const name = "Alex";

if (name) {
  console.log(name);
}
```

## Practice

1. Create a variable called `language` and print it.
2. Write an `if` block that prints `"Learning JavaScript"` when a value is true.
3. Add one single-line comment and one multi-line comment to your code.

## Related Topics

- [Variables](variables.md)
- [Control Flow](control_flow.md)

