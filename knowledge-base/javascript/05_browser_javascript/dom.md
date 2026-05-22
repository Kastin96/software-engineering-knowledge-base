# DOM

## Goal

Understand how JavaScript reads and changes HTML in the browser.

## Why It Matters

The DOM is how browser JavaScript interacts with a web page. Even if you later
use React or another framework, the framework still works with the DOM under the
hood.

## Explanation

DOM means Document Object Model. The browser turns HTML into a tree of nodes
that JavaScript can read and update.

Example HTML:

```html
<h1 id="title">Hello</h1>
<p class="description">Welcome to the page</p>
```

JavaScript can select these elements.

```javascript
const title = document.querySelector("#title");
const description = document.querySelector(".description");

console.log(title.textContent);
console.log(description.textContent);
```

## Selecting Elements

Use `querySelector` for one element.

```javascript
const button = document.querySelector(".save-button");
```

Use `querySelectorAll` for many elements.

```javascript
const items = document.querySelectorAll(".list-item");

items.forEach((item) => {
  console.log(item.textContent);
});
```

## Changing Text and HTML

Use `textContent` for text.

```javascript
const title = document.querySelector("#title");

title.textContent = "Updated title";
```

Use `innerHTML` only when you intentionally want to insert HTML.

```javascript
const container = document.querySelector("#container");

container.innerHTML = "<strong>Important</strong>";
```

Prefer `textContent` for user-provided values to avoid inserting unsafe HTML.

## Changing Classes

```javascript
const message = document.querySelector(".message");

message.classList.add("visible");
message.classList.remove("hidden");
message.classList.toggle("highlighted");
```

## Changing Attributes

```javascript
const link = document.querySelector("a");

link.setAttribute("href", "https://example.com");
link.setAttribute("target", "_blank");

console.log(link.getAttribute("href"));
```

## Creating Elements

```javascript
const list = document.querySelector("#users");

const item = document.createElement("li");
item.textContent = "Alex";

list.append(item);
```

## Rendering a List

```javascript
const users = [
  { id: 1, name: "Alex" },
  { id: 2, name: "Sam" },
];

const list = document.querySelector("#users");

list.innerHTML = "";

for (const user of users) {
  const item = document.createElement("li");
  item.textContent = user.name;
  list.append(item);
}
```

## Real Pain Points

- Code can run before the DOM element exists.

```javascript
document.addEventListener("DOMContentLoaded", () => {
  const title = document.querySelector("#title");
  title.textContent = "Ready";
});
```

- `querySelector` can return `null`, so production code should handle missing
  elements when the selector may not exist.

```javascript
const message = document.querySelector(".message");

if (message) {
  message.textContent = "Saved";
}
```

- Avoid putting user input into `innerHTML`. Use `textContent` unless you really
  need HTML.

## Practice

1. Select an element by id and change its text.
2. Select all items with a class and log their text.
3. Add and remove a CSS class with `classList`.
4. Render an array of users into a list.

## Related Topics

- [Events](events.md)
- [Forms](forms.md)
- [Arrays](../03_data_structures/arrays.md)

