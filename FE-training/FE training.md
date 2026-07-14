# What is npm?

npm is a package manager for JavaScript projects. We can use it to install, uninstall, update, and manage JS packages, run scripts, publish our own packages and track dependencies.

\`package.json\` is a JSON file located in the root of our project. It contains metadata about the project and defines:

- project name and version  
- needed dependencies for the projectscripts  
- how to publish our package  
- extra configuration  
- scripts

npm relies on package.json to install the correct dependencies, run scripts, publish packages, etc. Without it, npm cannot properly manage a project.

**Research:** What’s the difference between regular dependencies and dev dependencies?
# What is React?

React is a js library used to develop interactive web applications. Instead of manipulating the browser's DOM directly, React creates a virtual DOM in memory, where it does all the necessary manipulating, before making the changes in the browser DOM. This way, React changes only what needs to be changed and nothing else.

# What is TypeScript?

JavaScript is a loosely typed language. It can be difficult to understand what types of data are being passed around. Function parameters and variables don't have any information, so developers need to look at documentation, or guess based on the implementation. To solve this problem, TypeScript was invented. TypeScript allows specifying the types of data being passed around within the code, and has the ability to report errors when the types don't match. TypeScript uses compile time type checking, which means it checks if the specified types match before running the code, not while running the code.

**Exercise:** Create a new react project with TypeScript.

# list of subjects
- [[What are types]]
- [[Functions and arrow functions]]
- [[Objects]]
- [[Array methods]]
- [[Destructuring and spreading]]
- [[Utility types]]
- [[Enums]]
- [[Null handling]]
- [[Casting]]
- [[Generics]]
- [[Async]]
- [[Json schemas]]
- [[React Hooks]]
- [[Tanstack]]
