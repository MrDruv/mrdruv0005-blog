---
title: "What is Zustand and redux?"

date: 2025-07-15
draft: false
---

> replace all usestate with usestore()

## What Is Zustand?

Zustand (German for "state") is a lightweight state management library built on React hooks. It’s designed to be minimal, fast, and easy to use.

## Key Features

- Hooks-based API: You use it like a regular React hook.
- No context providers: You don’t need to wrap your app in _Provider_ components.
- Minimal boilerplate: Just a few lines to create a store.
- Scalable: Works for small apps and large ones with thousands of components.
- Performance-optimized: Avoids unnecessary re-renders.

```javascript
import { create } from "zustand";

const useStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));
```

You can use useStore() in any component to access or update the state. It’s that simple.

## What Is Redux?

Redux is a predictable state container for JavaScript apps. It centralizes your app’s state and makes it easier to manage, debug, and test.

### Core Concepts

- Store: Holds the entire state of your app.
- Actions: Describe what happened (e.g., { type: 'INCREMENT' }).
- Reducers: Pure functions that update the state based on actions.

### Redux Toolkit

Redux Toolkit is the modern way to use Redux—it reduces boilerplate and simplifies setup.

## Example

```javascript
import { createSlice, configureStore } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: { count: 0 },
  reducers: {
    increment: (state) => {
      state.count += 1;
    },
  },
});

const store = configureStore({ reducer: counterSlice.reducer });
```

## Quick Comparison

| Feature        | Zustand              | Redux Toolkit         |
| -------------- | -------------------- | --------------------- |
| Setup          | Very minimal         | Moderate              |
| Boilerplate    | Low                  | Reduced (via Toolkit) |
| DevTools       | Optional             | Built-in              |
| Performance    | High                 | High                  |
| Learning Curve | Easy                 | Moderate              |
| Use Case       | Small to medium apps | Medium to large apps  |

For **simple and fast** solution Zustand id fantastic.
For **Robust tooling and structure**, Redux(with redux toolkit) is a solid choice.

## side-by-side comparison of how you'd manage app state in:

- Vanilla React (with useState)
- React + Zustand
- React + Redux Toolkit

Let’s use a simple example: managing a counter (count) and a text input (message).

### Vanilla React (useState)

**App.js**

```javascript
// App.js
import { useState } from "react";

function App() {
  const [count, setCount] = useState(0);
  const [message, setMessage] = useState("");

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount((c) => c + 1)}>Increment</button>

      <input
        value={message}
        onChange={(e) => setMessage(e.target.value)}
        placeholder="Type something"
      />
      <p>{message}</p>
    </div>
  );
}
export default App;
```

Simple, but state is local, and difficult to share across components.

### React + Zustand

**store.js**

```javascript
// store.js
import { create } from "zustand";

const useStore = create((set) => ({
  count: 0,
  message: "",
  increment: () => set((state) => ({ count: state.count + 1 })),
  setMessage: (msg) => set({ message: msg }),
}));
export default useStore;
```

**App.js**

```javascript
//App.js
import useStore from "./store";

function App() {
  const count = useStore((state) => state.count);
  const increment = useStore((state) => state.increment);
  const message = useStore((state) => state.message);
  const setMessage = useStore((state) => state.setMessage);

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={increment}>Increment</button>

      <input
        value={message}
        onChange={(e) => setMessage(e.target.value)}
        placeholder="Type something"
      />
      <p>{message}</p>
    </div>
  );
}
export default App;
```

Clean and shareable across components—no _Provider_ needed.

### React + Redux Toolkit

**features/counterSlice.js**

```javascript
//features/counterSlice.js
import { createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: {
    count: 0,
    message: "",
  },
  reducers: {
    increment: (state) => {
      state.count += 1;
    },
    setMessage: (state, action) => {
      state.message = action.payload;
    },
  },
});
export const { increment, setMessage } = counterSlice.actions;
export default counterSlice.reducer;
```

**store.js**

```javascript
//store.js
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./features/counterSlice";

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});
```

**App.js**

```javascript
//App.js
import { useSelector, useDispatch } from "react-redux";
import { increment, setMessage } from "./features/counterSlice";

function App() {
  const count = useSelector((state) => state.counter.count);
  const message = useSelector((state) => state.counter.message);
  const dispatch = useDispatch();

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => dispatch(increment())}>Increment</button>

      <input
        value={message}
        onChange={(e) => dispatch(setMessage(e.target.value))}
        placeholder="Type something"
      />
      <p>{message}</p>
    </div>
  );
}
export default App;
```

Structured, powerful for scaling, with built-in DevTools and middleware.

Summary

| Setup                | React + useState | Zustand | Redux Toolkit    |
| -------------------- | ---------------- | ------- | ---------------- |
| Complexity           | Low              | Low     | Moderate         |
| Global State Sharing | Difficult        | Easy    | Easy             |
| Boilerplate          | None             | Minimal | Moderate         |
| Scalability          | Medium           | High    | Very High        |
| Provider Required    | No               | No      | Yes (<Provider>) |

## Clean Approach: Pick One

- Zustand for lightweight and fast workflows
- Redux Toolkit for robust structure and enterprise-level scaling

Thanks for reading!
