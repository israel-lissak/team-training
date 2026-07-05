# Fusion Team Training

Subjects

- npm cli and package.jsn  
- git

# Day one

## Getting started with npm

**npm** is a **package manager** for JavaScript projects.

1. Describe in your own words what is a *package manager*?  
2. What makes a directory an npm project?  
3. What’s the role of the “scripts” section?  
4. What’s the command to create a new project with npm CLI?  
5. List as many differences as possible between *dev dependencies* and regular *dependencies*

## JSON

JSON, JavaScript Object Notation is a common format for storing data, configuration and other kinds of information.

1. What does JSON stand for?.  
2. Research for alternative formats like JSON 

# What is npm?

npm is a package manager for JavaScript projects. We can use it to install, uninstall, update, and manage JS packages, run scripts, publish our own packages and track dependencies.

\`package.json\` is a JSON file located in the root of our project. It contains metadata about the project and defines:

- project name and version  
- needed dependencies for the projectscripts  
- how to publish our package  
- extra configuration  
- scripts

npm relies on package.json to install the correct dependencies, run scripts, publish packages, etc. Without it, npm cannot properly manage a project.

**Research:** What’s the difference between regular dependencies and dev dependencies?

# What is React?

React is a js library used to develop interactive web applications. Instead of manipulating the browser's DOM directly, React creates a virtual DOM in memory, where it does all the necessary manipulating, before making the changes in the browser DOM. This way, React changes only what needs to be changed and nothing else.

# What is TypeScript?

JavaScript is a loosely typed language. It can be difficult to understand what types of data are being passed around. Function parameters and variables don't have any information, so developers need to look at documentation, or guess based on the implementation. To solve this problem, TypeScript was invented. TypeScript allows specifying the types of data being passed around within the code, and has the ability to report errors when the types don't match. TypeScript uses compile time type checking, which means it checks if the specified types match before running the code, not while running the code.

**Exercise:** Create a new react project with TypeScript.

## What are types?

In TypeScript, a type is a label that describes the properties and methods a value has.

#### Primitive types

Primitive types are the most basic types in TypeScript and they are the building blocks for more complex types. There are 7 primitive types: string, number, boolean, bigInt, symbol, null and undefined.

```ts
const myStr: string = "Hello World!";  
const myNum: number = 42;  
const myBool: boolean = true;
```

#### Special types

There are several special types that are used to handle cases where the type might not be known in advance or other unique cases.

**any** - This type tells TypeScript to skip the type checking for this variable. We will not use it since it bypasses TypeScript’s type safety. If you’re certain it’s needed, ask a senior team member or search for a workaround.

```ts
const myStr: any = "Hello World!";
const myNum: any = 42;  
const myBool: any = true;  
  
const myFunc = (myVar: any) => {
  console.log(myVar.toUpperCase()); // This complies even though myVar may not have this method
}
```

**unknown** \- unknown is the type-safe counterpart of any \- it can hold any value, but can’t be used without narrowing its type first. We can’t access properties or methods on an unknown variable or call it as a function.

```ts
const myStr: unknown = "Hello World!";  
const myNum: unknown = 42;  
const myBool: unknown = true;  
  
