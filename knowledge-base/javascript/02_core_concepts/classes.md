# Classes

## Goal

Understand how to create objects with shared behavior using JavaScript classes.

## Why It Matters

Classes are common in application code, UI components, data models, services,
and many libraries. They provide a clear syntax for constructor functions and
prototype methods.

## Explanation

A class is a template for creating objects.

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

The `constructor` runs when a new object is created with `new`.

## Instance Properties

Instance properties belong to each created object.

```javascript
class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }

  getLabel() {
    return `${this.name}: $${this.price}`;
  }
}

const keyboard = new Product("Keyboard", 99);

console.log(keyboard.getLabel());
```

## Methods

Methods define behavior shared by all instances.

```javascript
class Counter {
  constructor() {
    this.value = 0;
  }

  increment() {
    this.value += 1;
    return this.value;
  }
}

const counter = new Counter();

console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
```

## Private Fields

Private fields start with `#` and can be used only inside the class.

```javascript
class BankAccount {
  #balance = 0;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount();

account.deposit(100);

console.log(account.getBalance()); // 100
```

## Static Methods

Static methods belong to the class itself, not to instances.

```javascript
class NumberHelper {
  static isEven(number) {
    return number % 2 === 0;
  }
}

console.log(NumberHelper.isEven(4)); // true
```

## Inheritance

`extends` lets one class reuse behavior from another class.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  speak() {
    return `${this.name} barks`;
  }
}

const dog = new Dog("Rex");

console.log(dog.speak()); // Rex barks
```

Use inheritance carefully. Composition is often simpler for application logic.

## Common Mistakes

- Forgetting `new`:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}

const user = new User("Alex");
console.log(user.name);
```

- Putting too much logic in the constructor:

```javascript
class UserReport {
  constructor(user) {
    this.user = user;
  }

  buildTitle() {
    return `Report for ${this.user.name}`;
  }
}
```

Keep constructors focused on initial setup.

- Exposing data that should stay private:

```javascript
class Session {
  #token;

  constructor(token) {
    this.#token = token;
  }

  hasToken() {
    return Boolean(this.#token);
  }
}
```

## Practice

1. Create a `User` class with `name` and `email`.
2. Add a method that returns a display name.
3. Create a class with a private field.
4. Add one static helper method to a class.

## Related Topics

- [`this`](this.md)
- [Prototypes](prototypes.md)
- [Modules](modules.md)

