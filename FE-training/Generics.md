Generics allow us to write reusable code that works with multiple types while still maintaining type safety.

Without generics, we often have to either:

* Write the same code multiple times for different types.
* Use `any`, which removes type safety.

Generics solve this problem by allowing us to define placeholders for types that will be determined later.

Consider the following function:

```ts
function identity<T>(value: T): T {
  return value;
}
```

`T` is a generic type parameter.

When the function is called, TypeScript infers the type of `T` from the argument:

```ts
const str = identity("hello"); // string
const num = identity(123);     // number
const bool = identity(true);   // boolean
```

The same function works with different types while preserving the correct return type.

Without generics, we might write:

```ts
function identity(value: any): any {
  return value;
}
```

This works, but TypeScript loses all information about the actual type.

---

### Working with Generic Type Variables

A common mistake is assuming that a generic type has certain properties.

For example:

```ts
function printLength<T>(value: T) {
  console.log(value.length);
}
```

TypeScript produces an error because not every type has a `length` property.

For example:

```ts
printLength("hello"); // length exists
printLength([1, 2, 3]); // length exists
printLength(42); // length does not exist
```

Since `T` could be any type, TypeScript prevents us from accessing properties that may not exist.

---

### Generic Constraints

Sometimes we want a generic type, but only if it satisfies certain requirements.

We can use `extends` to constrain a generic type.

Example:

```ts
function printLength<T extends { length: number }>(value: T) {
  console.log(value.length);
}
```

Now TypeScript knows that any value passed to the function must contain a `length` property.

Valid calls:

```ts
printLength("hello");
printLength([1, 2, 3]);
printLength({ length: 10 });
```

Invalid call:

```ts
printLength(42);
```

Error:

```txt
Argument of type 'number' is not assignable.
```

---

### Exercise 1 – Log a Generic Variable

Create a generic function called `logValue` that receives a value of any type and logs it to the console.

Example:

```ts
logValue("hello");
logValue(123);
logValue(true);
```

Expected implementation:

```ts
function logValue<T>(value: T): void {
  console.log(value);
}
```

Questions:

1. What type will TypeScript infer for `T` in each call?
2. Why is using a generic better than using `any`?

---

### Exercise 2 – Ensure the Variable Behaves Like a String

Create a generic function that receives a value and logs:

* Its length
* Its uppercase version

Example:

```ts
logStringInfo("hello");
```

Output:

```txt
Length: 5
Uppercase: HELLO
```

To make this work, ensure that the generic type contains all methods and properties of a string.

One possible solution:

```ts
function logStringInfo<T extends string>(value: T): void {
  console.log(value.length);
  console.log(value.toUpperCase());
}
```

Questions:

1. Why is `extends string` needed?
2. What error would occur if we removed the constraint?
3. Which string properties and methods become available after adding the constraint?

---

### Understanding Union Types with Generic Constraints

Consider the following code:

```ts
type MyType = string | number;

const a: MyType = "a";
const b: MyType = 1;
let c: MyType;
```

And the function:

```ts
function logStringInfo<T extends string>(value: T): void {
  console.log(value.length);
  console.log(value.toUpperCase());
}
```

### Case 1

```ts
logStringInfo(a);
```

Result:

❌ TypeScript error

Why?

Although `a` currently contains a string value, its declared type is:

```ts
string | number
```

TypeScript checks the declared type, not the current runtime value.

Since `number` does not satisfy `extends string`, the call is rejected.

---

### Case 2

```ts
logStringInfo(b);
```

Result:

❌ TypeScript error

`b` is also declared as:

```ts
string | number
```

and therefore does not satisfy the constraint.

---

### Case 3

```ts
logStringInfo(c);
```

Result:

❌ TypeScript error

The variable type is still:

```ts
string | number
```

so TypeScript cannot guarantee that it is a string.

---

### How Can We Make It Work?

#### Option 1 – Type Narrowing

```ts
if (typeof a === "string") {
  logStringInfo(a);
}
```

Inside the `if` block, TypeScript knows that `a` is a string.

#### Option 2 – Declare a String

```ts
const value = "hello";

logStringInfo(value);
```

#### Option 3 – Type Assertion

```ts
logStringInfo(a as string);
```

This compiles, but should be used carefully because you are overriding TypeScript's type checking.

---

### Additional Practice Questions

#### Question 1

Create a generic function called `getFirstElement` that returns the first item in an array.

Example:

```ts
getFirstElement([1, 2, 3]);
getFirstElement(["a", "b", "c"]);
```

---

#### Question 2

Create a generic function called `wrapInArray` that receives a value and returns an array containing that value.

Example:

```ts
wrapInArray(5);       // [5]
wrapInArray("hello"); // ["hello"]
```

---

#### Question 3

Create a generic function that receives an object and returns one of its properties by key.

Hint:

Use two generic type parameters.

Example:

```ts
getProperty(user, "name");
```

---

#### Question 4

Create a generic function that accepts only values with an `id` property.

Example:

```ts
printId({ id: 1, name: "John" });
```

Should compile.

```ts
printId({ name: "John" });
```

Should produce a TypeScript error.

---

#### Question 5

Implement a generic `Stack<T>` class with the following methods:

```ts
push(value: T): void
pop(): T | undefined
peek(): T | undefined
```

Test it with:

```ts
const numberStack = new Stack<number>();
const stringStack = new Stack<string>();
```

You can also read the following sections in the [TypeScript Generics Docs](https://www.typescriptlang.org/docs/handbook/2/generics.html) — “Hello World of Generics”, “Working with Generic Type Variables” and “Generic Constraints”.
