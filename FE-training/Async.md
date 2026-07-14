### What is Asynchronous Programming?

TypeScript (JavaScript) is **single-threaded**, meaning it executes one task at a time.

Some operations, however, take time to complete, such as:

* Fetching data from an API
* Reading a file
* Waiting for a timer
* Accessing a database

If JavaScript waited for these operations to finish before doing anything else, the application would become unresponsive.

Instead, JavaScript performs these operations **asynchronously**, allowing other code to continue executing while it waits for the result.

---

### Synchronous vs Asynchronous Code

#### Synchronous Example

```ts
console.log("Start");

console.log("Processing...");

console.log("End");
```

Output:

```text
Start
Processing...
End
```

Each line waits for the previous one to finish.

---

#### Asynchronous Example

```ts
console.log("Start");

setTimeout(() => {
  console.log("Timer finished");
}, 1000);

console.log("End");
```

Output:

```text
Start
End
Timer finished
```

Although `setTimeout` appears before `"End"`, JavaScript does not wait for it to finish.

---

### Promises

A **Promise** represents the eventual result of an asynchronous operation.

A Promise can be in one of three states:

* **Pending** – the operation is still running.
* **Fulfilled** – the operation completed successfully.
* **Rejected** – the operation failed.

Example:

```ts
const promise = new Promise<string>((resolve) => {
  setTimeout(() => {
    resolve("Finished!");
  }, 1000);
});
```

---

### Using `.then()`

To receive the result of a Promise, use `.then()`.

```ts
promise.then((result) => {
  console.log(result);
});
```
Output after one second:
```text
Finished!
```
---

### Handling Errors

Promises can also fail.
```ts
const promise = new Promise<string>((resolve, reject) => {
  reject("Something went wrong");
});
```
Handle the error with `.catch()`.

```ts
promise
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.error(error);
  });
```
---

### Async Functions 

An `async` function always returns a Promise.

Example:

```ts
async function getMessage() {
  return "Hello";
}
```

Even though the function returns a string, its actual return type is:

```ts
Promise<string>
```

Therefore:

```ts
const result = getMessage();

console.log(result);
```

Output:

```text
Promise { ... }
```

---

### Await

The `await` keyword pauses the execution of an `async` function until a Promise is resolved.

Example:

```ts
function fetchUser(): Promise<string> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("John");
    }, 1000);
  });
}

async function main() {
  const user = await fetchUser();

  console.log(user);
}
```
Output after one second:
```text
John
```
---

### Why Use async/await?

Without `async/await`:

```ts
fetchUser()
  .then((user) => {
    console.log(user);
  })
  .catch((error) => {
    console.error(error);
  });
```

With `async/await`:

```ts
try {
  const user = await fetchUser();
  console.log(user);
} catch (error) {
  console.error(error);
}
```

Many developers find `async/await` easier to read because it looks similar to synchronous code.

---

### Error Handling with try/catch

When using `await`, errors should usually be handled with `try/catch`.

```ts
async function loadUser() {
  try {
    const user = await fetchUser();

    console.log(user);
  } catch (error) {
    console.error(error);
  }
}
```

---

### Sequential vs Parallel Execution

#### Sequential

```ts
const user = await fetchUser();
const posts = await fetchPosts();
```

The second request starts **only after** the first finishes.

---

#### Parallel

```ts
const [user, posts] = await Promise.all([
  fetchUser(),
  fetchPosts()
]);
```

Both requests start immediately and execute in parallel.

This is usually faster when the operations are independent.

---

### Exercise 1 – Create an Async Function

Create an async function called `sayHello` that returns the string:

```text
Hello World
```

Questions:

1. What is the return type of the function?
2. What happens if you log the returned value without `await`?

---

### Exercise 2 – Simulate an API Call

Create a function called `fetchProduct` that returns a Promise.

Requirements:

* Wait 2 seconds.
* Resolve with the string `"Laptop"`.

Then create an async function that prints the returned value.

---

### Exercise 3 – Error Handling

Modify the previous exercise so that the Promise rejects with the message:

```text
Product not found
```

Handle the error using `try/catch`.

Questions:

1. What happens if the error is not caught?
2. Where should the `try/catch` block be placed?

---

### Exercise 4 – Promise.all

Create two asynchronous functions:

```ts
fetchUsers()
fetchOrders()
```

Each should resolve after a different delay.

Run them:

1. Sequentially.
2. Using `Promise.all()`.

Measure the execution time using:

```ts
console.time("test");

// code here

console.timeEnd("test");
```

Question:

Which approach is faster, and why?

---

### Exercise 5 – Predict the Output

Without running the code, predict the output.

#### Example A

```ts
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```

---

#### Example B

```ts
async function test() {
  console.log("1");

  await Promise.resolve();

  console.log("2");
}

console.log("3");

test();

console.log("4");
```

---

#### Example C

```ts
async function getValue() {
  return 10;
}

console.log(getValue());
```

What is printed?

Why?

---

### Additional Practice Questions

#### Question 1

Explain the difference between:

* Synchronous code
* Asynchronous code

Give one real-world example of each.

---

#### Question 2

When should you use:

* `.then()`
* `await`

Can both accomplish the same task?

---

#### Question 3

What is the difference between:

```ts
await fetchUser();
await fetchPosts();
```

and

```ts
await Promise.all([
  fetchUser(),
  fetchPosts()
]);
```

---

#### Question 4

Can `await` be used outside an `async` function?

Explain why or why not.

---

#### Question 5

A function is declared as:

```ts
async function getNumber() {
  return 5;
}
```

What is its actual return type?

How would you get the value `5` from it?