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
