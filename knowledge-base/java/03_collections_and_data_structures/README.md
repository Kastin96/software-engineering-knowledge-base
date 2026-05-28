# Java Collections and Data Structures

This section explains the most common Java data structures used in everyday
application code.

After finishing it, you should be able to choose between arrays, `List`, `Set`,
`Map`, `Queue`, and `Deque`, iterate safely, sort values and objects, and
explain the tradeoffs in interview-friendly language.

## Topics

- 01\. [Arrays](arrays.md)
- 02\. [`List`](list.md)
- 03\. [`Set`](set.md)
- 04\. [`Map`](map.md)
- 05\. [`Queue` and `Deque`](queue_deque.md)
- 06\. [Iterators](iterators.md)
- 07\. [Sorting](sorting.md)

## Suggested Learning Flow

Start with arrays because they explain fixed-size indexed storage. Then learn
`List`, `Set`, `Map`, `Queue`, and `Deque`, because they are the most common
collection shapes in backend Java code. Finish with iterators and sorting,
because they affect how collections are processed, modified, and ordered.

## Mini Goal

By the end of this section, write a small program that:

- stores several orders;
- removes duplicate customer emails;
- finds an order by id;
- processes tasks in first-in-first-out order;
- calculates a total;
- sorts results by date or amount;
- avoids unsafe modification while iterating.

## Interview Readiness

You should be able to answer:

- When would you use an array instead of a `List`?
- What is the difference between `ArrayList` and `LinkedList`?
- Why does `HashSet` need correct `equals` and `hashCode`?
- When is a `Map` better than scanning a list?
- When would you use `Queue`, `Deque`, `ArrayDeque`, or `PriorityQueue`?
- What causes `ConcurrentModificationException`?
- How do `Comparable` and `Comparator` differ?
