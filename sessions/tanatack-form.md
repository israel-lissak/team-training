# אלה הנקודות העיקריות שכדאי להכיר:

## 1. Form State

הטופס כולו מנוהל באמצעות `useForm`.

```tsx
const form = useForm({
  defaultValues: {
    firstName: "",
    age: 18,
  },
});
```

ה־`form` מחזיק:

* values
* errors
* touched
* dirty
* validating
* submitting

כל מצב הטופס נמצא במקום אחד.

---

## 2. Default Values

כמו בכל ספריית טפסים, מתחילים עם ערכי ברירת מחדל.

```tsx
defaultValues: {
  email: "",
  password: "",
}
```

חשוב לדעת:

* ניתן גם לאפס אליהם עם `form.reset()`.
* הם משמשים לחישוב `isDirty`.

---

## 3. Fields

כל שדה מנוהל בנפרד.

לדוגמה:

```tsx
<form.Field name="email">
  {(field) => (
    <input
      value={field.state.value}
      onChange={(e) => field.handleChange(e.target.value)}
    />
  )}
</form.Field>
```

לכל Field יש state משלו.

לדוגמה:

```tsx
field.state.value

field.state.meta.errors

field.state.meta.isDirty

field.state.meta.isTouched
```

---

## 4. Validation

אפשר לבצע ולידציה בכמה רמות.

### ברמת השדה

```tsx
validators: {
  onChange: ({ value }) =>
    value.length < 3
      ? "Too short"
      : undefined,
}
```

---

### ברמת הטופס

```tsx
validators: {
  onSubmit: ({ value }) => {
    if (value.password !== value.confirmPassword) {
      return {
        fields: {
          confirmPassword: "Passwords don't match",
        },
      };
    }
  },
}
```

---

## 5. Validation Timing

אפשר לבחור מתי תרוץ הוולידציה.

לדוגמה:

```tsx
validators: {
    onChange: ...
    onBlur: ...
    onSubmit: ...
    onMount: ...
}
```

זה אחד היתרונות הגדולים של הספרייה.

---

## 6. Async Validation

אפשר לבצע ולידציה אסינכרונית.

למשל:

```tsx
validators: {
  onChangeAsync: async ({ value }) => {
    const exists = await checkEmail(value);

    if (exists) {
      return "Email already exists";
    }
  }
}
```

שימושי לבדיקות מול שרת.

---

## 7. Meta State

לכל שדה יש metadata.

```tsx
field.state.meta
```

מכיל למשל:

* isDirty
* isTouched
* isValidating
* errors
* errorMap

לדוגמה:

```tsx
if (field.state.meta.isTouched) {
    ...
}
```

---

## 8. Form State

גם לטופס כולו יש State.

```tsx
form.state
```

לדוגמה:

```tsx
form.state.isDirty

form.state.isValid

form.state.isSubmitting

form.state.canSubmit
```

שימושי למשל:

```tsx
<button disabled={!form.state.canSubmit}>
    Save
</button>
```

---

## 9. Submission

השליחה מתבצעת כך:

```tsx
const form = useForm({
    onSubmit: async ({ value }) => {
        await api.save(value);
    },
});
```

או:

```tsx
<form
    onSubmit={(e) => {
        e.preventDefault();
        form.handleSubmit();
    }}
>
```

---

## 10. Type Safety

אחד היתרונות הגדולים.

```tsx
defaultValues: {
    age: 20,
}
```

כעת:

```tsx
field.state.value
```

יהיה מסוג

```tsx
number
```

ולא `any`.

TypeScript מסיק את כל סוגי השדות.

---

## 11. Nested Objects

נתמך בצורה טבעית.

```tsx
defaultValues: {
    user: {
        name: "",
        email: "",
    },
}
```

השדה:

```tsx
name="user.email"
```

עובד ללא קוד נוסף.

---

## 12. Arrays

גם מערכים נתמכים.

```tsx
defaultValues: {
    phones: [
        {
            number: "",
        },
    ],
}
```

אפשר להוסיף ולמחוק איברים בצורה נוחה.

---

## 13. Reactive State

לא כל שינוי גורם לכל הטופס להתרנדר.

רק השדה שהשתנה.

זו אחת הסיבות שהספרייה מהירה יותר מטפסים שמבוססים על state גלובלי.

---

## 14. Integration עם Zod

מאוד נפוץ.

```tsx
const schema = z.object({
    email: z.string().email(),
    age: z.number(),
});
```

