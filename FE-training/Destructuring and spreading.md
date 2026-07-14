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
