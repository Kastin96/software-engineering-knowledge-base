# Type Erasure

## Goal

Understand what type erasure means and how it limits Java generics at runtime.

## Why It Matters

Java generics are mostly a compile-time safety feature. At runtime, much generic
type information is erased. This explains why some code is impossible, why raw
types exist, and why frameworks sometimes need explicit type information.

## What Type Erasure Means

Generic type parameters are checked by the compiler and then erased to their
bound or to `Object`.

```java
public class Box<T> {
    private final T value;

    public Box(T value) {
        this.value = value;
    }

    public T value() {
        return value;
    }
}
```

At runtime, `T` is roughly treated like `Object` when it has no bound.

```java
Box<String> stringBox = new Box<>("hello");
Box<Integer> integerBox = new Box<>(123);

System.out.println(stringBox.getClass() == integerBox.getClass()); // true
```

Both objects are instances of the same runtime class: `Box`.

## Erasure With Bounds

```java
public class NumberBox<T extends Number> {
    private final T value;

    public NumberBox(T value) {
        this.value = value;
    }

    public double asDouble() {
        return value.doubleValue();
    }
}
```

Here `T` is erased to `Number`, because `Number` is the upper bound.

## Cannot Use `new T()`

This does not compile:

```java
public class Factory<T> {
    public T create() {
        // return new T();
        throw new UnsupportedOperationException();
    }
}
```

Java does not know the actual runtime type for `T`.

Pass a factory when you need object creation.

```java
import java.util.function.Supplier;

public class Factory<T> {
    private final Supplier<T> supplier;

    public Factory(Supplier<T> supplier) {
        this.supplier = supplier;
    }

    public T create() {
        return supplier.get();
    }
}
```

## Cannot Check Generic Type With `instanceof`

This does not compile:

```java
// if (value instanceof java.util.List<String>) { ... }
```

You can check the raw type:

```java
if (value instanceof java.util.List<?> list) {
    System.out.println("List size: " + list.size());
}
```

The element type is not safely known from that check.

## Heap Pollution

Heap pollution happens when a variable of a parameterized type refers to an
object that is not actually safe for that type.

```java
java.util.List raw = new java.util.ArrayList<String>();
raw.add(123);

java.util.List<String> strings = raw;

// Runtime failure:
// String first = strings.get(0);
```

The compiler warns about this because raw types bypass generic safety.

## Practical Example

```java
import java.util.ArrayList;
import java.util.List;

public class TypeSafeParser {
    public static List<Integer> parseIntegers(List<String> inputs) {
        List<Integer> result = new ArrayList<>();

        for (String input : inputs) {
            result.add(Integer.parseInt(input));
        }

        return List.copyOf(result);
    }
}
```

This method does not try to discover `T` at runtime. It keeps the type explicit
in the API and lets the compiler protect callers.

## Common Mistakes

- Expecting `List<String>` and `List<Integer>` to be different runtime classes.
- Trying to create `new T()` inside a generic class.
- Checking `instanceof List<String>`.
- Ignoring unchecked warnings caused by raw types.
- Assuming generics provide runtime validation of collection element types.

## Interview Questions

1. What is type erasure?
2. Are `List<String>` and `List<Integer>` different runtime classes?
3. Why can you not create `new T()`?
4. Why can you not use `instanceof List<String>`?
5. What is heap pollution?

## Practice

1. Compare `new Box<String>("a").getClass()` and `new Box<Integer>(1).getClass()`.
2. Rewrite a generic factory to accept `Supplier<T>`.
3. Explain why `instanceof List<?>` is allowed but `instanceof List<String>` is
   not.
4. Create a raw type warning and then fix it with generics.

## Related Topics

- [Raw Types and Type Safety](raw_types_type_safety.md)
- [Generic Classes and Methods](generic_classes_methods.md)
- [Wildcards](wildcards.md)

