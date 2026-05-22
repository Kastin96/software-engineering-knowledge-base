# Prototypes

## Goal

Understand how JavaScript objects can share behavior through prototypes.

## Why It Matters

JavaScript uses prototypes under the hood for object inheritance. Classes are a
modern syntax built on top of this prototype system.

## Explanation

Every normal JavaScript object has a prototype. A prototype is another object
that JavaScript can search when a property or method is not found directly on
the object.

```javascript
const user = {
  name: "Alex",
};

console.log(user.toString());
```

`toString` is not created directly on `user`, but JavaScript can find it through
the prototype chain.

## Prototype Chain

When you read a property, JavaScript checks:

1. the object itself;
2. the object's prototype;
3. the next prototype;
4. until it reaches `null`.

```javascript
const user = {
  name: "Alex",
};

console.log(user.name); // direct property
console.log(user.hasOwnProperty("name")); // method from prototype
```

## Constructor Functions

Before classes, constructor functions were a common way to create similar
objects.

```javascript
function User(name) {
  this.name = name;
}

User.prototype.sayHello = function () {
  return `Hello, ${this.name}`;
};

const alex = new User("Alex");
const sam = new User("Sam");

console.log(alex.sayHello()); // Hello, Alex
console.log(sam.sayHello()); // Hello, Sam
```

The method is shared through `User.prototype`.

## Object.create

`Object.create` creates a new object with a specific prototype.

```javascript
const userActions = {
  sayHello() {
    return `Hello, ${this.name}`;
  },
};

const user = Object.create(userActions);

user.name = "Alex";

console.log(user.sayHello()); // Hello, Alex
```

## Own Properties

Use `Object.hasOwn()` to check whether a property belongs directly to an object.

```javascript
const user = {
  name: "Alex",
};

console.log(Object.hasOwn(user, "name")); // true
console.log(Object.hasOwn(user, "toString")); // false
```

## Classes and Prototypes

Class methods are placed on the class prototype.

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

console.log(user.sayHello());
console.log(Object.hasOwn(user, "sayHello")); // false
```

The `sayHello` method is shared, not copied into every instance.

## Common Mistakes

- Thinking class methods are copied into every object:

```javascript
class Product {
  constructor(name) {
    this.name = name;
  }

  getName() {
    return this.name;
  }
}

const product = new Product("Keyboard");

console.log(Object.hasOwn(product, "name")); // true
console.log(Object.hasOwn(product, "getName")); // false
```

- Modifying built-in prototypes:

```javascript
// Avoid this in normal application code.
// Array.prototype.first = function () {
//   return this[0];
// };
```

Changing built-in prototypes can create unexpected behavior across a project.

## Practice

1. Create a constructor function with one prototype method.
2. Create an object using `Object.create`.
3. Use `Object.hasOwn()` to compare direct and inherited properties.
4. Explain how class methods relate to prototypes.

## Related Topics

- [Classes](classes.md)
- [`this`](this.md)