const myFunc = (myVar: unknown) => {  
  console.log((myVar).toUpperCase()); // This raises an error  
}
```

We have to narrow its type:
```ts
const myFunc = (myVar: unknown) => {  
  if(typeof myVar === "string") {  
    console.log(myVar.toUpperCase()); // This is fine
  } else if (typeof myVar === "number") {  
    console.log(myVar.toExponential()); // This is also fine 
  }  
}
```

We can cast it to string, but be careful - if you’re wrong, you might throw at runtime:
```ts
const myFunc = (myVar: unknown) => {  
  console.log((myVar as string).toUpperCase());  
}
```
**never** - The never type represents the type of values that never occur. It's used to indicate that something never happens or should never happen. A function or expression gets type never when it cannot produce a value or cannot complete normally.

For example, if a function throws an error:
```ts
const onlyAcceptsStrings = (str: string): string => {  
  if (typeof str === "string") {  
    return str; // the return type is string  
  }
  else
  {  
    throw new Error("Input is not a string"); // execution stops here; this branch is typed as never  
  }  
}
```

#### Union types

Union types are used when a value can be more than a single type. For example, we can create a variable that is either a string or number.
```ts
let myVar: string | number = "Hello World!";  
myVar = 42;  
myVar = "Another string";
```

## Functions and arrow functions

#### Functions

How we type functions in TypeScript:
```ts
function myFunc(name: string): string {  
  return name.toUpperCase();  
}
```
The “name” parameter is of type string, and the function’s return type is also string.

If a function doesn’t have a return value, its return value’s type is void:
```ts
function myFunc(name: string): void {  
  console.log(name);  
}
```

Optional parameters are parameters that might be passed and might not - if they aren’t passed, by default their value will be undefined. Default parameters are a way to assign a fallback value to a variable in case it isn’t passed.
```ts
function myFunc(name: string, work: string = "unemployed", city?: string): void {  
  console.log(`My name is ${name.toUpperCase()}, I work at ${work.toUpperCase()}.`);  
  if (city) {  
    console.log(`I live in ${city.toUpperCase()}.`);  
  }  
}
```

#### Arrow functions

Arrow functions allow us to write shortened function syntax
```ts
const myFunc = (age: number): string => {  
  return `You are ${age} years old.`;  
}
```
We will use the regular function syntax for top-level functions, and the arrow functions syntax for every other occasion.

**Exercise**: What are the differences between regular functions and arrow functions?  
**Exercise**: What are the differences between void and never?

## Objects

Objects are variables that can store both values and functions. Values are stored as key:value pairs called properties and functions are stored as key:function() pairs called methods. In general we will not use an object’s methods, since it’s hard to understand, reuse and type safely.

We can create a type to describe a certain object, and use it to type the object we want to create.

```ts
type Person = {
  name: string;
  age: number;
};

const person: Person = {
  name: "Alice",
  age: 30,  
}
```

We can access (and assign) an object’s properties with a dot notation:
```ts
person.name = "Bob";
```

We can nest an object in another object:
```ts
type PersonWithId = {
  id: number;
  person: Person;
}
```

## Array methods

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


## Destructuring and spreading

#### Destructuring

Destructuring lets us extract values from arrays or objects into individual variables.

```ts
const { name, age } = person;  
const [a, b ,c] = numbers;

const upperName = name.toUpperCase();
const sum = a + b + c;
```

#### Spreading

Spreading lets us expand arrays or objects. We can use it for copying, combining, or updating them.  
```ts
const combinedObj = { ...obj1, ...obj2 };  
const combinedArray = [...arr1, ...arr2];
```

## Utility types

TS has a collection of “utility types” available out of the box that can help us create new types. Go over a list of them [here](https://www.w3schools.com/typescript/typescript_utility_types.php) and do the following exercises.

### Exercises

#### 1. `Partial<T>`
```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
}

// Task: Write a function `updateProduct` that takes a product id (number)
// and an object of fields to update. The update object should allow
// any subset of Product's fields (except id, which can't change).
function updateProduct(id: number, changes: /* your type here */) {
  // ...
}

updateProduct(1, { price: 99 }); // should work
updateProduct(1, { name: "Widget", category: "Tools" }); // should work
```
*Hint: you'll need `Partial` combined with `Omit`.*

#### 2. `Required<T>`
```typescript
interface Config {
  host: string;
  port?: number;
  timeout?: number;
}

// Task: Write a function `connect` that only accepts a fully-specified
// config — no optional fields allowed. Use Required<Config> as the param type,
// then try calling it with a config missing `timeout` to see the error.
function connect(config: /* your type here */) {
  console.log(`Connecting to ${config.host}:${config.port}, timeout ${config.timeout}`);
}
```

#### 3. `Record<K, V>`
```typescript
// Task: Model a permissions map for three roles: "admin", "editor", "viewer".
// Each role maps to an array of allowed action strings, e.g. ["read", "write", "delete"].
// Define the type using Record, then build the object.

type Role = "admin" | "editor" | "viewer";

const permissions: /* your type here */ = {
  // fill in the three roles
};
```

#### 4. `Omit<T, K>`
```typescript
interface User {
  id: number;
  email: string;
  passwordHash: string;
  createdAt: Date;
}

// Task: Create a "public" version of User that's safe to send to the frontend
// — it should NOT include passwordHash. Use Omit to define it, then write
// a function `toPublicUser(user: User)` that returns that shape.
```

#### 5. `Pick<T, K>`
```typescript
interface Order {
  id: number;
  customerId: number;
  items: string[];
  total: number;
  status: "pending" | "shipped" | "delivered";
  shippingAddress: string;
}

