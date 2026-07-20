# TanStack Query — The Complete Beginner’s Guide

*By [Vinitkumar*](https://medium.com/@vinitkumar93341)

If you’ve been writing React for a while, you’ve probably written this code a hundred times:

```javascript
const [data, setData] = useState([])
const [loading, setLoading] = useState(false)
const [error, setError] = useState(null)

useEffect(() => {
  setLoading(true)
  fetch('/api/users')
    .then(res => res.json())
    .then(data => {
      setData(data)
      setLoading(false)
    })
    .catch(err => {
      setError(err)
      setLoading(false)
    })
}, [])

```

It works. But imagine doing this for every single API call in your app. That’s a lot of repeated boilerplate, and it doesn’t even handle caching, refetching, or background updates.

**TanStack Query solves all of this.** It’s a data fetching library that handles loading states, error states, caching, background refetching, and much more — so you don’t have to.

---

## What even is TanStack Query?

TanStack Query and React Query are the **same thing**. React Query got rebranded to TanStack Query to support more frameworks (Vue, Solid, etc). If you see either name, they refer to the same library.

At its core, TanStack Query is a **server state management library**. It manages data that lives on your server and needs to be fetched, cached, and kept in sync with your UI.

---

## Installation

```bash
npm install @tanstack/react-query

```

Then wrap your app with the `QueryClientProvider`:

```javascript
// main.jsx or App.jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

const queryClient = new QueryClient()

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <YourApp />
    </QueryClientProvider>
  )
}

```

This sets up the cache that TanStack Query uses throughout your app. Every component inside this provider can now use TanStack Query hooks.

---

## The Two Main Hooks

TanStack Query has two hooks you’ll use 90% of the time:

* `useQuery` $\rightarrow$ for **FETCHING** data (GET requests)
* `useMutation` $\rightarrow$ for **CHANGING** data (POST, PUT, DELETE requests)

---

## `useQuery` — Fetching Data

Here’s the same fetch from before, rewritten with TanStack Query:

```javascript
import { useQuery } from '@tanstack/react-query'

function UsersList() {
  const { data, isLoading, error } = useQuery({
    queryKey: ['users'],
    queryFn: () => fetch('/api/users').then(res => res.json())
  })

  if (isLoading) return <p>Loading...</p>
  if (error) return <p>Something went wrong!</p>

  return (
    <ul>
      {data.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  )
}

```

Much cleaner. No `useState`, no `useEffect`, no manual loading/error tracking.

### What does `useQuery` return?

```javascript
const {
  data,        // The actual response data
  isLoading,   // True while fetching for the first time
  isFetching,  // True whenever a fetch is happening (including background)
  error,       // Error object if something went wrong
  isError,     // True if there was an error
  isSuccess,   // True if data was fetched successfully
  refetch      // Function to manually trigger a refetch
} = useQuery({ ... })

```

---

## `queryKey` — The Most Important Concept

The `queryKey` is a **unique label** for your data. Think of it like a named box where TanStack Query stores the cached data.

```javascript
queryKey: ['users']           // Box labelled "users"
queryKey: ['friends']         // Box labelled "friends"
queryKey: ['discover']        // Box labelled "discover"

```

When you use the same key in two different components, they both share the same cached data and only one network request is made.

### Dynamic Query Keys

Query keys can include dynamic values like user IDs:

```javascript
// Fetching a specific user's profile
const { data: user } = useQuery({
  queryKey: ['user', userId],   // Unique per user
  queryFn: () => fetch(`/api/users/${userId}`).then(res => res.json())
})

// Fetching a user's friends
const { data: friends } = useQuery({
  queryKey: ['friends', userId],
  queryFn: () => fetch(`/api/friends/${userId}`).then(res => res.json())
})

```

Different key = different cache entry. So `['user', 'vinit-id']` and `['user', 'sara-id']` are stored separately.

> **Rule of thumb:** Whatever values your `queryFn` depends on should be in the `queryKey`.

```javascript
// CORRECT — userId is in the key
useQuery({
  queryKey: ['friends', userId],
  queryFn: () => fetchFriends(userId)
})

// WRONG — userId is used but not in key
useQuery({
  queryKey: ['friends'],
  queryFn: () => fetchFriends(userId) // userId changes but key never does
})

```

---

## Caching and Stale Data

This is where TanStack Query really shines.

When you fetch data, TanStack Query caches it under the `queryKey`. Next time you need that data, instead of showing a blank screen while fetching, it **instantly shows the old cached data** while silently fetching fresh data in the background. When the fresh data arrives, it updates the UI.

This is called **stale-while-revalidate** and it makes your app feel much faster.

### `staleTime`

By default, data becomes stale immediately. You can control this with `staleTime`:

```javascript
useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers,
  staleTime: 1000 * 60 * 5   // Data stays fresh for 5 minutes
})

```

* `staleTime: 0` $\rightarrow$ Refetch every time (default)
* `staleTime: 60000` $\rightarrow$ Don't refetch for 1 minute
* `staleTime: Infinity` $\rightarrow$ Never refetch unless you manually tell it to

### When does TanStack Query automatically refetch?

1. Component mounts for the first time
2. User navigates away and comes back to the component
3. User alt-tabs out of the browser and comes back
4. Network connection is restored after going offline
5. You manually invalidate the query

---

## `useMutation` — Changing Data

`useQuery` is for reading data. `useMutation` is for creating, updating, or deleting data.

```javascript
import { useMutation } from '@tanstack/react-query'

function AddUserButton() {
  const { mutate, isPending, error } = useMutation({
    mutationFn: (newUser) => fetch('/api/users', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(newUser)
    }).then(res => res.json()),
    onSuccess: (data) => {
      console.log('User created!', data)
    },
    onError: (error) => {
      console.log('Something went wrong!', error)
    }
  })

  return (
    <button 
      onClick={() => mutate({ name: 'Vinit', email: 'Vinit@gmail.com' })} 
      disabled={isPending}
    >
      {isPending ? 'Creating...' : 'Add User'}
    </button>
  )
}

```

### What does `useMutation` return?

```javascript
const {
  mutate,    // Call this to trigger the mutation
  isPending, // True while the request is in progress
  error,     // Error if something went wrong
  isSuccess, // True if mutation succeeded
  data,      // Response data from the mutation
  reset      // Reset mutation state back to idle
} = useMutation({ ... })

```

---

## Cache Invalidation — The Most Powerful Feature

After a mutation, your cached data is outdated. For example, after sending a friend request, your discover list should update to show the new relationship status.

You tell TanStack Query to throw away old cached data and refetch fresh data using `invalidateQueries`:

```javascript
import { useMutation, useQueryClient } from '@tanstack/react-query'

function SendFriendRequest({ userId }) {
  const queryClient = useQueryClient()

  const { mutate } = useMutation({
    mutationFn: (receiverId) => fetch('/api/friend-request', {
      method: 'POST',
      body: JSON.stringify({ receiverId })
    }),
    onSuccess: () => {
      // Throw away 'discover' cache and refetch
      queryClient.invalidateQueries({ queryKey: ['discover'] })
      // Throw away 'friends' cache and refetch
      queryClient.invalidateQueries({ queryKey: ['friends'] })
    }
  })

  return (
    <button onClick={() => mutate(userId)}>
      Send Friend Request
    </button>
  )
}

```

---

## A Real World Example — Friend System

Let’s put it all together with a real example similar to what you’d build in a social app:

```javascript
// api/friends.js — your API functions
export const fetchDiscover = () => 
  fetch('/api/discover').then(res => res.json())

export const sendFriendRequest = (receiverId) => 
  fetch('/api/friend-request/send', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ receiverId })
  }).then(res => res.json())

export const fetchFriends = () => 
  fetch('/api/friends').then(res => res.json())

```

```javascript
// components/DiscoverScreen.jsx
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query'
import { fetchDiscover, sendFriendRequest } from '../api/friends'

function DiscoverScreen() {
  const queryClient = useQueryClient()

  // Fetch users to discover
  const { data: users, isLoading } = useQuery({
    queryKey: ['discover'],
    queryFn: fetchDiscover
  })

  // Send friend request mutation
  const { mutate: sendRequest, isPending } = useMutation({
    mutationFn: sendFriendRequest,
    onSuccess: () => {
      // Refresh discover list after sending request
      queryClient.invalidateQueries({ queryKey: ['discover'] })
    }
  })

  if (isLoading) return <p>Loading users...</p>

  return (
    <div>
      {users.map(user => (
        <div key={user.id}>
          <p>{user.name}</p>
          {user.relationship === 'NONE' && (
            <button onClick={() => sendRequest(user.id)} disabled={isPending}>
              Add Friend
            </button>
          )}
          {user.relationship === 'REQUEST_SENT' && (
            <button disabled>Request Sent</button>
          )}
          {user.relationship === 'FRIEND' && (
            <button disabled>Already Friends</button>
          )}
        </div>
      ))}
    </div>
  )
}

```

---

## Dependent Queries

Sometimes you need to fetch data that depends on the result of a previous query. Use the `enabled` option:

```javascript
// First fetch the user
const { data: user } = useQuery({
  queryKey: ['user', userId],
  queryFn: () => fetchUser(userId)
})

// Then fetch their friends, but only after user is loaded
const { data: friends } = useQuery({
  queryKey: ['friends', user?.id],
  queryFn: () => fetchFriends(user.id),
  enabled: !!user  // Only run this query if user exists
})

```

* `enabled: false` $\rightarrow$ The query won't run.
* `enabled: !!user` $\rightarrow$ Run only when user is truthy.

---

## `useQuery` vs `useEffect` — When to use which?

| Task | Solution |
| --- | --- |
| **Fetching data from an API?** | `useQuery` ✅ |
| **Submitting a form?** | `useMutation` ✅ |
| **Running code when something changes that has nothing to do with data?** | `useEffect` ✅ |
| **Syncing two pieces of state?** | Neither, just derive it ✅ |

> **A good rule:** If it involves a network request, use TanStack Query. If it doesn’t, use `useEffect`.

---

## Summary

| Concept | What it does |
| --- | --- |
| **`useQuery`** | Fetch data, handles loading/error/cache automatically |
| **`useMutation`** | Change data, handles loading/error states |
| **`queryKey`** | Unique label for cached data |
| **`staleTime`** | How long before data is considered outdated |
| **`invalidateQueries`** | Throw away cache and refetch fresh data |
| **`enabled`** | Conditionally run a query |

---

## Quick Tips

* Always put everything your `queryFn` depends on inside the `queryKey`.
* Use `invalidateQueries` after mutations to keep data fresh.
* Use `staleTime` to avoid unnecessary refetches for data that doesn't change often.
* Separate your API functions into their own file — don’t write fetch calls directly inside `queryFn`.
* **Use React Query Devtools during development to see your cache in real time:**

```bash
npm install @tanstack/react-query-devtools

```

```javascript
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'

<QueryClientProvider client={queryClient}>
  <YourApp />
  <ReactQueryDevtools initialIsOpen={false} />  {/* Add this */}
</QueryClientProvider>

```

This gives you a visual panel showing all your queries, their status, and cached data. Super helpful for debugging!