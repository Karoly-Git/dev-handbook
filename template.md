# React Performance Optimization

## useMemo Hook

------------------------------------------------------------------------

# 1. What is useMemo

`useMemo` is a React hook used to **memoize (cache) the result of a
calculation**.

It prevents expensive calculations from running **on every render**.

React will only recompute the value when **dependencies change**.

This helps improve performance in components that re-render frequently.

------------------------------------------------------------------------

# 2. Basic Syntax

``` tsx
const memoizedValue = useMemo(() => {
  return expensiveCalculation();
}, [dependencies]);
```

Explanation:

    useMemo
       ↓
    runs the function
       ↓
    stores the result
       ↓
    reuses it unless dependencies change

------------------------------------------------------------------------

# 3. Simple Example

``` tsx
import { useMemo } from "react";

function Example() {

  const numbers = [1, 2, 3, 4, 5];

  const sum = useMemo(() => {
    return numbers.reduce((a, b) => a + b, 0);
  }, [numbers]);

  return <p>Sum: {sum}</p>;

}
```

The sum calculation will only run if `numbers` changes.

------------------------------------------------------------------------

# 4. Real World Example

Filtering and sorting lists is a common use case.

``` tsx
const visibleItems = useMemo(() => {

  return [...items]
    .filter(item =>
      item.name.toLowerCase().includes(search.toLowerCase())
    )
    .sort(
      (a, b) =>
        new Date(b.checkedInAt).getTime() -
        new Date(a.checkedInAt).getTime()
    );

}, [items, search]);
```

React will recompute the list only when:

    items changes
    or
    search changes

------------------------------------------------------------------------

# 5. When to Use useMemo

Use `useMemo` when:

-   Expensive calculations
-   Sorting large lists
-   Filtering large datasets
-   Complex derived values

Examples:

    .filter()
    .sort()
    .reduce()
    heavy calculations

------------------------------------------------------------------------

# 6. Important Rule

Do not overuse `useMemo`.

For small or cheap calculations, the performance gain is usually
negligible.

Use it only when:

    calculations are expensive
    or
    components render frequently

------------------------------------------------------------------------

# 7. Mental Model

    useMemo = remember a computed value

It caches the result and recalculates it only when dependencies change.
