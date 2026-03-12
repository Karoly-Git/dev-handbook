# React useEffect

## Side Effects in React

------------------------------------------------------------------------

# 1. What is useEffect

`useEffect` is a React hook used to **run side effects after a component renders**.

A side effect is anything that interacts with **something outside the React render process**.

Common examples include:

- API requests
- timers
- event listeners
- localStorage
- subscriptions

React runs the effect **after the component is rendered to the DOM**.

------------------------------------------------------------------------

# 2. Basic Syntax

```tsx
useEffect(() => {

  // side effect code

}, [dependencies]);
```

Explanation:

    React renders component
       ↓
    DOM updates
       ↓
    useEffect runs
       ↓
    side effect happens

------------------------------------------------------------------------

# 3. Run Once on Mount

Passing an empty dependency array makes the effect run **only once**.

```tsx
import { useEffect } from "react";

function Example() {

  useEffect(() => {
    console.log("Component mounted");
  }, []);

  return <div>Hello</div>;

}
```

Common uses:

- fetching data
- initializing libraries
- setting up subscriptions

------------------------------------------------------------------------

# 4. Run When State Changes

Effects can run when **specific values change**.

```tsx
import { useState, useEffect } from "react";

function Example() {

  const [count, setCount] = useState<number>(0);

  useEffect(() => {
    console.log("Count changed:", count);
  }, [count]);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );

}
```

The effect runs when:

    count changes

------------------------------------------------------------------------

# 5. Watching Multiple Dependencies

You can track multiple values.

```tsx
useEffect(() => {

  console.log("Filters changed");

}, [search, category]);
```

The effect runs when:

    search changes
    or
    category changes

------------------------------------------------------------------------

# 6. Cleanup Functions

Effects can return a **cleanup function**.

Cleanup runs when:

- the component unmounts
- dependencies change

```tsx
import { useEffect } from "react";

function Example() {

  useEffect(() => {

    const handleResize = (): void => {
      console.log(window.innerWidth);
    };

    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize);
    };

  }, []);

  return <div>Resize the window</div>;

}
```

------------------------------------------------------------------------

# 7. Data Fetching Example

Fetching data is one of the most common uses of `useEffect`.

```tsx
import { useEffect, useState } from "react";

type User = {
  id: number;
  name: string;
};

function Example() {

  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {

    async function fetchUsers(): Promise<void> {

      try {

        const response = await fetch("/api/users");

        if (!response.ok) {
          throw new Error("Failed to fetch users");
        }

        const data: User[] = await response.json();
        setUsers(data);

      } catch (err) {

        setError((err as Error).message);

      } finally {

        setLoading(false);

      }

    }

    fetchUsers();

  }, []);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>{error}</p>;

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );

}
```

------------------------------------------------------------------------

# 8. Common useEffect Mistakes

## Missing Dependencies

Bad example:

```tsx
useEffect(() => {
  console.log(count);
}, []);
```

Correct example:

```tsx
useEffect(() => {
  console.log(count);
}, [count]);
```

Rule:

    Every variable used inside the effect should appear in the dependency array.

------------------------------------------------------------------------

## Making useEffect Async

Bad example:

```tsx
useEffect(async () => {
  const data = await fetch("/api");
}, []);
```

Correct pattern:

```tsx
useEffect(() => {

  async function fetchData(): Promise<void> {
    const response = await fetch("/api");
    await response.json();
  }

  fetchData();

}, []);
```

------------------------------------------------------------------------

## Infinite Render Loop

Bad example:

```tsx
useEffect(() => {
  setCount(count + 1);
}, [count]);
```

This causes:

    render
       ↓
    effect runs
       ↓
    state updates
       ↓
    render again
       ↓
    infinite loop

------------------------------------------------------------------------

# 9. When NOT to Use useEffect

Do not use `useEffect` for **derived state**.

Bad example:

```tsx
const [fullName, setFullName] = useState<string>("");

useEffect(() => {
  setFullName(firstName + " " + lastName);
}, [firstName, lastName]);
```

Better:

```tsx
const fullName = `${firstName} ${lastName}`;
```

------------------------------------------------------------------------

# 10. Quick Reference

| Dependency | When Effect Runs |
|------------|------------------|
| none | every render |
| `[]` | once on mount |
| `[value]` | when value changes |

Examples:

```tsx
useEffect(() => {});
useEffect(() => {}, []);
useEffect(() => {}, [count]);
```

------------------------------------------------------------------------

# 11. Mental Model

    Render = pure calculation
    useEffect = side effects

Flow:

    State change
       ↓
    React renders UI
       ↓
    useEffect runs
       ↓
    side effects happen
```