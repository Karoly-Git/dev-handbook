# React Performance Optimization

## useCallback Hook

------------------------------------------------------------------------

# 1. What is useCallback

`useCallback` is a React hook used to **memoize (cache) a function**.

It prevents React from creating **a new function on every render**.

React will return the **same function reference** unless the
dependencies change.

This is useful when passing functions to **memoized components** or
when a function is used as a **dependency in other hooks**.

------------------------------------------------------------------------

# 2. Basic Syntax

```tsx
const memoizedCallback = useCallback(() => {
  // function logic
}, [dependencies]);
```

Explanation:

    useCallback
        ↓
    stores the function
        ↓
    returns the same function reference
        ↓
    recreates it only if dependencies change

------------------------------------------------------------------------

# 3. Simple Example

```tsx
import { useState, useCallback } from "react";

function Counter() {

  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount(prev => prev + 1);
  }, []);

  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );

}
```

The `increment` function will be **created once** and reused on
subsequent renders.

------------------------------------------------------------------------

# 4. Real World Example

`useCallback` is commonly used together with `React.memo` to prevent
unnecessary child component re-renders.

```tsx
import { useState, useCallback } from "react";
import Child from "./Child";

function Parent() {

  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Child clicked");
  }, []);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Parent Update
      </button>

      <Child onClick={handleClick} />
    </div>
  );

}
```

Without `useCallback`, React would create a **new function every render**,
which would cause the `Child` component to re-render.

------------------------------------------------------------------------

# 5. When to Use useCallback

Use `useCallback` when:

- Passing callbacks to `React.memo` components
- Preventing unnecessary child component re-renders
- Using functions as dependencies in hooks
- Working with large lists or frequent re-renders

Example situations:

    React.memo components
    event handlers passed to children
    useEffect dependencies
    list item callbacks

------------------------------------------------------------------------

# 6. Important Rule

Do not overuse `useCallback`.

Using it everywhere can actually **make code harder to read** and may
not improve performance.

Use it only when:

    a stable function reference is needed
    or
    you want to prevent unnecessary re-renders

------------------------------------------------------------------------

# 7. Mental Model

    useCallback = remember a function

React keeps the **same function reference** between renders and only
creates a new one when dependencies change.