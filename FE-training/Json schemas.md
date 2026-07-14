**JSON Schema** is a standard for describing the structure and validation rules of JSON data.

It allows you to define:
- Which properties an object can contain
- The data type of each property
- Which properties are required
- Value restrictions (minimum, maximum, patterns, etc.)
- Nested objects and arrays

JSON Schema is commonly used for:

- API validation
- Configuration files
- Form generation
- Data contracts between systems

---

### Basic Structure

A JSON Schema is itself a JSON document.

Example:

```json
{
  "type": "object",
  "properties": {
    "name": {
      "type": "string"
    },
    "age": {
      "type": "integer"
    }
  },
  "required": ["name"]
}
```

---

### Example

#### Schema

```json
{
  "type": "object",
  "properties": {
    "firstName": {
      "type": "string"
    },
    "age": {
      "type": "integer"
    },
    "isStudent": {
      "type": "boolean"
    }
  },
  "required": ["firstName"]
}
```

#### Valid JSON

```json
{
  "firstName": "John",
  "age": 25,
  "isStudent": true
}
```

#### Invalid JSON

```json
{
  "age": "25"
}
```

---

### Common Types

| Type | Example |
|--------|----------|
| string | "John" |
| integer | 25 |
| number | 25.5 |
| boolean | true |
| object | {} |
| array | [] |
| null | null |

---

### Arrays

```json
{
  "type": "object",
  "properties": {
    "tags": {
      "type": "array",
      "items": {
        "type": "string"
      }
    }
  }
}
```

---

### Nested Objects

```json
{
  "type": "object",
  "properties": {
    "address": {
      "type": "object",
      "properties": {
        "city": {
          "type": "string"
        },
        "zipCode": {
          "type": "string"
        }
      },
      "required": ["city"]
    }
  }
}
```

---

### Useful Validation Rules

#### Minimum and Maximum

```json
{
  "type": "integer",
  "minimum": 18,
  "maximum": 120
}
```

#### String Length

```json
{
  "type": "string",
  "minLength": 3,
  "maxLength": 20
}
```

#### Enum

```json
{
  "type": "string",
  "enum": ["admin", "user", "guest"]
}
```

---

### Exercise

Read the following description and create a JSON Schema for it.

A software company stores information about employees.

Each employee record contains:

- An employeeId that must be an integer.
- A fullName that must be a string.
- An email that must be a string.
- An isActive field that must be a boolean.
- A salary that must be a number.
- A department that can only be one of:
  - Engineering
  - HR
  - Sales
  - Marketing
- A skills field that contains an array of strings.
- A manager object containing:
  - managerId (integer)
  - managerName (string)
- The fields employeeId, fullName, email, and department are required.

#### Bonus Challenge

1. salary must be greater than or equal to 30,000.
2. fullName must contain at least 3 characters.
3. email must follow a valid email format.
4. skills must contain at least one item.
5. No additional properties should be allowed at the root level.

### Expected Learning Outcomes

- Define objects
- Define primitive types
- Create nested objects
- Create arrays
- Mark fields as required
- Use enums
- Add validation constraints
- Prevent unexpected properties with additionalProperties: false
