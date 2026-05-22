# Form Validation

## Goal

Validate form input with clear rules, useful error messages, and reusable
helpers.

## Example: Signup Validation

Input shape:

```javascript
const signupInput = {
  name: "Alex Smith",
  email: "alex@example.com",
  password: "correct horse battery staple",
  confirmPassword: "correct horse battery staple",
  acceptedTerms: true,
};
```

Validation helpers:

```javascript
function isBlank(value) {
  return value.trim().length === 0;
}

function isValidEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

function validateSignup(input) {
  const errors = {};

  if (isBlank(input.name)) {
    errors.name = "Name is required";
  }

  if (isBlank(input.email)) {
    errors.email = "Email is required";
  } else if (!isValidEmail(input.email)) {
    errors.email = "Email must be valid";
  }

  if (input.password.length < 12) {
    errors.password = "Password must be at least 12 characters";
  }

  if (input.confirmPassword !== input.password) {
    errors.confirmPassword = "Passwords must match";
  }

  if (!input.acceptedTerms) {
    errors.acceptedTerms = "You must accept the terms";
  }

  return errors;
}
```

Usage:

```javascript
const errors = validateSignup(signupInput);

if (Object.keys(errors).length > 0) {
  console.log("Show validation errors", errors);
} else {
  console.log("Submit form");
}
```

## Reading Browser Form Data

```javascript
function readSignupForm(form) {
  const formData = new FormData(form);

  return {
    name: String(formData.get("name") ?? ""),
    email: String(formData.get("email") ?? ""),
    password: String(formData.get("password") ?? ""),
    confirmPassword: String(formData.get("confirmPassword") ?? ""),
    acceptedTerms: formData.get("acceptedTerms") === "on",
  };
}
```

## Rendering Errors

```javascript
function renderFieldError(fieldName, message) {
  const element = document.querySelector(`[data-error-for="${fieldName}"]`);

  if (!element) {
    return;
  }

  element.textContent = message ?? "";
}

function renderSignupErrors(errors) {
  const fields = ["name", "email", "password", "confirmPassword", "acceptedTerms"];

  for (const field of fields) {
    renderFieldError(field, errors[field]);
  }
}
```

## Submit Handler

```javascript
async function handleSignupSubmit(event) {
  event.preventDefault();

  const form = event.currentTarget;
  const input = readSignupForm(form);
  const errors = validateSignup(input);

  renderSignupErrors(errors);

  if (Object.keys(errors).length > 0) {
    return;
  }

  await createUser({
    name: input.name.trim(),
    email: input.email.trim().toLowerCase(),
    password: input.password,
  });

  form.reset();
}
```

## What This Demonstrates

- Validation as pure logic.
- DOM reading at the boundary.
- Error rendering separated from validation.
- Normalization before API submission.
- Clear field-level error messages.

## Practice

1. Add a username rule with allowed characters.
2. Add a rule that blocks common weak passwords.
3. Return multiple errors per field instead of one.
4. Write unit tests for `validateSignup`.

## Related Topics

- [Forms](../05_browser_javascript/forms.md)
- [Unit Testing](../10_testing/unit_testing.md)
- [Pure Functions](../09_patterns_and_best_practices/pure_functions.md)