// Task: Define an "OrderSummary" type that includes only id, total, and status.
// Then write a function `getOrderSummary(order: Order): OrderSummary` that
// extracts just those three fields from a full Order.
```

#### 6. `Exclude<T, U>`
```typescript
type HttpMethod = "GET" | "POST" | "PUT" | "PATCH" | "DELETE";

// Task: Define a "SafeMethod" type that excludes the methods that modify data
// (POST, PUT, PATCH, DELETE) — leaving only the safe/read-only ones.
// Bonus: what's left in the type after excluding all four?
type SafeMethod = /* your type here */;
```

#### 7. `ReturnType<T>`
```typescript
function createUser(name: string, age: number) {
  return { id: Math.random(), name, age, createdAt: new Date() };
}

// Task: Without copy-pasting the shape manually, define a `NewUser` type
// that matches whatever createUser returns, using ReturnType.
type NewUser = /* your type here */;

// Then write a function `saveUser(user: NewUser)` that uses that type.
```

#### 8. `Parameters<T>`
```typescript
function sendEmail(to: string, subject: string, body: string, cc?: string[]) {
  // ...
}

// Task: Define a type `SendEmailArgs` that captures sendEmail's parameter
// tuple using Parameters. Then write a function `logEmailCall(args: SendEmailArgs)`
// that just console.logs the tuple — without redefining the parameter list.
type SendEmailArgs = /* your type here */;
```

#### 9. `Readonly<T>`
```typescript
interface AppSettings {
  apiUrl: string;
  retries: number;
}

// Task: Create a frozen config object using Readonly<AppSettings>, assign it
// real values, then try mutating one property afterward and observe the
// compile-time error TypeScript gives you.
const settings: /* your type here */ = {
  apiUrl: "https://api.example.com",
  retries: 3,
};

// settings.retries = 5; // <- try uncommenting this
```

## Enums

While TypeScript does have a built-in enum keyword, we don’t use it in our project. Instead we use a type alias based on a constant object.

We create an object where the keys are the enum’s options, and values are the enum’s values:
```ts
const fruitEnumType = {  
  apple: "apple",  
  orange: "orange",  
} as const;
```

We can then create a type from this object:
```ts
type fruitEnumType = typeof fruitEnumType[keyof typeof fruitEnumType];
```
This allows us to have a type that only conforms to a few options, and a value, without a “real” enum:
```ts
const apple: fruitEnumType = "apple"; // Valid: "apple" is assignable to type 'fruitEnumType'.  
const banana: fruitEnumType = "banana"; // Error: Type '"banana"' is not assignable to type 'fruitEnumType'.
```

#### Questions:

1. What is “as const” used for? Why did we use it when creating the enum?  
2. What are literal types?

#### Why would we use this instead of an enum? 

Enums in TypeScript compile to actual js objects in runtime, unlike TypeScript types - this adds unnecessary code during runtime. In addition, they can change during runtime, making them a less strict and less type-safe option than other alternatives. You can read more about this reasoning [here](https://www.totaltypescript.com/erasable-syntax-only).

## Self-Research

Read about keyof and typeof. Answer the following questions:

1. What do the keyof and typeof keywords do? explain in your own words.  
2. In the previous enums section, we create a type from an object - explain this process.  
3. **TODO**: another question

Read about Falsy and truthy values. Answer the following questions:

1. What are falsy and truthy values? list all the falsy and truthy values.  
2. What are the differences between null and undefined?  
3. What is a nullish value?  
4. For the next expressions, determine their value at **runtime**, and explain why:  
   
```ts
Boolean(0);  
Boolean(null);  
Boolean(" ");  
Boolean({});  
Boolean([]);  
Boolean(-1);  
Boolean([] == false);  
Boolean([] == 0);  
Boolean(undefined == null);  
Boolean(undefined === null);  
Boolean(NaN == false);  
Boolean(NaN == NaN);
```

## Null handling

#### Optional chaining

When accessing an object’s property/method, we may access an nullish value without meaning. In such a case, the optional chaining (?.) operator will return undefined instead of throwing an error.

```ts
type Person = {  
  firstName: null | string;  
  lastName: undefined | string;  
}

const person: Person = {  
  firstName: "Saul",  
  lastName: "Goodman",  
}

