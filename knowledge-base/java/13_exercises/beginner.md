# Beginner Exercises

## 1. Email Normalizer

Write a method:

```java
static String normalizeEmail(String email)
```

Expected behavior:

- trims surrounding whitespace;
- converts to lowercase;
- throws `IllegalArgumentException` when the email is `null`, blank, or missing
  `@`.

## 2. Price Total

Write a method:

```java
static int totalInCents(int unitPriceInCents, int quantity)
```

Expected behavior:

- returns `unitPriceInCents * quantity`;
- rejects negative price;
- rejects negative quantity.

## 3. Active User Emails

Given:

```java
record User(String email, boolean active) {
}
```

Write a method that accepts `List<User>` and returns `List<String>` containing
only active user emails.

Expected behavior:

- preserves order;
- returns an unmodifiable result;
- does not mutate the input list.

## 4. Duplicate Tags

Write a method that accepts `List<String>` and returns duplicate tags.

Expected behavior:

- returns each duplicate only once;
- ignores blank tags;
- preserves first duplicate discovery order.

## 5. Grade Label

Write a method:

```java
static String gradeLabel(int score)
```

Expected behavior:

- `90..100` returns `"A"`;
- `80..89` returns `"B"`;
- `70..79` returns `"C"`;
- `0..69` returns `"Needs improvement"`;
- values outside `0..100` throw `IllegalArgumentException`.

