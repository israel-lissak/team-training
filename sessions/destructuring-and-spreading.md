# 1. Array Destructuring

פירוק מערך למשתנים.

```ts
const numbers = [10, 20, 30];

const [a, b, c] = numbers;

console.log(a); // 10
console.log(b); // 20
console.log(c); // 30
```

---

## דילוג על איברים

```ts
const numbers = [10, 20, 30];

const [first, , third] = numbers;

console.log(first); // 10
console.log(third); // 30
```

---

## ערכי ברירת מחדל

```ts
const arr = [5];

const [a, b = 100] = arr;

console.log(a); // 5
console.log(b); // 100
```

---

## Rest בתוך Destructuring

```ts
const arr = [1, 2, 3, 4, 5];

const [first, ...rest] = arr;

console.log(first); // 1
console.log(rest); // [2,3,4,5]
```

---

# 2. Object Destructuring

הפירוק הנפוץ ביותר.

```ts
const user = {
    name: "Alice",
    age: 30
};

const { name, age } = user;

console.log(name);
console.log(age);
```

---

## שינוי שם המשתנה

```ts
const user = {
    name: "Alice"
};

const { name: userName } = user;

console.log(userName);
```

---

## ערך ברירת מחדל

```ts
const user = {
    name: "Alice"
};

const {
    name,
    city = "Unknown"
} = user;
```

---

## Rest באובייקטים

```ts
const user = {
    id: 1,
    name: "Alice",
    age: 30
};

const { id, ...details } = user;

console.log(details);
```

```
{
    name: "Alice",
    age: 30
}
```

---

# 3. Nested Destructuring

```ts
const user = {
    name: "Alice",
    address: {
        city: "London",
        zip: "12345"
    }
};

const {
    address: { city }
} = user;

console.log(city);
```

---

# 4. Destructuring בפרמטרים של פונקציה

אחד הדברים הכי נפוצים ב-React.

```ts
type User = {
    name: string;
    age: number;
};

function printUser({ name, age }: User) {
    console.log(name);
    console.log(age);
}
```

במקום

```ts
function printUser(user: User) {
    console.log(user.name);
    console.log(user.age);
}
```

---

# 5. Spread Operator במערכים

יצירת עותק

```ts
const arr = [1, 2, 3];

const copy = [...arr];
```

---

## חיבור מערכים

```ts
const a = [1, 2];

const b = [3, 4];

const result = [...a, ...b];
```

```
[1,2,3,4]
```

---

## הוספת איברים

```ts
const arr = [2, 3];

const newArr = [1, ...arr, 4];
```

```
[1,2,3,4]
```

---

# 6. Spread באובייקטים

הכי נפוץ ב-React.

```ts
const user = {
    name: "Alice",
    age: 30
};

const updated = {
    ...user,
    age: 31
};
```

---

## הוספת שדות

```ts
const user = {
    name: "Alice"
};

const full = {
    ...user,
    age: 30
};
```

---

## Override

```ts
const obj = {
    a: 1,
    b: 2
};

const result = {
    ...obj,
    b: 10
};
```

```
{
    a:1,
    b:10
}
```

**השדה האחרון מנצח.**

---

# 7. Rest vs Spread

זו אחת השאלות הכי נפוצות בראיונות.

## Spread

פורס ערכים.

```ts
const arr = [1,2,3];

const copy = [...arr];
```

---

## Rest

אוסף ערכים.

```ts
const [first, ...rest] = arr;
```

---

אפשר לזכור:

```
Spread
1 -> 1 2 3

Rest
1 2 3 -> Array
```

---

# 8. Destructuring עם Types

```ts
type User = {
    name: string;
    age: number;
};

const user: User = {
    name: "Alice",
    age: 20
};

const { name } = user;
```

TypeScript מסיק:

```
name: string
```

---

# 9. Spread עם Types

```ts
type User = {
    name: string;
    age: number;
};

const user: User = {
    name: "Alice",
    age: 20
};

const updated = {
    ...user,
    age: 21
};
```

TypeScript מסיק:

```
{
    name: string;
    age: number;
}
```

---

# 10. Immutable Updates

זה השימוש הכי חשוב ב-React.

לא נכון:

```ts
user.age = 31;
```

נכון:

```ts
const updated = {
    ...user,
    age: 31
};
```

אותו דבר עם מערכים.

```ts
const updated = [...items, newItem];
```

במקום

```ts
items.push(newItem);
```

---

# 11. Destructuring ב-React

Props

```tsx
type Props = {
    title: string;
};

function Card({ title }: Props) {
    return <h1>{title}</h1>;
}
```

---

State

```tsx
const [count, setCount] = useState(0);
```

זה בעצם Array Destructuring.

---

# 12. שימוש עם פונקציות

```ts
function sum(...numbers: number[]) {
    return numbers.reduce((a, b) => a + b);
}

const values = [1,2,3];

sum(...values);
```

יש כאן שני שימושים שונים:

* `...numbers` בפרמטרים הוא **Rest Parameter** – אוסף את כל הארגומנטים למערך.
* `...values` בקריאה לפונקציה הוא **Spread** – פורס את איברי המערך לארגומנטים נפרדים.

---

# 13. Shallow Copy (חשוב!)

Spread **לא** יוצר עותק עמוק (Deep Copy).

```ts
const user = {
    name: "Alice",
    address: {
        city: "London"
    }
};

const copy = {
    ...user
};

copy.address.city = "Paris";

console.log(user.address.city);
```

התוצאה תהיה:

```
Paris
```

כי `address` עדיין מצביע לאותו אובייקט בזיכרון.

---

# 14. Pitfalls שכדאי להכיר

* אי אפשר לעשות Spread על `null` או `undefined`.
* Spread יוצר **עותק רדוד (Shallow Copy)** בלבד.
* באובייקטים, מאפיינים שמופיעים מאוחר יותר מחליפים מאפיינים קודמים.
* Destructuring של מאפיין שלא קיים יחזיר `undefined`, אלא אם סיפקת ערך ברירת מחדל.
* `Rest` חייב להיות האלמנט האחרון ב-Destructuring:

  ```ts
  // ❌ לא תקין
  const [...rest, last] = arr;
  ```

---

## מה חשוב במיוחד?

ודא שאתה מבין היטב את הנושאים הבאים:

* ✅ Array Destructuring
* ✅ Object Destructuring
* ✅ Nested Destructuring
* ✅ Renaming (`{ name: userName }`)
* ✅ Default Values
* ✅ Rest (`...rest`)
* ✅ Spread במערכים ובאובייקטים
* ✅ ההבדל בין Rest ל-Spread
* ✅ Immutable Updates ב-React
* ✅ Spread עם פרמטרים של פונקציות
* ✅ Shallow Copy והמשמעות שלו