There are 3 rules for hooks:

* Hooks can only be called inside React function components.  
* Hooks can only be called at the top level of a component.  
* Hooks cannot be conditional

### useState

In vanilla JavaScript, variables hold data, but changing them doesn't force the UI to update. In React, **UI is a function of state**. `useState` is the tool that tells React: *"Hey, this data matters. If it changes, you need to re-render the component so the user sees the update."*

The most critical thing to understand deeply is that **state updates are asynchronous and batched**, and state behaves like a snapshot. When you call the setter function, you aren't changing the variable in the current execution of your function; you are scheduling a new render with a new value.

#### When to use it

* When you have data that changes over time *and* affects what is rendered on the screen (e.g., toggle switches, form inputs, fetched data, open/closed modals).

```ts
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  const handleIncrement = () => {
    // Correct way if new state depends on previous state:
    // Pass a callback function to avoid race conditions/stale state bugs.
    setCount(prevCount => prevCount + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>Increment</button>
    </div>
  );
}
```

### useEffect

Many developers mistakenly view `useEffect` as a component lifecycle method (`componentDidMount`, `componentDidUpdate`, `componentWillUnmount`). Instead, think of `useEffect` as a tool for **synchronization**. It synchronizes your React component with an *external system* (like a Web API, a database, a chat server, or the browser DOM).

`useEffect` runs *after* the render is committed to the screen. If you provide a dependency array, React will look at the variables in that array and ask: *"Did any of these change since the last render?"* If yes, it runs the effect again.

⚠️ **Crucial Rule:** Always return a **cleanup function** if your effect sets up a subscription, timer, or event listener. This prevents memory leaks.

#### When to use it

* Fetching data from an API when a component loads or when an ID changes.  
* Setting up an event listener (e.g., `window.addEventListener('resize', ...)`).  
* Setting up timers (`setTimeout` or `setInterval`).

```tsx
import { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    let isMounted = true; // Prevents updating state on unmounted component

    // 1. The Action (Side Effect)
    fetch(`https://api.example.com/users/${userId}`)
      .then(res => res.json())
      .then(data => {
        if (isMounted) setUser(data);
      });

    // 2. The Cleanup Function
    return () => {
      isMounted = false; // Cancels the state update if userId changes rapidly
    };

  }, [userId]); // Runs whenever userId changes
  if (!user) return <p>Loading...</p>;
  return <h1>{user.name}</h1>;
}
```

### useRef

`useRef` is like a "secret compartment" in your component. It returns a mutable object with a `.current` property.

The superpowers of `useRef` are:

1. **It persists data** across renders (just like `useState`).  
2. **Changing it does NOT trigger a re-render** (unlike `useState`).

If you try to keep a variable inside a component like `let myVar = 5;`, it gets re-initialized to `5` on every single render. `useRef` keeps the value alive across renders without forcing the UI to redraw.

#### When to use it

* **Accessing DOM elements directly** (e.g., focusing an input, measuring element size, playing/pausing a video HTML5 element).  
* **Storing a value that needs to persist** but has no visual impact on the UI (e.g., holding a timer ID for `clearInterval`, tracking how many times a component has rendered).

```tsx
import { useRef, useEffect } from 'react';

