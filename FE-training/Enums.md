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