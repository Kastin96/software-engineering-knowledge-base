# Arrays

## Goal

Understand Java arrays, when they are useful, and why collections are often a
better fit for application code.

## Why It Matters

Arrays are a core Java data structure. They are fixed-size, indexed, fast for
direct access, and used by many APIs. Even when you mostly work with `List`, you
will still see arrays in `main(String[] args)`, low-level code, varargs, and
performance-sensitive paths.

## Creating Arrays

```java
int[] scores = {90, 85, 100};
String[] roles = new String[3];

roles[0] = "admin";
roles[1] = "manager";
roles[2] = "user";
```

Array indexes start at `0`.

```java
System.out.println(scores[0]); // 90
System.out.println(scores.length); // 3
```

## Fixed Size

Array size cannot change after creation.

```java
String[] emails = new String[2];
emails[0] = "alex@example.com";
emails[1] = "sam@example.com";

// This would fail at runtime:
// emails[2] = "mira@example.com";
```

Use `List` when you need to add or remove items frequently.

## Looping Through Arrays

Use an enhanced `for` loop when you only need each value.

```java
int[] scores = {90, 85, 100};

for (int score : scores) {
    System.out.println(score);
}
```

Use an index loop when the position matters.

```java
for (int i = 0; i < scores.length; i++) {
    System.out.println("Score " + i + ": " + scores[i]);
}
```

## Arrays of Objects

```java
Product[] products = {
        new Product("Keyboard", 4999),
        new Product("Mouse", 1999)
};

for (Product product : products) {
    System.out.println(product.name() + ": " + product.priceInCents());
}

record Product(String name, int priceInCents) {
}
```

Arrays can store references to objects, not only primitive values.

## Arrays Utility Methods

`java.util.Arrays` provides common helpers.

```java
import java.util.Arrays;

int[] scores = {90, 85, 100};

Arrays.sort(scores);
System.out.println(Arrays.toString(scores)); // [85, 90, 100]
```

Convert an object array to a fixed-size list view:

```java
String[] roles = {"admin", "manager", "user"};
java.util.List<String> roleList = Arrays.asList(roles);
```

The list returned by `Arrays.asList` has fixed size. You can replace elements,
but you cannot add or remove elements.

## Practical Example

```java
import java.util.Arrays;

public class TopScore {
    public static void main(String[] args) {
        int[] scores = {76, 92, 88, 95, 81};

        Arrays.sort(scores);
        int topScore = scores[scores.length - 1];

        System.out.println("Top score: " + topScore);
    }
}
```

This is realistic for small fixed-size data, but for most application flows a
`List<Integer>` would be more flexible.

## Common Mistakes

- Accessing an index outside the array bounds.
- Using arrays when the size must change often.
- Confusing `array.length` with `list.size()`.
- Printing an array directly instead of using `Arrays.toString`.
- Assuming `Arrays.asList` returns a fully mutable `ArrayList`.

## Interview Questions

1. What does it mean that arrays have fixed size?
2. What is the difference between `array.length` and `list.size()`?
3. When would an array be better than a `List`?
4. What happens when you access an invalid array index?
5. Why can `Arrays.asList` surprise developers?

## Practice

1. Create an array of five product prices.
2. Calculate the total price using a loop.
3. Sort the prices and print the lowest and highest values.
4. Convert a `String[]` into a list and try to explain what can and cannot be
   changed.

## Related Topics

- [`List`](list.md)
- [Sorting](sorting.md)
- [Control Flow](../01_language_basics/control_flow.md)

