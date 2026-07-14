TS has a collection of “utility types” available out of the box that can help us create new types. Go over a list of them [here](https://www.w3schools.com/typescript/typescript_utility_types.php) and do the following exercises.

### Exercises

#### 1. `Partial<T>`
```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
}

// Task: Write a function `updateProduct` that takes a product id (number)
// and an object of fields to update. The update object should allow
// any subset of Product's fields (except id, which can't change).
function updateProduct(id: number, changes: /* your type here */) {
  // ...
}

updateProduct(1, { price: 99 }); // should work
updateProduct(1, { name: "Widget", category: "Tools" }); // should work
```
*Hint: you'll need `Partial` combined with `Omit`.*

#### 2. `Required<T>`
```typescript
interface Config {
  host: string;
  port?: number;
  timeout?: number;
}

// Task: Write a function `connect` that only accepts a fully-specified
// config — no optional fields allowed. Use Required<Config> as the param type,
// then try calling it with a config missing `timeout` to see the error.
function connect(config: /* your type here */) {
  console.log(`Connecting to ${config.host}:${config.port}, timeout ${config.timeout}`);
}
```

#### 3. `Record<K, V>`
```typescript
// Task: Model a permissions map for three roles: "admin", "editor", "viewer".
// Each role maps to an array of allowed action strings, e.g. ["read", "write", "delete"].
// Define the type using Record, then build the object.

type Role = "admin" | "editor" | "viewer";

const permissions: /* your type here */ = {
  // fill in the three roles
};
```

#### 4. `Omit<T, K>`
```typescript
interface User {
  id: number;
  email: string;
  passwordHash: string;
  createdAt: Date;
}

// Task: Create a "public" version of User that's safe to send to the frontend
// — it should NOT include passwordHash. Use Omit to define it, then write
// a function `toPublicUser(user: User)` that returns that shape.
```

#### 5. `Pick<T, K>`
```typescript
interface Order {
  id: number;
  customerId: number;
  items: string[];
  total: number;
  status: "pending" | "shipped" | "delivered";
  shippingAddress: string;
}

// Task: Define an "OrderSummary" type that includes only id, total, and status.
// Then write a function `getOrderSummary(order: Order): OrderSummary` that
// extracts just those three fields from a full Order.
```

#### 6. `Exclude<T, U>`
```typescript
type HttpMethod = "GET" | "POST" | "PUT" | "PATCH" | "DELETE";

// Task: Define a "SafeMethod" type that excludes the methods that modify data
// (POST, PUT, PATCH, DELETE) — leaving only the safe/read-only ones.
// Bonus: what's left in the type after excluding all four?
type SafeMethod = /* your type here */;
```

#### 7. `ReturnType<T>`
```typescript
function createUser(name: string, age: number) {
  return { id: Math.random(), name, age, createdAt: new Date() };
}

// Task: Without copy-pasting the shape manually, define a `NewUser` type
// that matches whatever createUser returns, using ReturnType.
type NewUser = /* your type here */;

// Then write a function `saveUser(user: NewUser)` that uses that type.
```

#### 8. `Parameters<T>`
```typescript
function sendEmail(to: string, subject: string, body: string, cc?: string[]) {
  // ...
}

// Task: Define a type `SendEmailArgs` that captures sendEmail's parameter
// tuple using Parameters. Then write a function `logEmailCall(args: SendEmailArgs)`
// that just console.logs the tuple — without redefining the parameter list.
type SendEmailArgs = /* your type here */;
```

#### 9. `Readonly<T>`
```typescript
interface AppSettings {
  apiUrl: string;
  retries: number;
}

// Task: Create a frozen config object using Readonly<AppSettings>, assign it
// real values, then try mutating one property afterward and observe the
// compile-time error TypeScript gives you.
const settings: /* your type here */ = {
  apiUrl: "https://api.example.com",
  retries: 3,
};

// settings.retries = 5; // <- try uncommenting this
```