אפשר להשתמש בסכמה כדי לבצע ולידציה ולהבטיח תאימות בין הטופס לשרת.

---

## 15. Integration עם TanStack Query

שילוב נפוץ:

```tsx
const mutation = useMutation({
    mutationFn: saveUser,
});

const form = useForm({
    onSubmit: async ({ value }) => {
        await mutation.mutateAsync(value);
    },
});
```

כך הלוגיקה של הטופס נשארת נפרדת מהתקשורת עם השרת.

---

זו אחת הנקודות החזקות של TanStack Form.

אם הכוונה היא שברגע שהמשתמש מקליד, רכיבים אחרים ב-UI יתעדכנו מיד (למשל תצוגה מקדימה, כפתור, סיכום וכו'), יש כמה דרכים לעשות זאת.

## 16. Subscribe

במקום לקרוא ישירות ל־`form.state.values`, משתמשים במנגנון ה־`Subscribe` כדי שרק החלק הרלוונטי יתרנדר מחדש.

```tsx
<form.Subscribe
  selector={(state) => state.values.firstName}
>
  {(firstName) => (
    <h2>Hello {firstName}</h2>
  )}
</form.Subscribe>
```

ברגע שהמשתמש מקליד:

```
J
Jo
Joh
John
```

הכותרת תתעדכן בכל הקלדה.

### 2. Subscribe לכמה שדות

```tsx
<form.Subscribe
  selector={(state) => ({
    first: state.values.firstName,
    last: state.values.lastName,
  })}
>
  {(user) => (
    <h2>
      {user.first} {user.last}
    </h2>
  )}
</form.Subscribe>
```

### 3. Subscribe לכל הטופס

```tsx
<form.Subscribe
  selector={(state) => state.values}
>
  {(values) => (
    <pre>{JSON.stringify(values, null, 2)}</pre>
  )}
</form.Subscribe>
```

זה שימושי לדיבוג או לתצוגת Preview.

### 4. בתוך Field

אם אתה כבר נמצא בתוך ה־Field:

```tsx
<form.Field name="email">
  {(field) => (
    <>
      <input
        value={field.state.value}
        onChange={(e) => field.handleChange(e.target.value)}
      />

      <p>{field.state.value}</p>
    </>
  )}
</form.Field>
```

הטקסט יתעדכן בכל הקלדה.

### 5. לפי מצב הטופס

אפשר להציג מידע בהתאם למצב:

```tsx
<form.Subscribe
  selector={(state) => state.isDirty}
>
  {(isDirty) => (
    isDirty && <span>Unsaved changes</span>
  )}
</form.Subscribe>
```

או:

```tsx
<form.Subscribe
  selector={(state) => state.canSubmit}
>
  {(canSubmit) => (
    <button disabled={!canSubmit}>
      Save
    </button>
  )}
</form.Subscribe>
```

### למה להשתמש ב־Subscribe?

אפשר תאורטית לעשות:

```tsx
const values = form.state.values;
```

אבל אז הקומפוננטה שקוראת את `form.state` עלולה להתרנדר מחדש על כל שינוי במצב הטופס.

לעומת זאת:

```tsx
<form.Subscribe
  selector={(state) => state.values.firstName}
>
```

מאזין **רק** לערך `firstName`, ולכן רק החלק הזה יתעדכן כשהוא משתנה. זה אחד היתרונות המרכזיים של TanStack Form מבחינת ביצועים.

### דוגמה אמיתית: כרטיס Preview שמתעדכן בזמן אמת

```tsx
<>
  <form.Field name="name">
    {(field) => (
      <input
        value={field.state.value}
        onChange={(e) => field.handleChange(e.target.value)}
      />
    )}
  </form.Field>

  <form.Field name="email">
    {(field) => (
      <input
        value={field.state.value}
        onChange={(e) => field.handleChange(e.target.value)}
      />
    )}
  </form.Field>

  <form.Subscribe
    selector={(state) => ({
      name: state.values.name,
      email: state.values.email,
    })}
  >
    {(user) => (
      <div>
        <h3>{user.name}</h3>
        <p>{user.email}</p>
      </div>
    )}
  </form.Subscribe>
</>
```

בכל הקלדה באחד מהשדות, אזור ה־Preview יתעדכן מיד, בלי לגרום לכל הטופס להתרנדר מחדש. זו הגישה המומלצת כאשר רוצים להציג שינויים בזמן אמת במקומות שונים ב־UI.
