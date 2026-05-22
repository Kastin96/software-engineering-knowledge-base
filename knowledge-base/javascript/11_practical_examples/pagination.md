# Pagination

## Goal

Implement pagination logic for lists, tables, and API requests.

## Example: Client-Side Pagination

```javascript
function paginateItems(items, { page, pageSize }) {
  const safePage = Math.max(1, page);
  const safePageSize = Math.max(1, pageSize);
  const totalItems = items.length;
  const totalPages = Math.max(1, Math.ceil(totalItems / safePageSize));
  const currentPage = Math.min(safePage, totalPages);
  const startIndex = (currentPage - 1) * safePageSize;
  const endIndex = startIndex + safePageSize;

  return {
    items: items.slice(startIndex, endIndex),
    page: currentPage,
    pageSize: safePageSize,
    totalItems,
    totalPages,
    hasPreviousPage: currentPage > 1,
    hasNextPage: currentPage < totalPages,
  };
}
```

Usage:

```javascript
const products = Array.from({ length: 42 }, (_, index) => ({
  id: index + 1,
  name: `Product ${index + 1}`,
}));

console.log(
  paginateItems(products, {
    page: 2,
    pageSize: 10,
  }),
);
```

## Example: API Query Pagination

```javascript
function buildPaginationQuery({ page, pageSize, search, sort }) {
  const params = new URLSearchParams({
    page: String(Math.max(1, page)),
    pageSize: String(Math.max(1, pageSize)),
  });

  if (search?.trim()) {
    params.set("search", search.trim());
  }

  if (sort) {
    params.set("sort", sort);
  }

  return params.toString();
}

const query = buildPaginationQuery({
  page: 1,
  pageSize: 20,
  search: "keyboard",
  sort: "price_desc",
});

console.log(`/api/products?${query}`);
```

## Example: Normalize Paginated API Response

Raw response:

```javascript
const response = {
  data: [{ id: 1, name: "Keyboard" }],
  meta: {
    page: 1,
    page_size: 20,
    total_items: 42,
  },
};
```

Implementation:

```javascript
function normalizePaginatedResponse(response) {
  const page = response.meta.page;
  const pageSize = response.meta.page_size;
  const totalItems = response.meta.total_items;
  const totalPages = Math.max(1, Math.ceil(totalItems / pageSize));

  return {
    items: response.data,
    page,
    pageSize,
    totalItems,
    totalPages,
    hasPreviousPage: page > 1,
    hasNextPage: page < totalPages,
  };
}

console.log(normalizePaginatedResponse(response));
```

## Example: Pagination State Update

```javascript
function goToNextPage(state) {
  if (!state.hasNextPage) {
    return state;
  }

  return {
    ...state,
    page: state.page + 1,
  };
}

function changePageSize(state, pageSize) {
  return {
    ...state,
    page: 1,
    pageSize,
  };
}
```

Resetting to page `1` after changing `pageSize` avoids requesting a page that no
longer exists.

## What This Demonstrates

- Safe page and page size handling.
- Derived metadata such as `totalPages`.
- Query string construction.
- API response normalization.
- Immutable pagination state updates.

## Practice

1. Add a `goToPreviousPage` helper.
2. Add a `goToPage(state, page)` helper that clamps the page.
3. Support cursor-based pagination with `nextCursor`.
4. Add tests for empty lists and page values outside the valid range.

## Related Topics

- [Arrays](../03_data_structures/arrays.md)
- [Fetch in the Browser](../05_browser_javascript/fetch_in_browser.md)
- [Test Cases](../10_testing/test_cases.md)

