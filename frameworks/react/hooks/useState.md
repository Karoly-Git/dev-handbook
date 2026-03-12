# React `useState`

## Basic Syntax

| Part | Meaning |
|-----|------|
| `state` | Current value stored in state |
| `setState` | Function used to update the state |
| `initialValue` | Starting value of the state |

```tsx
import { useState } from "react";

export default function Example() {
    const [state, setState] = useState<string>("Hello");

    return (
        <div>
            <p>{state}</p>
        </div>
    );
}
```

---

# Implementation Examples

## Counter Example

A simple example where clicking a button updates the state.

```tsx
import { useState } from "react";

export default function Example() {
    const [count, setCount] = useState<number>(0);

    const increase = (): void => {
        setCount(count + 1);
    };

    return (
        <div>
            <p>Count: {count}</p>

            <button onClick={increase}>
                Increase
            </button>
        </div>
    );
}
```

---

## Input State

Very common in forms when storing input values.

```tsx
import { useState } from "react";

export default function Example() {
    const [value, setValue] = useState<string>("");

    const handleChange = (e: React.ChangeEvent<HTMLInputElement>): void => {
        setValue(e.target.value);
    };

    return (
        <input
            type="text"
            value={value}
            onChange={handleChange}
        />
    );
}
```

---

## Object State

State can also store objects.

```tsx
import { useState } from "react";

type User = {
    name: string;
    age: number;
};

export default function Example() {
    const [user, setUser] = useState<User>({
        name: "John",
        age: 25
    });

    const changeName = (): void => {
        setUser({
            ...user,
            name: "Alice"
        });
    };

    return (
        <div>
            <p>{user.name}</p>
            <p>{user.age}</p>

            <button onClick={changeName}>
                Change Name
            </button>
        </div>
    );
}
```

---

## Functional Update

Use when the next state depends on the previous state.

```tsx
import { useState } from "react";

export default function Example() {
    const [count, setCount] = useState<number>(0);

    const increase = (): void => {
        setCount(prev => prev + 1);
    };

    return (
        <button onClick={increase}>
            Count: {count}
        </button>
    );
}
```

---

# Quick Tips

### State triggers re-render

Whenever `setState` is called, React re-renders the component.

---

### Do not mutate state

❌ Wrong

```tsx
user.name = "Alice";
```

✔ Correct

```tsx
setUser({ ...user, name: "Alice" });
```

---

### State updates are asynchronous

```tsx
setCount(count + 1);
console.log(count); // still old value
```

Use functional updates when necessary.

---

# Mental Model

Think of `useState` as:

```
state = memory
setState = update memory + re-render UI
```

Flow:

```
User action
      ↓
setState()
      ↓
React updates state
      ↓
Component re-renders
      ↓
UI updates
```