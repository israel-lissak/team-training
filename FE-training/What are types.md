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
