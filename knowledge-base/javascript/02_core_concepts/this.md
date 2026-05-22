# `this`

## Goal

Understand what `this` refers to in common JavaScript situations.

## Why It Matters

`this` is used in object methods, classes, callbacks, and event handlers. It can
be confusing because its value depends on how a function is called.

## Explanation

In JavaScript, `this` is usually determined by the call site: how the function is
called, not only where it is written.

## `this` in Object Methods

When a function is called as an object method, `this` refers to that object.

```javascript
const user = {
  name: "Alex",
  sayHello() {
    return `Hello, ${this.name}`;
  },
};

console.log(user.sayHello()); // Hello, Alex
```

## Losing `this`

If you store a method in a separate variable, it can lose its object context.

```javascript
const user = {
  name: "Alex",
  sayHello() {
    return `Hello, ${this.name}`;
  },
};

const sayHello = user.sayHello;

// In strict mode, this may be undefined here.
// console.log(sayHello());
```

You can avoid this by calling the method through the object.

```javascript
console.log(user.sayHello());
```

## Arrow Functions and `this`

Arrow functions do not create their own `this`. They use `this` from the outer
scope.

```javascript
const user = {
  name: "Alex",
  sayHello() {
    const buildMessage = () => {
      return `Hello, ${this.name}`;
    };

    return buildMessage();
  },
};

console.log(user.sayHello()); // Hello, Alex
```

Do not use an arrow function as an object method when you need `this` to refer
to the object.

```javascript
const user = {
  name: "Alex",
  // Avoid this style for object methods that need this.
  sayHello: () => {
    return "This arrow function does not have its own this";
  },
};

console.log(user.sayHello());
```

## `this` in Classes

Inside class methods, `this` refers to the instance.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  sayHello() {
    return `Hello, ${this.name}`;
  }
}

const user = new User("Alex");

console.log(user.sayHello()); // Hello, Alex
```

## Using `bind`

`bind` creates a new function with a fixed `this` value.

```javascript
const user = {
  name: "Alex",
  sayHello() {
    return `Hello, ${this.name}`;
  },
};

const sayHello = user.sayHello.bind(user);

console.log(sayHello()); // Hello, Alex
```

## Common Mistakes

- Expecting `this` to always mean the object where the function was written:

```javascript
const user = {
  name: "Alex",
  sayHello() {
    return `Hello, ${this.name}`;
  },
};

const fn = user.sayHello;

// The call below is not the same as user.sayHello().
// fn();
```

- Using arrow methods when normal methods are clearer:

```javascript
const calculator = {
  value: 10,
  double() {
    return this.value * 2;
  },
};

console.log(calculator.double()); // 20
```

## Practice

1. Create an object with a method that uses `this`.
2. Store the method in a variable and explain what changes.
3. Fix a lost `this` problem with `bind`.
4. Create a class with a method that reads an instance property.

## Related Topics

- [Classes](classes.md)
- [Prototypes](prototypes.md)
- [Strict Mode](strict_mode.md)

