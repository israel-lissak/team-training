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