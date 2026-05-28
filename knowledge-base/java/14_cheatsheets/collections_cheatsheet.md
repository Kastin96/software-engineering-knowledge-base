# Java Collections Cheatsheet

## Common Types

```java
List<String> names = new ArrayList<>();
Set<String> tags = new HashSet<>();
Map<String, User> usersById = new HashMap<>();
Queue<Job> jobs = new ArrayDeque<>();
Deque<String> stack = new ArrayDeque<>();
```

## List

```java
names.add("Alex");
names.get(0);
names.size();
names.isEmpty();
```

Use `List` for ordered values that may contain duplicates.

## Set

```java
Set<String> uniqueEmails = new LinkedHashSet<>(emails);
boolean added = uniqueEmails.add("alex@example.com");
```

Use `Set` for uniqueness.

## Map

```java
usersById.put("u-100", user);
User user = usersById.get("u-100");
User fallback = usersById.getOrDefault("missing", guest);
boolean exists = usersById.containsKey("u-100");
```

Use `Map` for lookup by key.

## Queue and Deque

```java
jobs.offer(job);
Job next = jobs.poll();

stack.push("page");
String last = stack.pop();
```

Use `ArrayDeque` as the usual default for queue/stack-like behavior.

## Sorting

```java
orders.sort(Comparator.comparing(Order::createdAt));
orders.sort(Comparator.comparingInt(Order::totalInCents).reversed());
```

## Immutable Copies

```java
List<String> safeList = List.copyOf(values);
Set<String> safeSet = Set.copyOf(values);
Map<String, User> safeMap = Map.copyOf(usersById);
```

## Watch Outs

- `List.of` returns an unmodifiable list.
- `HashSet` and `HashMap` do not guarantee order.
- Custom keys need correct `equals` and `hashCode`.
- Do not remove directly inside enhanced `for`; use iterator or `removeIf`.

