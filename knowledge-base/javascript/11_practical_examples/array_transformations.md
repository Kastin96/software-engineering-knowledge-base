# Array Transformations

## Goal

Transform raw arrays into useful application data without mutating the original
input.

## Example: Build an Order Summary

Input:

```javascript
const orders = [
  {
    id: "ord_1",
    customerId: "cus_1",
    status: "paid",
    items: [
      { sku: "keyboard", quantity: 1, price: 99 },
      { sku: "mouse", quantity: 2, price: 40 },
    ],
  },
  {
    id: "ord_2",
    customerId: "cus_2",
    status: "pending",
    items: [{ sku: "monitor", quantity: 1, price: 240 }],
  },
  {
    id: "ord_3",
    customerId: "cus_1",
    status: "paid",
    items: [{ sku: "usb-cable", quantity: 3, price: 12 }],
  },
];
```

Implementation:

```javascript
function calculateOrderTotal(order) {
  return order.items.reduce((total, item) => {
    return total + item.quantity * item.price;
  }, 0);
}

function buildPaidOrderSummary(orders) {
  return orders
    .filter((order) => order.status === "paid")
    .map((order) => ({
      id: order.id,
      customerId: order.customerId,
      itemCount: order.items.reduce((count, item) => count + item.quantity, 0),
      total: calculateOrderTotal(order),
    }))
    .sort((a, b) => b.total - a.total);
}

console.log(buildPaidOrderSummary(orders));
```

Output shape:

```javascript
[
  { id: "ord_1", customerId: "cus_1", itemCount: 3, total: 179 },
  { id: "ord_3", customerId: "cus_1", itemCount: 3, total: 36 },
];
```

## Example: Group Orders by Customer

```javascript
function groupOrdersByCustomer(orders) {
  return orders.reduce((groups, order) => {
    const currentOrders = groups[order.customerId] ?? [];

    return {
      ...groups,
      [order.customerId]: [...currentOrders, order],
    };
  }, {});
}

console.log(groupOrdersByCustomer(orders));
```

For large arrays, mutating the local accumulator can be acceptable and faster:

```javascript
function groupOrdersByCustomerFast(orders) {
  return orders.reduce((groups, order) => {
    groups[order.customerId] ??= [];
    groups[order.customerId].push(order);
    return groups;
  }, {});
}
```

The mutation is local to the function and does not mutate the original `orders`
array.

## Example: Find Duplicate Emails

```javascript
function findDuplicateEmails(users) {
  const seen = new Set();
  const duplicates = new Set();

  for (const user of users) {
    const email = user.email.trim().toLowerCase();

    if (seen.has(email)) {
      duplicates.add(email);
      continue;
    }

    seen.add(email);
  }

  return [...duplicates];
}
```

Usage:

```javascript
const users = [
  { id: 1, email: "alex@example.com" },
  { id: 2, email: "sam@example.com" },
  { id: 3, email: " ALEX@example.com " },
];

console.log(findDuplicateEmails(users)); // ["alex@example.com"]
```

## What This Demonstrates

- `filter` for selecting records.
- `map` for changing shape.
- `reduce` for totals and grouping.
- `Set` for duplicate detection.
- Local mutation when it makes a helper clearer or faster.

## Practice

1. Add an `averageOrderTotal` function.
2. Group paid orders and pending orders separately.
3. Return the top three orders by total.
4. Find duplicate product SKUs across all orders.

## Related Topics

- [Arrays](../03_data_structures/arrays.md)
- [Maps and Sets](../03_data_structures/maps_sets.md)
- [Functional Patterns](../09_patterns_and_best_practices/functional_patterns.md)

