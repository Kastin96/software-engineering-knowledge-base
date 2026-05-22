# Iterators and Generators

## Goal

Understand how JavaScript reads values one at a time with iterators and how
generators create iterable sequences.

## Why It Matters

Most everyday code uses arrays, but JavaScript also has an iteration protocol.
It powers `for...of`, spread syntax, maps, sets, strings, and custom iterable
objects.

## Iterables

An iterable is a value that can be used with `for...of`.

```javascript
const names = ["Alex", "Sam"];

for (const name of names) {
  console.log(name);
}
```

Strings are iterable too.

```javascript
for (const letter of "JS") {
  console.log(letter);
}
```

## Iterators

An iterator has a `next()` method.

```javascript
const iterator = ["a", "b"][Symbol.iterator]();

console.log(iterator.next()); // { value: "a", done: false }
console.log(iterator.next()); // { value: "b", done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

## Custom Iterable

Create an object that works with `for...of`.

```javascript
const range = {
  start: 1,
  end: 3,
  [Symbol.iterator]() {
    let current = this.start;
    const end = this.end;

    return {
      next() {
        if (current <= end) {
          const value = current;
          current += 1;
          return { value, done: false };
        }

        return { value: undefined, done: true };
      },
    };
  },
};

for (const number of range) {
  console.log(number);
}
```

## Generators

A generator function uses `function*` and `yield`.

```javascript
function* createNumbers() {
  yield 1;
  yield 2;
  yield 3;
}

for (const number of createNumbers()) {
  console.log(number);
}
```

Generators return iterators.

```javascript
const numbers = createNumbers();

console.log(numbers.next());
console.log(numbers.next());
```

## Generator Range

```javascript
function* range(start, end) {
  for (let current = start; current <= end; current += 1) {
    yield current;
  }
}

console.log([...range(1, 5)]); // [1, 2, 3, 4, 5]
```

## Lazy Values

Generators can produce values only when needed.

```javascript
function* createIds() {
  let id = 1;

  while (true) {
    yield id;
    id += 1;
  }
}

const ids = createIds();

console.log(ids.next().value); // 1
console.log(ids.next().value); // 2
```

## Real Pain Points

- Iterators are consumed. Once an iterator is done, calling `next()` continues to
  return `done: true`.
- Infinite generators are useful but need careful consumers. Do not spread an
  infinite generator into an array.
- Most application code does not need custom iterables. Use arrays first unless
  lazy or custom iteration gives a clear benefit.

## Practice

1. Use `for...of` with an array and a string.
2. Manually call `next()` on an array iterator.
3. Write a generator that yields numbers from 1 to 3.
4. Write a generator-based `range(start, end)` function.

## Related Topics

- [Arrays](../03_data_structures/arrays.md)
- [Symbols](symbols.md)
- [Destructuring, Spread, and Rest](../03_data_structures/destructuring_spread_rest.md)

