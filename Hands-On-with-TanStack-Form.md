# Hands-On with TanStack Form

* **Author:** [Massimiliano Attianese](https://medium.com/@BitrockIT/hands-on-with-tanstack-form-c2e217412561), Junior Front-end Developer @ Bitrock
* **Published in:** [Bitrock on Medium](https://medium.com/@BitrockIT/hands-on-with-tanstack-form-c2e217412561)
* **Date:** Oct 1, 2025 · 9 min read

**Front-end** form management is often complex: in this context, TanStack Form promises a comprehensive, agnostic and high-performance solution.

In this article, **Massimiliano Attianese**, Junior Front-end Developer at Bitrock, shares his first-hand experience.

Through practical tests, we will explore its most significant features in detail: from typing with TypeScript to the Headless and Framework Agnostic approach, to reactive state management.

The goal is to provide an honest assessment, highlighting the advantages and positive aspects that TanStack Form offers to **improve the quality of forms and the development experience in modern applications**.

---

## Introduction

The TanStack Form library offers developers the ability to manage forms completely in a simple way, ensuring security and high performance.

One of the most noteworthy features is its native **TypeScript support**. Thanks to strong typing, precise auto-completion is achieved, minimizing errors and ensuring the creation of more secure forms, significantly enhancing the development experience.

Another key feature of the library is its **“Headless and Framework Agnostic”** approach, which allows developers to manage the module logic entirely independently of their interface. This enables the creation of highly customized, reusable form components that are easily integrable into any front-end stack, promoting code modularity and maintainability.

From a performance perspective, TanStack Form stands out for its **reactive and highly granular state management** approach. This means that, with every change, only the components actually involved are updated, thus ensuring a fast and fluid interface. All of this happens without the need for boilerplate or complex abstractions, allowing for optimized performance with clear, lightweight code that remains under the developer’s full control.

As can be read in the library’s documentation, it was born from the need for a **complete solution for form management**. Frameworks often provide only basic functionalities, forcing developers to rely on different libraries or create custom implementations, increasing the risk of unsatisfactory performance, inconsistent code within the project (or across various projects), and extended development times.

The use of TanStack Form, conversely, guarantees a powerful, flexible, and easy-to-use **All-in-one solution**, with a complete set of tools to address the most common form-related issues, including:

* Reactive data binding and advanced state management
* Complex validation and centralized error handling
* Accessibility and responsive design
* Cross-platform compatibility and style customization

---

## Library Testing

I had the opportunity to directly test the library by creating two forms dedicated to user creation. The two examples differ both in structure (the **Basic Form**, simpler and more essential, and the **Advanced Form**, with a larger number of fields and more complex logic) and in the approach used for their implementation. Thanks to this type of approach, I was able to gain a comprehensive view of the library’s potential and form a personal opinion on the development experience.

### Installation

Installation is simple and immediate: as with any other library, it is sufficient to execute the `npm i @tanstack/react-form` command indicated in the documentation. What really makes the difference is the documentation itself, which guides the developer in choosing the command best suited to their environment and package manager, making the process even more linear and unambiguous.

```bash
npm install @tanstack/react-form

```

---

## Form Construction

### Form

Even in practical use, TanStack Form keeps its promise of being simple and intuitive, thanks to its **“Headless and Framework Agnostic”** approach. The documentation plays a fundamental role as it allows you to select the framework and version you are working with, offering detailed instructions and targeted examples.

In the case of the test performed, for example, the application is developed in React, and the documentation directly proposes the use of the specific hook `useForm`, which allows defining the form constant as the starting base for module construction.

```tsx
import { useForm } from '@tanstack/react-form'

const form = useForm({
  defaultValues: {
    firstName: '',
    lastName: '',
    email: '',
  },
  onSubmit: async ({ value }) => {
    // Send form data to an API
    console.log(value)
  },
})

```

In the object we create within the hook, we can define the various `formOptions`, including:

* `defaultValues`: to define the initial values of the form
* `validators`: to define the validation rules applicable to the various listeners
* `onSubmit`: post-submit execution for a valid form
* `onSubmitInvalid`: to handle errors when the form is invalid
* `transform`: to map/transform form data before submission
* `listeners`: hooks for events like `onChange`, `onBlur`, `onSubmitAsync`, etc.

The **defaultValues** is also the origin of the form’s strong typing; through it, the library manages to infer the entire form structure, automatically propagating the rules to the field components that we will create from the form constant (`form.Field`).

During form compilation, I found this feature extremely convenient as the automatic completion due to typing greatly sped up code writing. Moreover, the near real-time signaling of errors due to typos, wrong types, etc., helped prevent errors that are often difficult to spot.

### Form State

TanStack Form maintains a **centralized form state**, accessible via `form.state`. Among the most useful values are `isDirty`, `isTouched`, `isSubmitting`, and `isValid`, which allow for a coherent response to any changes. For example, you can disable the submit button until the form is valid, or show a loading indicator during an asynchronous submit. Therefore, it is possible to build reactive interfaces without having to write manual synchronization logic.

```tsx
<form.Subscribe
  selector={(state) => [state.canSubmit, state.isSubmitting]}
  children={([canSubmit, isSubmitting]) => (
    <button type="submit" disabled={!canSubmit}>
      {isSubmitting ? 'Submitting...' : 'Submit'}
    </button>
  )}
/>

```

---

## Different Approaches to Form Construction

The library is agnostic to the framework being used, as it centralizes the logic in **wrappers** for the various fields we will create (`form.Field`), and this characteristic offers us the possibility to adopt two different approaches for form construction.

### 1. Standard Approach

It consists of directly using the main TanStack Form API. In this case, the developer defines the form by creating the fields, manually managing the internal state, validations, and submission. This method offers a lot of flexibility: every aspect of the form can be customized, but it requires writing more code.

* Definition of the form constant with `useForm`.

```tsx
const form = useForm({
  defaultValues: {
    username: '',
  },
})

```

* Creation of fields with `form.Field`, managing all the input logic specifically and independently.

```tsx
<form.Field
  name="username"
  children={(field) => (
    <div>
      <label htmlFor={field.name}>Username</label>
      <input
        id={field.name}
        value={field.state.value}
        onBlur={field.handleBlur}
        onChange={(e) => field.handleChange(e.target.value)}
      />
    </div>
  )}
/>

```

### 2. Approach with useAppForm

Another possibility is the use of `useAppForm`, a wrapper created to simplify the construction of standardized forms. This approach pre-configures much of the common logic, consequently making the code more readable and consistent across different forms, reducing the risk of errors and accelerating development.

* Definition of structure and reusable components using `createFormHookContexts` and `createFormHook`:

```tsx
import { createFormHook, createFormHookContexts } from '@tanstack/react-form'

const { fieldContext, useFieldContext, formContext } = createFormHookContexts()

```

* Example of reusable input:

```tsx
const BasicTextField = ({ label }: { label: string }) => {
  const field = useFieldContext<string>()
  return (
    <div>
      <label htmlFor={field.name}>{label}</label>
      <input
        id={field.name}
        value={field.state.value}
        onBlur={field.handleBlur}
        onChange={(e) => field.handleChange(e.target.value)}
      />
      {field.state.meta.errors ? (
        <em className="error">{field.state.meta.errors.join(', ')}</em>
      ) : null}
    </div>
  )
}

```

* Definition of the form constant with `useAppForm`:

```tsx
const { useAppForm } = createFormHook({
  fieldContext,
  formContext,
  fieldComponents: { BasicTextField },
})

```

* Example of form construction with the `useAppForm` approach:

```tsx
const MyForm = () => {
  const form = useAppForm({
    defaultValues: {
      firstName: '',
    },
  })

  return (
    <form.Provider>
      <form onSubmit={(e) => { e.preventDefault(); form.handleSubmit(); }}>
        <form.Field name="firstName" children={() => <BasicTextField label="First Name" />} />
        <button type="submit">Submit</button>
      </form>
    </form.Provider>
  )
}

```

In my test, I used both methods: for the simpler form, I used the **standard approach**, while for the more complex one, I opted for the **useAppForm approach**. I found both approaches functional and suitable for specific situations, especially thanks to the abstraction of logic into the wrappers provided by the library, which allows for total control over the customization of the various inputs. I was particularly satisfied with the second approach described, as despite the form being large and complex with the presence of complex and nested fields (like arrays), after organizing the code with the construction of various reusable inputs, the form code turned out to be very clear and readable.

---

## Validation

The rules for validating the various fields can be declared in the `validators` property of the form constant, with the possibility of using dedicated libraries like **Zod** (used in the test) or **Yup**, or the validation rule can be defined in the field itself (in the `validators` property of `Form.Field`).

### Field

Within TanStack Form, every form input is represented as a **field**. By using `form.useField("field-name")`, we obtain a **form.Field object** that is strictly typed with respect to the corresponding value defined in `defaultValues`.

Every field encapsulates all the necessary logic to manage its own state. To mention some of the properties available to us:

* **`value`**: Current value of the field and automatic update when it changes.
* **`error`**: Any validation errors, derived both from the rules defined in validators and from specific listeners.
* **`listeners`**: Custom hooks like `onChange`, `onBlur`, or `onSubmitAsync`, which allow dynamic reaction to input events.
* **`mode`**: To define whether the field in question represents a single value or a list of values (**Array**).

This structure allows fields to be autonomous yet perfectly integrated into the form, maintaining the same philosophy of agnosticism.

In practice, **each field becomes an independent, typed unit**, with full control over its own logic, but at the same time an integral part of the overall form flow, ensuring security, consistency, and ease of maintenance.

### Validation and Error Handling

Each field can automatically manage its own validation, combining the rules defined in validators and any custom listeners. Any errors are exposed in `field.error` and can be shown directly in the UI. This makes it easy to distinguish between **local errors** (single field) and **global errors** (form-wide), ensuring immediate and consistent feedback to the user.

Example of field validation without using validation schemas:

```tsx
<form.Field
  name="email"
  validators={{
    onChange: ({ value }) =>
      !value
        ? 'Email is required'
        : !/^\S+@\S+$/.test(value)
          ? 'Invalid email format'
          : undefined,
  }}
  children={(field) => (
    <div>
      <input
        value={field.state.value}
        onChange={(e) => field.handleChange(e.target.value)}
      />
      {field.state.meta.errors && (
        <span className="error">{field.state.meta.errors.join(', ')}</span>
      )}
    </div>
  )}
/>

```

---

## ArrayField

After seeing how `Form.Field` makes the management of individual fields elegant and readable, the same philosophy of simplicity is found in **Array.Field**: with TanStack Form, it is surprisingly quick to define **dynamic collections of inputs**, add or remove elements, and keep them constantly synchronized with the form state. To achieve this, you just need to specify a key and `mode="array"`, and what is returned is effectively a new `Form.Field`. This means that the same API and consistency are maintained, validating and controlling every element of the array exactly as one would with a single field, but with all the flexibility of a dynamic set.

```tsx
<form.Field name="skills" mode="array">
  {(field) => {
    return (
      <div className="flex flex-col gap-5 justify-center items-start">
        {(field.state.value ?? []).map((_, i) => {
          return (
            <form.Field key={i} name={`skills[${i}]`}>
              {() => {
                return (
                  <div className="flex gap-5 items-end">
                    <form.AppField name={`skills[${i}]`} />
                  </div>
                )
              }}
            </form.Field>
          )
        })}
      </div>
    )
  }}
</form.Field>

```

---

## NestedFields

Just like for arrays, the management of **nested fields** in TanStack Form is also surprisingly straightforward. Defining nested structures does not require complicated patterns or particular configurations: simply declare the field path using **dot notation** (e.g., `user.address.city`), and the form automatically takes care of linking it to the correct state. The positive aspect is that in this case too, you are always working with **Form.Field**, maintaining the same API and the same guarantees of validation and synchronization seen for single fields and arrays. This makes it easy to model complex forms that faithfully reflect the application’s data structure.

---

## Real and Complex Cases

Thanks to the modular structure of the fields, TanStack Form easily supports more complex scenarios: **multi-step forms**, **conditional fields** that appear or change validation based on other values, **asynchronous submissions** with server error handling. The combination of secure typing, consistent API, and flexible listeners allows even advanced cases to be tackled without losing readability or control.

---

## Conclusions

TanStack Form manages to combine **secure typing, flexibility, and performance** in a surprisingly balanced way. From the single field to complex multi-step forms, every element is designed to be:

* **Autonomous, yet integrated**: fields, arrays, and nested fields share the same intuitive API.
* **Typed and controlled**: values, listeners, and errors are always typesafe.
* **Easy to integrate into the UI**: controlled, reusable, and consistent components.

Compared to other libraries, it stands out for its headless approach, optimized performance, and a consistent API that makes the construction of advanced forms **faster, safer, and more readable**.

My overall assessment is thus **positive**: the user experience is solid and natural, hinting at even greater potential when leveraged alongside the rest of the **TanStack ecosystem** (like Query, Table, or Router). In this sense, TanStack Form is not just a form library, but a piece that could become increasingly central in modern front-end development.