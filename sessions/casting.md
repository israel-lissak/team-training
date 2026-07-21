# Casting

Read the following article on type casting in TypeScript: [TypeScript Casting](https://www.w3schools.com/typescript/typescript_casting.php) and answer the questions below.

## Part 1 - Multiple Choice

### Question 1

What is type casting in TypeScript?

A. Changing a variable's value

B. Telling TypeScript to treat a value as a different type

C. Converting data at runtime

D. Creating a new object

---

### Question 2

Which syntax is the preferred way to cast a value in TypeScript?

A.

```ts
<string>value
```

B.

```ts
value as string
```

C.

```ts
cast(value, string)
```

D.

```ts
value -> string
```

---

### Question 3

Why is the `as` syntax generally preferred over angle brackets (`<Type>`)?

A. It is faster.

B. It works with JSX/TSX files.

C. It performs runtime conversion.

D. It prevents all type errors.

---

### Question 4

What does this code print?

```ts
const value: unknown = "Hello";

console.log((value as string).length);
```

A. Compilation error

B. `5`

C. `undefined`

D. Runtime error

---

### Question 5

Which type is most commonly cast into another type?

A. never

B. unknown

C. boolean

D. symbol

---

## Part 2 - True / False

### Question 6

Type casting changes the value at runtime.

True / False

---

### Question 7

The following are equivalent in most TypeScript files.

```ts
value as string
```

```ts
<string>value
```

True / False

---

### Question 8

Casting tells the compiler to trust you.

True / False

---

### Question 9

Casting guarantees that the value actually has that type at runtime.

True / False

---

### Question 10

Casting should only be used when you know more than the compiler.

True / False

---

# Part 3 - Predict the Output

### Question 11

```ts
const value: unknown = "TypeScript";

console.log((value as string).toUpperCase());
```

What is printed?

---

### Question 12

```ts
const value: unknown = 42;

console.log((value as number) + 8);
```

What is printed?

---

### Question 13

```ts
const value: unknown = "123";

console.log((value as number) + 1);
```

What is printed?

Explain why.

---

### Question 14

```ts
const value: unknown = {};

console.log((value as string).length);
```

Will it:

A. Print a number

B. Print undefined

C. Throw a compile error

D. Throw a runtime error

---

# Part 4 - Is the Cast Safe?

For each example answer:

✅ Safe

⚠️ Unsafe but compiles

❌ Does not compile

---

### Question 15

```ts
const value: unknown = "Hello";

const text = value as string;
```

---

### Question 16

```ts
const value: unknown = 123;

const text = value as string;
```

---

### Question 17

```ts
const value = "Hello";

const num = value as number;
```

---

### Question 18

```ts
const value = "Hello";

const num = value as unknown as number;
```

---

# Part 5 - Fix the Code

### Question 19

The following code compiles but is unsafe.

```ts
const value: unknown = 15;

console.log((value as string).toUpperCase());
```

How would you rewrite it safely?

---

### Question 20

Replace the cast with proper type narrowing.

```ts
function printLength(value: unknown) {
    console.log((value as string).length);
}
```

---

# Part 6 - Short Answer

### Question 21

What is the difference between:

* Type casting
* Type conversion

---

### Question 22

When should you use `unknown` instead of `any` together with casting?

---

### Question 23

Why is this dangerous?

```ts
const value = getData();

const user = value as User;
```

---

### Question 24

Give one real-world situation where casting is appropriate.

---

# Bonus

### Question 25

Why does this compile?

```ts
const value = "Hello";

const num = value as unknown as number;
```

Should you ever write code like this?

---

# Answers

### 1

✅ B

---

### 2

✅ B

---

### 3

✅ B

---

### 4

✅ B (`5`)

---

### 5

✅ B (`unknown`)

---

### 6

❌ False

Casting only affects the compiler.

---

### 7

✅ True

Except in JSX/TSX, where angle-bracket syntax conflicts with JSX.

---

### 8

✅ True

---

### 9

❌ False

It only changes the static type seen by TypeScript.

---

### 10

✅ True

---

### 11

```
TYPESCRIPT
```

---

### 12

```
50
```

---

### 13

```
1231
```

Explanation:

`as number` does **not** convert `"123"` into the number `123`; the runtime value is still a string, so `+ 1` performs string concatenation.

---

### 14

✅ B

`length` on a plain object is `undefined`; accessing it does not throw in this case.

---

### 15

✅ Safe

---

### 16

⚠️ Unsafe but compiles

The compiler trusts the cast, but the runtime value is still a number.

---

### 17

❌ Does not compile

TypeScript reports that converting a `string` directly to `number` may be a mistake because the types do not sufficiently overlap.

---

### 18

⚠️ Unsafe but compiles

Using `unknown` as an intermediate type bypasses the compiler's overlap check.

---

### 19

```ts
const value: unknown = 15;

if (typeof value === "string") {
    console.log(value.toUpperCase());
}
```

---

### 20

```ts
function printLength(value: unknown) {
    if (typeof value === "string") {
        console.log(value.length);
    }
}
```

---

### 21

* **Type casting** changes only the type that TypeScript assumes during compilation.
* **Type conversion** changes the actual value at runtime (for example, `Number("123")` or `String(42)`).

---

### 22

Because `unknown` forces you to validate or narrow the value before using it, making your code safer than `any`.

---

### 23

Because the compiler trusts the cast, but if `getData()` doesn't actually return a `User`, your code may fail at runtime when you access missing properties or methods.

---

### 24

Examples include:

* Parsing JSON after validating its shape.
* Working with DOM APIs such as `document.getElementById(...) as HTMLInputElement`.
* Handling values from third-party libraries with imprecise typings.

---

### 25

It compiles because every type can be cast to `unknown`, and `unknown` can then be cast to any other type. This effectively bypasses TypeScript's safety checks. You should avoid this pattern unless you have a very specific reason and are certain the runtime value matches the target type.
