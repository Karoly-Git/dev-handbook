# React `useMemo`

`useMemo` is a **React performance optimization hook** that **memoizes (caches) the result of a calculation**.

React components run their code **every time they render**. If a calculation is expensive, this can slow down the UI.

`useMemo` ensures the calculation **only runs when its dependencies change**.

---

# Syntax

```tsx
const memoizedValue = useMemo(() => {
    return calculation();
}, [dependencies]);
```

| Part             | Description                           |
| ---------------- | ------------------------------------- |
| `useMemo()`      | React hook that memoizes a value      |
| Function         | The calculation to run                |
| Dependency array | When React should recompute the value |
| Return value     | The cached result                     |

---

# Basic Example

Without `useMemo`, calculations run **on every render**.

```tsx
import { useState } from "react";

export default function Example() {
    const [count, setCount] = useState<number>(0);

    const doubled = count * 2;

    return (
        <>
            <p>Doubled: {doubled}</p>

            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
        </>
    );
}
```

Here `doubled` recalculates **every render**, even if unnecessary.

---

# Expensive Calculation Example

Imagine a slow calculation.

```tsx
function slowCalculation(num: number): number {
    console.log("Running slow calculation...");

    for (let i = 0; i < 1000000000; i++) {}

    return num * 2;
}
```

Without `useMemo`, this runs **every render**.

---

# Using `useMemo`

```tsx
import { useMemo, useState } from "react";

function slowCalculation(num: number): number {
    console.log("Running slow calculation...");

    for (let i = 0; i < 1000000000; i++) {}

    return num * 2;
}

export default function Example() {
    const [count, setCount] = useState<number>(0);
    const [number, setNumber] = useState<number>(5);

    const result = useMemo(() => {
        return slowCalculation(number);
    }, [number]);

    return (
        <>
            <p>Result: {result}</p>

            <button onClick={() => setCount(count + 1)}>
                Re-render
            </button>

            <button onClick={() => setNumber(number + 1)}>
                Change Number
            </button>
        </>
    );
}
```

Behavior:

| Action           | Slow Calculation Runs |
| ---------------- | --------------------- |
| Re-render button | ❌ No                  |
| Change number    | ✅ Yes                 |

---

# Filtering Lists (Real World Example)

A common real-world case is **filtering lists**.

Without `useMemo`:

```tsx
const filteredUsers = users.filter((user) =>
    user.name.includes(search)
);
```

This runs **every render**.

With `useMemo`:

```tsx
import { useMemo } from "react";

const filteredUsers = useMemo(() => {
    return users.filter((user) =>
        user.name.includes(search)
    );
}, [users, search]);
```

Now filtering only runs when:

* `users` changes
* `search` changes

---

# When NOT to Use `useMemo`

Do not use `useMemo` for simple calculations.

Bad example:

```tsx
const value = useMemo(() => count + 1, [count]);
```

Why this is unnecessary:

* The calculation is trivial
* `useMemo` itself has overhead

Rule of thumb:

Use `useMemo` when:

* Expensive calculations
* Large lists
* Complex filtering/sorting
* Preventing unnecessary re-renders

---

# `useMemo` vs `useCallback`

| Hook          | Memoizes       |
| ------------- | -------------- |
| `useMemo`     | A **value**    |
| `useCallback` | A **function** |

Example:

```tsx
const memoizedValue = useMemo(() => computeValue(), []);
const memoizedFunction = useCallback(() => computeValue(), []);
```

---

# Quick Tip

Use `useMemo` mainly for **expensive operations like filtering or sorting large datasets**.

Example:

```tsx
const sortedItems = useMemo(() => {
    return items
        .filter((item) => item.active)
        .sort((a, b) => a.name.localeCompare(b.name));
}, [items]);
```

This prevents **filtering and sorting from running on every render**.
