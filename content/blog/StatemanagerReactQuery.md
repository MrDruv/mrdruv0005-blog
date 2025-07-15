---
title: "State Manager(React Query)"

date: 2025-07-15
draft: false
---

## What is State Manager?

A state manager is any system, library or pattern used to store, update and share state(data) in your application.

We encounter two major type of states in **React**:

1. Client state:

- Lives in Frontend
- Best tools: - useState(native react) - Zustand - Redux

  2.Server State:

- Comes from Backend
- Best tools:
  - React Query (aka TanStack Query)

### So Why Use React Query for Server State?

React Query simplifies and automates everything about API data:

| Feature                  | Manual `fetch()`        | React Query                      |
| ------------------------ | ----------------------- | -------------------------------- |
| **Data fetching**        | Manually handled        | Declarative with `useQuery`      |
| **Caching**              | Not available natively  | Automatic & configurable         |
| **Refetch on focus**     | Must implement manually | Built-in behavior                |
| **Error handling**       | Requires `try/catch`    | Standardized error reporting     |
| **Pagination & retries** | Manual logic needed     | Easy with built-in options       |
| **Background refresh**   | Not supported           | Stale-while-revalidate & polling |
| **Loading state**        | Manual `useState` logic | Built-in with `isLoading` flag   |

Instead of writing boilerplate like:

```javascript
useEffect(() => {
  fetch("/api/data")
    .then((res) => res.json())
    .then(setData)
    .catch(setError);
}, []);
```

You use React Query’s useQuery like:

```javascript
const { data, isLoading, error } = useQuery(["todos"], fetchTodos);
```

It handles the rest.

Thanks for reading!
