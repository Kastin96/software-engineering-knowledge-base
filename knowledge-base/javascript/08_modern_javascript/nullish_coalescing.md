# Nullish Coalescing

## Goal

Understand how to provide default values only when a value is `null` or
`undefined`.

## Why It Matters

Default values are common in configuration, forms, API responses, pagination,
and UI settings. Nullish coalescing avoids replacing valid values like `0`,
`false`, or an empty string.

## Explanation

Nullish coalescing uses `??`.

```javascript
const savedTheme = null;
const theme = savedTheme ?? "light";

console.log(theme); // light
```

It returns the right side only when the left side is `null` or `undefined`.

```javascript
console.log(null ?? "default"); // default
console.log(undefined ?? "default"); // default
console.log(0 ?? "default"); // 0
console.log(false ?? "default"); // false
console.log("" ?? "default"); // ""
```

## Difference From ||

The `||` operator uses truthiness. It replaces all falsy values.

```javascript
const pageSize = 0;

console.log(pageSize || 20); // 20
console.log(pageSize ?? 20); // 0
```

If `0` is a valid value, `??` is usually better.

## Practical Defaults

```javascript
function createPagination(options) {
  return {
    page: options.page ?? 1,
    pageSize: options.pageSize ?? 20,
  };
}

console.log(createPagination({ page: 0, pageSize: 0 }));
```

## With Optional Chaining

```javascript
const config = {};

const timeout = config.network?.timeout ?? 5000;

console.log(timeout);
```

## Nullish Assignment

`??=` assigns a value only when the current value is `null` or `undefined`.

```javascript
const settings = {
  theme: null,
};

settings.theme ??= "light";

console.log(settings.theme); // light
```

## Real Pain Points

- Use `??` when `0`, `false`, or `""` are valid values. Using `||` can replace
  them by accident.
- Do not use defaults to hide missing required configuration. Required values
  should be validated.

```javascript
function readRequired(value, name) {
  if (value == null) {
    throw new Error(`${name} is required`);
  }

  return value;
}
```

- Be clear about whether `null` means "missing" or "intentionally empty" in your
  data model.

## Practice

1. Replace a `||` default with `??` where `0` is valid.
2. Use `??` to default a missing theme.
3. Combine `?.` and `??` to read nested settings.
4. Use `??=` to initialize a missing property.

## Related Topics

- [Optional Chaining](optional_chaining.md)
- [Operators](../01_language_basics/operators.md)
- [Environment Variables](../06_node_javascript/environment_variables.md)

