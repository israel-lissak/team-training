Array methods are one of the most important features in JavaScript and TypeScript. They allow you to work with arrays in a clean, readable, and declarative way.

Instead of writing loops (`for`, `while`), you describe **what** you want to do with the array.

We'll use this array throughout the examples:

```ts
const numbers = [1, 2, 3, 4, 5];
```

---

### `map()`

`map()` creates a **new array** by transforming every element of the original array.

#### Syntax

```ts
const newArray = array.map((item) => {
  return something;
});
```

#### Example

```ts
const doubled = numbers.map((num) => num * 2);

console.log(doubled);
```

Output:

```ts
[2, 4, 6, 8, 10]
```

The original array is not modified:

```ts
console.log(numbers);

// [1, 2, 3, 4, 5]
```

#### Visualization

```
1 → 2
2 → 4
3 → 6
4 → 8
5 → 10

↓

[2, 4, 6, 8, 10]
```

---

### `map()` with Objects

```ts
const users = [
  { id: 1, name: "Dan" },
  { id: 2, name: "Noa" },
  { id: 3, name: "Amit" },
];

const names = users.map(user => user.name);

console.log(names);
```

Output:

```ts
["Dan", "Noa", "Amit"]
```

---

### `map()` in React

One of the most common React patterns:

```tsx
const users = ["Dan", "Noa", "Amit"];

return (
  <ul>
    {users.map(user => (
      <li key={user}>{user}</li>
    ))}
  </ul>
);
```

---

### `filter()`

`filter()` creates a new array containing only the elements that satisfy a condition.

#### Example

```ts
const even = numbers.filter(num => num % 2 === 0);

console.log(even);
```

Output:

```ts
[2, 4]
```

---

### `filter()` with Objects

```ts
const users = [
  { name: "Dan", active: true },
  { name: "Noa", active: false },
  { name: "Amit", active: true },
];

const activeUsers = users.filter(user => user.active);

console.log(activeUsers);
```

Output:

```ts
[
  { name: "Dan", active: true },
  { name: "Amit", active: true }
]
```

---

### `find()`

`find()` returns the **first element** that matches a condition.

```ts
const result = numbers.find(num => num > 3);

console.log(result);
```

Output:

```ts
4
```

If no element matches:

```ts
undefined
```

---

### `some()`

`some()` checks whether **at least one** element satisfies a condition.

```ts
const result = numbers.some(num => num > 4);

console.log(result);
```

Output:

```ts
true
```

Another example:

```ts
numbers.some(num => num > 10);
```

Output:

```ts
false
```

---

### `every()`

`every()` checks whether **all** elements satisfy a condition.

```ts
numbers.every(num => num > 0);
```

Output:

```ts
true
```

```ts
numbers.every(num => num % 2 === 0);
```

Output:

```ts
false
```

---

### `reduce()`

`reduce()` is one of the most powerful array methods.

It reduces an array to a single value.

#### Example: Sum

```ts
const sum = numbers.reduce((acc, current) => {
  return acc + current;
}, 0);

console.log(sum);
```

Output:

```ts
15
```

#### Step-by-step

```
acc = 0

0 + 1 = 1
1 + 2 = 3
3 + 3 = 6
6 + 4 = 10
10 + 5 = 15
```

---

### `forEach()`

`forEach()` executes a function for every element in the array.

Unlike `map()`, it **does not return a new array**.

```ts
numbers.forEach(num => {
  console.log(num);
});
```

Output:

```
1
2
3
4
5
```

If you try:

```ts
const result = numbers.forEach(num => num * 2);

console.log(result);
```

Output:

```ts
undefined
```

Use `forEach()` when you want to perform side effects (such as logging or updating the DOM), not when you want to create a new array.

---

### `sort()`

`sort()` sorts the array.

```ts
const nums = [8, 2, 5, 1];

nums.sort((a, b) => a - b);

console.log(nums);
```

Output:

```ts
[1, 2, 5, 8]
```

> **Important:** `sort()` mutates (modifies) the original array.

If you want to keep the original array unchanged:

```ts
const sorted = [...nums].sort((a, b) => a - b);
```

---

### `flatMap()`

`flatMap()` combines `map()` and `flat()`.

```ts
const words = ["hi", "bye"];

const letters = words.flatMap(word => word.split(""));

console.log(letters);
```

Output:

```ts
["h", "i", "b", "y", "e"]
```

---

### Method Chaining

One of the biggest advantages of array methods is that they can be chained together.

```ts
const users = [
  { name: "Dan", age: 18 },
  { name: "Noa", age: 25 },
  { name: "Amit", age: 30 },
];

const result = users
  .filter(user => user.age >= 21)
  .map(user => user.name);

console.log(result);
```

Output:

```ts
["Noa", "Amit"]
```

---

### When Should You Use Each Method?

| Method      | Returns                               | Mutates Original Array? | Common Use Case                      |
| ----------- | ------------------------------------- | ----------------------- | ------------------------------------ |
| `map()`     | New array                             | ❌ No                    | Transform every element              |
| `filter()`  | New array                             | ❌ No                    | Keep only matching elements          |
| `find()`    | First matching element or `undefined` | ❌ No                    | Find a single item                   |
| `some()`    | `boolean`                             | ❌ No                    | Check if at least one item matches   |
| `every()`   | `boolean`                             | ❌ No                    | Check if all items match             |
| `reduce()`  | Any value                             | ❌ No                    | Sum, group, build objects, aggregate |
| `forEach()` | `undefined`                           | ❌ No                    | Perform side effects                 |
| `sort()`    | The same array                        | ✅ Yes                   | Sort an array                        |
| `flatMap()` | New array                             | ❌ No                    | Map and flatten nested arrays        |

---

### Quick Decision Guide

* **Want to transform every item?** → `map()`
* **Want to keep only certain items?** → `filter()`
* **Want to find the first matching item?** → `find()`
* **Want to check if at least one item matches?** → `some()`
* **Want to check if every item matches?** → `every()`
* **Want to reduce the array into a single value?** → `reduce()`
* **Want to perform an action without returning anything?** → `forEach()`
* **Want to sort the array?** → `sort()`

These are the array methods you'll use most frequently in modern JavaScript, TypeScript, and React applications. A very common React pattern is chaining methods like:

```ts
const visibleUsers = users
  .filter(user => user.active)
  .map(user => user.name);
```

This style is concise, readable, and encourages writing immutable code.

