# DevTools

## Goal

Understand how browser DevTools help debug JavaScript, DOM, network requests,
storage, and performance.

## Why It Matters

DevTools are essential for frontend debugging. They let you inspect the page as
the browser sees it, pause JavaScript, inspect network requests, view storage,
and understand runtime behavior.

## Opening DevTools

Common shortcuts:

```text
F12
Ctrl+Shift+I
Cmd+Option+I on macOS
```

You can also right-click a page and choose "Inspect".

## Elements Panel

Use the Elements panel to inspect HTML and CSS.

Useful tasks:

- find the actual element rendered on the page;
- check applied CSS rules;
- test a class or style change;
- confirm whether an element exists.

Example DOM code to inspect:

```javascript
const message = document.querySelector("#message");

message.textContent = "Saved";
message.classList.add("success");
```

In Elements, verify that the text and class were applied.

## Console Panel

Use the Console panel to see logs and run small JavaScript expressions.

```javascript
document.querySelector("#message")?.textContent;
```

This is useful for quick checks, but do not use the console as a replacement for
real code changes.

## Sources Panel and Breakpoints

Use the Sources panel to pause code.

```javascript
function calculateTotal(items) {
  return items.reduce((sum, item) => sum + item.price, 0);
}
```

Set a breakpoint inside the function and inspect:

- current variable values;
- call stack;
- step over;
- step into;
- step out.

You can also add a `debugger` statement while learning.

```javascript
function calculateTotal(items) {
  debugger;
  return items.reduce((sum, item) => sum + item.price, 0);
}
```

Remove `debugger` before committing code.

## Network Panel

Use the Network panel to inspect HTTP requests.

Check:

- request URL;
- method;
- status code;
- request headers;
- response body;
- timing;
- failed or canceled requests.

Example:

```javascript
async function loadUsers() {
  const response = await fetch("/api/users");
  return response.json();
}
```

If the UI is wrong, inspect the actual response before changing rendering code.

## Application Panel

Use the Application panel to inspect browser storage.

Useful areas:

- local storage;
- session storage;
- cookies;
- cache;
- service workers.

Example:

```javascript
localStorage.setItem("theme", "dark");
```

Check whether the value appears under local storage for the current origin.

## Performance Tools

Use performance tools when a page feels slow.

Start with:

- recording a user interaction;
- checking long tasks;
- looking for expensive JavaScript;
- checking repeated layout or rendering work.

For many everyday bugs, Console, Sources, Network, and Application are enough.

## Real Pain Points

- The Network panel shows what the server actually returned. Trust it more than
  assumptions about the API response.
- Breakpoints are often better than many logs when a value changes across
  several steps.
- DevTools changes to HTML or CSS are temporary unless you copy the fix back
  into the project files.
- A cached response or stale local storage value can make a fixed bug look like
  it still exists.

## Practice

1. Inspect an element and identify its applied classes.
2. Set a breakpoint in a function and step through it.
3. Inspect a failed request in the Network panel.
4. View and delete a local storage value.

## Related Topics

- [DOM](../05_browser_javascript/dom.md)
- [Fetch in the Browser](../05_browser_javascript/fetch_in_browser.md)
- [Console](console.md)
