# Generics Basics

## Goal

Understand why Java has generics and how they make collections and APIs safer.

## Why It Matters

Generics let Java catch type mistakes at compile time. Without generics, code
would rely on `Object`, casts, and runtime failures. In backend Java, generics
show up constantly in `List<User>`, `Map<String, Order>`, `Optional<Product>`,
repository APIs, response wrappers, validators, and test helpers.

## The Problem Without Generics

Before generics, code often used raw collections.

```java
import java.util.ArrayList;
import java.util.List;

List emails = new ArrayList();
emails.add("alex@example.com");
emails.add(123);

String firstEmail = (String) emails.get(0);
String secondEmail = (String) emails.get(1); // ClassCastException at runtime
```

The compiler cannot protect this list from mixed types.

## Generic Collections

Generics move the type rule into the declaration.

```java
import java.util.ArrayList;
import java.util.List;

List<String> emails = new ArrayList<>();
emails.add("alex@example.com");

// Does not compile:
// emails.add(123);

String firstEmail = emails.get(0);
```

`List<String>` means this list should contain strings. Reads are also safer
because no cast is needed.

## Type Parameters

The type inside angle brackets is a type argument.

```java
List<String> names = List.of("Alex", "Sam");
List<Integer> scores = List.of(90, 85, 100);
Map<String, Integer> scoreByUser = Map.of("alex", 90);
```

`List` is the generic type. `String` and `Integer` are type arguments.

## Generics Use Reference Types

Generics work with reference types, not primitive types.

```java
List<Integer> scores = List.of(90, 85, 100);
```

Use wrapper types such as `Integer`, `Long`, `Double`, and `Boolean`.

## Practical Example

```java
import java.util.List;

public class EmailPrinter {
    public static void main(String[] args) {
        List<String> emails = List.of(
                "alex@example.com",
                "sam@example.com"
        );

        printEmails(emails);
    }

    static void printEmails(List<String> emails) {
        for (String email : emails) {
            System.out.println(email.toLowerCase());
        }
    }
}
```

The method signature communicates exactly what the method expects: a list of
strings that represent emails.

## Common Mistakes

- Using raw `List`, `Map`, or `Set` in modern code.
- Using `List<Object>` when the method only needs to read unknown values.
- Forgetting that generics cannot use primitive types directly.
- Adding casts that generics would make unnecessary.
- Hiding important type information behind `Object`.

## Interview Questions

1. What problem do generics solve?
2. Why is `List<String>` safer than raw `List`?
3. Can Java generics use primitive types like `int`?
4. What is a type parameter?
5. Why do generics improve readability?

## Practice

1. Create a `List<String>` of emails and print each value.
2. Create a `Map<String, Integer>` for product stock counts.
3. Rewrite a raw list example using generics.
4. Explain why the compiler rejects adding an `Integer` to `List<String>`.

## Related Topics

- [Generic Classes and Methods](generic_classes_methods.md)
- [Raw Types and Type Safety](raw_types_type_safety.md)
- [Collections and Data Structures](../03_collections_and_data_structures/README.md)

