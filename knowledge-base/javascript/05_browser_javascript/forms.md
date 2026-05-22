# Forms

## Goal

Understand how to read, validate, and submit form data in the browser.

## Why It Matters

Forms are used for login, signup, search, checkout, filters, settings, comments,
and many other user workflows.

## Basic Form

```html
<form id="signup-form">
  <input name="name" placeholder="Name" />
  <input name="email" placeholder="Email" />
  <button type="submit">Create account</button>
</form>
```

Handle submit:

```javascript
const form = document.querySelector("#signup-form");

form.addEventListener("submit", (event) => {
  event.preventDefault();

  const formData = new FormData(form);
  const name = formData.get("name");
  const email = formData.get("email");

  console.log(name, email);
});
```

## FormData

`FormData` reads values from form controls with `name` attributes.

```javascript
const formData = new FormData(form);

const user = {
  name: formData.get("name"),
  email: formData.get("email"),
};

console.log(user);
```

## Reading Values Directly

For simple cases, you can select inputs directly.

```javascript
const emailInput = document.querySelector("#email");

console.log(emailInput.value);
```

## Basic Validation

```javascript
function validateUser(user) {
  const errors = {};

  if (!user.name.trim()) {
    errors.name = "Name is required";
  }

  if (!user.email.includes("@")) {
    errors.email = "Email must be valid";
  }

  return errors;
}
```

Use it on submit:

```javascript
form.addEventListener("submit", (event) => {
  event.preventDefault();

  const formData = new FormData(form);

  const user = {
    name: String(formData.get("name") ?? ""),
    email: String(formData.get("email") ?? ""),
  };

  const errors = validateUser(user);

  if (Object.keys(errors).length > 0) {
    console.log(errors);
    return;
  }

  console.log("Submit user:", user);
});
```

## Showing Errors

```html
<p id="email-error"></p>
```

```javascript
const emailError = document.querySelector("#email-error");

emailError.textContent = "Email must be valid";
```

## Resetting a Form

```javascript
form.reset();
```

## Sending Form Data as JSON

```javascript
async function submitUser(user) {
  const response = await fetch("/api/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(user),
  });

  if (!response.ok) {
    throw new Error("Failed to submit user");
  }

  return response.json();
}
```

## Real Pain Points

- A button inside a form submits by default unless its type is set. Use
  `type="button"` for non-submit buttons.
- `FormData.get()` can return `null`, so normalize values before validation.
- Client-side validation improves user experience, but server-side validation is
  still required because browser code can be bypassed.

## Practice

1. Create a form with `name` and `email` fields.
2. Read values with `FormData`.
3. Validate empty name and invalid email.
4. Submit valid data with `fetch`.

## Related Topics

- [Events](events.md)
- [Fetch in the Browser](fetch_in_browser.md)
- [Objects](../03_data_structures/objects.md)

