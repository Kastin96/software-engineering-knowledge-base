# Symbols

## Goal

Understand what symbols are and why JavaScript uses them.

## Why It Matters

Symbols are less common than strings, numbers, arrays, and objects, but they are
important for unique object keys and built-in JavaScript protocols such as
iteration.

## Explanation

A symbol is a unique primitive value.

```javascript
const firstId = Symbol("id");
const secondId = Symbol("id");

console.log(firstId === secondId); // false
```

The description helps with debugging, but it does not make symbols equal.

## Symbols as Object Keys

Symbols can be used as object property keys.

```javascript
const internalId = Symbol("internalId");

const user = {
  name: "Alex",
  [internalId]: 123,
};

console.log(user.name);
console.log(user[internalId]);
```

Symbol keys do not appear in `Object.keys`.

```javascript
console.log(Object.keys(user)); // ["name"]
console.log(Object.getOwnPropertySymbols(user)); // [Symbol(internalId)]
```

This can help avoid accidental property name conflicts.

## Well-Known Symbols

JavaScript has built-in symbols that customize language behavior.

`Symbol.iterator` tells JavaScript how to iterate over an object.

```javascript
const numbers = {
  values: [1, 2, 3],
  [Symbol.iterator]() {
    return this.values[Symbol.iterator]();
  },
};

for (const number of numbers) {
  console.log(number);
}
```

## Symbol.for

`Symbol.for` uses a global symbol registry.

```javascript
const first = Symbol.for("app.user");
const second = Symbol.for("app.user");

console.log(first === second); // true
```

Use this only when shared symbol identity is intentional.

## Practical Example

Use a symbol to attach internal metadata without colliding with normal keys.

```javascript
const metadataKey = Symbol("metadata");

function attachMetadata(record, metadata) {
  return {
    ...record,
    [metadataKey]: metadata,
  };
}

const user = attachMetadata(
  { id: 1, name: "Alex" },
  { loadedAt: new Date().toISOString() },
);

console.log(user.name);
console.log(user[metadataKey]);
```

## Real Pain Points

- Symbols are not private security boundaries. Code with access to the symbol can
  read the value, and symbols can be discovered with `Object.getOwnPropertySymbols`.
- JSON serialization ignores symbol-keyed properties.

```javascript
const key = Symbol("key");
const data = {
  visible: true,
  [key]: "hidden from JSON",
};

console.log(JSON.stringify(data)); // {"visible":true}
```

- Use normal string keys for normal data. Symbols are best for uniqueness,
  metadata, or language protocols.

## Practice

1. Create two symbols with the same description and compare them.
2. Use a symbol as an object key.
3. Read symbol keys with `Object.getOwnPropertySymbols`.
4. Create a simple object that implements `Symbol.iterator`.

## Related Topics

- [Iterators and Generators](iterators_generators.md)
- [Objects](../03_data_structures/objects.md)
- [JSON](../03_data_structures/json.md)
