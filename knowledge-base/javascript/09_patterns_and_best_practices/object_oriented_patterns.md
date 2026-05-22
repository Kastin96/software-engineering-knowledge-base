# Object-Oriented Patterns

## Goal

Use objects and classes to group related data and behavior.

## Why It Matters

Object-oriented patterns are useful for modeling domain concepts, services,
stateful components, and behavior that belongs with specific data.

## Objects With Methods

An object can group data and behavior.

```javascript
const cart = {
  items: [],
  addItem(item) {
    this.items.push(item);
  },
  getTotal() {
    return this.items.reduce((total, item) => total + item.price, 0);
  },
};

cart.addItem({ name: "Keyboard", price: 99 });

console.log(cart.getTotal());
```

## Classes

Classes are useful when you need many objects with the same behavior.

```javascript
class Cart {
  constructor() {
    this.items = [];
  }

  addItem(item) {
    this.items.push(item);
  }

  getTotal() {
    return this.items.reduce((total, item) => total + item.price, 0);
  }
}

const cart = new Cart();

cart.addItem({ name: "Mouse", price: 40 });

console.log(cart.getTotal());
```

## Encapsulation

Encapsulation keeps internal details behind methods.

```javascript
class BankAccount {
  #balance = 0;

  deposit(amount) {
    if (amount <= 0) {
      throw new Error("Deposit amount must be positive");
    }

    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}
```

Callers use methods instead of changing internal state directly.

## Composition

Composition builds behavior by combining smaller parts.

```javascript
function createLogger(prefix) {
  return {
    log(message) {
      console.log(`[${prefix}] ${message}`);
    },
  };
}

class UserService {
  constructor(logger) {
    this.logger = logger;
  }

  createUser(name) {
    this.logger.log(`Creating user ${name}`);

    return {
      name,
    };
  }
}

const service = new UserService(createLogger("UserService"));

service.createUser("Alex");
```

## Inheritance

Inheritance can share behavior between related classes.

```javascript
class Notification {
  constructor(message) {
    this.message = message;
  }

  format() {
    return this.message;
  }
}

class EmailNotification extends Notification {
  format() {
    return `Email: ${this.message}`;
  }
}

const notification = new EmailNotification("Welcome");

console.log(notification.format());
```

Prefer inheritance when the relationship is truly "is a". Use composition when
objects only need shared capabilities.

## Real Pain Points

- Classes with too many responsibilities become hard to test and change.
- Deep inheritance chains are difficult to understand.
- Overusing `this` in callbacks can create context bugs.
- Public mutable fields make it easy for callers to bypass rules.

## Practice

1. Create a class that stores a list of items and calculates a total.
2. Add a private field to protect internal state.
3. Inject a logger object into a service class.
4. Refactor a simple inheritance example into composition.

## Related Topics

- [Classes](../02_core_concepts/classes.md)
- [`this`](../02_core_concepts/this.md)
- [Prototypes](../02_core_concepts/prototypes.md)

