---
title: "refactoring my todo app"

date: 2025-07-15
draft: false
---

First i have build Endeavour-Todo-App using below tech.

- React(**useState**, **useEffect**)
- Nestjs + prisma
- postgreSQL

now refactoring few things.

## Refactoring 1 (Zustand integration for global state)

> adding useStore() instead of useState()

> PR: https://github.com/MrDruv/Endeavour-with-react/pull/9#issue-3231491369

**useStore instead of useState**

**useState**:React's built-in _Local State_.

**useStore**:Zustand's _global state_

**useState**

```javascript
const [count, setCount] = useState(0);
```

**useStore**

```javascript
const { tasks, setTasks } = useStore();
```

## Refactoring 2 (State Manager- ReactQuery)

> PR: https://github.com/MrDruv/Endeavour-with-react/pull/9#issue-3231491369

Instead of writting boilerplate like below:

```javascript
useEffect(() => {
  fetch("/api/data")
    .then((res) => res.json())
    .then(setData)
    .catch(setError);
}, []);
```

I used ReactQuery like this:

```javascript
const { data, isLoading, error } = useQuery(["todos"], fetchTodos);
```

It handles the rest.

So,

- React Query -> handles server state(todo, user info)
- Zustand -> handles client state (current input, expanded task, token, UI flags)