person.firstName.toUpperCase() // raises a ts error, because firstName can be null  
person.firstName?.toUpperCase() // doesn't raise a ts error, if firstName is null, returns undefined  
person.lastName.toUpperCase() // raises a ts error, because lastName can be undefined  
person.lastName?.toUpperCase() // doesn't raise a ts error, if lastName is undefined, returns undefined
```
#### Null coalescing

The nullish coalescing operator (??) will return its left-hand value, unless it’s nullish \- in which case it will return the right-hand value.

```ts
const nullish = null;  
const three = nullish ?? 3;  
const nullish2 = undefined;  
const four = nullish2 ?? 4;
```

It’s equivalent to writing this condition:

```ts
const nullish = null;  
let three;

if (nullish === null || nullish === undefined) {  
  three = 3;  
} else {  
  three = nullish;  
}
```

#### Null assertion

If we’re certain that a possibly nullish value is defined but TypeScript can’t infer that, we can assert it using the null assertion operator (!):

```ts
const PossiblyNullish: string | null | undefined = "Not nullish!";
PossiblyNullish!.toUpperCase(); // No TypeScript error
```

## Casting

In TypeScript, **Type Casting** (more accurately called **Type Assertion**) is a way to tell the compiler: *"Trust me, I know what I'm doing. I know the type of this value better than you do."*

It is important to understand that type assertion happens **purely at compile time**. It does not change the actual data at runtime; it only changes how TypeScript treats the data while checking your code.

There are two syntaxes for casting, but `as` is the standard and preferred one (especially in React, as the other syntax conflicts with JSX):

```typescript
// Preferred (works everywhere)
const username = someValue as string;

// Alternative syntax (Does NOT work in React .tsx files)
const username = <string>someValue;

```

---

### 1. What Can You Do? (What Works)

You can use type assertion when moving from a **more general/flexible type** to a **more specific type**, or when dealing with types that TypeScript cannot fully infer.

#### A. Casting from `any` or `unknown`

If you fetch data from an API or use a third-party library without types, you often get an `any` or `unknown` type. You can cast it to your expected structure.

```typescript
const apiResponse: unknown = { name: "Alice", age: 30 };

// TypeScript won't let you do apiResponse.name because it's 'unknown'
const user = apiResponse as { name: string; age: number }; 
console.log(user.name); // Works!

```

#### B. Narrowing DOM Elements

In browser environments, methods like `document.getElementById` return a generic `HTMLElement` or `null`. If you know the exact element type, you can cast it.

```typescript
// TypeScript only knows this is a generic HTMLElement
const myInput = document.getElementById("username-input") as HTMLInputElement;

// Now you can access input-specific properties without errors
myInput.value = "JohnDoe"; 

```

#### C. Specific Literal Types

You can cast a generic string to a specific set of allowed string constants (literal union types).

```typescript
type Status = "success" | "error" | "pending";

const responseStatus: string = getStatusFromSomewhere();
const status = responseStatus as Status;

```

---

### 2. What Will NOT Work? (The Constraints)

TypeScript wants to protect you from making obvious mistakes. Therefore, **you cannot cast between two completely unrelated types** (e.g., casting a `string` directly into a `number`).

#### The Illegal Cast

```typescript
const age: number = 25;
const name = age as string; // ❌ Compile Error: Conversion of type 'number' to type 'string' may be a mistake...

```

#### How people bypass this (The "Double Cast" Danger)

If you cast a value to `any` or `unknown` first, TypeScript loses its memory of the original type, allowing you to cast it to *anything*.

```typescript
const age: number = 25;
const name = (age as unknown) as string; // ⚠️ This compiles, but it's dangerous!

```

> **Warning:** At runtime, `name` is still the number `25`. If you try to call string methods on it (like `name.toUpperCase()`), your app will **crash** with a runtime error.

---

### 3. When is it Right to Use It? (Best Practices)

Type casting should be used **sparingly**. Overusing it defeats the purpose of TypeScript because you are turning off the type-checker.

#### ✅ When to Use It:

1. **Interacting with the DOM:** As shown above with `HTMLInputElement`. TypeScript cannot scan your HTML file to know what element has what ID.
2. **Dealing with legacy or untyped JS code:** When migrating JavaScript codebases or using libraries that lack `@types`.

#### ❌ When NOT to Use It (And what to do instead):

1. **Don't use it to hide errors:** If TypeScript says a type might be `undefined`, don't just force it with `as string` to make the red squiggly line go away.
2. **Instead of casting, use Type Guards (`typeof`, `instanceof`):** Instead of forcing a type, check it at runtime so your app is safe.

```typescript
// ❌ BAD: Forcing it blindly
function processId(id: string | number) {
  const strId = id as string;
  console.log(strId.toUpperCase()); // Will crash if id is a number!
}

