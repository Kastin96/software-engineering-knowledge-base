# Events

## Goal

Understand how JavaScript responds to user actions and browser changes.

## Why It Matters

Events power interactive pages: clicks, typing, form submissions, keyboard
shortcuts, scrolling, loading, and many other browser behaviors.

## Explanation

An event is something that happens in the browser. You can listen for an event
and run code when it happens.

```html
<button id="save-button">Save</button>
```

```javascript
const button = document.querySelector("#save-button");

button.addEventListener("click", () => {
  console.log("Button clicked");
});
```

## Event Object

The browser passes an event object to the listener.

```javascript
const input = document.querySelector("#name");

input.addEventListener("input", (event) => {
  console.log(event.target.value);
});
```

## Common Events

Click:

```javascript
button.addEventListener("click", () => {
  console.log("clicked");
});
```

Input:

```javascript
input.addEventListener("input", (event) => {
  console.log(event.target.value);
});
```

Submit:

```javascript
const form = document.querySelector("#signup-form");

form.addEventListener("submit", (event) => {
  event.preventDefault();
  console.log("form submitted");
});
```

Keyboard:

```javascript
document.addEventListener("keydown", (event) => {
  if (event.key === "Escape") {
    console.log("Escape pressed");
  }
});
```

## Event Bubbling

Many events bubble from the target element up through parent elements.

```html
<div id="panel">
  <button id="save-button">Save</button>
</div>
```

```javascript
const panel = document.querySelector("#panel");

panel.addEventListener("click", (event) => {
  console.log("Clicked inside panel:", event.target);
});
```

## Event Delegation

Event delegation uses one listener on a parent instead of many listeners on
children.

```html
<ul id="todo-list">
  <li data-id="1">Learn DOM</li>
  <li data-id="2">Learn events</li>
</ul>
```

```javascript
const list = document.querySelector("#todo-list");

list.addEventListener("click", (event) => {
  const item = event.target.closest("li");

  if (!item) {
    return;
  }

  console.log("Clicked item id:", item.dataset.id);
});
```

This works well for lists that change over time.

## Removing Event Listeners

Use the same function reference to remove a listener.

```javascript
function handleClick() {
  console.log("clicked");
}

button.addEventListener("click", handleClick);
button.removeEventListener("click", handleClick);
```

## Real Pain Points

- Anonymous listeners cannot be removed later because there is no function
  reference to pass to `removeEventListener`.
- Dynamic lists are often easier with event delegation than attaching listeners
  to every item.
- Form submit events reload the page unless you call `event.preventDefault()`
  when handling the submit in JavaScript.

## Practice

1. Add a click listener to a button.
2. Read input text from an `input` event.
3. Handle a form submit without reloading the page.
4. Use event delegation for a list of items.

## Related Topics

- [DOM](dom.md)
- [Forms](forms.md)
- [Browser APIs](browser_apis.md)

