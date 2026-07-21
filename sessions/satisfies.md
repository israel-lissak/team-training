# satisfies

Read this article: [TypeScript satisfies](https://www.totaltypescript.com/clarifying-the-satisfies-operator) and answer the questions below.

The satisfies keyword allows you to check compatibility with a type, but keeps the original inferred type of the value. satisfies is a way to validate that a value satisfies a certain type - It’s only validation! satisfies does not change the inferred type of the expression!.

What does this mean in practice?

Let's look at these lines of code:

```ts
type fruit = "apple" | "banana" | "orange";
type color = "red" | "yellow" | "orange";

const myFruit: fruit = "apple";
const myFruit2 = "banana" satisfies fruit;

let myColor: color = "red";
let myColor2 = "yellow" satisfies color;
```

myFruit’s is typed with colon annotation, this means that regardless of what TypeScript infers its type to be, it will be replaced with fruit.

myFruit2 is typed with `satisfies fruit`. This means that TypeScript infers the expressions type, and then simply checks that its value conforms to the fruit type (without changing the type!).

This means that myFruit’s type is fruit, but myFruit2’s type is “banana”.

### Question 1

What are the types of myColor and myColor2? explain why.

### Question 2

What is the main difference between using `:` and `satisfies`?

A.

* `:` validates the value, while `satisfies` changes the type.

B.

* `:` replaces the inferred type with the annotated type, while `satisfies` only validates compatibility and keeps the inferred type.

C.

* They always produce exactly the same type.

D.

* `satisfies` can only be used with objects.

**Answer:** B

---

### Question 3

Given:

```ts
type Fruit = "apple" | "banana" | "orange";

const fruit = "banana" satisfies Fruit;
```

What is the type of `fruit`?

A. `string`

B. `"banana"`

C. `Fruit`

D. `never`

**Answer:** B

---

### Question 4

Given:

```ts
type Fruit = "apple" | "banana" | "orange";

const fruit: Fruit = "banana";
```

What is the type of `fruit`?

A. `"banana"`

B. `Fruit`

C. `string`

D. `never`

**Answer:** B

---

### Question 5

Will this compile?

```ts
type Fruit = "apple" | "banana";

const fruit = "orange" satisfies Fruit;
```

A. Yes

B. No

**Answer:** B

**Why?**

`satisfies` validates that the value is assignable to `Fruit`. Since `"orange"` isn't part of the union, TypeScript reports an error.

---

### Question 6

Given:

```ts
type Fruit = "apple" | "banana";

const fruit = "apple" satisfies Fruit;

function eatApple(fruit: "apple") {}

eatApple(fruit);
```

Will this compile?

A. Yes

B. No

**Answer:** A

**Why?**

The type of `fruit` remains `"apple"`, so it matches the function parameter exactly.

---

### Question 7

Given:

```ts
type Fruit = "apple" | "banana";

const fruit: Fruit = "apple";

function eatApple(fruit: "apple") {}

eatApple(fruit);
```

Will this compile?

A. Yes

B. No

**Answer:** B

**Why?**

Although the current value is `"apple"`, the variable's type is `Fruit`, which could also be `"banana"`. TypeScript therefore doesn't allow passing it to a function requiring only `"apple"`.

---

### Question 8

What is the type of `color2`?

```ts
type Color = "red" | "green" | "blue";

const color2 = "green" satisfies Color;
```

A. `Color`

B. `"green"`

C. `string`

D. `never`

**Answer:** B

---

### Question 9

Predict the types.

```ts
type Status = "loading" | "success";

const s1: Status = "loading";
const s2 = "loading" satisfies Status;
```

What are the types of `s1` and `s2`?

A.

```text
s1 -> "loading"
s2 -> Status
```

B.

```text
s1 -> Status
s2 -> "loading"
```

C.

```text
s1 -> string
s2 -> string
```

D.

```text
s1 -> Status
s2 -> Status
```

**Answer:** B

---

### Question 10

Why is `satisfies` useful?

A. It narrows every variable to `never`.

B. It keeps literal types while still checking compatibility.

C. It converts every value into the target type.

D. It disables type checking.

**Answer:** B

---

### Question 11 (Hard)

Consider:

```ts
type Fruit = "apple" | "banana";

const fruit = "banana" satisfies Fruit;

let x = fruit;
```

What is the type of `x`?

A. `"banana"`

B. `Fruit`

C. `string`

D. `never`

**Answer:** A

**Why?**

`fruit` has the literal type `"banana"`, and assigning it to another variable preserves that literal type.

---

### Bonus Challenge

Without using the TypeScript Playground, determine the types of all variables:

```ts
type Direction = "left" | "right";

const d1: Direction = "left";
const d2 = "left" satisfies Direction;

let d3 = d1;
let d4 = d2;
```

1. What is the type of `d1`?
2. What is the type of `d2`?
3. What is the type of `d3`?
4. What is the type of `d4`?
5. Which variables can be passed to a function that accepts only `"left"`?
6. Explain your reasoning.

**Answers:**

* `d1` → `Direction`
* `d2` → `"left"`
* `d3` → `Direction`
* `d4` → `"left"`

Only `d2` and `d4` can be passed to a function that accepts only `"left"`, because they retain the literal type. `d1` and `d3` have the broader union type `Direction`, which could also be `"right"`.
