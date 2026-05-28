# Sorting

## Goal

Understand how to sort arrays, lists, and custom objects using natural ordering
and comparators.

## Why It Matters

Sorting appears in reports, API responses, admin screens, search results,
leaderboards, and coding interviews. Good sorting code makes ordering rules
explicit and easy to change.

## Sorting Arrays

```java
import java.util.Arrays;

int[] scores = {90, 85, 100};

Arrays.sort(scores);

System.out.println(Arrays.toString(scores)); // [85, 90, 100]
```

`Arrays.sort` sorts the array in place.

## Sorting Lists

```java
import java.util.ArrayList;
import java.util.List;

List<String> names = new ArrayList<>(List.of("Mira", "Alex", "Sam"));

names.sort(String::compareTo);

System.out.println(names); // [Alex, Mira, Sam]
```

The list must be mutable. `List.of(...)` returns an unmodifiable list, so create
a mutable copy if you need to sort in place.

## Natural Ordering With `Comparable`

Use `Comparable` when a class has one obvious natural order.

```java
public record ProductCode(String value) implements Comparable<ProductCode> {
    @Override
    public int compareTo(ProductCode other) {
        return value.compareTo(other.value);
    }
}
```

```java
List<ProductCode> codes = new ArrayList<>(List.of(
        new ProductCode("P-200"),
        new ProductCode("P-100")
));

codes.sort(null);
System.out.println(codes);
```

`sort(null)` uses natural ordering.

## Custom Ordering With `Comparator`

Use `Comparator` when the ordering depends on the use case.

```java
import java.util.Comparator;
import java.util.List;

record Order(String id, int totalInCents) {
}

List<Order> orders = new java.util.ArrayList<>(List.of(
        new Order("o-1", 2500),
        new Order("o-2", 9900),
        new Order("o-3", 4900)
));

orders.sort(Comparator.comparingInt(Order::totalInCents));

System.out.println(orders);
```

Reverse the order with `reversed`.

```java
orders.sort(Comparator.comparingInt(Order::totalInCents).reversed());
```

## Multiple Sort Keys

```java
import java.util.Comparator;
import java.util.List;

record User(String lastName, String firstName, int age) {
}

List<User> users = new java.util.ArrayList<>(List.of(
        new User("Smith", "Alex", 30),
        new User("Smith", "Mira", 25),
        new User("Brown", "Sam", 28)
));

users.sort(
        Comparator.comparing(User::lastName)
                .thenComparing(User::firstName)
                .thenComparingInt(User::age)
);
```

This is the standard readable way to express multiple ordering rules.

## Null Handling

If a field can be `null`, handle it explicitly.

```java
users.sort(
        Comparator.comparing(
                User::lastName,
                Comparator.nullsLast(String::compareTo)
        )
);
```

Better yet, avoid nullable fields in domain objects when possible.

## Practical Example

```java
import java.time.LocalDate;
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

public class RecentOrders {
    public static void main(String[] args) {
        List<Order> orders = new ArrayList<>(List.of(
                new Order("o-1", LocalDate.parse("2026-05-01"), 2500),
                new Order("o-2", LocalDate.parse("2026-05-03"), 9900),
                new Order("o-3", LocalDate.parse("2026-05-02"), 4900)
        ));

        orders.sort(
                Comparator.comparing(Order::createdAt).reversed()
                        .thenComparing(Order::id)
        );

        System.out.println(orders);
    }

    record Order(String id, LocalDate createdAt, int totalInCents) {
    }
}
```

This mirrors API response sorting: newest orders first, stable tie-breaker by id.

## Common Mistakes

- Sorting an unmodifiable `List.of(...)` list in place.
- Using `Comparable` when there is no single natural order.
- Writing subtraction-based comparators, such as `a - b`, which can overflow.
- Forgetting null handling when nullable fields are possible.
- Sorting at many call sites instead of centralizing a repeated business order.

## Interview Questions

1. What is the difference between `Comparable` and `Comparator`?
2. Does sorting a list mutate the list?
3. Why can subtraction-based comparators be dangerous?
4. How do you sort by multiple fields?
5. How can you sort with null values safely?

## Practice

1. Sort a list of names alphabetically.
2. Sort orders by total descending.
3. Sort users by last name, then first name.
4. Create a value object with natural ordering.

## Related Topics

- [Arrays](arrays.md)
- [`List`](list.md)
- [`Set`](set.md)

