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
