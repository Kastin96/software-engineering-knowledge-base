# Array Methods

## Read Without Changing

```javascript
items.length;
items[0];
items.at(-1);
items.includes(value);
```

## Add or Remove With New Arrays

```javascript
const added = [...items, newItem];
const removed = items.filter((item) => item.id !== id);
const replaced = items.map((item) =>
  item.id === id ? { ...item, ...updates } : item,
);
```

## map

Transforms every item.

```javascript
const names = users.map((user) => user.name);
```

Input length and output length are the same.

## filter

Keeps matching items.

```javascript
const activeUsers = users.filter((user) => user.active);
```

Output length can be smaller.

## find

Returns the first matching item or `undefined`.

```javascript
const user = users.find((user) => user.id === id);
```

Use `?? null` when you want a stable `null` result.

```javascript
const user = users.find((user) => user.id === id) ?? null;
```

## findIndex

Returns the first matching index or `-1`.

```javascript
const index = users.findIndex((user) => user.id === id);
```

## some

Checks whether at least one item matches.

```javascript
const hasAdmin = users.some((user) => user.role === "admin");
```

## every

Checks whether all items match.

```javascript
const allActive = users.every((user) => user.active);
```

## reduce

Combines values into one result.

```javascript
const total = items.reduce((sum, item) => {
  return sum + item.price * item.quantity;
}, 0);
```

Group by key:

```javascript
const usersByRole = users.reduce((groups, user) => {
  groups[user.role] ??= [];
  groups[user.role].push(user);
  return groups;
}, {});
```

Create lookup:

```javascript
const usersById = users.reduce((lookup, user) => {
  lookup[user.id] = user;
  return lookup;
}, {});
```

## sort

Sorts the array in place.

```javascript
const sorted = [...users].sort((a, b) => a.name.localeCompare(b.name));
```

Number sort:

```javascript
const sortedNumbers = [...numbers].sort((a, b) => a - b);
```

Descending:

```javascript
const newestFirst = [...posts].sort((a, b) => b.createdAt - a.createdAt);
```

## flat

Flattens nested arrays.

```javascript
const values = [[1, 2], [3, 4]].flat();
// [1, 2, 3, 4]
```

## flatMap

Maps and flattens one level.

```javascript
const tags = posts.flatMap((post) => post.tags);
```

## slice

Returns part of an array without mutating it.

```javascript
const pageItems = items.slice(startIndex, endIndex);
```

## splice

Mutates the array. Use carefully.

```javascript
items.splice(index, 1);
```

Prefer immutable alternatives for app state:

```javascript
const nextItems = items.filter((_, itemIndex) => itemIndex !== index);
```

## join

Creates a string.

```javascript
const label = names.join(", ");
```

## Unique Values

```javascript
const uniqueTags = [...new Set(tags)];
```

## Common Patterns

Update by id:

```javascript
function updateById(items, id, updates) {
  return items.map((item) =>
    item.id === id ? { ...item, ...updates } : item,
  );
}
```

Remove by id:

```javascript
function removeById(items, id) {
  return items.filter((item) => item.id !== id);
}
```

Toggle value:

```javascript
function toggleSelected(items, id) {
  return items.map((item) =>
    item.id === id ? { ...item, selected: !item.selected } : item,
  );
}
```

Paginate:

```javascript
function paginate(items, page, pageSize) {
  const startIndex = (page - 1) * pageSize;
  return items.slice(startIndex, startIndex + pageSize);
}
```

## Method Choice

- Use `map` for transformation.
- Use `filter` for selection.
- Use `find` for one item.
- Use `some` for existence.
- Use `every` for all-match checks.
- Use `reduce` for totals, grouping, and lookups.
- Use `sort` on a copy when original order matters.