//  GOOD: Type Guard (Safe at runtime and compile time)
function processIdSafe(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase()); // TypeScript automatically knows 'id' is a string here
  } else {
    console.log(id.toFixed(2));    // TypeScript automatically knows 'id' is a number here
  }
}

```

3. **Instead of casting, use Zod or Type Validation for API data:** Casting an API response (`data as User`) assumes the backend didn't change. If the backend changes, your frontend types are lying to you. Use validation libraries to parse and confirm data shapes at runtime.

---

### 4. The `satisfies` operator (TypeScript 4.9+)

TypeScript 4.9 introduced the `satisfies` operator as a compile-time check that an expression conforms to a target type while preserving the expression's more specific inferred type. Like casting, it has no runtime effect — it's purely for the type system — but it's safer than `as` because it doesn't change the value's inferred type.

What `satisfies` does:
- Ensures the value is compatible with a target type — compiler error if it isn't.
- Preserves literal and extra properties on the value instead of widening them to the target type.
- Is only a compile-time check; it does not perform runtime conversion.

Examples:

```typescript
interface User { id: number; name: string }

// Using `as` — the variable is typed as `User`, extra properties are lost
const u1 = { id: 1, name: 'Alice', age: 30 } as User;
// u1 has type User — `age` is not available on the type

// Using `satisfies` — the value must match User, but its full type is preserved
const u2 = { id: 1, name: 'Alice', age: 30 } satisfies User;
// u2 has type { id: number; name: string; age: number }, and the compiler verified it fits `User`

// Preserves literal types
const cfg1 = { mode: 'dark' } as { mode: string };
// cfg1.mode is type string

const cfg2 = { mode: 'dark' } satisfies { mode: string };
// cfg2.mode is type 'dark' (literal preserved)
```

When to prefer `satisfies`:
- When you want the compiler to check that a value implements an interface or shape, but you also want to keep the value's more specific type information (e.g., literal types, extra fields).
- When defining configuration or constant objects where preserving literals matters (discriminated unions, exact values).

When not to use it:
- If you intentionally want to widen or change the inferred type to the target type, `as` may be more appropriate — but use with caution.

Quick exercise (try in your editor):

```typescript
type Color = 'red' | 'blue' | 'green';

const palette = { primary: 'red', secondary: 'pink' } satisfies { primary: Color };
// Should error because 'pink' is not a valid `Color` for `secondary` if you later rely on it.
```


### 5. Exercises

#### Exercise 1: DOM Element Casting
Write a TypeScript function that:
- Gets a button element by ID "submit-btn"
- Casts it to `HTMLButtonElement`
- Adds a click event listener that logs "Button clicked!"

```typescript
function setupButton(): void {
  // TODO: Get the element, cast it, and add the event listener
}
```

#### Exercise 2: API Response Casting
You receive data from an API with type `unknown`. Define an interface `User` with properties `id` (number), `name` (string), and `email` (string). Cast the API response to this type and log the user's email.

```typescript
interface User {
  // TODO: Define the interface
}

const apiData: unknown = { id: 1, name: "Bob", email: "bob@example.com" };

// TODO: Cast apiData to User type and log the email
```

#### Exercise 3: Type Guard vs. Casting
Refactor the following code to use a type guard instead of casting:

```typescript
// ❌ Current (BAD - uses casting)
function getLength(value: string | number): number {
  return (value as string).length; // This will crash if value is a number!
}

// ✅ TODO: Rewrite using a type guard
function getLengthSafe(value: string | number): number {
  // Your code here
}
```

#### Exercise 4: Literal Type Casting
Create a type `Color` with literal values `"red"`, `"blue"`, or `"green"`. Write a function that:
- Takes a string parameter
- Casts it to the `Color` type
- Returns a message like "Color: red"

```typescript
type Color = // TODO: Define the literal type

function displayColor(colorString: string): string {
  // TODO: Cast and return message
}
```

#### Exercise 5: When NOT to Cast
Identify what's wrong with this code and provide a safer alternative:

```typescript
// ❌ Problematic code
const data: unknown = fetchUserData();
const user = data as { name: string; age: number };
console.log(user.age + 1); // Assumes age exists and is a number

