# React Performance Optimization

## React.memo

------------------------------------------------------------------------

# 1. What is React.memo

`React.memo` is a **higher-order component** that memoizes a component.

It prevents a component from **re-rendering when its props have not
changed**.

React compares the previous props with the new props.

If they are the same, the component render is skipped.

------------------------------------------------------------------------

# 2. Basic Syntax

``` tsx
const MemoizedComponent = React.memo(Component);
```

or

``` tsx
export default React.memo(Component);
```

------------------------------------------------------------------------

# 3. Simple Example

Child component:

``` tsx
type Props = {
  name: string;
};

function Child({ name }: Props) {
  console.log("Child rendered");

  return <div>Hello {name}</div>;
}

export default React.memo(Child);
```

Parent component:

``` tsx
import { useState } from "react";
import Child from "./Child";

function App() {

  const [count, setCount] = useState(0);

  return (
    <>
      <Child name="Pikachu" />

      <button onClick={() => setCount(count + 1)}>
        Re-render {count}
      </button>
    </>
  );

}
```

Result:

    Parent renders
    Child does not re-render

Because the `name` prop did not change.

------------------------------------------------------------------------

# 4. How React.memo Works

    Parent render
          ↓
    compare previous props
          ↓
    props equal → skip render
    props changed → render component

------------------------------------------------------------------------

# 5. Common Use Cases

`React.memo` is often used for:

-   Table rows
-   List items
-   UI components that render frequently
-   Expensive components

Example structure:

    Table
     ├── TableHeader
     ├── TableBody
     │     ├── TableRow
     │     ├── TableRow
     │     └── TableRow

Each `TableRow` can be wrapped with `React.memo`.

------------------------------------------------------------------------

# 6. When to Use React.memo

Use `React.memo` when:

    component renders often
    props rarely change
    rendering is expensive

Avoid using it for very small components.

------------------------------------------------------------------------

# 7. Mental Model

    React.memo = remember the last rendered component

If props stay the same, React skips rendering.
