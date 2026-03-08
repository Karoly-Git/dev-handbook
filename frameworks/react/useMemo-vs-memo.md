# React Performance Optimization

## useMemo vs React.memo

------------------------------------------------------------------------

# 1. Overview

`useMemo` and `React.memo` are both used for **performance
optimization** in React.

However, they solve **different problems**.

    useMemo → memoizes values
    React.memo → memoizes components

------------------------------------------------------------------------

# 2. useMemo

`useMemo` caches the result of a calculation.

Example:

``` tsx
const filteredItems = useMemo(() => {
  return items.filter(item => item.active);
}, [items]);
```

React recalculates the value only when `items` changes.

Purpose:

    avoid expensive recalculations

------------------------------------------------------------------------

# 3. React.memo

`React.memo` prevents unnecessary component renders.

Example:

``` tsx
const Row = React.memo(function Row({ item }) {
  return <div>{item.name}</div>;
});
```

The component only re-renders if `item` changes.

Purpose:

    avoid unnecessary component rendering

------------------------------------------------------------------------

# 4. Visual Comparison

Without optimization:

    Parent render
       ↓
    Child render

With `React.memo`:

    Parent render
       ↓
    Child render only if props changed

With `useMemo`:

    render
       ↓
    reuse cached value

------------------------------------------------------------------------

# 5. Using Both Together

These tools are often used together in data-heavy components.

Example:

``` tsx
const filteredItems = useMemo(() => {
  return items.filter(item => item.active);
}, [items]);

const Row = React.memo(function Row({ item }) {
  return <div>{item.name}</div>;
});
```

Result:

    useMemo → prevents expensive filtering
    React.memo → prevents unnecessary row renders

This pattern is common in tables and dashboards.

------------------------------------------------------------------------

# 6. Comparison Table

  Feature                 useMemo         React.memo
  ----------------------- --------------- ------------
  Memoizes                value           component
  Prevents                recalculation   re-render
  Used inside component   yes             no
  Wraps component         no              yes

------------------------------------------------------------------------

# 7. Mental Model

    useMemo → remember a value
    React.memo → remember a component render