// ✅ TODO: Provide a safer approach (consider using a type guard or validation)
```

## Generics

Read the following sections in the [TypeScript Generics Docs](https://www.typescriptlang.org/docs/handbook/2/generics.html)  - “Hello World of Generics”, “Working with Generic Type Variables” and “Generic Constraints”

Questions:

1. Create a function that logs a generic variable.  
2. Ensure that the variable has all the methods and properties of a string.  
3. Explain what will happen when we’ll call the function with each of the following variables:  
```ts
type myType = string | number;  
const a: myType = "a";  
const b: myType = 1;  
const c: myType;  
```

**TODO**: add questions

## Async

**TODO**: look at the old training 

## Json schemas

**JSON Schema** is a standard for describing the structure and validation rules of JSON data.

It allows you to define:
- Which properties an object can contain
- The data type of each property
- Which properties are required
- Value restrictions (minimum, maximum, patterns, etc.)
- Nested objects and arrays

JSON Schema is commonly used for:

- API validation
- Configuration files
- Form generation
- Data contracts between systems

---

### Basic Structure

A JSON Schema is itself a JSON document.

Example:

```json
{
  "type": "object",
  "properties": {
    "name": {
      "type": "string"
    },
    "age": {
      "type": "integer"
    }
  },
  "required": ["name"]
}
```

---

### Example

#### Schema

```json
{
  "type": "object",
  "properties": {
    "firstName": {
      "type": "string"
    },
    "age": {
      "type": "integer"
    },
    "isStudent": {
      "type": "boolean"
    }
  },
  "required": ["firstName"]
}
```

#### Valid JSON

```json
{
  "firstName": "John",
  "age": 25,
  "isStudent": true
}
```

#### Invalid JSON

```json
{
  "age": "25"
}
```

---

### Common Types

| Type | Example |
|--------|----------|
| string | "John" |
| integer | 25 |
| number | 25.5 |
| boolean | true |
| object | {} |
| array | [] |
| null | null |

---

### Arrays

```json
{
  "type": "object",
  "properties": {
    "tags": {
      "type": "array",
      "items": {
        "type": "string"
      }
    }
  }
}
```

---

### Nested Objects

```json
{
  "type": "object",
  "properties": {
    "address": {
      "type": "object",
      "properties": {
        "city": {
          "type": "string"
        },
        "zipCode": {
          "type": "string"
        }
      },
      "required": ["city"]
    }
  }
}
```

---

### Useful Validation Rules

#### Minimum and Maximum

```json
{
  "type": "integer",
  "minimum": 18,
  "maximum": 120
}
```

#### String Length

```json
{
  "type": "string",
  "minLength": 3,
  "maxLength": 20
}
```

#### Enum

```json
{
  "type": "string",
  "enum": ["admin", "user", "guest"]
}
```

---

### Exercise

Read the following description and create a JSON Schema for it.

A software company stores information about employees.

Each employee record contains:

- An employeeId that must be an integer.
- A fullName that must be a string.
- An email that must be a string.
- An isActive field that must be a boolean.
- A salary that must be a number.
- A department that can only be one of:
  - Engineering
  - HR
  - Sales
  - Marketing
- A skills field that contains an array of strings.
- A manager object containing:
  - managerId (integer)
  - managerName (string)
- The fields employeeId, fullName, email, and department are required.

#### Bonus Challenge

1. salary must be greater than or equal to 30,000.
2. fullName must contain at least 3 characters.
3. email must follow a valid email format.
4. skills must contain at least one item.
5. No additional properties should be allowed at the root level.

### Expected Learning Outcomes

- Define objects
- Define primitive types
- Create nested objects
- Create arrays
- Mark fields as required
- Use enums
- Add validation constraints
- Prevent unexpected properties with additionalProperties: false


## React Hooks

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

## Tanstack Router

What is routing? Routing is the process of determining what content or component to show based on the current URL.

**Exercises:** 

1. Read the [TanstackRouter docs](https://tanstack.com/router/latest/docs/overview) and implement a simple router: create 3 buttons - when clicking them, each should navigate to a different route.  
2. Change the router’s config so when inputting a non-existent URL, it will find the closest route and navigate to it.

## Tanstack Query

## Tanstack Form

Read the code Guidelines! 

# Resources

- [Pro Git Book](https://git-scm.com/book/en/v2)