function AutoFocusInput() {
  const inputRef = useRef(null); // Initially null, will hold the DOM node
  const renderCount = useRef(0);  // Tracking renders silently
  renderCount.current += 1;

  console.log(`Component rendered ${renderCount.current} times`);

  useEffect(() => {
    // Focus the input element directly using the DOM node
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} type="text" placeholder="I will focus automatically" />;
}
```

### useMemo

On every render, React recalculates everything inside your component function. If you have a loop that runs 10,000 times to filter a list, React will rerun that entire loop on *every single render*, even if the list hasn't changed.

`useMemo` stands for **Memoization** (caching). It tells React: *"Run this expensive calculation once. Cache the result. Don't run it again unless one of the dependencies changes."*

🛑 **Premature Optimization Warning:** Do not use `useMemo` everywhere. It has its own performance overhead (memory for caching and checking dependencies). Only use it if you are dealing with genuinely expensive computations or trying to maintain referential equality.

#### When to use it

* Filtering, sorting, or transforming large arrays of data.  
* Optimizing child component re-renders when passing objects/arrays as props (to maintain referential equality).

```tsx
import { useState, useMemo } from 'react';

function HeavyCalculation({ items, filterQuery }) {
  const [theme, setTheme] = useState('light');

  // Expensive calculation cached with useMemo
  const filteredItems = useMemo(() => {

    console.log("Filtering items... (This is expensive)");

    return items.filter(item => item.name.toLowerCase().includes(filterQuery.toLowerCase()));

  }, [items, filterQuery]); // Only reruns if items or filterQuery change

  return (

    <div>
      {/* Toggling the theme causes a re-render, but does NOT trigger the heavy filtering thanks to useMemo */}
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme ({theme})
      </button>

      <ul>
        {filteredItems.map(item => <li key={item.id}>{item.name}</li>)}
      </ul>
    </div>
  );
}
```

### exercises

#### useState – Exercise 1: Double Counter

Build a component with:

* A `+1` button  
* A `+5` button  
* A `Reset` button  
* A display showing the current value

**Requirements:**

* Use `useState`.  
* Whenever the new state depends on the previous state, use a functional update (`prev => ...`).  
* Display a `"High Score!"` message when the value exceeds 20.

#### useState – Exercise 2: Todo List

Build a basic Todo List.

**Requirements:**

* An input field for a new task.  
* An Add button.  
* Display all tasks in a list.  
* A Delete button for each task.  
* Display the total number of tasks.

**Learning Focus:**

* Working with array state.  
* Updating state immutably.

#### useEffect – Exercise 1: Digital Clock

Build a clock that displays the current time.

**Requirements:**

* Use `setInterval`.  
* Update the displayed time every second.  
* Clean up the interval using `clearInterval` when the component unmounts.

**Learning Focus:**  
 Understanding that `useEffect` synchronizes your component with an external system (a timer).

#### useEffect – Exercise 2: User Search

Create a component with a search input.

**Requirements:**

* When the user types, fetch data from: https://jsonplaceholder.typicode.com/users

* Filter the users based on the entered text.  
* Show a loading indicator while the request is in progress.  
* Prevent state updates if the component has already unmounted.

**Bonus:**  
 Add a 500ms debounce.

**Learning Focus:**  
 Using dependencies and cleanup functions correctly.

#### useRef – Exercise 1: Automatic Focus

Build a login form.

**Requirements:**

* A Username field.  
* A Password field.  
* When the component loads, automatically focus the Username field.  
* Add a "Focus Password" button that moves focus to the password field.

**Learning Focus:**  
 Direct DOM access using refs.

#### useRef – Exercise 2: Render Counter

Build a component with:

* A text input.  
* A display showing the current input value.  
* A render counter.

**Requirements:**

* Store the render count using `useRef`.  
* Every text change should trigger a re-render.  
* The render counter should update without causing additional renders itself.

**Learning Focus:**  
 Understanding the difference between `useState` and `useRef`.

#### useMemo – Exercise 1: Expensive Calculation

Build a component with:

* An input field for a number.  
* A button that increments a separate counter.

Create a function that performs an expensive computation (for example, a loop with millions of iterations).

**Requirements:**

* Use `useMemo` so the computation only runs when the number changes.  
* Clicking the counter button should not trigger the expensive computation again.

**Learning Focus:**  
 Understanding memoization and avoiding unnecessary calculations.

#### useMemo – Exercise 2: Large List Filtering

Create a list of 10,000 users.

**Requirements:**

* A search input.  
* Filter the list based on the entered text.  
* Use `useMemo` so filtering only runs when:  
  * The user list changes.  
  * The search text changes.

**Bonus:**  
 Log a message to the console whenever the filtering operation actually runs.

**Learning Focus:**  
 Understanding that `useMemo` caches a computed value, not a function.

#### Final Challenge

Build a **User Management App** that combines all four hooks:

* `useState` – Manage form data and users.  
* `useEffect` – Load users from an API.  
* `useRef` – Automatically focus the search field.  
* `useMemo` – Filter users based on the search text.

This provides practice with a realistic CRUD-style screen that closely resembles what you'll encounter in real-world React applications.

### You might not need `useEffect`

**TODO: Add an example of resetting a state on “isOpen” change via a `useEffect` and ask if it can be done differently.**

### You might not need `useRef`

**TODO: Add an example of manipulation the CSS property `transform` via `useRef` and ask if it can be done differently.**

### You might not need `useMemo`

**Answer: React Compiler**