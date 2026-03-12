# Senior React Developer Knowledge

## Core Areas a Senior React Developer Should Know

| Area | Key Knowledge |
|-----|---------------|
| React Fundamentals | Components, hooks, lifecycle, composition |
| Rendering & Performance | Virtual DOM, reconciliation, memoization |
| State Management | useState, useReducer, Context, global state tools |
| Architecture | Scalable project structure, custom hooks |
| TypeScript | Typed props, events, generics |
| Data Fetching | fetch, axios, React Query |
| Testing | Jest, React Testing Library |
| Routing | React Router, nested routes |
| Forms | React Hook Form, validation |
| Tooling | Vite, Webpack, ESLint |
| Performance | Code splitting, lazy loading |
| Leadership | Code reviews, mentoring |

---

# Implementation Examples

## Custom Hook Example

Use custom hooks to reuse logic across components.

```tsx
import { useState, useEffect } from "react";

type Collection = {
    id: number;
    name: string;
};

export const useCollections = () => {
    const [collections, setCollections] = useState<Collection[]>([]);

    useEffect(() => {
        fetchCollections();
    }, []);

    const fetchCollections = async (): Promise<void> => {
        const response = await fetch("/api/collections");

        if (!response.ok) {
            throw new Error("Failed to fetch collections");
        }

        const data: Collection[] = await response.json();
        setCollections(data);
    };

    return collections;
};
```

---

## Memoized Component

Use `React.memo` to prevent unnecessary re-renders.

```tsx
import React from "react";

type Props = {
    label: string;
};

const Button = React.memo(({ label }: Props) => {
    return (
        <button>
            {label}
        </button>
    );
});

export default Button;
```

---

## Reducer State Management

Use `useReducer` when state logic becomes complex.

```tsx
import { useReducer } from "react";

type Action = "increment" | "decrement";

const reducer = (state: number, action: Action): number => {
    switch (action) {
        case "increment":
            return state + 1;

        case "decrement":
            return state - 1;

        default:
            return state;
    }
};

export default function Counter() {
    const [count, dispatch] = useReducer(reducer, 0);

    return (
        <>
            <button onClick={() => dispatch("increment")}>
                +
            </button>

            <button onClick={() => dispatch("decrement")}>
                -
            </button>

            <p>{count}</p>
        </>
    );
}
```

---

## Data Fetching with React Query

Modern applications often use React Query for server state.

```tsx
import { useQuery } from "@tanstack/react-query";

type Collection = {
    id: number;
    name: string;
};

const getCollections = async (): Promise<Collection[]> => {
    const res = await fetch("/api/collections");

    if (!res.ok) {
        throw new Error("Failed to fetch collections");
    }

    return res.json();
};

export default function Example() {
    const { data, isLoading } = useQuery({
        queryKey: ["collections"],
        queryFn: getCollections
    });

    if (isLoading) {
        return <p>Loading...</p>;
    }

    return (
        <ul>
            {data?.map((c) => (
                <li key={c.id}>
                    {c.name}
                </li>
            ))}
        </ul>
    );
}
```

---

# Quick Tip

Senior React developers focus on **architecture and performance**, not just syntax.

Key principles:

1. Keep components small and reusable.
2. Extract logic into custom hooks.
3. Avoid unnecessary re-renders.
4. Keep server state separate from UI state.
5. Prefer composition over complex inheritance.

Example minimal component:

```tsx
type Props = {
    title: string;
};

export default function Card({ title }: Props) {
    return (
        <div>
            <h2>{title}</h2>
        </div>
    );
}
```
