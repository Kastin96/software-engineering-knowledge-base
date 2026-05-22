# Object Transformations

## Goal

Convert object shapes safely for UI, APIs, and internal application logic.

## Example: Normalize an API User

Raw API data:

```javascript
const apiUser = {
  id: 42,
  first_name: "Alex",
  last_name: "Smith",
  email: "  ALEX@EXAMPLE.COM ",
  profile: {
    avatar_url: "https://example.com/avatar.png",
  },
  flags: {
    is_active: true,
    email_verified: false,
  },
};
```

Implementation:

```javascript
function normalizeEmail(email) {
  return email.trim().toLowerCase();
}

function normalizeUser(apiUser) {
  return {
    id: String(apiUser.id),
    fullName: `${apiUser.first_name} ${apiUser.last_name}`,
    email: normalizeEmail(apiUser.email),
    avatarUrl: apiUser.profile?.avatar_url ?? null,
    active: Boolean(apiUser.flags?.is_active),
    verified: Boolean(apiUser.flags?.email_verified),
  };
}

console.log(normalizeUser(apiUser));
```

Output shape:

```javascript
{
  id: "42",
  fullName: "Alex Smith",
  email: "alex@example.com",
  avatarUrl: "https://example.com/avatar.png",
  active: true,
  verified: false,
}
```

## Example: Build an API Payload

Form state:

```javascript
const formState = {
  name: "  Alex Smith ",
  email: " alex@example.com ",
  role: "admin",
  marketingEmails: false,
};
```

Implementation:

```javascript
function buildCreateUserPayload(formState) {
  const [firstName = "", ...lastNameParts] = formState.name.trim().split(/\s+/);

  return {
    first_name: firstName,
    last_name: lastNameParts.join(" "),
    email: formState.email.trim().toLowerCase(),
    role: formState.role,
    preferences: {
      marketing_emails: formState.marketingEmails,
    },
  };
}

console.log(buildCreateUserPayload(formState));
```

## Example: Pick Public Fields

```javascript
function toPublicUser(user) {
  const { passwordHash, internalNotes, ...publicUser } = user;

  return publicUser;
}

const user = {
  id: "user_1",
  email: "alex@example.com",
  passwordHash: "secret-hash",
  internalNotes: "VIP account",
  active: true,
};

console.log(toPublicUser(user));
```

## Example: Update Nested Settings

```javascript
function updateNotificationSetting(user, channel, enabled) {
  return {
    ...user,
    settings: {
      ...user.settings,
      notifications: {
        ...user.settings.notifications,
        [channel]: enabled,
      },
    },
  };
}

const userWithSettings = {
  id: "user_1",
  settings: {
    theme: "dark",
    notifications: {
      email: true,
      sms: false,
    },
  },
};

console.log(updateNotificationSetting(userWithSettings, "sms", true));
```

## What This Demonstrates

- Converting API naming conventions.
- Keeping mapping logic in named functions.
- Using optional chaining for optional API fields.
- Removing internal fields with rest syntax.
- Updating nested objects without mutating the original.

## Practice

1. Normalize a product API response into UI-friendly fields.
2. Build an API payload from checkout form state.
3. Remove private fields from a user object.
4. Update one nested setting immutably.

## Related Topics

- [Objects](../03_data_structures/objects.md)
- [Optional Chaining](../08_modern_javascript/optional_chaining.md)
- [Immutability](../09_patterns_and_best_practices/immutability.md)

