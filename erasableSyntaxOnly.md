
# TypeScript 5.8 Ships --erasableSyntaxOnly To Disable Enums
Author: Matt Pocock

In TypeScript 5.8, a new flag is dropping. It's called erasableSyntaxOnly.

It disables a bunch of features that I don't think should ever have been part of TypeScript.

Let's talk about it.

## What Does erasableSyntaxOnly Do?
`erasableSyntaxOnly` marks enums, namespaces and class parameter properties as errors. These pieces of syntax are not considered erasable.

So the code below would result in three errors:

```ts
// Error! Not allowed
enum Example {
  foo,
}

// Error! Not allowed
namespace OhNo {
  export const foo = 1;
}

class X {
  // Error! Not allowed
  constructor(private foo: string) {}
}

```

## What Does 'erasable' Mean?
'Erasable' syntax means that the syntax can be deleted without the runtime behaviour being affected.

Normal type annotations are erasable. Removing the type annotation below results in legal JavaScript:

```ts
const foo: string = 'foo';

const foo         = 'foo';
```

Same with function parameters:

```ts
const func = (a: string) => {}

const func = (a        ) => {}
```

Enums, namespaces and class parameter properties do not obey this rule. This makes it much harder for bundlers to work with them.

## Why Is `erasableSyntaxOnly` Being Added?
Node's recent TypeScript support, by default, only supports erasable syntax.

So turning on `erasableSyntaxOnly` is a really good option there.

But I think the TypeScript team is looking toward a future where these syntaxes will no longer be used.

There are several proposals floating to add types to JavaScript. One of the most popular is types as comments, which would allow JavaScript to treat types in code as ignorable at runtime.

This would mean that the following code would be valid JavaScript:

```ts
const x: number = 12;

const func = (a: number, b: number) => a + b;
```

This would be an enormous boon to the ecosystem, and bring us closer to TypeScript in the browser.

But the issue is that this only works with erasable syntax. Anything that requires a more complex transformation, like enums or namespaces, won't work in this model.

## What about import aliases?
Import aliases (via the paths config option) are a popular way of handling imports. However, they won't work with `erasableSyntaxOnly`.

The recommended solution is to rely on the non-TypeScript method for handling this: package.json#imports. TypeScript and Node handle this out of the box.

## When Will This Land?
This is slated to launch in TypeScript 5.8, which is due pretty soon.

You can try it right now on the TypeScript playground. Enjoy!

