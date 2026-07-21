# Null Handling

## Part 1 - Multiple Choice

### Question 1

What does the optional chaining operator (`?.`) do?

A. Throws an error if the value is null

B. Returns `false` if the value is null

C. Returns `undefined` instead of throwing when the value is `null` or `undefined`

D. Converts `null` into an empty string

---

### Question 2

Given:

```ts
const name: string | null = null;

const result = name?.toUpperCase();
```

What is the value of `result`?

A.
```ts
"NULL"
```
B.
```ts
undefined
```
C.
```ts
null
```
D.
```
Throws an exception
```
---

### Question 3

What does the nullish coalescing operator (`??`) check?

A. Whether the value is falsy

B. Whether the value is `null` or `undefined`

C. Whether the value is an empty string

D. Whether the value is `false`

---

### Question 4

Given:

```ts
const value = 0 ?? 5;
```

What is the result?

A.

```ts
5
```

B.

```ts
undefined
```

C.

```ts
0
```

D.

Throws an error

---

### Question 5

When should you use the null assertion operator (`!`)?

A. Whenever TypeScript reports an error

B. Only when you're certain the value isn't null or undefined

C. Instead of optional chaining

D. Instead of `??`

---

# Part 2 - Predict the Output

---

### Question 6

```ts
const text: string | null = null;

console.log(text?.toUpperCase());
```

What is printed?

---

### Question 7

```ts
const text: string | null = null;

console.log(text ?? "Hello");
```

What is printed?

---

### Question 8

```ts
const text: string | undefined = "";

console.log(text ?? "Default");
```

What is printed?

---

### Question 9

```ts
const age = 0;

console.log(age ?? 18);
```

What is printed?

---

### Question 10

```ts
const age = null;

console.log(age ?? 18);
```

What is printed?

---

# Part 3 - Is There a TypeScript Error?

Answer **Yes** or **No**.

---

### Question 11

```ts
const user: { name: string | null } = {
    name: null
};

user.name.toUpperCase();
```

TypeScript error?

---

### Question 12

```ts
const user: { name: string | null } = {
    name: null
};

user.name?.toUpperCase();
```

TypeScript error?

---

### Question 13

```ts
const user: { name: string | null } = {
    name: "Alice"
};

user.name!.toUpperCase();
```

TypeScript error?

---

### Question 14

```ts
const user: { name?: string } = {};

user.name.toUpperCase();
```

TypeScript error?

---

### Question 15

```ts
const user: { name?: string } = {};

user.name?.toUpperCase();
```

TypeScript error?

---

# Part 4 - Choose the Best Operator

Choose one:

* `?.`
* `??`
* `!`

---

### Question 16

You want to safely call a method that might not exist.

Operator?

---

### Question 17

You want to provide `"Unknown"` when a value is `null` or `undefined`.

Operator?

---

### Question 18

You know a value cannot be null, even though TypeScript thinks it can.

Operator?

---

### Question 19

You want to safely access

```ts
user.address.city
```

when `address` might be missing.

Operator?

---

### Question 20

You want to replace only `null` and `undefined`, **but keep** values like:

* `0`
* `false`
* `""`

Operator?

---

# Part 5 - Fix the Code

Rewrite each snippet correctly.

---

### Question 21

```ts
const user: {
    name: string | null;
} = {
    name: null
};

console.log(user.name.toUpperCase());
```

---

### Question 22

```ts
const price: number | undefined = undefined;

console.log(price);
```

Print `0` instead of `undefined`.

---

### Question 23

```ts
type User = {
    profile?: {
        email: string;
    };
};

const user: User = {};

console.log(user.profile.email);
```

Fix it.

---

### Question 24

```ts
const input: string | null = "hello";

console.log(input.toUpperCase());
```

Assume you know `input` can never be null at this point.

Rewrite it.

---

### Question 25 (Challenge)

Given:

```ts
type User = {
    name?: string | null;
};

const user: User = {};
```

Write **one line** that prints:

```
UNKNOWN
```

if the name is missing or null, otherwise prints the uppercase version of the name.

---

## ⭐ Bonus Challenge

Without using `if`, write an expression that:

```ts
const person = {
    firstName: null as string | null,
};
```

Returns:

```
"Guest"
```

when `firstName` is null, otherwise returns the uppercase version of the name.

---

החידון הזה מכסה כמעט את כל מה שמצופה לדעת על **Null Handling** ב-TypeScript:

* ✅ Optional Chaining (`?.`)
* ✅ Nullish Coalescing (`??`)
* ✅ Null Assertion (`!`)
* ✅ ההבדל בין `??` ל-`||` (באמצעות דוגמאות עם `0` ו-`""`)
* ✅ זיהוי שגיאות TypeScript
* ✅ בחירת האופרטור המתאים
* ✅ כתיבת קוד ולא רק קריאה שלו
* ✅ תרחישים מציאותיים ברמת ריאיון עבודה.
