# Browser DOM Cheatsheet

## Select Elements

```javascript
const element = document.querySelector("#app");
const items = document.querySelectorAll(".item");
```

Handle missing elements:

```javascript
const button = document.querySelector("#save");

if (!button) {
  throw new Error("Save button not found");
}
```

## Text and HTML

```javascript
element.textContent = "Saved";
element.innerHTML = "<strong>Saved</strong>";
```

Prefer `textContent` for user-provided values.

## Classes

```javascript
element.classList.add("active");
element.classList.remove("hidden");
element.classList.toggle("selected");
element.classList.contains("active");
```

## Attributes

```javascript
link.setAttribute("href", "https://example.com");
link.getAttribute("href");
button.removeAttribute("disabled");
```

Boolean properties:

```javascript
button.disabled = true;
checkbox.checked = false;
```

## Data Attributes

```html
<button data-user-id="42">Open</button>
```

```javascript
const userId = button.dataset.userId;
```

## Create and Insert Elements

```javascript
const item = document.createElement("li");
item.textContent = "Alex";

list.append(item);
```

Replace content:

```javascript
container.replaceChildren(item);
```

Remove element:

```javascript
element.remove();
```

## Render List

```javascript
function renderUsers(users) {
  const list = document.querySelector("#users");

  const items = users.map((user) => {
    const item = document.createElement("li");
    item.textContent = user.name;
    return item;
  });

  list.replaceChildren(...items);
}
```

## Events

```javascript
button.addEventListener("click", handleClick);
button.removeEventListener("click", handleClick);
```

```javascript
function handleClick(event) {
  console.log(event.currentTarget);
}
```

Use `currentTarget` for the element that owns the listener. Use `target` for the
actual clicked element.

## Event Delegation

```javascript
list.addEventListener("click", (event) => {
  const item = event.target.closest("[data-user-id]");

  if (!item) {
    return;
  }

  openUser(item.dataset.userId);
});
```

## Forms

```javascript
form.addEventListener("submit", (event) => {
  event.preventDefault();

  const formData = new FormData(form);

  const input = {
    email: String(formData.get("email") ?? ""),
    acceptedTerms: formData.get("acceptedTerms") === "on",
  };

  console.log(input);
});
```

## localStorage

```javascript
localStorage.setItem("theme", "dark");
localStorage.getItem("theme");
localStorage.removeItem("theme");
```

Objects:

```javascript
localStorage.setItem("settings", JSON.stringify(settings));

const settings = JSON.parse(localStorage.getItem("settings") ?? "{}");
```

## URLSearchParams

```javascript
const params = new URLSearchParams({
  page: "1",
  search: "keyboard",
});

const url = `/api/products?${params.toString()}`;
```

## Fetch From Browser

```javascript
async function loadUsers() {
  const response = await fetch("/api/users");

  if (!response.ok) {
    throw new Error(`Request failed with status ${response.status}`);
  }

  return response.json();
}
```

POST JSON:

```javascript
await fetch("/api/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify(user),
});
```

## Abort Request

```javascript
const controller = new AbortController();

fetch("/api/search", {
  signal: controller.signal,
});

controller.abort();
```

## DOM Ready

```javascript
document.addEventListener("DOMContentLoaded", () => {
  initApp();
});
```

Use this when the script can run before the elements exist.

## Useful Browser APIs

```javascript
window.location.href;
window.location.pathname;
window.history.pushState({}, "", "/settings");
window.innerWidth;
window.innerHeight;
```

Clipboard:

```javascript
await navigator.clipboard.writeText("Copied text");
```

Feature check:

```javascript
if ("clipboard" in navigator) {
  await navigator.clipboard.writeText("Copied text");
}
```

## Practical Rules

- Use `textContent` for user text.
- Use event delegation for dynamic lists.
- Call `preventDefault()` for JS-handled form submits.
- Check `response.ok` after `fetch`.
- Clear listeners, intervals, and pending requests when no longer needed.
- Treat local storage as convenience storage, not secure storage.
